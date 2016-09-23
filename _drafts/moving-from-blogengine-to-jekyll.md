---
layout: post
title:  "DRAFT - Moving from Blogengine.net to Jekyll"
categories: ["jekyll", "blog"]
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

Push to github to make sure github pages builds everything correctly

Create a new post, remove original default post

Follow instructions at http://philippkueng.ch/migrate-from-blogengine-dot-net-to-jekyll.html#comment-1327752341 and
http://doingthedishes.com/2011/04/14/moving-to-jekyll.html:
- export old blog to BlogML
- download App_Data folder (contains images, etc) and move images into /images
- use script to convert BlogML into md (script is modified version of this: https://gist.github.com/eduncan911/10331596)
- cd _imports and then: ruby -r './blogml.rb' -e 'Jekyll::BlogML.process()'
- copy contents of \_imports/\_posts to \_posts

Custom domain
- change UK2 DNS: set DNS servers to UK2 defaults (replace ns41.domaincontrol.com/ns42.domaincontrol.com with dns3.uk2.net/dns1.uk2.net/dns2.uk2.net)
- Setup apex domain (andrewt.com): https://help.github.com/articles/setting-up-an-apex-domain/
- setup www subdomain (www.andrewt.com): https://help.github.com/articles/setting-up-a-www-subdomain/
