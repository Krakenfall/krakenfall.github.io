---
layout: post
title: "Continous Deployment with Azure App Service"
date: 2017-11-08
excerpt: ""
tags: [devops, visual studio team services, team foundation server, vsts, tfs, continuous delivery, visual studio, azure, app service, azure app service]
comments: true
---

I've been digging through some Azure services lately and found something I didn't really know about. Azure App Service is one of Microsoft's Platform as a Service offerings that provides a quick way to host several types of web services, including apps written in .NET, .NET Core, Java, Ruby, Node.js, PHP, and Python. With either the Azure services API or Microsoft's VSTS extensions, continuous delivery is surprisingly easy to achieve. So let's do it. Below, I'm going to quickly set up a continuous delivery pipeline with VSTS, git, and Azure App service.

## Prerequisites

For the proceeding steps, to follow along, you'll need a few things to start. I'm going going to create a new web app, push it to GitHub, create a build and deployment in VSTS, and publish the app to Azure App Service. For that, you'll need the following:
* An Azure Subscription, trial or otherwise
* A GitHub account
* Access to a VSTS account
* Visual Studio 2017 Community Edition or higher (2015 versions will probably work)

## Create a New Web App

## Push the App to GitHub

## Create a Release Pipeline in VSTS

## Publish to Azure App Service
