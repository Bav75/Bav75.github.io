---
layout: post
title:      "Approaching your first project"
date:       2019-06-07 18:06:06 -0400
permalink:  approaching_your_first_project
---


This post will outline methods I employed in approaching and completing my first project as part of the Flatiron School curriculum. While these may not neccessarily apply in all situations, I've found these approaches to be useful in my limited project experience, and I've noticed significant progress in my pace of development as I've iterated and continued to expand my thinking on these approaches. 

1. Understand your development environment and file structure 

A very undermentioned step to approaching any project is in the initial setup of the development environment and the overall file structure. As people grow in their development career, the "correct" file structure / environment typically becomes ingrained in them either via their company (i.e. a mandated approach to structuring your code base) or from observing enough quality and well maintained code bases over a period of time. For the new developer however, it isn't always as clear as to what is "correct" or what neccessarily makes sense for the scope of the project they're tackling. 

This however, is a critical first step in hitting the ground running on any new project. Thanks to the wonder of open source code, there's a ton of 3rd party functionality you could be leaving on the table by not having your project configured in a compatible / standardized format; not to mention potential performance impacts depending on your  handling of dependencies in your project structure. 


Below, I've outlined a few rule of thumbs that have helped me take advantage of the functionality available on the web in my projects, as well as approaches to tracking down formats / structures that work when you're completely new to a certain language or framework:

* Look for your programming language's applicable dependency manager 
In the case of my Flatiron School project - I created a command line interface program in Ruby and found myself making heavy use of bundler, the primary dependency manager for the language. Each language has it's own equivalent (i.e. Maven for Java).

Often times many of these dependency managers have built in commands for initializing an entire directory outlined to a specific, standardized format. In the case of bundler, you simply need to input the below command to initialize a directory which matches the typical structure of a Ruby Gem. 

```
bundler gem [insert_your_gem_name]
```

You'll notice that the directory contains everything you need to deploy a Ruby Gem, including: a gem spec file to declare and manage all of your dependencies, a lib folder with a built-in module namespace so that you can quickly start building out your domain models, and a fully equipped bin folder with a pre-configued console to allow you to easily test your code interactively. 

* Look up examples of other well maintained or populare code bases on Github

A simple Google search for the "most downloaded" gems / libraries / modules / dependencies (depending on your langauge) will provide you with a host of resources for some of the most heavily used 3rd party code bases applicable to you. 

In the case of my CLI project, I wanted to refactor some of my binary files and files responsible for managing my program's environment; I ended up doing a google search for the top Ruby code bases and found myself combing over the Github repo for rake (one of the top 5 most used code bases in the language). Using this as a gauge for quality, I was able to decipher what they were doing with managing their environment and pull out the pieces that were applicable to my project. 

2. Clearly understand your domain model and your desired functionality

It is essential to have a clear understanding of your program's domain model at the onset before you starting coding anything. If you don't have a roadmap for what your program's objects are ultimately representing and the functionality you expect from each object / the program overall, it will be nearly impossible to build a program that scales with future needs / use cases, let alone something that runs at all. 

* Create a simple narrative for your program and think of functionality you as an end user would want 

The method I recently implemented to map out my CLI project's domain model invovled creating a simple narrative around the program I wanted to build and supplementing that narrative with functionality I would want as the end user. I knew that I wanted to be able to scrape websites for apartment listings and ultimately have some ability to search these listings and specify my favorites for recall at a later period. 

Out of this simple narrative, along with the functionality I desired (search, easy access to favorited apartment listings), the domain model objects organicaly fell out; I ended up with a objects for scraping, for users, for apartments, and for favorites. 

Determining the functionality of each of these objects also naturally followed as I understood the narrative of the problem I was looking to solve for through every step of the project. Apartments had properties you would expect, like sqft, bedrooms, prices, etc..., and users had knowledge of their favorites, which in turn would be instances of favorite objects that have knowledge of their designated user and apartment properties. This allowed for my objects to maintain a relationship with one another without being responsible for maintaining logic outside of the scope of their domain. Building your domain model in such a way not only makes intuitive sense for other people using your code base, but also allows you to swap out or expand functionality at a later point in time. 

3. Start small and iterate quickly 

It is all too easy to get bogged in grandious planning and drawing up elaborate use cases before you've actually laid down any ground work on a new project. 

Don't be fooled though, aside from a basic narrative and some initial functionality, don't get so hung up on planning out every edge case in your project. It is much better to begin building off of that basic narrative and deliver something small and airtight (from a design perspective) that works. From there, feel free to iterate over your existing design, taking on new features / functionality one at a time. 

Rome wasn't built in a day, and neither was rake, bundler, or any of the other large code bases you likely make use of on a regular basis.

While these may seem like moot points and common sense to most experienced developers, at times, we all find ourselves violating some of the most basic principles, even when we know better. Coming back to basics like these from time to time have paid dividends to me in terms of my ability to get up and running on any new projects I've encountered. 




