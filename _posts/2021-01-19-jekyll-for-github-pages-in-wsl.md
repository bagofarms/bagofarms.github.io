---
layout: post
title:  "Setting up Jekyll for GitHub Pages in Windows Subsystem for Linux (WSL)"
date:   2021-01-18 12:30:00 -0500
tags:   development wsl windows
---

When I wanted to start my blog up again, I knew I wanted to use Jekyll.  However, it had been a long time since I had set up a Jekyll site from scratch, and I ran into some roadblocks that were mostly caused by my outdated knowledge of Jekyll and GitHub Pages.  Here's the process I used to get this site up and running in January 2021 using Windows 10 1909 and WSL 2 running Ubuntu 20.04 LTS.  These instructions are adapted from Colin Westwater's tutorial

## Checking GitHub's Compatibility

The first thing to do is to check the [GitHub Pages Dependencies](https://pages.github.com/versions/).  The instructions on this page use Jekyll 3.9.0 because that's the latest version that is compatible with GitHub Pages as of this writing.  There are other version dependencies for different gems, but I did not run into any issues with the default jekyll install.  If you start adding more gems, you'll need to refer back to that page to determine the latest version you can install.

## Installing Ruby and Jekyll

Open up a WSL 2 shell for Ubuntu 20.04 LTS and run the following to install Ruby and a couple of other dependencies:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ruby-full build-essential zlib1g-dev
```

Now we will set up environment variables so that Ruby installs gems to our home directory.  WSL has some permissions restrictions for gem installs, so this is essential:

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Now, Install `bundler` and `jekyll`.  Again, I'm using 3.9.0 here, but you should replace that with whatever the latest version GitHub Pages supports.

```bash
gem install bundler
gem install jekyll --version 3.9.0
```

## Initializing the Website

Make a directory for your website, go into that directory, and run

```bash
git init
jekyll 3.9.0 new .
```

This will inialize the Git repository and create all the folder structure and set up a site with the default "Minima" theme.  Open up the `Gemfile` that was created, and change the commented-out line containing `github-pages` to look like this:

```
gem "github-pages", "~> 209", group: :jekyll_plugins
```

I'm using `209` as the version number here because that's what the Dependency Versions page tells me to use.  It's important to lock this version number in so that you are always using the version of `github-pages` that matches with the version of `jekyll` you're using.

Use `git add` and `git commit` to commit your changes to the `master` branch.  Then, rename the branch by running.

```bash
git branch -m main
```

You can preview the site by running `jekyll serve`.  You may need to comment out the `github-pages` line in your Gemfile for this to work.

For more information, check out the [GitHub Documentation](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll).

## Modifying the Minima CSS

This is a little beyond the scope of this post, but I wanted to note this here for future reference.  If you're using the Minima theme, and you want to customize the CSS, you'll need to create `assets/main.scss` and put this at the top of the file:

```scss
---
---

@import "{{ site.theme }}";
```

This applies to any gem-based Jekyll theme.  You need to place your own `.css` or `.scss` file in the same location as the base theme.  If you load up your website in your browser, use the inspector to find out where the CSS is being loaded from.  The documentation for your theme may also tell you the proper procedure.

## Publishing the Site

Create a new GitHub repository called `soandso.github.io` where `soandso` is replaced with whatever you like.  Then, follow the instructions in GitHub to push your code to that repository.  For me, I didn't have to do any additional configuration, and my site was immediately available at the URL matching the repository name.