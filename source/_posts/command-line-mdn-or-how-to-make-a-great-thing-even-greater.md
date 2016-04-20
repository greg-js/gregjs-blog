title: "Command-line mdn, or how to make a great thing even greater"
date: 2016-04-01 13:40:00
categories: linux
tags:
- js
- node
- open source
- mdn
---

How do you make a fantastic online collaborative resource website even better than it already is? Right! You write a command-line interface for it!

I had been toying with the idea of writing a CLI for everyone's favorite webdev reference site, [MDN](https://developer.mozilla.org/en-US/docs/Web), but never really got round to it. A few days ago, I set out to finally write the thing, but after doing a little bit of work on it, I did what smart people do and searched more thoroughly for existing projects on GitHub.

And what do you know, an awesome dev called Rafael Rinaldi had **just** published his own command-line `mdn` tool. Not only that, but his approach was better than mine too! I was thinking more of using `man` to page through the entire contents of a given article, but Rafael's idea to print just the first paragraph of the description along with the API and a link is much better.

{% asset_img example.gif mdn example %}

Besides JavaScript, it also supports searching for CSS definitions and I submitted a few pull requests to add an `-o` option to open the relevant page in your local browser as well as some general formatting fixes.

Anyway, Rafael's `mdn` tool is awesome and it's become an integral part of my workflow already - I mean, come on, with all this new JS stuff coming out every minute, who has time to read stuff in a browser anymore! If you like both MDN and the command-line (and if you're reading this, then you probably do), I highly recommend you use his tool and of course contribute if you can!
