title: "awman / arch-wiki-man: man the Arch Wiki offline"
date: 2016-02-04 17:19:56
categories: javascript
tags:
- open source
- js
- npm
---

**Note**: Kyle from [kmkeen.com](http://kmkeen.com) pointed me to an existing project which solves the same problem this project was trying to solve. It's super fast and Python-based, [check it out](http://kmkeen.com/arch-wiki-lite/). Read on to read about `arch-wiki-man` if you prefer a Node-based solution or want a somewhat fancier menu.

{% raw %}
<hr />
{% endraw %}

I got round to finishing my [command-line Arch Wiki reader](https://www.npmjs.com/package/arch-wiki-man) the other day. Basically, it comes with a dependency on my [`arch-wiki-md-repo`](https://www.npmjs.com/package/arch-wiki-md-repo), which gives you a local (frequently and automatically updated) copy of the entire (English) Arch Wiki in markdown format. It also gives you an `awman` command (as in `arch-wiki-man`) to `man` or `apropos` the wiki. This works by querying a local database and then converting markdown to [troff](https://en.wikipedia.org/wiki/Troff) on the fly, saving it to a temporary file and spawning a `man` process to open it.

{% asset_img helpscreen.png image title %}

I personally find it useful to quickly consult the Arch Wiki without having to head to my browser, but another good use case might be if you think you won't have internet access later but know you will probably want to check the wiki. But mostly, I just made it for learning purposes and because I really wanted this for myself and the existing solutions didn't work for me at all.

<!-- more -->

## Install

Make sure you have Node and npm and install my package globally:

{% codeblock lang:bash %}
npm install -g arch-wiki-man
{% endcodeblock %}

It will take a minute or two because of the rather meaty ([but freaking fantastic](https://github.com/wooorm/remark)) markdown parser it comes bundled with.

## Use

Do an `awman --help` or `awman -h` to get some quick help. Basically, feed it one or more search terms and it will query the local database for matches in the title. Use the `-d` or `--desc-search` option for searching in descriptions and the `-k` or `--apropos` option for searching in the entire contents (which makes the search run slower).

If just one article matches your search, it gets converted into `troff`, saved to a temporary file and opened with `man`.

{% asset_img manscreen.png man screenshot %}

And if multiple articles match, you get a little selection screen:

{% asset_img menu.png Menu screenshot %}

When inside the `man` page, press `h` to get help on key bindings but the most important ones are: `j` for one line down, `k` for one line up, `d` for half a page down, `u` for half a page up, `/` for search, `n` for next, `N` for previous and `q` for quit. Oh, and to exit the menu without selecting anything, just do a `ctrl+c`.

## Update

I maintain an npm module with the latest updates to the wiki and push updates every two days. Using semantic versioning, I set it up so `arch-wiki-man` fetches the last version whenever you reinstall the package. This means the easiest way to get the last updates to the wiki into your local copy is:

{% codeblock lang:bash %}
npm install -g arch-wiki-man
{% endcodeblock %}


There we go, I hope it proves useful to someone out there who isn't me! If you like it, star it [on GitHub](https://github.com/greg-js/arch-wiki-man) and if it doesn't work for you, make a pull request or file an issue! Or just send me some hate mail <3
