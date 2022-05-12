---
title: "This site setup"
date: 2022-05-12T19:45:40+08:00
draft: false
---

This site is built using a framework called Hugo, which is a static site generator. I now have it setup using github actions to automatically re-build the static pages whenever I push code to my Github repo.

<!--more-->

Its pretty cool to see github actions runners do the work automatically. 

For more info have a look here: https://gohugo.io/hosting-and-deployment/hosting-on-github/

Update: I made a few small tweaks to the code to enable the layout to display code blocks a little better, as the code blocks were getting cut off. In order to do this I had to change the file under themes/layouts/_default/single.html

I changed this:

                  <article class="flex-l flex-wrap justify-between mw8 center ph3">

To this (note the mw8 changed to mw9):

                  <article class="flex-l flex-wrap justify-between mw9 center ph3">

As the theme was a git sub-module, I had to disconnect it from the original github repo to make this change, I used the following as a guide: https://stackoverflow.com/questions/1759587/how-to-un-submodule-a-git-submodule