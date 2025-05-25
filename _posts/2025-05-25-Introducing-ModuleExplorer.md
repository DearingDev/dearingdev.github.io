---
title: Introducing ModuleExplorer
date: 2025-05-25 14:14:43 -0500
categories:
  - powershell
tags:
  - powershell
  - module
  - pwshspectreconsole
---

## Meet ModuleExplorer: Your New Best Friend for PowerShell Module Discovery!

I'm happy to share something I've been tinkering with over the past week: **ModuleExplorer**, my first-ever PowerShell module! As someone who loves PowerShell and wants to help others (especially those just starting out) get the most out of it, I built **ModuleExplorer** to tackle a common problem: figuring out what's actually in your PowerShell modules.

### What's ModuleExplorer All About?

**ModuleExplorer** is a terminal-based user interface (TUI) that helps you browse and understand PowerShell modules, their commands, and parameters right from your terminal window. Think of it as a friendly tour guide for all the amazing cmdlets, functions, and aliases lurking inside your installed modules. 

### Why Did I Build This?

Let’s be honest...it can be a lot to take in when you’re first exploring a new PowerShell module you've and trying to understand all it can do. I wanted to create something that would make the process of discovering a module much simpler and more intuitive. My goal was to provide a straightforward, interactive way to see what modules offer and ultimately become more comfortable and efficient with PowerShell. (It was also a great excuse to play around with [PwshSpectreConsole](https://github.com/ShaunLawrie/PwshSpectreConsole).)

### What Makes ModuleExplorer Awesome?

- **Effortless Module Browsing:** You can easily navigate through all the PowerShell modules installed on your system.
- **Command Overview:** See _all_ the commands (cmdlets, functions, aliases) within a module with a quick glance.
- **Smart Searching:** Got a ton of modules? No problem! Just type in a search term and find exactly what you’ve been looking for.
- **Dive Deep into Help:** Forget bouncing around in browser tabs or typing numerous `Get-Help` commands. Get a description at a glance, or quickly view the examples, parameters, or the full help documentation with ease. 
- **Rich UI:** This is where it gets really fun! ModuleExplorer uses [PwshSpectreConsole](https://github.com/ShaunLawrie/PwshSpectreConsole) to give you a seriously enhanced and interactive experience. Expect a cleaner look, better navigation, and just a more enjoyable way to explore.

### Ready to Explore?

Here's how:

1. **Install the Module:** 
```powershell
Install-Module -Name ModuleExplorer -Scope CurrentUser
```

2.  **Launch the TUI:** 
```powershell
Show-ModuleExplorer
```

This will launch the TUI launcher, where you can view your modules and start exploring!  

### Join the PowerShell Community!

I’m genuinely excited to share **ModuleExplorer** with the PowerShell community! I really hope it helps you discover new things and makes using PowerShell more accessible and enjoyable. Give it a try, play around, and let me know what you think! Your feedback is super valuable.

The full project can be found on my [Github](https://github.com/DearingDev/ModuleExplorer).