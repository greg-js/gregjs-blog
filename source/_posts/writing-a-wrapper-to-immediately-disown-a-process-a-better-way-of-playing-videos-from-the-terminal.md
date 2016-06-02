title: "Writing a wrapper to immediately disown a process: A better way of playing videos from the terminal"
categories: linux
tags:
  - bash
  - command line
date: 2016-06-02 18:14:48
---

Oof, two months without updates! This is starting to look like a typical developer blog! Well, I won't bore you with the reasons behind my temporary absence and get right to the content: I'll be explaining below how to write a wrapper for a program (in this case, mpv) that automatically detaches itself from the terminal window from which you run it. Enjoy!

Years ago, I discovered VLC and started using it for all my video watching, as it is in my opinion by far the best option on Windows. When I switched to Linux full-time though, I found out there is another piece of open source software that does the same thing, and by golly, it does it even better: {% link mpv https://mpv.io mpv website %}

{% asset_img mpv.website.png mpv website %}

If you've read this blog before, you probably know I love the command line, and that's exactly why I ended up using mpv for all my videos. VLC is still great, mind you, but configuring it can be time-consuming with its many dialog windows - trust me, a long time ago, someone actually paid me to write tips for a VLC blog..

In contrast to the typical Windows-centric approach VLC uses, mpv's configuration options are at the same time mind-bogglingly elaborate and as simple as running a command from the terminal and/or editing the config file with your favorite editor (vim).

Maybe I'll share some of my other mpv settings later, but right now I'd like to focus on this little tip for playing videos from the terminal in mpv. You might find it very useful if you live in the terminal like I do, and particularly if you use a tiling window manager like {% link i3 https://i3wm.org i3 website %} or {% link awesome https://awesome.naquadah.org awesome %}.

You might also use the same simple wrapper for running other scripts/programs in the background and immediately disown them.

<!-- more -->

The problem the wrapper solves is that of mpv spawning a child process when run from the command line. It then displays a lot of information in the current terminal window while the video plays (see the screenshot below). This can be very useful in some cases, but sometimes you just want to play one or a number of videos in a new window, and be free to close the spawning terminal window without also closing the video.

{% asset_img mpv.png mpv command line information %}

So here's what I use (of course I made this an executable script and put it in a folder that was added to my `$PATH`, in my case `~/bin/`):

{% codeblock lang:bash ~/bin/play %}
#!/bin/bash
## mpv wrapper to make it run in the background and disowned by the terminal window

mpv --really-quiet "$@" &
disown
{% endcodeblock %}

See how simple?

Just to make it as clear as possible for those among you who are new-ish to bash/linux, I'll explain it in some more detail below, but if you just want to copy, paste and use it, go ahead.

### `#!/bin/bash`

This is what is called a shebang, after the ha**sh** (`#`) and the **bang** (`!`) it starts with. This tells your operating system to run the following script with `/bin/bash`.

### `## mpv wrapper ...`

This is just a comment to detail what the script is for. The second hash is actually unnecessary, but I like putting it there to clearly separate the comment from the shebang.

### `mpv --really-quiet`

This runs mpv in (you guessed it) `really-quiet` mode. That means it won't display all that (useful) information in the terminal window.

### `"$@"`

This is a reference to *all* the arguments you run the script with. Without this, you would only be able to use `play` with just one file. With it, you can do something like `play video1.mpg video2.mp4 video3.mkv` or `play ./vid*`.

Note the double quotation marks! Without those, you'd get in trouble if your filenames contain spaces.

### `&`

This simply means: run this process in the background. Since your goal is to disown the process, you must do this for the next step.

### `disown`

This disowns the backgrounded process. Now the video becomes detached from the terminal window from which you ran it. You can watch the video and, if you want, close the terminal window.

There you go, you can use the same simple procedure to run your own scripts or programs as detached processes. It really does come in very handy once in a while, but personally, I use it mainly for mpv :-)
