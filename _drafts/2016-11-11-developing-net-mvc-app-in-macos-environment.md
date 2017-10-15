---
layout: post
title: "Setup environment for ASP.NET development on macOS"
description: "ASP.NET MVC development is finally possible on macOS"
date: 2016-11-11
tags: ["mac", "aspnet", "setup"]
comments: true
share: false
---

Since becoming open source, ASP.NET framework has undergone great changes. The
next generation ASP.NET Core is a re-implementation of ASP.NET as a modular web
framework. One of many great features about new ASP.NET Core is cross-platform.
Now we can develop and run ASP.NET apps on Windows, Linux and macOS. I mainly
use Mac for development environment. Here is how I setup my Mac for ASP.NET
development.

## Prerequisite

- openSSL (latest version)
- .NET Core SDK (compiler and debugger)
- Visual Studio Code (recommend for IDE-like feel, really, it's awesome)
- NodeJS with npm
- Dependencies: Bower, Yeoman and generator-aspnet

## Installation

You can install [.NET Core SDK for macOS](https://www.microsoft.com/net/core#macos), [Visual Studio Code for macOS](https://code.visualstudio.com/download) from Microsoft website and openSSL by Homebrew.

I prefer installing all the prerequisites by [Homebrew](http://brew.sh/) for easy management.

```
brew update

brew install openssl

# .NET Core SDK
brew cask install dotnet

# visual studio code text editor
brew cask install visual-studio-code

# nodeJS direct install, but you can use nvm for version manager
brew install node
```

Let the terminal does its things for a minute then restart the terminal.

Now you have NodeJS installed on your Mac. You need some npm dependencies.

```
# Yeoman
npm install -g yo

# Bower
npm install -g bower

# ASP.NET app scaffold
npm install -g generator-aspnet
```

Now your environment is ready.

## Visual Studio Code

Visual Studio Code is just a text editor but very powerful. The best thing is
that it's completely free to use. Thank Microsoft! As Sublime Text, and lately
MacVim user, I was blown away. It gives you Visual Studio features like
debugging, IntelliSense (autocompletion engine), integrated terminal, Git
built-in and numerous other plugins to serve your needs.

First, you can open visual studio code directly from terminal with `code`
command. For example, open current directory with visual studio code you type
`code .`.

You also need to install C# plugin for Razor syntax highlight, C# autocompletion
and other related stuff. Open Extensions `Command-Shift-X` and search for C# to
install.

**Bonus:** If you are a Vim lover like me, and like to write code with Vim, you
might want to install Vim plugin for Visual Studio Code. Just open
`Command-Shift-X` to open Extensions and search for Vim. The plugin is very
well-crafted that it supports many Vim features such as Ex commands, marks, registers,
and many more. Refer to its [Github
page](https://github.com/VSCodeVim/Vim/blob/master/ROADMAP.md) to learn what
features are currently supported

Set `jk` for `ESC` and use Vim Ctrl key settings:

```
"vim.insertModeKeyBindings": [
        {
            "before": [ "j", "k" ],
            "after": [ "<Esc>" ]
        }
],
"vim.useCtrlKeys": true
```

*Extra Bonus:* [Dracula theme](https://draculatheme.com/)

## New App Scaffolding

For Developing ASP.NET apps on Windows, Visual Studio does the app
scaffolding for us. On macOS, we use `yo` with generator-aspnet plugin:

```
yo aspnet
```

For full options and flags, refer to `yo aspnet --help`.

After answering some questions, it automatically generate folders structure for
us.

Open Visual Studio Code from the root folder, <code>Ctrl+`</code> to toggle
integrated terminal then run this command to resolve the app's dependencies:

```
dotnet restore
```

We compile the app by:

```
dotnet build
```

Or compile and run the app by one command:

```
dotnet run
```

It will automatically compile and fire up the server for us.

### Sub generators

We can also generate classes, controllers and models with generator-aspnet. For
full list of subgenerators, refer to its [Github page](https://github.com/OmniSharp/generator-aspnet).
However, we have to hop into the right directory, otherwise it would create
files in the root directory. It is not a problem through since we have
terminal built-in, it would not take much effort. Just another thing to love Visual
Studio Code.

Generate new controller:

```
cd Controllers
yo aspnet:mvccontroller NameOfNewController
```

Generate new model:

```
cd Models
yo aspnet:class NewModellName
```

Generate new view:

```
cd Views/Model_Name
yo aspnet:mvcview Index
```

## Compiling single file

Sometimes you want to casually experiment or try to figure out a chunk of code,
you can compile a single `.cs` file. All you have to do is hopping into newly
created directory and type:

```
dotnet new
```

This command will create the `Program.cs` file and `package.json` file in current
directory. Then you resolve dependencies with:

```
dotnet restore
```

You compile and run the `Program.cs` file with

```
dotnet run
```

The output will display in integrated teminal of Visual Studio Code.

