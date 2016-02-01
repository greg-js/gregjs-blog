title: 'New version of Pacman released: Updating yaourt and package-query'
date: 2016-02-01 15:53:39
categories: linux
tags:
- pacman
- arch linux
---

Pacman 5.0.0 [has been released](https://projects.archlinux.org/pacman.git/tree/NEWS?h=v5.0.0), and for those of us who use it along with [yaourt](https://aur.archlinux.org/packages/yaourt/), that means a `pacman -Syu` willl temporarily be unable to satisfy its dependencies. The reason is that your system's version of `package-query` will prevent you from updating to the latest `pacman`.

Most among you will probably find it easy to upgrade, but if you're feeling fuzzy or made a little error on the way (causing you to fail to load the shared library `libalpm.so.9`..), here's how to do it asap:

{% codeblock lang:bash %}
# remove problematic packages:
sudo pacman -Rdd package-query yaourt
# make sure to update pacman here:
sudo pacman -Syu
# get the latest PKGBUILD for package-query and yaourt:
git clone https://aur.archlinux.org/package-query.git
git clone https://aur.archlinux.org/yaourt.git
# compile and install
cd package-query && makepkg -sri && cd ..
cd yaourt && makepkg -sri
{% endcodeblock %}
