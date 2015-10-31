title: How to set up Mailgun on Digital Ocean
categories: linux
tags:
  - vps
  - linux
  - mailgun
date: 2015-10-31 02:50:09
---


Mailgun is an awesome e-mail service that makes it super easy for developers to automate e-mail sending. I'm not going to spend much time singing its praises here. Just [check it out for yourself](http://www.mailgun.com). Suffice it to say that if you've ever thought about programmatically sending, receiving or tracking e-mail, using just about any programming language under the sun, you *really* should consider using Mailgun.

I'm currently using it for a number of things, such as having my little [$5/month Digital Ocean server](https://www.digitalocean.com/?refcode=30a11cb68f93) (which is also running this website by the way) mailing me sysadmin-related notifications, error logs, updates on some websites I'm regularly scraping and other niceness.

{% asset_img mailgun.png Mailgun logo %}

So yeah, I *love* Mailgun. But I have to say, it was a bit of a chore to set it up on my server. The official guidelines didn't quite work, and none of the help or blog posts I found online about this seemed up to date. After some experimentation, I got it to work though. Turns out you just have to tweak the settings a little bit to make it work with Digital Ocean.

In case anyone is finding themselves in the same situation I did: don't despair! Here is how to set it up:

<!-- more -->

If you've already signed up for Mailgun and you're reading this, you probably know what you need to do already. There are five DNS records you need to add on your server. Two for sending, one for tracking and two for receiving.

The most straightforward way to get to the settings on Digital Ocean is to log in to your account, go to `Networking` and select your server.

But here's the rub: if you enter the information like Mailgun tells you to, it won't work. Then you'll probably go online and look for answers, and you'll find plenty of blog posts and even some official Digital Ocean tutorials about this. Problem solved, right?

Well, not quite. The proposed solutions (at least the ones I found, and believe me, I looked) won't work either. Probably something changed recently or something so the solutions are out of date.

The solution below definitely works right now, but chances are it will go out of date at some point in the future. So if you're struggling with setting it up, try changing the settings as described below. But if it doesn't work, just think about it and look at the output of your zone file and you'll figure it out eventually!

But anyway, here is how I got it working.

## Digital Ocean DNS setup for Mailgun

### DNS records for sending

First of all, you should probably wrap the values (the `v=spf1` one and the `k=rsa` one) in double quotes. I found it worked without the quotes for me, but all the other blogs mention it, so just do it to be safe.

For the first `v=spf1` TXT setting, do as the Mailgun notification tells you, and put `@` instead of the hostname.

As for the second `k=rsa` TXT setting, put `mailo._domainkey.`, followed by the subdomain if you set one (like `mail` for example), but do **not** write the hostname. Look at the zone output below, you want the *output* to be the same as what mailgun wants you to *input*.

### DNS record for tracking

Similar to the `k=rsa` TXT setting above, you'll have to put `email.`, followed by the subdomain if you set it. The hostname will get added automatically.

Also don't forget to add a `.` after the `mailgun.org` value, so it makes `mailgun.org.`

### DNS records for receiving

These `MX` records are actually optional (unless you want to use mailgun for receiving mail), but to make it work on Digital Ocean, just add a `.` after the values, just like above.

<hr>

That's it. That should do it! It can take a while for the DNS changes to propagate so be patient. But just in case it still doesn't work, keep trying and look at the zone file.

There, I hope this was useful for someone out there! Now draw that shiny new Mailgun of yours and start shooting some mails at your users (or at yourself)!
