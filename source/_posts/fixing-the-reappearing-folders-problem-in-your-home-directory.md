title: Fixing the reappearing folders problem in your home directory
categories: linux
tags:
  - linux
  - arch linux
date: 2015-10-28 01:23:42
---

When I saw someone on Reddit asking about a series of folders constantly reappearing in their home directory, I replied with a quick [link to the relevant Arch wiki article](https://wiki.archlinux.org/index.php/Xdg_user_directories).

Judging from the amount of upvotes that got, and from my own struggles with this annoying problem a few months back, I guess this has quite a few people dumbfounded. So allow me to explain in some more detail what exactly the problem is, and how to fix it.

{% asset_img my-home-dir.png My fixed home dir %}

<!-- more -->

## The problem with the *special* directories

If you use Linux, chances are that your `$HOME` directory contains a couple of folders: `Desktop`, `Documents`, `Downloads`, `Music`, `Pictures`, `Public`, `Videos` and possibly `Templates` (or translations of these). These directories were there when you first installed your OS, they've got pretty little custom icons in GUI file managers, and they're set as default destinations for certain actions.

There is a problem though. What if you don't want these folders in your home dir? Or what if you want to rename them? What if you just want to make them all lower-case? Well, if you've ever tried to naively change these dirs, you probably discovered that they will reappear at some point in the future.

Not only do they reappear when you reboot your machine, but they also reappear when you launch certain programs (Firefox for example). Now this is not good. It's your machine, your home directory! Who are *they* to tell you what folders you should keep and how to name? What is this, Russia under the Bolsheviks?!

If you just want to fix the issue, [skip to below](#How_to_manage_your_user_dirs). If you feel like reading just a little bit more, check out the bad solution below to learn what not to do and why.

## A bad solution

Let's say you want to remove some of the *special* folders altogether - the `Desktop` for example, since it's pretty much useless if you use a window manager like i3. As a Linux person, your first instinct might be to write a tiny script to delete the folder. Then simply call the script using a crontab or systemd timer at regular intervals.

Well, this will work - in fact I did this for a while on my own system for a few of the systems - but it's not ideal. Unless you set your script up to run on a very regular basis, you'll still see the folders from time to time. And forget about it if you just wanted to rename the folders!

So why is this happening? Well, it turns out that these special user directories are your so-called [XDG user dirs](http://freedesktop.org/wiki/Software/xdg-user-dirs/). Every time you login (or every time they are needed), your OS will create them automatically for you. How nice.

## How to manage your user dirs

So that's more than enough introduction, how do you fix it? Well, first install the `xdg-user-dirs` package if you don't have it already.

When you've got that installed, run `xdg-user-dir DESKTOP`. That should return whatever is set as the *special* desktop dir on your system. From here on out you have a few options.

If you want to rename some or all of the directories, run `xdg-user-dirs-update`. This should create the file `~/.config/user-dirs.dirs` on your system. Simply open that file and change the paths. Here's a screenshot of my own XDG config file on my home system. Note that I added some extra dirs, because why the hell not.

{% asset_img user-dirs.dirs.png My /etc/xdg/user-dirs.dirs file %}

And finally, if you want to get rid of these folders entirely, make sure you do *not* have a `~/.config/user-dirs.dirs` file and change `enabled=True` to `enabled=False` in `/etc/xdg/user-dirs.conf`.
