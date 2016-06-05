+++
author = "Ryan King"
categories = ["Web Development"]
date = "2016-05-30T17:52:48-04:00"
description = "An overview of the inner workings of my website."
featured = ""
featuredalt = ""
featuredpath = ""
title = "See You Later, Hostgator!"
type = "post"
draft = true

+++

Since everything appears to be working smoothly, I will be giving the what/why/how behind my website.

# Hugo

The core of my website (and the current object of my affections) is [Hugo](http://gohugo.io), which is a static site generator. An SSG, unlike a Content Management System (e.g. _Wordpress_), does not "talk to the database" to get the required data (name, recent posts, etc.) and construct the page, rather, whenever a change is made to the site's content, it "generates" a "static site" then pushes the source to the server.

