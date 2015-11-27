title: 'The new-on-GitHub blues and my new project: command line Arch wiki'
date: 2015-11-25 17:49:27
categories: life
tags:
- js
- open source
- arch linux
---

Since I got back from Belgium last week, I've been stuck in a bit of a rut, GitHub-wise. I finished my hexo plugin so I had to start looking for something else to do while I finish up [my reading](https://github.com/getify/You-Dont-Know-JS).

First I started building a Reddit bot, but after a few hours I saw that someone else had already done exactly what I'd wanted to do. I didn't feel like continuing writing Python anyway -- and the node helper libraries aren't as well-developed -- so I let the project go.

Then I figured I'd just fix some Hexo issues. It looked like I was making some headway solving what I thought I had identified as a glaring bug in the code.

Sadly, there turned out to be a very good reason for why the code looked the way it did, and my 'solutions' ended up causing a lot more problems down the line. So after two whole days of tinkering I had to write that off too as an epic fail.

<!-- more -->

Next, I decided I was going to make the [highlight.js](https://github.com/isagalaev/highlight.js) styles available in stylus format for use in Hexo. That meant figuring out how to scrape a github repository, process the contents and serve them up in a different repo, and then using that on stylized CSS files.

This time I got half-way there, but I struggled hard with processing the styles. I'd built the scraping and automated updating of the repo part of the project, but I still can't for the life of me figure out why my converted styles won't look right in my tests!

At last, I decided to put making the highlight styles repo on hold for now, but use the part of the project that works for another project I'd been meaning to start. More on that in a minute.

## The new-on-GitHub blues

I wrote the above to illustrate the issues I'm having as someone who's new on GitHub. I'm trying hard to be useful and to learn how to contribute valuable code to projects, which are maintained by people much more experienced and more knowledgeable than I am.

A cursory glance at developer forums, chats and community websites reveals that a whole lot of people in similar positions are experiencing these problems as well.

{% asset_img think.jpg %}

The amount of resources, guides, books and classes designed to help people who are new to programming is staggering. So staggering even that the threat of educational oversaturation is rising in JavaScript-land.

What I mean is that for absolute newbies, there are so many courses, tutorials, classes, blog posts, plans and a whole lot more to do that you could fill several life times educating yourself, yet have no time left to actually do projects. This is something I realized not too long ago, after spending several months in a slightly-too-comfortable education bubble myself.

However, once a self-taught student has progressed to a certain level, that overwhelming amount of resources shrinks (or perhaps it simply becomes less useful). What is needed at that point is not more books or courses, but rather guidance and mentorship.

And such guidance and mentorship is hard to come by online. So many people are willing to commit many hours a day to contributing to open source -- because at some point in one's development it is probably the best and cheapest way to learn.

But contributing to open source is hard and nerve-wrecking if you're an experienced developer and therefore understandably frightening for new programmers. I'm pretty dead set on doing a lot of open source to get better at this, but it really is very, very difficult.

There are some communities, websites and applications to help you out though, and I'll make a separate post about these very soon. But for now, if you're reading this and you nodded your head throughout, let me just tell you that you're not alone and that you'll get there. Just don't get discouraged and keep at it! At least, that's what I tell myself?

## About that new project

So what I did in the end was I took my own advice. Despite the setbacks, I didn't get discouraged (well, maybe a little) and I kept at it. And now I'm working on a new project for which I'll hopefully get the first tidbits out tomorrow.

I'll be building a command-line interface for browsing a local copy of the [Arch Wiki](https://wiki.archlinux.org/) that gets updated automatically on a regular basis. I'm kind of way out of my comfort zone here, but I figure that's probably how I'll learn best, so I won't stop until I'm done (I hope).

By the way, I only started this after I made sure there weren't any licensing issues with me hosting Arch Wiki content. I'm pretty sure I understood it correctly, but in case you think I'm wrong, please get in touch with me asap.

Also, I realize there are already some similar projects like this. However none I've found seems to work on my machine or solves the problems I wanted to solve. Regardless, I just really want to do this, whether others have already done it better or not.

Here's what's on my to-do list:

- Scrape the entire Arch Wiki
- Turn HTML into markdown
- Make local TOC and symlink to the articles
- Implement daily/hourly update of all the content
- Make automatically updating github repo with the results from above
- Build CLI to locally browse the wiki using the repo above

Wish me luck! And best of luck to you if you're singing the new-on-GitHub blues too!
