title: Forwarding mail to your Gmail account with Mailgun
date: 2015-12-19 15:18:27
categories: linux
tags:
- mailgun
- vps
---

_also:_ **[How to set up Mailgun on Digital Ocean](/linux/2015/how-to-set-up-mailgun-on-digital-ocean/)**

I've been using Mailgun to send myself e-mails from my server with scraped data, logs, warnings and errors for a while now and it's been working peachy. The free tier offers more than enough to satisfy most non-commercial users and it's all a breeze to use -- especially given the helper libraries the service provides in a host of different languages including NodeJS.

{% asset_img DO.logo.png Digital Ocean logo %}

In my other article (linked above), I go into a bit more detail on how to set it up [on your own $5/month Digital Ocean server](https://www.digitalocean.com/?refcode=30a11cb68f93), but that focused more on *sending* mail. Just the other day, I configured it to also *receive* mail. Or, rather, to *forward* it to a Gmail account I made just for this.

It turns out it's really easy, but I did run into some annoying issues that caused me to lose quite some time over this. So let me walk you through troubleshooting said issues just in case the same happens to you.

<!-- more -->

## Set up the domain

When you're first setting up your domain on Mailgun, the website will recommend that you use a subdomain such as `mail.mywebsite.com`. The good people at Mailgun will also reassure you with the information that using such a subdomain won't prevent you from sending mail from `something@mywebsite.com` (as opposed to `something@mail.mywebsite.com`).

Well, this is all 100% true for _sending_ mail, but not so much for _receiving_ it. So when you want to receive mail, say at `admin@mywebsite.com`, make sure you have a `mywebsite.com` domain name set up and verified on mailgun! If you only have something like `mail.mywebsite.com` set up, it will tell you it's verified, but the MX records won't be correct and you'll get `5550 5.7.1 Relaying denied` errors returned to you.

Again, for more information on how to verify a domain, [read my other article](/linux/2015/how-to-set-up-mailgun-on-digital-ocean/) or keep a very close eye on the Zone File information at the bottom of your Digital Ocean DNS setup.

## Open your ports

If you're using `ufw` for port management and your policy is to deny as much traffic as you can, you need to make sure you're not blocking the ports on your server that Mailgun makes use of. Here's a quick one-liner just for that:

{% codeblock lang:bash line_number:false %}
sudo ufw allow submission
{% endcodeblock %}

This should suffice (if you're even blocking all other ports with your `ufw` firewall in the first place), but Mailgun _may_ in some cases use ports 25 or 2525 as well, just in case this doesn't do the trick. Also note the above line will open port 587 to the world.

If that poses unacceptable security concerns in your opinion, then you might instead open it only to Mailgun's SMTP/API IP, which is currently `50.56.21.178`. [Read this](https://github.com/miniwark/miniwark-howtos/wiki/Firewall-setup-on-Ubuntu-12.04#user-content-mail-services-setup) to see how, but know that the IP in question could change without warning in the future.

## Create a route in the Mailgun dashboard

For forwarding mail to a Gmail account, you'll need to create a route under the `Routes` section in your Mailgun dashboard.

Under `priority`, fill in a low number like 1, under `Filter Expression`, put either something like `match_recipient(".*@myapp.com")` or `catch_all()`, and under `Action`, put `forward("my.name@gmail.com")` (obviously you need to change this to match your own server and Gmail names).

The filter expression will filter messages, in this case on recipient address (but you can filter on basically anything). Whatever gets through will move to what you set as the action, with the _lowest_ priority winning out in case multiple rules apply. This is pretty straightforward if all you want to do is forward mail, but here is some more information [in the Mailgun docs](https://documentation.mailgun.com/api-routes.html) if you're still confused.

## Do not use your own e-mail address for testing!

This is actually the part I lost an inordinate of time at. I had gone through all the steps and every individual part seemed to be working fine, but whenever I sent a test mail, it would just fail silently! No error message or anything, my mail would simply not get delivered.

Only after an hour and a half or so did I realize what the problem was: Gmail filters mail that is sent from and routed to the same e-mail address or something. I confess I didn't get to the bottom of why this was happening, but here's the gist of it:

- My test mail is sent from some Gmail address, say `greg@gmail.com`, to a recipient on the server's domain name, say `admin@gregjs.com`
- My server gets it, and sends it through Mailgun for processing
- Mailgun runs the filter, and if it gets picked up, it forwards it to the address I set up, in this case **back to** `greg@gmail.com`
- Gmail doesn't like this and doesn't show you the e-mail
- I am left scratching my head, wondering why it isn't working

As you can see from this, the solution is simple: send your test e-mail from a different e-mail address! If you set everything up correctly, it will work this time. Or rather, you will discover that it was working all along.

There you go. I hope this article resolved your issues if receiving mail through Mailgun wasn't working out for you. Feel free to comment with a question if it still doesn't work, or troubleshoot your issues using [the Mailgun docs](https://documentation.mailgun.com/) or [Digital Ocean tutorials](https://www.digitalocean.com/community/tutorials).
