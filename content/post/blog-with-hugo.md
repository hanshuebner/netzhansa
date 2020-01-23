---
title: "New blog with hugo"
date: 2020-01-23T16:14:46+01:00
---

A dear friend told me that [HUGO](https://gohugo.io/) is how blogging
is done today and as it was time to finally move away from
[Blogger](https://netzhansa.blogspot.com), I decided to give it a
spin.  Hugo is a static site generator that uses
[Markdown](https://guides.github.com/features/mastering-markdown/) as
its authoring format.  While I certainly have my reservations against
Markdown (or anything that is not XML, for that matter), I'll gladly
admit that it makes creating content very easy.  This may help my goal
of writing and publishing more, so there.

HUGO is a static site generator, so it takes a bunch of input files
and creates HTML files that can then be deployed on any web server.
The design of the output can be customized by
[themes](https://themes.gohugo.io/), and there is a large number of
prexisting templates to choose from.  I'm going with
[Cupper](https://themes.gohugo.io/cupper-hugo-theme/) for now, as I
like the style and it works well both on the desktop and on mobile.

In order to make publishing as easy as possible for me, I'm hosting
the source code for my website on [GitHub](https://github.com/), using
a [webhook](https://developer.github.com/webhooks/) to run HUGO on my
web server that runs
[Apache](https://en.wikipedia.org/wiki/Apache_HTTP_Server) using a
trivial CGI shell script:

```
#!/bin/sh

echo Content-Type: text/plain
echo

export PATH=/usr/local/bin:$PATH
cd /usr/local/www/site/netzhansa.com
git pull
hugo
```

I was happy to learn that, according to Wikipedia, Apache is still
serving some 30% percent of the Web.  Not wanting to support the
operations of my Web server too much, I'm going for the mainstream
choice.

So here you have it, and I have one excuse less not to write about my
projects.  I'm sure that some of the things will be interesting to
some of my friends.
