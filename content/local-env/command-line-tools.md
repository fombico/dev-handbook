---
title: "Command Line Tools"
date: 2020-02-17T20:41:48-05:00
draft: true
weight: 10
---

---

Here’s a list of command line tools we typically use in local development.

## brew

Also known as Homebrew, brew is a package manager for macOS. Many other command line tools can be downloaded via brew.

[Homepage](https://brew.sh/)

## scoop

A command line installer for Windows, similar to Homebrew. Software is installed in your home directory, 
so you don't see permission popups. It's also easy to find or delete a program, since it's all in one place.

[Homepage](https://scoop.sh/)

## cf CLI

The cf CLI (Command Line Interface) is the command line client for Cloud Foundry, 
including VMware's distribution of Cloud Foundry, Tanzu Application Service.

[Installation](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

## fly CLI

The fly CLI is the command line interface to [Concourse](https://concourse-ci.org/), an open-source CI/CD tool.

[Documentation](https://concourse-ci.org/fly.html)

To install using brew:
```
brew cask install fly
```

## minikube

A CLI tool for running a Kubernetes cluster locally. You can use this for day-to-day local Kubernetes development

[Installation](https://kubernetes.io/docs/setup/learning-environment/minikube/)

## skaffold

A CLI tool for building, pushing, and deploying Kubernetes objects.
Skaffold can automatically create containers from your apps and upload them to a local repository,
removing the need for having to run scripts/docker commands any time you want to test a change. 
It does not include a cluster component, so you'll need something like minikube to use this locally. 

[Homepage](https://skaffold.dev/)  

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

## gradlew alias

Avoid the awkward `./gradlew` with a `gw` alias. Add it to your `.bashrc` or `.zshrc` today!

```
alias gw=./gradlew
```

## watch

Great if you want to watch a command line output every few seconds.

e.g. Every 5 sec, watch my kubernetes deployments

```
watch -n 5 "kubectl get all"
```

Install with brew:
```
brew install watch
```