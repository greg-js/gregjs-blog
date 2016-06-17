title: "Faster and more convenient file system navigation with wd or warp-dir -- or wdn?"
date: 2016-06-16 14:08:46
categories: linux
tags:
- command line
- bash
- zsh
- node
- open source
---

I trust most of you know the basics of navigating the file system on the command line. `cd`, `pwd`, `ls` and all that jazz. Maybe you've got some tricks under you belt too. Things like `cd -`, `CTRL-R`, `ls -alh`, `cd *somewhere*` and a whole lot more (check out {% link this resource http://conqueringthecommandline.com/book/basics Conquering the command line chapter 1 %} if any of this sounds new to you).

But that's not what this post is about. This is about taking shortcuts on the command line. More precisely, making warp points out of specific directories and then warping to it using `wd`. Here's an example of how it works (assume that the path at the beginning of each line represents the current working directory):

{% codeblock lang:bash line_number:false %}
[~/some/crazy/path]$ wd add myproject
[~/some/crazy/path]$ cd ~
[~] wd myproject
[~/some/crazy/path]$ wd list
  myproject  ->  ~/some/crazy/path
[~/some/crazy/path]$
{% endcodeblock %}

You wouldn't **believe** (or maybe you would) how convenient and awesome this is, and that's just the basic functionality. There are a couple of easy ways to add this feature to your own command-line, each with their pros and cons.

<!-- more -->

## Option #1: use `zsh` and install `wd`

The original `wd` is a package written by `mfaerevaag` for `zsh` - an alternative Unix shell that's compatible with `bash` while offering a host of additional features. If you've never heard of {% link zsh https://wiki.archlinux.org/index.php/zsh zsh wiki article %}, you might want to try it out. Personally, I've been using it as my shell for about 9 months now and I haven't looked back.

If you do like `zsh`, you're set to use the original `wd` package, which can be found {% link on GitHub https://github.com/mfaerevaag/wd wd %}. It's as easy as copy-pasting one of the two lines under `Automatic` in the `readme`. Or...

## Option #2: use `zsh` with `oh-my-zsh` and use the `wd` plugin

One of the major reasons why `zsh` is such a commonly-used shell is because of all its plugins and themes, and because of how easy it is to configure your shell exactly to your liking using the massively popular {% link oh-my-zsh framework https://github.com/robbyrussell/oh-my-zsh oh-my-zsh %}.

If you want to get a running start with `zsh`, `oh-my-zsh` is a most convenient way to quickly and easily get a nicely configured prompt (using one of the many themes). Then just follow the advice in the GitHub `readme` to enable the plugins you'd like to have and you're off. The one we're talking about here is called - you guessed it - `wd`.

{% asset_img example.png example usage %}

There are some who have made the argument that `oh-my-zsh` has become rather bloated. I won't take a position on this here, but I *will* point you to an alternative `zsh` configuration framework called {% link prezto https://github.com/sorin-ionescu/prezto prezto %} and I'll tell you also that I personally use it (check the screenshot above and note the prompt, the git branch info and my Node version on the right). `wd` is not a module included by default in `prezto` though, so if you got his route you will have to install it using option #1 above.

But what if you don't want to use `zsh`? How can you get access to the awesome power of `wd`?

## Option #3: install `ruby` and `warp-dir`

Sadly, no one (to my knowledge) has written a `bash` implementation of `wd`, so if you use `bash` or `fish` or `tcsh` or whatever, you'll have to get a little creative.

If you're a Ruby person, you probably already have it installed on your system. Although even non-Ruby people like me have it installed too because there is a ton of useful software written in Ruby (one that springs to mind right now is the CSS preprocessor {% link Sass http://sass-lang.com Sass website %}. Once you have it, you can then easily get the `warp-dir` gem by running:

{% codeblock line_number:false %}
gem install warp-dir --no-ri --no -rdoc
warp-dir install --dotfile ~/.bash_profile
{% endcodeblock %}

Check out the {% link warp-dir docs https://github.com/kigster/warp-dir warp-dir %} for further instructions and a comparison with the original `wd`, detailing the extra features this package sports over it, such as running commands at given warp points.

## Option #4: install `node` and `wdn`

OK, I'll admit it, I didn't actually know the Ruby package above existed until I set out to write this article. If I'd known, there's a possibility I might not have bothered to reimplement `wd` in `node`. That's right, I've been toying with building an extended version of `wd` using my favorite language.

Right now, if you can't use or don't want the original `wd`, you're better off installing `kigster`'s `warp-dir` package, as it's more mature and has more features than mine, which I've only just thrown together. However, if you like Node or have it installed already, or if you hate Ruby (*but why?*), my {% link wdn package https://github.com/greg-js/wdn wdn %} does have all the basic features implemented already, so it's in a usable state and can be installed like so:

{% codeblock this assumes bash, you know what to do if you use a different shell lang:bash line_number:false %}
npm install -g greg-js/wdn
echo '\nwdn() {
  source $(npm root -g)/wdn/bin/wdn.sh
}' >> ~/.bashrc
{% endcodeblock %}

I will make another post about it once I get round to polishing my package (*heheh*) and implementing some of the extra features I had in mind. For now, the only notable difference is that you can add new warp points by explicitly declaring them:

{% codeblock line_number:false %}
[~]$ wdn add tmp /tmp
[~]$ wdn tmp
[/tmp]$
{% endcodeblock %}

## Wrap up

That's it! Whichever route you go, I highly recommend you try out warp directories. It absolutely changes the way you navigate the file system! Soon though, you won't be able to live without it.

Also, remember that the `wdn` package is still far from ready and while basic functionality works, it has quite a few shortcomings right now and it **will** go through some changes soon. I'll work on it at first opportunity, but first I have more `(p)React` work to do. Thanks for reading and I'll see you on the flip-side.
