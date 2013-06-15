---
layout: post
title: "Private docs hosting on Heroku"
date: 2012-12-31 18:00
comments: true
categories: 
---


[GitHub Pages](http://pages.github.com/) are beautiful, but if you need nice and smooth private static site hosting it is not gonna work: `gh-pages` are public even for private repos. Of course, I could have gone with S3, but for fun I hacked together [NodeJS](http://nodejs.org/)-powered [MongoDB](http://www.mongodb.org/)-backed [Heroku](http://www.heroku.com/) hosted [solution](http://static-sites-hosting.herokuapp.com/)

<!-- more -->

It allows you to upload static site in `.zip` archive. For
convenience, it uses GitHub-provided authentication. Heroku 
doesn't provide filesystem write access, so the app stores all
your stuff in a free 0.5 GB [MongoLab](https://mongolab.com) database.

Also, it provides nice command-line snippets for uploading (for CI/makefile usage, I guess):

```
cat docs.zip | curl -F "siteName=YOUR_SITE_NAME" -F "archive=@-" http://static-sites-hosting.herokuapp.com/publish/
```

Or, if you want to allow access only for your friends & yourself:

```
cat docs.zip | curl -F "siteName=YOUR_SITE_NAME" -F "users=YOUR_GITHUB_USERNAME, friend1, friend2" -F "archive=@-" http://static-sites-hosting.herokuapp.com/publish/
```

Kinda good-looking GUI done with [Twitter-Bootstrap](http://twitter.github.com/bootstrap/)

So, [heroku-static-sites-hosting](https://github.com/darvin/heroku-static-sites-hosting) on GitHub and [hosted version](http://static-sites-hosting.herokuapp.com/) on Heroku

PS: I was using [ZappaJS](https://github.com/zappajs/zappajs), CoffeeScript web-framework for initial version. I rewrote it on [ExpressJS](http://expressjs.com/), 'cause Zappa is to magical for my taste.
