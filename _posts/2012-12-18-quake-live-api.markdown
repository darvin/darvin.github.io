---
layout: post
title: "Quake Live API"
date: 2012-12-18 23:41
comments: true
categories: 
---


<img src="http://www.wonderlandblog.com/.a/6a00d834515f7269e2011168984a2a970c-800wi" class="post" alt="Up" width="40%" heigth="40%" />


I spent an evening investigating their API, and here are some results:

<!-- more -->

{% gist 4325640 _QuakeLiveApi.md %}

The purpose of this small research is to write [Hubot](http://hubot.github.com) a script that notifies when particular users are playing.

So, to keep things straight, clean and simple, I had to write [NodeJS](nodejs.org) a library. Because I kinda don't really like JavaScript, I had to write it on [CoffeeScript](http://coffeescript.org).

Using wonderful Pivotal's [Jasmine](http://pivotal.github.com/jasmine/) testing framework, full TDD approach and very convenient [Guard](https://github.com/kapoq/guard-jasmine-node) [Jasmine Node](https://github.com/mhevery/jasmine-node), I managed to roll out something working pretty quickly. So, that's it: [Quake Live API](https://github.com/darvin/quake-live-api-node) library for NodeJS
