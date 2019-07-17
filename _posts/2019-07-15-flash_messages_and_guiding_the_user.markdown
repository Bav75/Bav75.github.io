---
layout: post
title:      "Flash messages and Guiding the User "
date:       2019-07-15 13:35:03 -0400
permalink:  flash_messages_and_guiding_the_user
---


Today we'll be looking at Flash messages; what they are, and how they'll help us create a better user experience across our web apps. 

For the purposes of this article, we'll be referring to Flash messages in the context of their implementation in Sinatra via the Sinatra Flash gem. It should be noted that Ruby On Rails also has extensive Flash message capabilities (as do most web-application frameworks) which we will not touch upon in this article. 

**What exactly are Flash messages?**

Flash messages are small notifications provided to users as they navigate throughout a web-application, and are  displayed following a redirect within the web-application. Flash messages function somewhat similarly to a Session object in that  they provide a convenient means of storing information we need to persist over a controller action, and they function unlike Sessions in that they typically persist information for the duration of a single redirect. 

The duration of the Flash message is the key draw here, as persisting information for a single redirect allows you to implement a host of user specific notifications which we will touch upon below. 

In the Sinatra Flash gem, Flash messages are implemented as hashes (like Sessions in Sinatra) and you can easily store information within the hash using the following syntax:

```
flash[:message] = "I'm a new flash message."
```

**How to use Flash messages in your web-applications**

Flash messages serve a unique purpose in helping guide users of our web-applications by allowing us to very easily provide detailed feedback to users. 

Take for example a situation where we have a sign-up form for a new user of an application. We certainly want to check the data users are providing us in the sign-up form as a first step in preventing the saving of any invalid data to our database. In this instance, let's look at a case where a user provides incomplete information (like leaving out an email address or a password). 

```
email = params[:email]
password = params[:password]

if email.empty? || password.empty?
```

Within this block of code, we can incorporate Flash messages to persist information specific to this invalid request provided by the user, thereby providing them with useful feedback rather than a redirect back to the log-in page which may otherwise leave users confused. 

```
<Controller.rb>

email = params[:email]
password = params[:password]

if email.empty? || password.empty?
flash[:message] = "Please provide all of the requested sign-up information. You provided the following: email - #{email}, password - #{password}."
redirect '/login'
```

```
<login_view.erb>

<% flash.keys.each do |type| %>
<%= flash[type] %>
```


Upon being redirected to the login route and being served the login view, the user will receive any messages / information saved in the Flash message hash. In this case we implemented a simplified way of showing the information within the Flash message, although one could easily picture more visually appealing or user friendly approaches (such as notifications which can be clicked on to close, or messages set to disappear after a number of seconds). 

A constant factor which must remain in mind while developing our web-applications is how our choices will impact the end user's experience. Keeping even small matters in mind, like providing user feedback when validating submitted form data, will help you make decisions that lead to more robust and intuitive applications which will guide, not punish, your users and lead them more quickly to the desired behavior within the application. Keeping with this principle, tools like Sinatra Flash, and Flash messages more generally, are simple to implement means of guiding and not punishing our users.  






