title: "kbwarrior: yet another chrome extension for keyboard-based, vim-style web browsing"
date: 2016-10-05T16:38:18.099Z
category: javascript
tags:
- open source
- kbw
- extension
- vim
---

I like vim. No really, I *like* like vim. I like it so much I want my web browsing experience to be more like it. Quite a few people feel the same way it seems, because there are a lot of options when it comes to Chrome/Chromium extensions that add some vim-like features to the browser (not to mention [conkeror](http://conkeror.org), [Vimperator](https://addons.mozilla.org/en-GB/firefox/addon/vimperator/) or [Pentadactyl](http://5digits.org/pentadactyl/), the Firefox add-ons that started it all):

* [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb)
* [cVim](https://chrome.google.com/webstore/detail/cvim/ihlenndgcmojhcghmfjfneahoeklbjjh)
* [Vrome](https://chrome.google.com/webstore/detail/vrome/godjoomfiimiddapohpmfklhgmbfffjj)
* [Vichrome](https://chrome.google.com/webstore/detail/vichrome/gghkfhpblkcmlkmpcpgaajbbiikbhpdi)
* [Surfingkeys](https://chrome.google.com/webstore/detail/surfingkeys/gfbliohnnapiefjpjlpjnehglfpaknnc)
* [Keyboard Control for Chrome](https://chrome.google.com/webstore/detail/keyboard-control-for-chro/mhofehfbkjmeldlgkbleegeffhaocceg)

And my apologies to the authors of the other extensions out there with similar feature sets.

So with all this out there, why the hell did I go and write my own? Well, for one I just wanted to try my hand at writing an extension. But also, while a lot of great work has gone into the above tools, I always felt like either something was lacking or too much was going on. Either it didn't have a certain feature I wanted, or something kept getting in my way while browsing (or, *horror*, it forcibly implemented smooth scrolling everywhere).

Thus was born my own addition to the chromiverse: [kbwarrior](https://chrome.google.com/webstore/detail/kbwarrior/apiogmmklkamhdnjjikooemepogmhjel). I'm not sure what I was thinking when I came up with the name but hell, I'm sticking with it since Google made me design a logo for it to put in the web store and, considering my GIMP skills or lack thereof, I'm not exactly jumping to do that again.

{% asset_img logo.jpg kbwarrior logo %}

`kbwarrior` is limited in its feature set but does exactly what I want it to and nothing more (or, at least, it will once I'm done building it). This is what it does for me and what it could do for you as well:

<!-- more -->

## Page Navigation

Just the most useful vim-style shortcuts are included. `h`, `j`, `k`, `l`, `d` and `u` are a given.

I also readded `backspace` to go back in history, because Google removed that for us a while ago and I still simply cannot cope with that one.

Of course these and all other keyboard shortcuts are disabled when in an input field or textarea, so don't worry about going insane.

## Tab Navigation

`n` to go to the next tab, `p` to go to the previous one. Simple as apple pie. I just really dislike the awkward default shortcuts (`ctrl+tab` and `ctrl+shift+tab`).

## Hint mode

`f` brings up hints on all visible links on the page. Type one of the hints to "click" on the link, without having to reach for your mouse. What a world we live in. Type `f` again to turn off the hints.

## Insert mode

Unlike the other extensions, my "insert mode" is simply another hint mode, but just for the input fields and textareas on the page. Type the hint and start typing. And again, without using that ludicrous mousey thing on your desk! What a relief!

## Auto-disable

Some websites implement their own shortcuts and these (as well as those of other extensions) interfere with them. So I decided to just disable the key bindings on certain pages that are sure to prove troublesome. These include Reddit, Facebook and GitHub, but I'll probably add to the list over time.

## What next?

Honestly, I wrote this mostly for myself and I think I'll probably be one of the only people using this but I do plan on adding some more key bindings, such as a `forward in history` one and a `close tab` one. I'll probably add a feature to open links in a new window as well at some point.

Also, I feel like the hints could look a little better. Maybe I'll display them in a different way later on.

## How do I install/complain?

Install: [get it from the Chrome Webstore](https://chrome.google.com/webstore/detail/kbwarrior/apiogmmklkamhdnjjikooemepogmhjel).

Complain: [file an issue on GitHub](https://github.com/greg-js/kbwarrior/issues).
