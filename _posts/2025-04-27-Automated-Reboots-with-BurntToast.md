---
title: "Automated Reboot Notifications with BurntToast: A Simple Guide"
date: 2025-04-27 18:00:31 -0500
categories:
  - powershell
tags:
  - pdqconnect
  - powershell
  - burnttoast
---
## BurntToast

The BurntToast PowerShell module is a great way to display toast notifications in Windows 10 and Windows 11.

Oftentimes, I find myself wanting to notify a user to restart their computer and this module offers a creative way to do just that. There is the built-in `msg` command that will let you send a message but the window it presents feels a little bit dated and the button isn't actionable.

![img](assets/images/Pastedimage20250427180320.png)

So let's take a look and see how BurntToast can improve our workflow! The documentation for module can be found on Github at the following link:

https://github.com/Windos/BurntToast

### Installing the module and creating a protocol handler

We need a protocol handler to allow us to trigger a reboot by simply opening a specially formatted URL (like `ToastReboot://reboot`) with BurntToast. This script automates the installation of the module and creates of the protocol handler. I use PDQ Connect to run this script as SYSTEM as part of a 2 step package.

> **Note: This script modifies the registry. Please test it thoroughly in your environment to verify that it does not cause any issues.**

**Step 1**

```powershell
# This script is designed to be run as an administrator or as the SYSTEM account.

$ModuleName = 'BurntToast'
$MinimumVersion = '0.8.5'

try {
    # Check if the module is installed and at least the minimum version.
    $InstalledModule = Get-InstalledModule -Name $ModuleName -ErrorAction Stop
    if ($InstalledModule.Version -lt $MinimumVersion) {
        Write-Warning "BurntToast module is installed, but version ($($InstalledModule.Version)) is older than the minimum required ($MinimumVersion).  Consider updating."
    }
}
catch {
    Write-Warning "BurntToast module not found. Attempting to install..."
    # Check for NuGet provider, install if missing
    if ( -not ( Get-PackageProvider -ListAvailable | Where-Object Name -eq "Nuget" ) ) {
        Write-Warning "NuGet provider not found. Installing..."
        try {
            Install-PackageProvider -Name NuGet -Force -ErrorAction Stop
        }
        catch {
            Write-Error "Failed to install NuGet provider: $($_.Exception.Message)"
            exit 1  # Exit the script if NuGet provider installation fails
        }
    }

    # Install the module
    try {
        Install-Module -Name $ModuleName -Force -ErrorAction Stop
    }
    catch {
        Write-Error "Failed to install BurntToast module: $($_.Exception.Message)"
        exit 1 # Exit the script if module installation fails
    }
}


# Import the module
try {
    Import-Module -Name $ModuleName -Force -ErrorAction Stop
}
catch {
    Write-Error "Failed to import BurntToast module: $($_.Exception.Message)"
    exit 1 # Exit the script if module import fails
}


# Checking if ToastReboot:// protocol handler is present
try {
    New-PSDrive -Name HKCR -PSProvider Registry -Root HKEY_CLASSES_ROOT -ErrorAction SilentlyContinue | Out-Null
}
catch {
    Write-Error "Failed to create HKCR PSDrive: $($_.Exception.Message)"
    exit 1
}

$ProtocolHandler = Get-Item 'HKCR:\ToastReboot' -ErrorAction SilentlyContinue

if (-not $ProtocolHandler) {
    # Create handler for reboot
    try {
        New-Item -Path 'HKCR:\ToastReboot' -ItemType Directory -Force -ErrorAction Stop
        Set-ItemProperty -Path 'HKCR:\ToastReboot' -Name '(Default)' -Value 'url:ToastReboot' -Force -ErrorAction Stop
        Set-ItemProperty -Path 'HKCR:\ToastReboot' -Name 'URL Protocol' -Value '' -Force -ErrorAction Stop
        New-ItemProperty -Path 'HKCR:\ToastReboot' -Name 'EditFlags' -Value 2162688 -PropertyType DWord -Force -ErrorAction Stop
        New-Item -Path 'HKCR:\ToastReboot\Shell\Open\command' -ItemType Directory -Force -ErrorAction Stop
        Set-ItemProperty -Path 'HKCR:\ToastReboot\Shell\Open\command' -Name '(Default)' -Value "C:\Windows\System32\shutdown.exe -r -t 30" -Force -ErrorAction Stop
        Write-Host "ToastReboot protocol handler created."
    }
    catch {
        Write-Error "Failed to create ToastReboot protocol handler: $($_.Exception.Message)"
        exit 1
    }
}
```

### Use BurntToast to display a toast notification

Now we need to display the actual toast notification. I use PDQ Connect to run this as the logged on user:

> **Note: I do not add a Dismiss button. You can make assumptions on why that is the case ;)**

```powershell
# This script is meant to be run in the user context
# It will create a toast notification to inform the user that updates have been installed and prompt them to reboot or snooze the message.

try {
    # Define Toast Notification Content
    $Text1 = New-BTText -Content "Message from IT Department"
    $Text2 = New-BTText -Content "Updates have been installed on your computer. Please select if you'd like to reboot now, or snooze this message."

    # Define Buttons
    $Button = New-BTButton -Content "Snooze" -snooze -id 'SnoozeTime'
    $Button2 = New-BTButton -Content "Reboot now" -Arguments "ToastReboot:" -ActivationType Protocol

    # Define Snooze Selection Box Items
    $15Min = New-BTSelectionBoxItem -Id 15 -Content '15 minutes'
    $1Hour = New-BTSelectionBoxItem -Id 60 -Content '1 hour'
    $4Hour = New-BTSelectionBoxItem -Id 240 -Content '4 hours'
    $Items = $15Min, $1Hour, $4Hour

    # Define Snooze Selection Box
    $SelectionBox = New-BTInput -Id 'SnoozeTime' -DefaultSelectionBoxItemId 15 -Items $Items

    # Define Actions
    $Action = New-BTAction -Buttons $Button, $Button2 -inputs $SelectionBox

    # Define Binding and Visual
    $Binding = New-BTBinding -Children $Text1, $Text2
    $Visual = New-BTVisual -BindingGeneric $Binding

    # Define Content
    $Content = New-BTContent -Visual $Visual -Actions $Action -Scenario Reminder

    # Submit Notification
    Submit-BTNotification -Content $Content
}
catch {
    # Error handling
    Write-Error "Error creating/submitting toast notification: $($_.Exception.Message)"
}
```

And here is what our notification looks like! You can select the drop-down menu to select other snooze intervals.

![img](assets/images/Pastedimage20250427182916.png)

These scripts can be found on my github:

[BurntToast Scripts](https://github.com/DearingDev/toolbelt/tree/main/BurntToast)

### Ideas for improvement

While this is a straightforward notification, it hints at BurntToast’s real power: ensuring vital information _reaches_ your users. How often do important updates get buried in overloaded inboxes? BurntToast lets you cut through the clutter—prompting users about expiring passwords (no more relying on Jim to check his emails!), clearly communicating policy changes, and proactively announcing application updates. Stop relying on hope and start using BurntToast to guarantee your messages get seen.


#### Special thanks
Special shout out to Kelvin Tegelaar. His script was the inspiration for this and it can be found on his blog: https://www.cyberdrain.com/monitoring-with-powershell-notifying-users-of-windows-updates/




