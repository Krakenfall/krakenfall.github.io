---
layout: post
title: "Continous Deployment with Azure App Service"
date: 2017-11-08
excerpt: "With Azure App Service, CI/CD is surprisingly easy to achieve. I'm going to quickly create a CD pipeline with git, Azure App Service, and VSTS."
tags: [devops, visual studio team services, team foundation server, vsts, tfs, continuous delivery, visual studio, azure, app service, azure app service]
comments: true
---

I've been digging through some Azure services lately and found something I didn't really know about. Azure App Service is one of Microsoft's Platform as a Service offerings that provides a quick way to host several types of web services, including apps written in .NET, .NET Core, Java, Ruby, Node.js, PHP, and Python. With either the Azure services API or Microsoft's VSTS extensions, continuous delivery is surprisingly easy to achieve. So let's do it. Below, I'm going to quickly set up a continuous delivery pipeline with VSTS, git, and Azure App service.

## Prerequisites

For the proceeding steps, to follow along, you'll need a few things to start. I'm going going to create a new web app, push it to GitHub, create a build and deployment in VSTS, and publish the app to Azure App Service. For that, you'll need the following:
* An Azure Subscription, trial or otherwise
* A GitHub account with a personal access token 
* A GitHub repo, checked out locally
* Build and release administration access in a VSTS project
* Visual Studio 2017 Community Edition or higher (2015 versions should work)

Most people I know that use git prefer to work with it in the command line, but if you're up for trying something new, VS2017 integration with GitHub is really nice and allows you to clone, commit, and push code from inside Visual Studio. I'll be using it here.

<p align="center">
<img src="https://i.imgur.com/2BDN9zA.png" alt="github_vs_integration" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
<sup> Cloning from GitHub in VS 2017 Community Edition </sup>
</p>

## Create a New Web App
Create a new Visual Studio project in your local git repo. For this, I'm creating a .NET MVC app called SuperBasicWebApp using the default template. 
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
Before we commit our app to remote, we need to add a gitignore for .NET apps. GitHub has a handy pre-made gitignore file for Visual Studio over at [https://raw.githubusercontent.com/github/gitignore/master/VisualStudio.gitignore](https://raw.githubusercontent.com/github/gitignore/master/VisualStudio.gitignore). Make sure it's committed and pushed to remote before you try to commit your web app. I ran into an issue where committing the gitignore file locally with the web app unstaged caused it not to be recognized. I just ended up creating the gitignore in GitHub's online editor and pulling it locally, which caused it to be recognized by Visual Studio.

Next, stage, commit, and push your app to GitHub. If you're using Visual Studio, go to View -> Team Explorer -> your repo -> Changes.

<p align="center">
<img src="https://i.imgur.com/zWU9JRC.png" alt="stage_and_commit" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
</p>

<p align="center">
<img src="https://i.imgur.com/eo25NvQ.png" alt="commit_success" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
</p>
Click on Sync and push your changes to GitHub remote.

<p align="center">
<img src="https://i.imgur.com/axp028a.png" alt="sync_and_push" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
</p>
<p align="center">
<img src="https://i.imgur.com/khifxxW.png" alt="push_success" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
</p>

## Create a Build in VSTS
Now, we're going to set up a build and release of SuperBasicWebApp in VSTS. I created the "Continuous Delivery Demo" project in my personal VSTS. Navigate over to the Builds section of the project and create a new build. To keep this simple, I'm going to use the template called "ASP.NET Core (.NET Framework)" to get started. I don't know why it has "Core" in there, but it should work.

<p align="center">
<img src="https://i.imgur.com/oemSAjT.png" alt="use_asp_net_template" style="display: block; margin: 0 auto;" height="75%" width="75%"><br>
<sup> Searching for "ASP.NET" should bring up the right template </sup>
</p>

Apply the template. You should now have a set of build tasks, including running tests, in your build definition (which is what we're creating here, if you're unfamiliar with VSTS). Click on "Process" and both name and choose "Hosted" in the agent queue drop-down on the right. You can delete the existing ugly name - let's go for simplicity for now.

<p align="center">
<img src="https://i.imgur.com/2X2ugbU.png" alt="name_and_choose_agent" style="display: block; margin: 0 auto;" height="75%" width="75%"><br>
</p>


Next, we need to give VSTS access to checkout SuperBasicWebApp. Click on "Get Sources" and choose GitHub. Click on "Use GitHub personal access token to authorize" and enter your personal access token I mentioned in the prerequisites.

<p align="center">
<img src="https://i.imgur.com/qEbypgb.png" alt="set_sources" style="display: block; margin: 0 auto;" height="75%" width="75%"><br>
</p>
<p align="center">
<img src="https://i.imgur.com/MPBHaUw.png" alt="enter_pat" style="display: block; margin: 0 auto;" height="40%" width="40%"><br>
</p>

If you don't know how to get a personal access token, sign into your GitHub account and navigate to  Profile -> Settings -> Developer Settings -> Personal Access Tokens. If you're wondering what permissions to give the token, I just gave mine notifications and repo permissions.
<p align="center">
<img src="https://i.imgur.com/me4BoWv.png" alt="github_pat" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

Once you enter the personal access token, you should be able to select your repo and branch for this build. Also, as a best practice, change "Clean" to true and "Clean options" to "All build directories." Always build with a clean workspace to ensure that what you ship is exactly what's in your source control at build time (or what's in the commit). Persistence build workspaces are a bad idea.
<p align="center">
<img src="https://i.imgur.com/Ucc23A8.png" alt="clean_workspace" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

Click on the Triggers psuedo-tab and enable Continuous Integration, which will trigger a build whenever changes are pushed to origin/master. 
<p align="center">
<img src="https://i.imgur.com/y6GoAtF.png" alt="ci_trigger" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

The last order of business is to specify the solution file to build in the "Build solution" task. The default is a recursive minimatch pattern that will build all \*.sln files found in your repo. We'll put in the solution file's path relative to the root of the repo. Unlink the "Build solution" task and click on the chain link symbol to unlink the build from the packages.config file. This is a relatively new feature of VSTS which I haven't explored yet, but I do know that the minimatch pattern will build all solutions in the repo.
<p align="center">
<img src="https://i.imgur.com/RHgHNDP.png" alt="change_build_solution" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>
Instead, click on the ellipses (...) that appears after unlinking and navigate to your solution file in the navigation window that pops up. Choose it and click OK.
<p align="center">
<img src="https://i.imgur.com/3DAr4Kr.png" alt="choose_solution" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

In the MSBuild Arguments field of the "Build solution" task, remove the last argument, ` /p:DeployIisAppPath="Default Web Site"`. We don't need this. Azure App Service uses web deploy packages to deploy ASP.NET apps. The default parameters here are for building a web deploy package, but the last part is not needed for Azure App Service.

Save and queue the build.
<p align="center">
<img src="https://i.imgur.com/5llIgsS.png" alt="build_success" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

## Create a Release in VSTS
Head on over to "Releases" and create a new release. Again for simplicity, we'll use the Azure App Service template, which should be near the top of suggestions:
<p align="center">
<img src="https://i.imgur.com/Ae2iUdd.png" alt="use_app_service_template" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>
When you apply the template, rename the the environment name to something useful. For instance, I named mine "Development", in case I wanted to clone and make "Test" and "Production" environments.
<p align="center">
<img src="https://i.imgur.com/RrBMa74.png" alt="rename_environment" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

Click on the "Add artifact" box and choose the build we set up earlier.
<p align="center">
<img src="https://i.imgur.com/fmuXBPe.png" alt="add_artifact" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>
Click on the circle at the top right corner of the artifact box and enable the continuous deployment trigger.
<p align="center">
<img src="https://i.imgur.com/8VMpmKVg.png" alt="ci_release_trigger" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

Next, click on the warning just under the "Tasks" psuedo-tab, which should read "Some settings need attention". This will prompt you to link your Azure subscription to the Release. If you have not yet linked your Azure sub, click on "Manage" and follow the prompts. Select the Azure subscription when you finish linking it, authorize it, and proceed below.

The next field asks for an App service name, but we don't have one, yet. Now is the time to head over to the Azure Portal and create an app service. Navigate to the [App Service page](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Web%2Fsites) in the Azure Portal and click on the plus sign. Choose "Web App" for the type, which you may have to search for. Click create on the next blade.

<p align="center">
<img src="https://i.imgur.com/L0e2kp9.png" alt="add_app_service" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

Fill in the details for your app. I recommend creating a resource group to use with all Azure resources related to your app or the environment (e.g. dev) so you can better manage/delete those resources. 
<p align="center">
<img src="https://i.imgur.com/Buf6w5C.png" alt="create_web_app" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>

You may need to create a new App Service Plan to continue. I created a quick service plan that's hosted in Central US and uses the Free Tier, which allows for 60min CPU time a day, for demonstration purposes. For more information and specifications for the various pricing tiers, refer to [Azure's pricing page](https://azure.microsoft.com/en-us/pricing/details/app-service/).

Create the app service and continue. The deployment of the app service may take a minute, but once it's finished, you can return to the release in VSTS, click the refresh button, and select your new app service.
<p align="center">
<img src="https://i.imgur.com/Buf6w5C.png" alt="select_app_service" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>
Save the release.

## Publish to Azure App Service
Now to the good stuff! Queue a new release and watch it go.
<p align="center">
<img src="https://i.imgur.com/Riri1IS.png" alt="create_new_release" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>
If you go back to the Releases page, you can select your build and see the history of releases.
<p align="center">
<img src="https://i.imgur.com/nshNkoV.png" alt="release_page" style="display: block; margin: 0 auto;" height="60%" width="60%"><br>
</p>
Double click on that and you can see the details of the release. Click on Logs and you can watch the stdout of release tasks while they're running. Once the release finishes, you're done! I can now navigate to [http://superbasicwebapp.azurewebsites.net](http://superbasicwebapp.azurewebsites.net) and viola! The site is up and running in Azure app service.

## Update and Deploy
Since we set up CI triggers on both the build and release, when we commit to origin/master next, our app will automatically build and deploy to Azure App Service! Pretty cool!

From here, I would highly recommend setting up branch protection on your master branch, such as only allowing code merges through reviewed pull requests and developing through feature branches. I would also recommend using App Insights to monitor your website for availability. Finally, remember that robust testing is critical to this process. If you don't have good testing, then you can quickly shoot yourself in the foot with CI/CD. Instead of providing value faster to your customers, you will instead erode your customers' trust in your product, which is incredibly difficult to come back from. Rather, CI/CD is a better way to remove the heartache from any SDLC and get the heart of development - developing!

I hope this post was useful to you. Let me know down in the comments if you have any pointers or questions. I did follow the guide as I was writing it, but I shotgunned this post, so I'm sure there's a mistake or two.

Thanks for reading and have a great year!
