---
layout: post
title:  "Jekyll and Windows Subsystem for Linux"
subtitle: "How to setup a Jekyll project with Windows Subsystem for Linux"
date:   2018-03-04 22:00:00 +0200
tags: jekyll wsl windows ubuntu github
---

[Jekyll](https://jekyllrb.com/) is a static site generator which is well suitable for blogging. One of the attractions of Jekyll is that [Github](https://github.com/) provides a built-in support for it meaning that you can host your blog free in [Github pages](https://pages.github.com/).

I was setting up a local Jekyll project targeted for Github Pages using Window Subsystem for Linux (WSL) and there were a couple of extra steps required in addition to the official quick-start instructions. I would like to share those issues here.

## Install Ruby

Jekyll is written in [Ruby](http://www.ruby-lang.org/en/) so the first thing to do is to install Ruby:

```
sudo apt install ruby-full
```

## Create a Jekyll project

After installing you should be able install the required gems and create and test a Jekyll project:

```
gem install jekyll bundler
jekyll new my-awesome-site
cd my-awesome-site
bundle exec jekyll serve
```

Now you should be able to see the Jekyll site in [http://127.0.0.1:4000](http://127.0.0.1:4000)

## Github Pages

To deploy the Jekyll site into Github Pages you need modify the `Gemfile` as descripbed in the comments in that file. In essence you must remove the `gem jekyll` line and uncomment `gem github-pages` line.

This is where I encountered the first problem as installing github-pages gem was not successful. It turned out a package `zlib` was missing in my WSL. More details on the problem are discussed [here](https://github.com/flapjack/omnibus-flapjack/issues/72) but for me the following worked:

```
sudo apt install zlib1g-dev
```

The installation took a lot of time but after that I was able to install github-pages gem.

## Syntax highlighting

When fiddling with the markup I soon noticed another issue. When trying to write a Javascript snippet with syntax highlighting like

{% highlight javascript %}

function hello() {
  console.log('hello jekyll');
}

{% endhighlight %}

Jekyll started giving `tag was never closed` errors. The error only occured if the first line in the post was inside a Jekyll block. A workaround was found  [here](http://blog.slaks.net/2013-08-09/jekyll-tag-was-never-closed/) and adding the following line in `_config.yml` fixed the issue:

```
excerpt_separator: ""
```

After that I was enable to write posts and deploy the site automatically in Github Pages just by creating a new repository named `<my_username>.github.io` and pushing the project into that repo.
