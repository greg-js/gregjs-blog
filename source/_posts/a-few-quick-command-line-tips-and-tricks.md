title: A few quick command-line tips and tricks
date: 2015-11-16 16:07:52
categories: linux
tags:
- command line
- tips
---

I'll probably be updating this blog a bit less the coming week as I'm about to leave for a short trip to belgium, where I grew up.

Of course I'll be visiting my parents and a few friends, but this Wednesday I'll also have the pleasure to see the [one and only Richard Stallman](https://rms.sexy) deliver a [lecture about the free software philosophy in Gent](http://freeasinfreedom.be/). This will be my first time seeing RMS in person and I'm quite excited!

I wasn't so sure what to put here for a quick post before I go, but I thought I'd just share a few quick tips and tricks for users and lovers of the command-line! For most of you, this will be old news, but I'm sure there are lots of people out there for whom any of these quick tips would be a revelation and/or a huge productivity boost. It's unlikely that I will reach those guys and gals through this modest blog of mine, but let me give it a try anyhow!

I'll probably do more of these posts later, this is just a small taste of what's to come! Read on for the first tips!

<!-- more -->

### Refer to previous commands

Ever typed a command, then realized you forgot to `sudo` it, and then typed again? No longer!

{% codeblock lang:bash line_number:false %}
# to repeat the previous command with sudo:
sudo !!

# to see a list of your previous commands
history

# to repeat any numbered command in the history
!x    # where x is the number of the command
{% endcodeblock %}

Also, to do a reverse history search, press `CTRL+R` on the command line, then type part of the command you want to repeat. Browse through multiple matches by pressing `CTRL+R` again. This will revolutionize your command-line workflow!

### Emacs or vi key bindings

By default, the bash shell will support emacs key bindings. Now, I'm a *huge* vim fanatic, but on the command-line I'll use these. Still, if you prefer to use vim key bindings (allowing you to go into command mode to modify your commands among other things), put this in your `~/.bashrc`, `~/.profile` or `~/.zshrc` (or whatever..):

{% codeblock lang:bash line_number:false ~/.bashrc %}
set -o vi

# or, to make sure you're using emacs mode:
set -o emacs
{% endcodeblock %}

Then source the file and you'll have vi or emacs mode enabled.

If you set your bash prompt to vi-mode, I'm sure you know how to use it. Still, as I mentioned earlier, even as a vim junkie, I still use emacs mode simply because I find it more convenient on the command line. Here are a couple of key bindings I use **all** the time:

- `CTRL-A` : move cursor to beginning of line
- `CTRL-E` : move cursor to end of line
- `CTRL-U` : delete the line entirely
- `CTRL-W` : delete the previous word
- `ALT-D`  : delete current word up to the next whitespace
- `CTRL-F` : move forward one character
- `CTRL-B` : move backward one character
- `ALT-F`  : move forward one word
- `ALT-B`  : move backward one word
- `CTRL-P` : go to previous command in history
- `CTRL-N` : go to next command in history

Now, there is a *lot* more where this came from, but this is it for now as I need to run and catch my plane. Hopefully this was helpful in some way to *someone* out there. I'll make more posts like this in the future. Wish me a safe flight and I'll be back next week!
