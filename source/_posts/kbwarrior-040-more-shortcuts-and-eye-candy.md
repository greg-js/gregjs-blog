title: "kbwarrior 0.4.0: more shortcuts and eye candy"
date: 2016-10-10T12:47:26.837Z
category: javascript
tags:
- extension
- kbw
- open source
- vim
---

Just a quick update about `kbwarrior`, [my keyboard-browsing extension for chrome/chromium](https://chrome.google.com/webstore/detail/kbwarrior/apiogmmklkamhdnjjikooemepogmhjel). I know most of my readers (well, more like all of you who get here by googling for JavaScript, vim or linux stuff) prefer my tutorial-style articles, and I'll do some more soon enough, but I just really enjoy posting my changelogs here.

{% asset_img screen1.jpg kbwarrior screenshot %}

So, since [last week's changes](https://gregjs.com/javascript/2016/kbwarrior-yet-another-chrome-extension-for-keyboard-based-vim-style-web-browsing/), here's what's new:

* A nicer and more consistent look for the hints
* `g` and `G` shortcuts to scroll to the top and bottom of the page
* `Shift + backspace` for going forward in history (it is a little strange, but I feel like it makes sense if backspace is a backward jump)
* `q` to close the current tab
* Better detection of input fields so the shortcuts should interfere much less often
* Link hint mode now includes clickable buttons
* Holding `shift` when typing the hints opens links in a new window
* You can now also cancel hint or insert mode with the `ESC` key or, my personal favorite, `Ctrl+[`
* YouTube was added to the disabled sites list

The one thing that's still bothering me is how some of the keys interfere with page-specific shortcuts. My non-solution of disabling them on such websites is clearly unsatisfactory but I'm not sure how to handle it without greatly increasing the complexity, which would run diametrically against the whole reason for why I made this in the first place. I'll be focusing on some other things for now, but I hope it's useful for someone out there. I know it is for me ;-)
