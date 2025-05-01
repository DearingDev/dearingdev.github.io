---
title: "From Obsidian Vault to Static Site: Automating with PowerShell & Jekyll"
date: 2025-05-01 09517:20 -0500
categories:
  - tutorial
tags:
  - githubpages
  - obsidian
  - powershell
  - jekyll
---
> This tutorial is a continuation of my "Building a Website with GitHub Pages" post found [here](https://www.dearing.dev/posts/Building-a-Website-with-GitHub-Pages/).
{: .prompt-info }

Obsidian is a fantastic tool for note-taking and personal knowledge management. But what about sharing that knowledge? While Obsidian offers publishing option, I’m going to show you how I use a PowerShell script to synchronize my Obsidian vault to a Jekyll-powered website hosted with GitHub Pages.

## Setting up your environment

### Download Obsidian

Download the latest version of [Obsidian](https://obsidian.md/) from their website.

### Create a new vault

Create a new vault in your desired location.

### Create new directories in the vault

1. Create a folder named `posts`
2. Create a folder named `assets`
	1. Create a subfolder named `img`
3. Create a folder named `templates`

### Update Obsidian Settings

#### Files and Links

1. Change **New Link Format** to `Absolute path in vault`
2. Uncheck `Use [[Wikilinks]]`
3. Set the **Default ocation for new attachments** to be `In the folder specified below`
	1. Set the **Attachment folder path** to be `assets/img`

Your settings should look something like this:

![](assets/img/Pasted%20image%2020250430110111.png)

#### Templates

Set your **Template folder location** to `templates`

![](assets/img/Pasted%20image%2020250430113149.png)

### Create a new template

Create a new note and name it `Blog Template` or whatever you would like to call it. We are going to use this template to bootstrap our posts with the necessary frontmatter that Jekyll requires. You can start creating properties by typing `---` at the top of your new note.

![](assets/img/Pasted%20image%2020250430114942.png)

Create the following properties:
- **Title** (leave empty)
- **Date** (`{{date}} {{time:HH:mm:ss}} -0500`)
	- You can adjust the offset to match your location
- **Categories** (leave empty)
- **Tags** (leave empty)

When you are finished it should look like this:

![](assets/img/Pasted%20image%2020250430115308.png)
You can add other things to your templates if you'd like but we are going to leave it at that for now.

### Back to the settings

We are going to leverage the built in **Daily notes** plugin to help us create a new blog post and call the template we just created.

1. Change the **Date format** to `YYYY-MM-DD-`
2. (Optional:) Change the **New file location** to `drafts` (or another desired folder)
3. Change the **Template file location** to `templates/Blog Template`

![](assets/img/Pasted%20image%2020250430115905.png)

## Writing Your First Post with Obsidian

Now that all of the settings are out the way, you are ready to create your first post with Obsidian.

Open **today's daily note**.

![](assets/img/Pasted%20image%2020250430121605.png)

You will see a note similar to the one below:

![](assets/img/Pasted%20image%2020250430121706.png)

Add in your title, categories,  and tag and start writing your post!

## Using PowerShell to Sync Your Vault to GitHub

> There are definitely some better options out there, however I wanted to challenge myself to use PowerShell to automate my website deployment process.

The script automates the following tasks:

1. **Post Synchronization:** Copies posts from a local Obsidian vault directory to a remote repository.
2. **Image Synchronization:** Copies images referenced in the posts to the same repository and encodes the image file names.
3. **Stale Image Removal:** Removes images from the repository that are no longer referenced in the published posts.
4. **Git Commit and Push:** Commits the changes to the repository and optionally pushes them to a remote repository.

The script can be found on my GiHub [here](https://github.com/DearingDev/toolbelt/blob/main/obsidian_sync.ps1).

