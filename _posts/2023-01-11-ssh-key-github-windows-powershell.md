---
layout: post
title:  "Git in PowerShell on Windows: Installing Git, Generating an SSH Key, Adding It to your SSH-Agent, and Using GitHub"
date:   2023-01-11 22:00:00 -0500
tags:   development powershell windows git github ssh
---

I've been a long-time fan of Linux and MacOS for development, but I recently set a challenge for myself to set up my development environment in Windows PowerShell. One part of that is working with Git and repositories in GitHub.

## Step 1: Install and configure OpenSSH
1. Click the **Start** menu and navigate to Settings -> Apps -> Apps & Features -> Optional Features (or just press the Windows key and search for "Optional Features")
2. In the "Optional Features" window, check to make sure OpenSSH Client is in the list. Otherwise click **Add a feature** and add it.
3. Press the Windows key or open the Start menu and search for "Services"
4. In the "Services" window, double-click on "OpenSSH Authentication Agent"
5. Change "Startup type" to `Automatic`
6. Click the **Start** button under "Service Status"

## Step 2: Install Git
1. Download [Git for Windows](https://git-scm.com) and run the installer.
2. At the "Adjusting your PATH environment" step, select the "Git from the command line and also from 3rd-party software" option.
3. At the "Choosing the SSH executable" step, select "Use external OpenSSH".
4. Choose the rest of the options at your discretion.
5. (Optional) [Install Posh Git and other helpers](https://git-scm.com/book/en/v2/Appendix-A%3A-Git-in-Other-Environments-Git-in-PowerShell).
    1. Right-click PowerShell and click "Run as Administrator"
    2. Run `Set-ExecutionPolicy -Scope LocalMachine -ExecutionPolicy RemoteSigned -Force`
    3. Close that window and open PowerShell normally.
    4. Run `Install-Module posh-git -Scope CurrentUser -Force`
    5. Run `Import-Module posh-git`
    6. Run `Add-PoshGitToProfile -AllHosts` to load posh-git automatically whenever PowerShell starts.

## Step 3: Generate the SSH Key and add it to GitHub
1. Open PowerShell
2. Run `ssh-keygen -t ed25519 -C "youremail@example.com"` and follow the prompts.
3. Run `ssh-add.exe ~\.ssh\id_ed25519` to add the key to your ssh-agent.
4. Run `Get-Content ~\.ssh\id_ed25519.pub | clip`. This will copy the public key to your clipboard.
5. [Add a new SSH key in GitHub](https://github.com/settings/ssh/new) and paste the public key into the "Key" field

## References
1. https://phoenixnap.com/kb/generate-ssh-key-windows-10
2. https://dev.to/aka_anoop/how-to-enable-openssh-agent-to-access-your-github-repositories-on-windows-powershell-1ab8