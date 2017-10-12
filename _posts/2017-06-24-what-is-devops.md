---
layout: post
title: "What is DevOps?"
date: 2017-06-24
excerpt: "DevOps unfortunately means many things to many people, so I'm going to try to nail down a meaning based on my experience and exposure."
tags: [devops, software, culturechangeishard]
comments: true
---
*NOTICE: I've been lazy editing this post in GitHub, so please note it is unfinished*

My name is Clinton Barr and I'm a software developer who has worked in DevOps. Unfortunately, DevOps means many things to many people, so I'm going to try to nail down a meaning based on my experience and exposure. I apologize in advance, because I'm going to say "DevOps" practically a million times. I wish there were a few synonyms for DevOps. Maybe I'll come up with a few.

> _DevOps is a set of principles that works to tear down walls in a software organization and get people communicating._

DevOps helps smooth over pain points in the [SDLC](https://en.wikipedia.org/wiki/Systems_development_life_cycle) of applications and services. It does this by focusing on the what and how. It aims to increase development velocity through improvement in communication, automation of process, and reducing blockers by enhancing team coordination.

<p align="center">
<img src="http://imgur.com/Qp1Mglu.gif" alt="Buster_Keaton_gif_almost_hit_by_train" style="display: block; margin: 0 auto;" height="225" width="300"><br>
<sup>Let's avoid these kinds of situations, please</sup>
</p>

It's not new; DevOps has essentially been around for decades in one form or another. It's just that now, DevOps is recognizable. Now that it's known industry-wide, everyone is exposed to the idea that you should continuously integrate, universally automate, and talk to each other. Go figure. 

## [](#definition-1)Defining DevOps
At the very highest level, I think DevOps is an organizational approach that seeks to break down barriers to code quality, stability and high availability by bridging the communications and technical gap between different teams. This boils down into a few priorities, such as increasing the velocity of code delivery, reducing barriers to communication, reducing the amount of unneeded processes/procedures, and increasing the amount of useful information available to you and your team.

Practically, for a software company, this includes, but is not limited to doing the following:

*   Build a [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) pipeline for your applications with
    *   Automated, centralized builds
    *   Team code reviews
    *   Robust automated testing
    *   Automated code deployments
    *   Agile/Scrum Planning
*   Automate every procedure within reason
    *   If the procedure needs to be done more than once, strongly consider automating it
    *   If the procedure is difficult to automate, figure out why – what part of your stack is causing this pain point? How can it be improved?
    *   Treat the development of the automation as you would any app
        * Will it have a positive return on investment?
        * Make it maintainable!
        * Follow separation of concerns/single purpose principle!
        * Write tests!
*   Create a robust monitoring system for production systems to answer these questions
    *   Is the application/service up?
    *   Is the application/service functional?
    *   Is the application/service healthy?
    *   If the answer is no for any of the above, am I or the right people being notified?
*   Increase communication between teams and foster a culture of continuous improvement and trust
    *   Use team chat apps to provide open and visible communications between and within teams (For example, Slack or TFS Rooms)
    *   Include build and release notifications in a useful way (Team chat webhooks, email (eww, email))
    *   Build a robust documentation store that is visible, accessible and easy to use. Documentation is communication too.
    *   Build bridges between operations/development/business to increase the flow of requirements, timelines, understanding
    *   Identify tools/services that will assist in automating all of the above. Use the best ones.

<small><i>I may update this list as I remember items. This is mostly a list of priorities for a DevOps rollout.</i></small>

If some of this seems hard to do in your organization, don’t forget that DevOps is an approach, a set of principles that lead towards continuous improvement and trust in an organization (and, therefore, better apps/services). DevOps can be applied in small steps and is very worthy goal.

## [](#is-devops-a-job-1)DevOps as a job

Many think DevOps is not a job, that DevOps Engineers shouldn't exist. It was for me. I held DevOps positions at my first employer for over 3 years. At first glance, I'd say that if someone will pay you for it...chances are that it _is_ a job. But people care about the distinction, so I imagine the debate essentially goes down like this:

>Question: Is DevOps a job?
>
>DevOps Engineer 1: "*Of course it's a job! It's my job!*"
>
>Software Engineer 1: "*Of course it's not a job! DevOps is a way of doing things, not a career!*"
>
>IT Director 1: "*GET BACK TO WORK*"

I was at .NET Fringe 2017 this year and talked to a software engineer at a large, well-known company who told me just that to my face: DevOps is not a job. I stammered some half answer in response, which I didn't get to finish because we were doing some weird timed social thing that made us switch partners to talk to someone else. But the thing is, I actually agree. DevOps is not a job, it's a way of doing things. Why then, is DevOps Engineer a thing? Well, there are a few possibilities:

1. Some executives think DevOps is a fancy term for code-junkie sysadmins.
2. Some executives really like buzzwords and want to buy into the latest fad
3. Some executives realize their company has a major need for changes in the modus operandi, but not all parties are on deck for that change. 

I don't really agree with #1. If DevOps is a set of principles, then applying them will require change, which isn't likely to be catalyzed by creating a new position and filling it with a sysadmin. #2 will just waste money. I hope you don't find yourself under a manager like this. If you do, I hope they listen to their subject matter experts. #3 is what I encountered. One of the key points of DevOps is people over process. You need more than just improved communication in an organization, you need trust. What if that trust doesn't already exist? I realize this still isn't an optimal situation, but where I worked, my boss recognized the need to change how we do things and gave me the power to make those changes. It was awesome. The work I did directly contributed not only to my employer's trust in our development process, production hosting and recovery systems, but also the principles that DevOps represents. These are the same principles that a sufficiently mature company wouldn't have to utter the word "DevOps" to have in their operations. Some companies need a DevOps engineer. To echo the [words of Adam Jacob from Chef](https://www.youtube.com/watch?v=_DEToXsgrPc), DevOps is something that everyone can do. If you're committed to the continuous improvement of communication, process, and your company's product, you're doing DevOps too. It's rad.

<p align="center"><img src="http://imgur.com/5iNEBAj.gif" alt="they_just_like_close_calls" style="display: block; margin: 0 auto;" height="227" width="300"></p>

## [](#resources-1)Resources
Here are a few resources you can use to get started finding more about DevOps:

*   [https://blog.chef.io/2010/07/16/what-devops-means-to-me/](https://blog.chef.io/2010/07/16/what-devops-means-to-me/)
*   [http://radify.io/blog/four-principles-of-devops/](http://radify.io/blog/four-principles-of-devops/)
*   [Chef Style DevOps Kungfu - Adam Jacob Keynote](https://www.youtube.com/watch?v=_DEToXsgrPc)
*   [https://edwardcoffey.com/words/devops-more-than-automation/](https://edwardcoffey.com/words/devops-more-than-automation/)
*   [https://aws.amazon.com/devops/what-is-devops/](https://aws.amazon.com/devops/what-is-devops/)
*   PluralSight: [Continuous Integration and Continuous Delivery: The Big Picture](https://app.pluralsight.com/library/courses/continuous-integration-delivery-big-picture)
*   PluralSight: [Continuous Monitoring: The Big Picture](https://app.pluralsight.com/library/courses/continuous-monitoring-big-picture)
*   PluralSight: [Implementing DevOps in the Real World](https://app.pluralsight.com/library/courses/implementing-devops-real-world)
    
    *Note: If you don’t have PluralSight, I highly recommend using it. It has a ton of great classes on software and best practices.*
    
    <br>
    <br>
That's all I've got. Thanks for reading and I hope you decide to do some DevOps of your own!
<br>

<p align="center"><img src="http://imgur.com/APeZCFZ.gif" alt="they_just_like_close_calls" style="display: block; margin: 0 auto;" height="200" width="300"></p>

