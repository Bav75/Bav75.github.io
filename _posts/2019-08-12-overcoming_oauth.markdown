---
layout: post
title:      "Overcoming Oauth "
date:       2019-08-12 20:37:33 +0000
permalink:  overcoming_oauth
---


I recently finished up my first Ruby On Rails application and was  excited, and at other times overwhelmed, by the amount of choices available to me in approaching this project. One of those choices will be the highlight of today's post - integrating Oauth into your application. 

For the purposes of this article, I will be highlighting the process of integrating Oauth through StackExchange as that is the authenticator I decided to use for my Rails application. 

**Configuring Your Application For Oauth**

Your first consideration for integrating an Oauth authentication system into your app is which Oauth "framework" you'd like to work with. In the context of this post, I decided to go with the OmniAuth Gem which has a wide array of integrations (both official & community created) with a number of popular websites. As I was looking to also incorporate the StackExchange API into my app, OmniAuth was a clear choice for me as they had a built in integration (which they define as "strategies") with StackExchange. 

After adding OmniAuth to my Gemfile, I also added initialization code for OmniAuth within my config/initializers directory. Code in this directory is always run at application startup and for the purposes of OmniAuth, the code we'll look to enter configures environment variables with key information that StackExchange will need to receive from our app in order to authenticate our users. 

```

Rails.application.config.middleware.use OmniAuth::Builder do
    provider :stackexchange, ENV['CLIENT_ID'], ENV['CLIENT_SECRET'], public_key: ENV['KEY'], site: 'stackoverflow', callback_url: 'http://localhost:3000/auth/stackexchange/callback'
end

```

**Tokens, Secrets, and Authentication Identification**

I'd like to highlight a useful Gem being used in the above code called "dotenv-rails". This Gem provides useful macros for configuring environment variables and makes the process much less manual. Another highlight in the above code are the following three key words: client_id, client_secret, and key. 

These 3 keywords represent identification for my rails application which I registered on StackExchange's API site. An additional point of note on the set-up process for StackExchange was the manual configuration of a callback url. The callback url is where the authenticating server sends a request to in order to confirm it is indeed communicating with the client which it just issued an "access or authorization" token to. This token represents the authenticating server's stamp of approval for the client / user. 

**Wrapping Things Up**

Once you've properly configured OmniAuth, the last thing you'll have to do is configure your routes / views appropriately to connect to the authenticating server and to receive a callback. Below is an example fo the callback route used in my application

```

  get '/auth/:provider/callback', to: 'sessions#create'

```

You should now be able to successfully have your users authenticate themselves within your application using an external provider with Omniauth!



