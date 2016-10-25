---
layout: post
title:  "Moving from Blogengine.net to Jekyll"
categories: ["jekyll", "blog"]
---
I have been using [BlogEngine.NET](http://dotnetblogengine.net/) for several years, but it always seemed a hassle (and expense) to have to host a web app and keep it up to date.
My blogging needs are very simple and I don't want to use a database (I didn't with BlogEngine), so when I heard about [Jekyll](https://jekyllrb.com/) I thought I should take a look.
<!--more-->

I originally used BlogEngine because it was open source, built on technology I understood (ASP.NET), and I didn't need to setup/pay for a database to use it.
However, I did need to pay to host it. This was fine when I also hosted other apps, but I don't really need to any more so I looked around for a free alternative.

# Jekyll
[Jekyll](https://jekyllrb.com/) is a simple, blog-aware, static site generator built in Ruby.
You write your blog (or other) pages in html and Markdown, and then it builds the entire site structure from them.
You can run Jekyll locally and preview your entire site before committing any changes, and because your content is just static files it is easy to store it in source control.

# GitHub Pages
[GitHub Pages](https://pages.github.com/) are free websites for GitHub users and projects and are Jekyll-aware - you can create a git repository
for your site and just commit the html/Markdown changes, and they will build the site for you using Jekyll.
For a user site you get a url such as http://_username_.github.io, but it is fairly simple to use your existing custom domain to point at the site too.

# Installing Jekyll locally on Windows
The easiest way to do this is to follow the [instructions from David Burela](https://davidburela.wordpress.com/2015/11/28/easily-install-jekyll-on-windows-with-3-command-prompt-entries-and-chocolatey/)
(these are linked from the [Jekyll docs](https://jekyllrb.com/docs/windows/)).

# Installing GitHub Pages locally on Windows
I just followed [Jens Willmer's instructions](http://jwillmer.de/blog/tutorial/how-to-install-jekyll-and-pages-gem-on-windows-10-x46) -
note that installing ruby2.devkit via chocolatey seems to fail, but configuring as per the instructions fixes it.
Also, once installed you need to use

```
bundle exec jekyll s
```

to run jekyll locally, rather than

```
jekyll s
```

# Local configuration
After installing I changed the site title, description and url in \_config.yml, and created my own about page (about.md).
To run with a different config locally, you can create a second config file (eg. I used \_config\_dev/yml) in which to place overrides
(eg. the local url I use is _http://127.0.0.1:4000/_), and then use the following command to tell Jekyll to use the other config file:

```
bundle exec jekyll serve --drafts -c \_config.yml,\_config\_dev.yml
```
(_--drafts_ allows me to also see my drafts when running locally).

I then removed the original default post, created a new post, and pushed to github to make sure github pages built everything correctly.

# Migrating from BlogEngine
Jekyll has a lot of [importers](https://import.jekyllrb.com/docs/home/) for migrating from many different blogging systems, but nothing explicitly for BlogEngine.net.
However, I obviously wasn't the first to do this and I found instructions
[here](http://philippkueng.ch/migrate-from-blogengine-dot-net-to-jekyll.html#comment-1327752341)
and [here](http://doingthedishes.com/2011/04/14/moving-to-jekyll.html).
In a nutshell:

- export old blog to BlogML
- download App_Data folder (contains images, etc) and move images into /images in your new site
- cd _imports and then: ruby -r './blogml.rb' -e 'Jekyll::BlogML.process()' (script is modified version of this: https://gist.github.com/eduncan911/10331596)
- copy contents of \_imports/\_posts to \_posts

# Custom domain
I use a custom domain (andrewt.com) for my site, and setting this up was quite straightforward.
I first had to change the DNS servers at my domain host (they were originally pointing at GoDaddy where my site used to live).
I then just followed the GitHub Pages instructions to setup an [apex domain](https://help.github.com/articles/setting-up-an-apex-domain/) (_andrewt.com_)
and the [www subdomain](https://help.github.com/articles/setting-up-a-www-subdomain/) (_www.andrewt.com_).

# Themes
Jekyll has a built-in theme system, but at the time of writing GitHub pages doesn't support anything other than the default theme.
However, it does allow styling so I think you can probably just take the styles from a theme you like and apply them to your site.
I'm going to try this next.
