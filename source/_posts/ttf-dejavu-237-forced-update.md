title: "ttf-dejavu 2.37 forced update"
date: 2016-11-04T12:17:03.351Z
category: linux
tags:
- arch linux
---

So I just got back from my trip to Belgium and the Netherlands and I had a whale of a time.

Upon my return to my faitful linux desktop though (by the way, did you know that Linux distros have [surpassed 2% of total market share](https://www.w3counter.com/globalstats.php?year=2016&month=10) for four consecutive months now? itshappening.gif), I was greeted by [this Arch Linux News article](https://www.archlinux.org/news/ttf-dejavu-237-will-require-forced-upgrade/) and [this blog post](https://bugs.archlinux.org/task/32312). A simple `sudo pacman -Syu` wasn't going to cut it this time.

So most of you probably already fixed the issue, but there might be people out there who don't have `pacman` helpers installed to automatically check relevant news updates (I use [pacnanny](https://aur.archlinux.org/packages/pacnanny/) by the way, but there are others). In such cases, a regular `-Syu` update which contains the `ttf-dejavu` v2.37 patch will cause this set (or a very similar one) of `pacman` errors:

{% codeblock lang:bash line_number:false %}
error: failed to commit transaction (conflicting files)
ttf-dejavu: /etc/fonts/conf.d/20-unhint-small-dejavu-sans-mono.conf exists in filesystem
ttf-dejavu: /etc/fonts/conf.d/20-unhint-small-dejavu-sans.conf exists in filesystem
ttf-dejavu: /etc/fonts/conf.d/20-unhint-small-dejavu-serif.conf exists in filesystem
ttf-dejavu: /etc/fonts/conf.d/57-dejavu-sans-mono.conf exists in filesystem
ttf-dejavu: /etc/fonts/conf.d/57-dejavu-sans.conf exists in filesystem
ttf-dejavu: /etc/fonts/conf.d/57-dejavu-serif.conf exists in filesystem
Errors occurred, no packages were upgraded.
{% endcodeblock %}

Read the linked news and blog articles above for more information, but if you're in a hurry, what you need to do is force-upgrade the `ttf-dejavu` package separately - either before or after updating the other packages. Here's what you need to know:

* to upgrade everything except for the `ttf-dejavu` package, do `pacman -Syu --ignore ttf-dejavu`
* to force-upgrade the `ttf-dejavu` package separately, do `pacman -S --force ttf-dejavu`

There you go, hope I saved someone out there a few minutes ;-)
