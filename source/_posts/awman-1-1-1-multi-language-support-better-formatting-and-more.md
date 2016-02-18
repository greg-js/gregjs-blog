title: 'awman 1.1.1: AUR, multi-language support, better formatting and more'
date: 2016-02-15 20:06:16
categories: javascript
tags:
- open source
- js
- npm
- arch linux
---

It's been a while since my last update. Some of that is due to falling ill yet again. The rest is because my `arch-wiki-man`/`awman` project turned out to be quite a bit more in demand than I thought it would be.

My cautious announcement about it was greeted by a pretty enthusiastic response, a number of questions, a few feature requests and even a bug report. As someone who is still relatively new to programming, I have to say I felt rather overwhelmed - and overjoyed!

Just in case some of you are interested, here's what's changed in the past week and a half:

<!-- more -->

+ **Using the API**: When someone asked me why I was going through so much trouble scraping and parsing pages from the wiki for my underlying [offline-arch-wiki](https://github.com/greg-js/offline-arch-wiki) project, my response was, somewhat embarrassingly, "_OMG there's an API?!_". Turns out there most definitely is one and I'm now using it. Oh man, if only I'd known earlier..
+ **Multi-language support**: Instead of 2.000-ish English articles, the database now holds over 4.000 articles in over a dozen languages. By default, searches are in English, but you can search in any of the other languages with `awman -l <language>` - look at the available languages with `awman --list-languages`. Not all of them have been implemented correctly yet though. Just the ones that are hosted on the same URL.
+ **On the AUR**: A few people expressed a desire to be able to install the package through the [Arch User Repository](https://aur.archlinux.org/) rather than npm. So I decided to figure out how to do that and am now [maintaining the package there](https://aur.archlinux.org/packages/arch-wiki-man/) as well. In fact, if you're on Arch (and if you're interested in this package, you probably are), this is probably the most straightforward way to install it now, although I'll only be updating this once a week instead of once every two days for the npm package.
+ **Better formatting**: I worked out a number of kinks in the formatting. There are still a few left though. And unfortunately, I spent a whole lot of time trying to get my own wikitext parser going to greatly simplify things but got very stuck somewhere. I'll be working on that some more later and hopefully will get an even better (as in, much faster) version out soon.

That's probably about the most important stuff. I still have a bunch of things on my todo-list for this project, but first I need to get healthy again.. Once I do, I'll tackle the task of writing my own markdown to troff parser to make the project a **lot** lighter.

Let me end with a huge thank you to everyone who's using my code, and especially thanks to all those who have given advice, posted enthusiastic comments, requested features, submitted bugs, starred me on GitHub and voted for me on the AUR! I know most or all of you I want to thank aren't reading this, but for those who do, seriously, you don't know how much it means to me!
