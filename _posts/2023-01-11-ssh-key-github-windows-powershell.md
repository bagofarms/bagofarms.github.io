---
layout: post
title:  "Git in PowerShell on Windows: Installing Git, Generating an SSH Key, Adding It to your SSH-Agent, and Using GitHub"
date:   2023-01-11 22:00:00 -0500
tags:   development powershell windows git github ssh
---

I've been a long-time fan of Linux and MacOS for development, but I recently set a challenge for myself to set up my development environment in Windows PowerShell. One part of that is working with Git and repositories in GitHub.

## Step 1: Installing Git

## Step 2: Installing and configuring OpenSSH
(from https://phoenixnap.com/kb/generate-ssh-key-windows-10)
Settings -> Apps -> Apps & Features -> Optional Features (or just press Windows and type Optional Features)
Check to make sure OpenSSH Client is there. Otherwise click "Add a feature" and add it

(from https://dev.to/aka_anoop/how-to-enable-openssh-agent-to-access-your-github-repositories-on-windows-powershell-1ab8)
Press the Windows key or open the Start menu and search for "Services"
Double-click on OpenSSH Authentication Agent
Change Startup type to `Automatic`
Click **Start** under Service Status

## Step 3: Generating the SSH Key and using it with GitHub
1. Open Powershell
2. Run `ssh-keygen -t ed25519 -C "youremail@example.com"` and follow the prompts.
3. Run `ssh-add.exe ~\.ssh\id_ed25519`
4. Run `Get-Content ~\.ssh\id_ed25519.pub | clip`. This will copy the public key to your clipboard.
5. [Add a new SSH key in GitHub](https://github.com/settings/ssh/new) and paste the public key into the "Key" field