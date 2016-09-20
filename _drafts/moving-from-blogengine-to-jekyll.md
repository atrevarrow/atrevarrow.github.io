---
layout: post
title:  "DRAFT - Moving from Blogengine.net to Jekyll"
categories: jekyll blog
---
First, install jekyll (on Windows)
Jekyll
Install as per https://davidburela.wordpress.com/2015/11/28/easily-install-jekyll-on-windows-with-3-command-prompt-entries-and-chocolatey/
(linked from https://jekyllrb.com/docs/windows/)

Then github pages
github-pages
Install as per http://jwillmer.de/blog/tutorial/how-to-install-jekyll-and-pages-gem-on-windows-10-x46
(also linked from https://jekyllrb.com/docs/windows/)
Note that installing ruby2.devkit via chocolatey seems to fail, but configuring as per the instructions fixes it.
Also, once installed you need to use "bundle exec jekyll s" rather than "jekyll s"

Change defaults: site title/description, about page, etc.

Add \_config\_dev.yml
use bundle exec jekyll serve --drafts -c \_config.yml,\_config\_dev.yml to override url
(--drafts is so I can see my drafts locally)

Create a new post, remove original default post
