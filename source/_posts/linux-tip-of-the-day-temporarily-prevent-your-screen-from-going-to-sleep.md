title: "Linux tip of the day: temporarily prevent your screen from going to sleep"
date: 2016-01-15 16:12:00
categories: linux
tags:
	- arch linux
  - bash
  - command line
  - shell
---

I just wrote a quick little script to temporarily prevent my screen from going to sleep. Since this has been bugging me for a while, but I only just thought to actually address it, I figured there may be someone out there who can benefit from it also.

I'm not sure whether only people on Arch Linux have this problem sometimes, or Linux users in general, but if you've ever had your screen turn off in the middle of a Skype session (sadly we're all stuck with that program..) or while watching a video in your browser, or during some other full-screen activity that doesn't automatically turn off your power saving settings, this might be just what you need.

{% codeblock lang:sh $HOME/bin/prevent-screen-sleep %}
#!/bin/bash

if [[ $1 =~ ^0$ ]]; then
  xset s on +dpms
  echo "No longer preventing screen sleep"
  killall $(basename $0)
  exit 0
elif ! [[ $1 =~ ^[0-9]+$ ]]; then
  echo "example: $(basename $0) 60 ==> prevent sleeping for 60 minutes"
  echo "example: $(basename $0) 0  ==> revert to default settings"
  exit 1
fi

xset s off -dpms

echo "Preventing screen sleep for $1 minutes"
(sleep ${1}m && xset +dpms xset s on) &
{% endcodeblock %}

<!-- more -->

Put this script in a folder that's in your `$PATH` - I put it in `$HOME/bin/prevent-screen-sleep` myself - and make it executable (`sudo chmod +x prevent-screen-sleep`). Now, if for example you want to disable the screen saver and the energy star features for an hour, enter `prevent-screen-sleep 60`.

The settings will go back to normal after the amount of minutes you specify (multiple calls will overlap, not extend the original timer), but you can also manually reset with `prevent-screen-sleep 0`.

Just a little personal thing to solve a tiny issue I've been having, but who knows, if you got here by googling around, use it and enjoy!
