title: 'wdn v3.1.0: ssh support & breaking change in warp point storage'
date: 2016-06-29 14:02:34
categories: javascript
tags:
- node
- open source
- wdn
- command line
---

Well, I don't think anyone is actually using [my `wdn` reimplementation](https://github.com/greg-js/wdn) of `wd`, but it's been a fun project for me, so let me tell you what's new in the latest version.

When I pushed from v2.x to v3.x, I changed the way warp points are stored on disk from a one-file-per-point to a one-file-per-scope deal. This breaks the existing points on anyone (no one but myself :P) who migrated from 2.x to 3.x, but allowed me to implement the one feature I was originally wanting to implement from the moment I started this project: `ssh` support.

Basically, local warp points are now stored in `~/.config/wdn/local` and for every remote host you have set up in your `~/.ssh/config`, a new file is created there to hold an array of remote points.

Check out the readme on the GitHub page if you want to know more, but here's an example of how it works.

{% asset_img screen.gif wdn example %}

Check it out if you think it looks interesting. Feedback, ideas, bug reports, critiques and pull requests welcome!
