title: Read Medium stories on the command line with mediumcli
date: 2016-09-21T06:39:34.808Z
category: javascript
tags:
- command line
- open source
- node
---

Hey, you! Do you like the command line? Yeah? How about Medium stories? Not really? Okay, move along then.

But if you *do* have a weakness for both useful little command line utilities and a nice Medium story here and there, you'll probably want to take a look at [djadmin's mediumcli](https://github.com/djadmin/medium-cli).

{% asset_img medium.gif screenshot %}

<!-- more -->

Installation is simple (if you already have Node.js):

{% codeblock lang:bash line_number:false %}
npm install -g mediumcli
{% endcodeblock %}

Using it? Pretty simple too:

{% codeblock lang:bash line_number:false %}
# display the help
medium -h

# browse the top stories
medium top

# list by tag (accepts a --latest | -l option)
medium tag javascript

# search stories
medium search node

# open selected story in web browser rather than terminal (boo)
medium top -o
{% endcodeblock %}

But, I hear you say, why would anyone want to read articles in the terminal instead of a browser? Well, honestly, I do it because the command line interface makes finding a specific article or reference quick and convenient. And also it makes me feel really really cool.

*note:* At the time of writing the npm package is actually one version behind (v`1.3.1`) on the github project (v`1.3.2`), making the `-o`/`--open` option inaccessible to those who install via npm, so install from the github source if you really want that feature. The author has already been notified and will most likely update soon though, at which point I will remove this note as well.

Oh, and if you like this, you'll probably enjoy [command-line MDN](linux/2016/command-line-mdn-or-how-to-make-a-great-thing-even-greater) and/or [arch-wiki-man](https://github.com/greg-js/arch-wiki-man) too.

One more thing, as you can see I finally caved and added some social links to my blog. The reason is I've been getting quite a bit of traffic from twitter and reddit lately, and I figure I may as well embrace it even though I originally wanted to keep this blog as bare-bone, personal and non-spammy as possible. But it turns out I feel really flattered when people actually want to share my articles, so if you like something, by all means, share on!
