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
* A GitHub repo, checked out locally 
* Access to a VSTS account
* Visual Studio 2017 Community Edition or higher (2015 versions should work)

Most people I know that use git prefer to work with it in the command line, but if you're up for trying something new, VS2017 integration with GitHub is really nice and allows you to clone, commit, and push code from inside Visual Studio. I'll be using it here.

<p align="center">
<img src="https://i.imgur.com/2BDN9zA.png" alt="github_vs_integration" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
<sup> Cloning from GitHub in VS 2017 Community Edition </sup>
</p>


## Create a New Web App
Create a new Visual Studio project in your local git repo. For this, I'm creating a .NET MVC app using the default template. 
<p align="center">
<img src="https://i.imgur.com/6xhK1ZW.png" alt="create_new_app" style="display: block; margin: 0 auto;" height="75%" width="75%"><br>
</p>

Thankfully, you can choose to generate unit tests, so I'll be that much closer to CI out of the box.

<p align="center">
<img src="https://i.imgur.com/UKgyOlr.png" alt="add_unit_tests" style="display: block; margin: 0 auto;" height="75%" width="75%"><br>
<sup> Check the box to add unit tests </sup>
</p>

Once Visual Studio finishes creating your application, let's update the title of the site and some branding to make it ours. Open the Index.cshtml file in `{SolutionDir}/{ProjectDir}/Views/Home` and update the `ViewBag.Title` and jumbotron div element contents with whatever.
```xml
@{
    ViewBag.Title = "SuperBasicWebApp";
}

<div class="jumbotron">
    <h1>Super Basic Web App</h1>
    <p class="lead">We made a basic app!</p>
    <p><a href="http://www.nyan.cat/" class="btn btn-primary btn-lg">Learn more &raquo;</a></p>
</div>

<div class="row">
    <div class="col-md-4">
        <h2>Getting started</h2>
        <p>
            ASP.NET MVC gives you a powerful, patterns-based way to build dynamic websites that
            enables a clean separation of concerns and gives you full control over markup
            for enjoyable, agile development.
        </p>
        <p><a class="btn btn-default" href="http://www.nyan.cat/">Learn more &raquo;</a></p>
    </div>
    <div class="col-md-4">
        <h2>Get more libraries</h2>
        <p>NuGet is a free Visual Studio extension that makes it easy to add, remove, and update libraries and tools in Visual Studio projects.</p>
        <p><a class="btn btn-default" href="http://www.nyan.cat/">Learn more &raquo;</a></p>
    </div>
    <div class="col-md-4">
        <h2>Web Hosting</h2>
        <p>You can easily find a web hosting company that offers the right mix of features and price for your applications.</p>
        <p><a class="btn btn-default" href="http://www.nyan.cat/">Learn more &raquo;</a></p>
    </div>
</div>
```
Finally, let's save, build, and run it before committing to make sure everything is in order. 

<p align="center">
<img src="https://i.imgur.com/8ieyBk1.png" alt="run_the_app" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
<sup> Everything looks good </sup>
</p>

## Push the App to GitHub

## Create a Release Pipeline in VSTS

## Publish to Azure App Service

## Update and Deploy
