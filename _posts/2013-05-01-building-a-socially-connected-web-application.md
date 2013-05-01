---
layout: post
comments: true
title: Building a Socially Connected Web Application
type: post
date: 2013-05-01 3:48:25
---

##Building a Socially Connected Web Application

> These days, it's hard to come across a site that doesn't allow you to sign in with Facebook, Twitter, or some other social network. We didn't want SwarmBots to be an exception, so we went ahead and set up the ability to connect with both Facebook and Twitter. Thanks to some fairly-well documented APIs, this was only mildly troublesome, and ultimately very doable.
> The first step was to figure out Facebook signin and sessions. Using the node.js REM library made this task straightfoward and allowed the first iteration of the website to have the ability to connect to Facebook.
>
> With Facebook succesfully integrated, out sights were turned to Twitter. Thankfully REM is also compatible with Twitter, so starting a connection was no problem, making that connection work with a web application on the other hand, posed some problems. The real issue stems from the different ways in which needed to make use of each of the services. With Facebook, we wanted to use the platform as a way of handling users on our site. While it may seem nice to have the same functionality through Twitter, we made the assumption that it may be better to allow users to simply senda tweet to an official SwarmBots account and participate that way.
>
> This meant that we needed to listen to a stream of tweets containing the word swarmbots. In order to do that, we needed to authorize only a single account. Under that condition, it doesn't make sense to host that functionality inside a web application where the authorized session could become a vulnerability for the functionality. In addition, our website is hosted for free on Heroku, which in order to conserev resources will put an instance to sleep and only wake it up once every 10 minutes. This meant that our stream would be offline most of the time, and that is less than ideal. In the end it was decided that we stream tweets from the local client, also being used to dispatch commands to bots, since all of the database commands would be available to it there anyway. 

{% include disqus.html %}