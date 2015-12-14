title: 'Huge Arch update!'
date: 2015-12-12 12:44:13
categories: linux
tags:
- arch linux
---

I've been using Arch Linux as my daily driver for almost six months now and this has been by far the biggest update I've seen so far! Just wanted to share this whopper of an update:

{% asset_img arch-update.png huge pacman update %}

In case you're wondering, the reason for this is the switch to the new C++ ABI. In my case everything went right, but if you're wondering what to do here and for some reason you never read the [Arch News article](https://www.archlinux.org/news/c-abi-change/), all you need to do to make sure everything keeps working peachily is rebuild your non-repo packages.

The Arch team provided a handy little script to identify the packages in question:

{% codeblock lang:bash %}
#!/bin/bash
while read pkg; do
    mapfile -t files < <(pacman -Qlq $pkg | grep -v /$)
    grep -Fq libstdc++.so.6 "${files[@]}" 2>/dev/null && echo $pkg
done < <(pacman -Qmq)
{% endcodeblock %}

For me that just came to a half dozen packages and it took less than twenty minutes to rebuild them against the new ABI.

When I first started using a rolling release distro, I feared I'd have to deal with breaking changes all the time. In practice, I only had everything break after an update once in six months, and that was because I'd been running the proprietary graphics drivers (_shame on me_). Even then, it was relatively easy to boot from a live USB, chroot in and fix my setup. Apart from that, everything's been smooth sailing, including this humongous update!
