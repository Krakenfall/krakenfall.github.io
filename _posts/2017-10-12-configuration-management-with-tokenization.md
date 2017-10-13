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
1. Install the Slow Cheetah extension from NuGet
2. Create transform files for a config
3. Create transform rules
4. Build the application
5. Add a task to the deployment to replace your tokens

There are several ways to do #5, some of which I'll describe, but for simplicity, I'll be showing how you can do this with VSTS. Let's get started.

