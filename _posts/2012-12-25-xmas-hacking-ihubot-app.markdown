---
layout: post
title: "Xmas hacking - iHubot app"
date: 2012-12-25 07:42
comments: true
categories: 
---

When all proper people are celebrating Xmas, I like a true code janky hacked together a standalone [Hubot](http://hubot.github.com) iOS app. 

<img src="http://darvin.github.com/iHubot/images/screen1-big.png" class="post" alt="Up" />

It's powered by Heroku installation with self-written [hubot-http](https://github.com/darvin/hubot-http) adapter, which just responds to `POST`s on `ihubot.herokuapp.com/hubot/tell` address. So, source of app is on [github](https://github.com/darvin/iHubot)



Stuff that I have used for this 5-hour hackaton:

<!-- more -->
 * [Hubot](http://hubot.github.com) himself. 
 * [UIBubbleTableView](http://alexbarinov.github.com/UIBubbleTableView/) for beautiful chat bubbles message list view
 * [AFNetworking](http://afnetworking.com) - probably, the best networking library for Objective-C
 * [LBYouTubeView](https://github.com/larcus94/LBYouTubeView) - to show YouTube
 * [OLImageView](https://github.com/ondalabs/OLImageView) - to show GIFs
 * [SDWebImage](https://github.com/rs/SDWebImage) - to fetch images into `UIImageView`s

