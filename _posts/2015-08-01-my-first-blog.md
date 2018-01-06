---
title: my first blog
date: 2015-08-01 15:27:31
---

Hello! 

It's been a exactly one year, one month since I've worked as a professional software developer (WOO!), and boy have I obtained a plethora of knowledge! I wish I had started this blog the first day as a professional developer so I can record everything that I have learned, but at least I'm starting it now!

You're not a programmer if you don't learn something new everyday (while struggling), and today was learning how to create Github pages for a Repository using a Jekyll template. Shout out to [Muan](https://twitter.com/muanchiou), who I am borrowing this elegant template from! :) 

READY, SET, GO!

###URL Structure for Jekyll using Github Pages for a Repository

When I first started this GitHub page, I was creating it for a repository, but then realized (after I had basically finished) that I could make one as a User and didn't have to deal with this issue. However, I still learned about the differences between making Github pages for a Repository and for a User and/or Organization.

When you create a GitHub page for a *Repository*, it creates a URL structure of:  `username.github.io/project-name/`. The project name subdirectory doesn't work so smoothly using Jekyll, since the default baseurl is `/` (which is for Github Pages for a User or Organization). As a result, you must set your `baseurl` config to `/project-name` in order to render the correct url path. Now wherever you are referencing links, you must precede each link with `{{ "{{ site.baseurl " }}}}` to capture your newly config baseurl. 

For example:

When referencing CSS links, your link would look like this: 
`<link href="{{ "{{ site.baseurl " }}}}stylesheets/style.css" rel="stylesheet" type="text/css" />`

Hope that helps and prevents you from wondering why in the heck the urls aren't working as you expected! 


