---
title: "Command Line Tools"
date: 2020-02-17T20:41:48-05:00
draft: true
weight: 10
---

---

Here’s a list of command line tools we typically use in local development on Macs.

## brew

Also known as Homebrew, brew is a package manager for macOS. Many other command line tools can be downloaded via brew.

[Homepage](https://brew.sh/)

## cf CLI

The cf CLI (Command Line Interface) is the command line client for Cloud Foundry, 
including the Pivotal's distribution of Cloud Foundry, PCF.

[Installation](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html#pkg-mac)

## fly CLI

The fly CLI is the command line interface to [Concourse](https://concourse-ci.org/), an open-source CI/CD tool.

[Documentation](https://concourse-ci.org/fly.html)

## git aliases

Here's a list of some of the git aliases we've used in past projects. 

```
git config --global alias.gst git status
git config --global alias.st status
git config --global alias.di diff
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
git config --global alias.sta stash
git config --global alias.llog "log --date=local"
git config --global alias.flog "log --pretty=fuller --decorate"
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
git config --global alias.lol "log --graph --decorate --oneline"
git config --global alias.lola "log --graph --decorate --oneline --all"
git config --global alias.blog "log origin/master... --left-right"
git config --global alias.ds "diff --staged"
git config --global alias.fixup "commit --fixup"
git config --global alias.squash "commit --squash"
git config --global alias.unstage "reset HEAD"
git config --global alias.rum "rebase master@{u}"
```
