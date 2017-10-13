---
layout: post
title: "Configuration Management with Tokenization"
date: 2017-10-12
excerpt: "We'll look at how to manage configurations of deployed applications using tokenization and the Microsoft stack."
tags: [devops, visual studio team services, team foundation server, vsts, tfs, configuration management, visual studio, slow cheetah, tokenization, configuration transforms, config transforms]
comments: true
---

You're in the Microsoft stack. Your company makes you use Outlook. VSTS/TFS is your build system. You get Visual Studio and Office 365 licenses (hopefully OneDrive too). Ok, not all of these strictly need to be true, but let's take a look at how you can manage configurations of deployed applications using Visual Studio build transformations and tokenization.

## The Short of It
The idea is that we'll be creating tokens in our configs that will be present in the build output of the application. Those tokens we can later search and replace based on variables in the deployment. This way, each build of the application is detached from environment-specific configuration details. To achieve this, we'll perform the following steps:
1. [Install the Slow Cheetah extension from NuGet](#install-slow-cheetah)
2. [Create transform files for a config](#creating-transform-files)
3. [Create transform rules](#creating-transform-rules)
4. [Build the application](#build)
5. [Add a task to the deployment to replace your tokens](#deployment)

There are several ways to do #5, some of which I'll describe, but for simplicity, I'll be showing how you can do this with VSTS. Let's get started.

## Install Slow Cheetah
First, we'll need to install the "Microsoft.VisualStudio.SlowCheetah" extension, which provides the ability to create transform files. If you know how to install NuGet packages, install it and move on to [Creating Transform Files](#creating-transform-files). Otherwise, I'm assuming you have at least Visual Studio 2015 installed and a pre-existing project. Open the NuGet package manager for your project. I've generated an out-of the box ASP.NET WebApi project for this:

<p align="center">
<img src="https://i.imgur.com/QmeCLfH.png" alt="open_nuget_management" style="display: block; margin: 0 auto;" height="50%" width="50%"><br>
<sup> Open the NuGet Package Manager</sup>
</p>

Click on the browse tab and search for "Microsoft.VisualStudio.SlowCheetah". You should find it pretty quick.

<p align="center">
<img src="https://i.imgur.com/fiKqo3k.png" alt="install_slow_cheetah" style="display: block; margin: 0 auto;" height="75%" width="75%"><br>
<sup> Install Microsoft.VisualStudio.SlowCheetah </sup>
</p>


## Creating Transform Files
Once Slow Cheetah is installed, you can now create transforms for your configs. In the Solution Explorer, right-click on your config and choose the new context menu option, "Add Config Transform."

<p align="center">
<img src="https://i.imgur.com/VX274v6.png" alt="add_transform" style="display: block; margin: 0 auto;"><br>
<sup> Add Config Transform </sup>
</p>
<p align="center">
<img src="https://i.imgur.com/OACEIC2.png" alt="config_with_transforms" style="display: block; margin: 0 auto;"><br>
</p>

Once your transform files are added, there will be one sub-file per build configuration in your project. In my project, I only have two build configurations, Debug and Release, so there are only two transforms, `Web.Debug.config` and `Web.Release.config`. These transforms will be were we put our transform rules, which will insert the specified values into the actual Web.config.


## Creating Transform Rules


```XML
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <appSettings>
    <add key="SuperImportKey" value="SOME PIG" />
    <!-- More settings -->
  </appSettings>
</configuration>
```

## Build

## Deployment
