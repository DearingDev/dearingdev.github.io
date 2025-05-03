---
title: The PowerShell Pipeline
date: 2025-05-03 07:35:57 -0500
categories:
  - powershell
tags:
  - powershell
---
When I first started learning PowerShell, one of the most fascinating things to discover was the "pipeline".  I was already amazed with the data that I could gather by running commands, but using the pipeline took that to the next level. The pipeline is a really powerful and fundamental part of what makes PowerShell so great. It allows you to string together commands and manipulate the data you are working in a very easy way. Think of it like an assembly line for data!

## What is the pipeline?

At its core, the pipeline is just the `|` (pipe) character. You simply put it in between PowerShell commands and it takes the output of one command and feeds it as the input to the next.

Imagine a potter's workshop. The first step is the _throwing wheel_ (Command 1), where the potter shapes a lump of clay into a basic pot. The second step is the _trimming wheel_ (Command 2), where the potter refines the shape and removes excess clay.

The pipeline is like the **transfer mechanism** (i.e. conveyor belt) that takes the freshly shaped pot directly from the throwing wheel to the trimming wheel. The pot doesn’t need to be stored anywhere in between; it just moves directly from one process to the next.

## Example: Getting Large Files

Let's start with something relatively basic. Let's say you want to see a list of all files in your current working directory.

You can do that by running the command: `Get-ChildItem`

Now, what if you only want to see the files that are larger than 100MB? Well without the pipeline you are going to need to loop through each file and examine the size. It would look something like this:

```powershell
# Get all files
$files = Get-ChildItem

# Loop through each file
foreach ($file in $files) {
  # Check if the file is larger than 100MB
  if ($file.Length -gt 100MB) {
    # Display the file
    $File
  }
```

But with the pipeline, we can string two commands that make this process way more efficient. It's just:

```powershell
Get-ChildItem | Where-Object {$_.Length -gt 100MB}
```

See how easy that was? `Get-ChildItem` spits out the list of files. The `|` (pipeline) takes that list of files and sends it down the "conveyor belt" over to `Where-Object`, which filters it based on the criteria that was specified.

## Why is this so awesome?

- **Chaining Commands:** In the example above we chained just two commands together, but in reality you can chain even more commands together. In the 
- **Readability:** Using pipelines make your scripts easier to read because they break down complex tasks into smaller, more logical steps. In our example, above we were able to replace an entire loop with a single command.
- **Efficiency:** PowerShell is built and optimized for the pipeline, so it is often more efficient than running multiple separate commands. In the example without the pipeline, we had to store the output of `Get-ChildItem` in a variable. This takes up more system resources than just piping output directly to `Where-Object` and getting the items we were actually looking for.

## More examples:

- **Finding process using more than 100 MB of memory:
```powershell
Get-Process | Where-Object {$_.WorkingSet -gt 500MB}
```
- **Sorting files by last modified date (newest first)**
```powershell
Get-ChildItem | Sort-Object LastWriteTime -Descending
```
- **Getting a list of running services and showing just their names:**
```powershell
Get-Service | Select-Object Name
```

## Objects, not just Text

One of the most important things to realize is that PowerShell doesn't just deal with plain text like many other shells. These commands send *objects* down the pipeline.

Think of the "pottery" again. The throwing wheel doesn’t just produce a lump of clay; it creates a _shaped pot_. It is specific object with defined characteristics like its height, diameter, and thickness. These characteristics are like the object's properties. `Get-ChildItem` doesn't just spit out a text string like "myfile.txt". It sends and object that represents that file. That object has properties like `Name`, `Length`, `LastWriteTime`, and so on. That's why you can access things like `$_.Length` in the `Where-Object` example from earlier. You are accessing a property of that file.

>Want to explore what properties are available for an object? Use `Get-Member`. For example, if you run `Get-ChildItem | Get-Member`, you'll see a list of all the properties and method available on a `FileInfo` object.
{: .prompt-tip }
## Where to Go from Here?

- **Experiment:** The best way to learn is to try new thing. Play around with the different commands and see what happens when you pipe them together.
- **Read the Help:** Use `Get-Help <Command>` to learn about each command's options and what kind of output it produces.  
- **Explore Command Commands:** Get familiar with commands like this and try stringing them together and see what happens~
	- `Where-Object`: Filtering
	- `Sort-Object`: Sorting
	- `Select-Object`: Choosing specific properties
	- `ForEach-Object:`Processing each item individually
	- `ConverTo-Json`: Formatting the output as JSON

The PowerShell pipeline is an absolute game-changer when you are trying to manipulate data. Once you get the basics down, you'll be able to build powerful scripts and automate tasks with ease. Don't be afraid to dive in and see what you can accomplish.