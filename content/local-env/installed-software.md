---
title: "Installed Software"
date: 2020-02-15T21:19:02-05:00
draft: true
weight: 20
---

---

Here's a list of software we typically use in local development.


| Name | Use | <i class="fab fa-apple"></i> | <i class="fab fa-windows"></i> | Cost | Link |
| ---- | --- | --- | ------- | ---- | ---- |
| [IntelliJ IDEA Ultimate Edition](#intellij-idea-ultimate-edition) | Spring/Kotlin/Java IDE | ✔ | ✔ | [Paid](https://www.jetbrains.com/idea/buy/#commercial?billing=yearly) | [https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/) |
| [IntelliJ Rider](#intellij-rider) | .NET/F# IDE | ✔ | ✔ | [Paid](https://www.jetbrains.com/rider/buy/#personal?billing=yearly) | [https://www.jetbrains.com/rider/](https://www.jetbrains.com/rider/) |
| [Postman](#postman) | API Client | ✔ | ✔ | Free, [Paid](https://www.postman.com/pricing) | [https://www.postman.com/](https://www.postman.com/) |
| [Visual Studio Code](#visual-studio-code) | Lightweight IDE | ✔ | ✔ | Free | [https://code.visualstudio.com/](https://code.visualstudio.com/) |
| [Sublime Text 3](#sublime-text-3) | Text Editor | ✔ | ✔ | Free* | [https://www.sublimetext.com/3](https://www.sublimetext.com/3) |
| [Docker Desktop](#docker-desktop) | Docker | [✔](https://hub.docker.com/editions/community/docker-ce-desktop-mac) | [✔](https://hub.docker.com/editions/community/docker-ce-desktop-windows) | Free | [https://hub.docker.com/](https://hub.docker.com/) |
| [Sourcetree](#sourcetree) | Git GUI | ✔ | ✔ | Free* | [https://www.sourcetreeapp.com/](https://www.sourcetreeapp.com/) |
| [GitX-dev](#gitx-dev) | Git GUI | ✔ | ✘ | Free | [https://rowanj.github.io/gitx/](https://rowanj.github.io/gitx/) |
| [iTerm2](#iterm2) | Terminal | ✔ | ✘ | Free | [https://iterm2.com/index.html](https://iterm2.com/index.html) |
| [Flycut](#flycut) | Clipboard Tool | ✔ | ✘ | Free | [https://github.com/TermiT/Flycut/releases](https://github.com/TermiT/Flycut/releases) |
| [ShiftIt](#shiftit) | Window Management Tool | ✔ | ✘ | Free | [https://github.com/fikovnik/ShiftIt/releases](https://github.com/fikovnik/ShiftIt/releases) |
| [Windows Subsystem for Linux](#windows-subsystem-for-linux) | Linux shell on Windows | ✘ | ✔ | Free | [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10) |
| [Git for Windows](#git-for-windows) | Git GUI and CLI | ✘ | ✔ | Free | [https://github.com/git-for-windows/git/releases](https://github.com/git-for-windows/git/releases) |

{{% notice info %}}
Consider moving this page into its own section, and the content below into pages within that section.
{{% /notice %}}


## IntelliJ IDEA Ultimate Edition

This is our go-to IDE for Spring Boot projects. 

A free version, the Community Edition, is also available and can be used to develop and run Spring projects. 
However, it is missing useful additional features, such as:
- Database Viewer
- Integration with Spring (e.g. auto-complete properties, bean navigation, etc)
- Services Viewer (for repos with multiple runnable services)

[Click here to see a comparison of Ultimate vs Community Edition](https://www.jetbrains.com/idea/features/editions_comparison_matrix.html).

IntelliJ has a [Perpetual License Fallback](https://sales.jetbrains.com/hc/en-gb/articles/207240845-What-is-perpetual-fallback-license-),
allowing you to use a specific version of the software without an active subscription for it.

[Click here for a printable PDF of the default shortcuts we use](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf).

### IntelliJ IDEA Plugins

#### [Lombok](https://plugins.jetbrains.com/plugin/6317-lombok) 
A free plugin that adds first-class support for [Project Lombok](https://projectlombok.org/).
Since we typically use Lombok to reduce boilerplate code in Java projects, 
we need this plugin to automatically generate the code in the IDE. 

#### [Presentation Assistant](https://plugins.jetbrains.com/plugin/7345-presentation-assistant) 
A free plugin that briefly shows the shortcut commands as they are used.
Useful when pairing with new IntelliJ users.  

## IntelliJ Rider

Also from IntelliJ, Rider is a .NET IDE for Windows and Mac. It is an excellent choice for .NET Core projects.

There is a 30-day free trial, but no community edition.

## Visual Studio Code

A cross-platform, open-source, lightweight IDE. Great for web projects, 
and where you don't need the full power of IntelliJ.
It's also a free alternative to Rider if you want to try .NET projects on a Mac.

## Postman

An client for testing out APIs. Consider the paid version if you want to share API collections, 
test suites, and more.

## Sublime Text 3

A light-weight text editor. Typically used for editing files, or taking notes.
It can restore unsaved files if the program is accidentally closed. 
You could use it as an IDE, but we have better options. 
However it does open directories, which is great for config repos, or just quickly browsing. 

{{% notice note %}}
Sublime Text may be downloaded and evaluated for free, however a license must be purchased for continued use. 
There is currently no enforced time limit for the evaluation.
{{% /notice %}}

### Creating a symlink

Create a symlink so you can open a file or directory from the terminal using sublime:

```
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime
```

Usage:
```
sublime ~/workspace/config-server
```

## Docker Desktop

Docker Desktop community edition, for running docker images locally.

To get around the login page, use the links below:
- Mac: [docker-ce-desktop-mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac)
- Windows: [docker-ce-desktop-windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

## Sourcetree

A Git GUI from Atlassian that's available on Windows and Mac.

{{% notice note %}}
While free, Sourcetree asks you to create an account (via Google, Atlassian, etc) and login before you can use it.
{{% /notice %}}

## GitX-dev

Also known as just GitX, this is a lightweight Git GUI. There's other GitX forks out there, 
but this is the one we typically use.

## iTerm2

iTerm2 is an alternative to the default mac Terminal app. 
It has neat features, like highlighting all the search results in your window.

[iTerm2 Color Themes](https://iterm2colorschemes.com/) 

## Flycut

Flycut is a clipboard manager for mac. It lets you navigate between a history of copied items.
 
The shortcut is typically  ⌘ + ⇧ + V.

## ShiftIt

ShiftIt is a window management tool for mac. 
This lets you maximize a window, or place it on the left/right/bottom/top half of your screen. 

## Windows Subsystem for Linux

From the [faq](https://docs.microsoft.com/en-us/windows/wsl/faq):

> The Windows Subsystem for Linux (WSL) is a new Windows 10 feature that enables you to run native Linux command-line 
> tools directly on Windows, alongside your traditional Windows desktop and modern store apps.
>
> ...
>
> This allows those who want/need to use Bash, common Linux tools (sed, awk, etc.) and many Linux-first tools 
> (Ruby, Python, etc.) to use their toolchain on Windows.

Highly recommend having this for development on Windows.

## Git for Windows

A lightweight set of tools for using git on Windows, including:
- Git BASH - a BASH emulation that can run some common linux commands
- Git GUI - provides a graphical view of the git tree
- Shell Integration - adds a context menu item to open your folder in the Git BASH or Git GUI program

It is not a full alternative to the Windows Subsystem for Linux, but is a good option if WSL is not allowed.
