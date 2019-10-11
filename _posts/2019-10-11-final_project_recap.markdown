---
layout: post
title:      "Final Project Recap"
date:       2019-10-11 17:55:00 +0000
permalink:  final_project_recap
---


Today marks the submission of my final project as part of the Flatiron School curriculum! It's amazing to see the amount of personal progress I've made over the past few months and I'm excited to see where things will go from here. As a departure from more purely technical posts, today I'll be reviewing my final project and highlighting some of the design choices made, as well as walking through some debugging that took place during the deployment of app. 
# Project Overview 
Our final project focused on creating a single page application utilizing React and Redux on the frontend, and a Rails API on the backend. Additionally, we needed to deploy our frontend and backend as separate applications to heroku. In terms of project scope, I knew that I wanted my application to interface with a public API and display real data. I decided to work with Stack Overflow's public API for retrieving data on all currently available bounties on the website. 

For those unfamiliar with the bounty system on Stack Overflow, essentially whenever there's a question which someone feels hasn't been given enough attention by the community they can place a bounty on the question to help attract attention. Bounties offer reputation awards to those who correctly answer the question on top of what is normally provided and they serve as an incentive to help drive overall community participation. My application serves as a simple management tool for those looking to participate in the system and answer bounties. 

With this as the project backdrop, we should discuss an important design choice made at the beginning of any Flatiron School project: 
# Data Sourcing and Public API Interfacing 
One of the first choices you'll need to make when approaching your project is whether or not you want to make use of real data in your application. Your data source will have large implications on how you structure your application and is something you'll need to keep track of as your design the application. If you decide to go the route of using real data, you'll need to find a data source. 

Real data is plentiful (albeit some of the more interesting bits are not always free) on the web and is typically made available through Public APIs. Currently there are dozens of websites solely dedicated to tracking, documenting, and categorizing Public APIs, and I've outlined some of my personal favorite resources below: 

* https://github.com/public-apis/public-apis
* https://any-api.com/
* https://rapidapi.com/collection/list-of-free-apis

A few things to keep in mind when deciding which API to make use of:

**Look for well documented APIs**
Interfacing with a poorly documented API can be a nightmare and make your projects unncessarily difficult. Although this seems obvious, this is something easy to miss, particularly when the API is providing data you really want to work with. It's easy to get distracted by the data and end up running into issues with a poorly desing API. 

**Be respectful of the API guidelines**
Typically a Public API will clearly document how users of the API should be interfacing with it. They'll also detail what is considered malicious behavior for users of the API; typically these are things like hitting the API endpoint multiple times within a short period of time or hitting the API endpoint multiple times with the identical request parameters. Patterns of behavior like this can put uneccessary load on the API so always remember to respect the guidelines provided by the API developers. 

With the data source handled, let's discuss the deployment of the application to Heroku and the subsequent debugging process which took place. 

# Deploying to Heroku and Debugging
Getting setup with Heroku was actually a quite straightforward process. The website offers very detailed guides and getting my frontend and backend applications deployed to Heroku was pretty painless. After deploying both applications, I navigated to my frontend app on Heroku and was greeted with the application background. Everything was working fine until I attempted to login (which requires a fetch request to my backend API) to which my console responded with an error.

Running into errors during deployment is to be expected, particularly when you're moving from a local environment to a hosted environment. In order to properly debug what was going wrong, I had to start with the chrome developer console. 

There were two errors generated in my developer console:
1. A server response with status code 503
2. A CORS error stating that my fetch request had been blocked by the server (making a request from an unspecified origin). 

When confronted with multiple errors you need to start eliminating dead ends as soon as possible. As the error was triggered only when attempting to login, I knew the issue stemmed from my backend API. Additionally, I knew that in my local environment I had properly configured CORs, so something had altered in my deployment to Heroku, but what exactly? The server response of 503 was also indicative of a larger issue with the API server itself. A quick search revealed that 503 meant the server was unavailable. How could this be? An application deployed on Heroku is live hosted and the server should be automatically be up and running; something must have crashed the API server immediately at startup. 

Heroku offers a number of useful debugging tools in situations like these. The first of which is:

```
$ heroku ps
```

This command allows you to see which dynos you currently have up and running on your Heroku app. A quick inspection confirmed my dyno (and subsequently my server) had crashed immediately on startup. Something was clearly wrong with the startup processes being run by Rails upon starting up the web server. In order to better inspect Rails, I decided to access the console. Heroku conveniently allows you to run your usual rails commands using the below syntax: 

```
$ heroku run rails console 
```

Upon running this command, I was greeted with an error message stating I must use Bundler 2 or greater with my lockfile. Another quick search unveiled certain issues with Rails deployments to Heroku and Bundler versioning. After rolling back Bundler to an older version, re-bundling my application, and re-deploying to Heroku, I was successfully able to make requests to the server! 

Debugging, particularly late in the deployment of a project, can be a tricky process but there are steps that can be taken to reduce complexity. Eliminating dead ends quickly (like the CORs issue) is crucial in making progress and the power of a Google search can never be underestimated. There's also a plethora of tools available to you depending on your dev environment and deployment solution (rails commands and heroku). By mixing in these practices (and remembering to stay calm and cool) you too can deploy that final project successfully! 


