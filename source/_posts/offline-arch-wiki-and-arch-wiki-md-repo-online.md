title: offline-arch-wiki and arch-wiki-md-repo online
categories: javascript
tags:
  - linux
  - arch linux
  - js
  - open source
date: 2015-12-14 14:34:59
---

There were some serious delays as I struggled with some issues (particularly ones relating to automating `git push`es, to not getting kicked off from the server for launching thousands of requests at the same time and, _ehm_, to going on an unexpected trip with my girlfriend to celebrate my birthday!) but I finally have part of my arch wiki project working roughly the way I want it to.

First, I built a [little tool](https://github.com/greg-js/offline-arch-wiki) to scrape _every_ article in the official Arch Wiki, convert it to markdown, save it to the filesystem and store it in a json object. Later on, I'll come back to it to clean up the code, add more info to the database and maybe to fix all the interlinking going on in the articles. I might also allow it to take any similarly structured wiki instead of just the Arch one, but we'll see, this already took a lot longer than expected..

Sadly, I think I made some mistakes in how I structured the requests. I originally thought that performance would be better if I collected all the article urls first, then fetched all their html, then converted them all and then saved them all. Now I'm thinking I would have been much better off if I'd just did all that asynchronously to every url instead of waiting for the whole promise arrays to resolve before moving to the next step in the pipeline. I might go back and do it over to check how that would impact performance. Realistically though, I probably won't, since it _works, you know_?

Next, I used the above tool to set up [an automatically updating repository on github](https://github.com/greg-js/arch-wiki-md-repo) containing all Arch wiki articles in markdown format. It should always be pretty current as it updates itself with the most recent changes every six hours.

Anyway, I could move on to building a command line arch wiki browser now, but I have some other work to take care of first. When I get back, I'll go for the CLI arch wiki browser, or I may start working on something else I've had in mind, or maybe I'll even finally build out my landing page for this website! But we'll see, I really just wanted to post a quick update on my recent github activity and jot down some thoughts about how it went and what I'll do next. See you next time.
