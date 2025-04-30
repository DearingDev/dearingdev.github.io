---
title: "From Obsidian Vault to Static Site: Automating with PowerShell & Jekyll"
date: 2025-04-30 09:17:20 -0500
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