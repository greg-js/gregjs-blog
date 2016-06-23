title: "clg: command-line goodness for working with static site generators"
date: 2016-06-23 02:39:43
categories: javascript
tags:
- node
- open source
- command line
- static content
---

I've been playing with preact [in my archives](/archives) but for the most part, this blog is statically generated with [hexo](https://hexo.io). Pretty happy with my setup but the other day I was thinking about using [metalsmith](https://metalsmith.io) for a new project.

Turns out I really like it. However, I wrote the [hexo-cli-extras](https://github.com/greg-js/hexo-cli-extras) plugin some months ago to make command-line blogging more convenient for myself. Now, I found myself inconvenienced without it.

I googled around, but nothing. So, what the hey, I blew off the original project for a while and built a simple filesystem-based [metalsmith version of my plugin](https://github.com/greg-js/metalsmith-hammer). Today I looked at the code though and started thinking. Aren't all static site generators kind of the same? If it works on one, it'll work on others.

So I've started work on [clg](https://github.com/greg-js/clg) (**c**ommand-**l**ine **g**oodness). If you use a static site generator and you love the command line, you might like this!

{% asset_img example.gif example %}

<!-- more -->

## Features

I only started this project yesterday so I've obviously not done everything I've wanted to do yet, but there are some nifty features here already:

- works out of the box or with minimal configuration with a bunch of static site generators (`metalsmith`, `hexo`, `wintersmith`, `jekyll`, `octopress`) and it should work on a lot of others too (just haven't tested it yet) - as long as you're using `git` and it's a `node` or `ruby` project
- some configuration possible on a project-by-project basis using a simple `.clg.json` file
- get handy, pretty lists of posts that match your queries
- open files automatically in your `$EDITOR` or GUI editor

## To come

Apart from the obvious lack of tests, which I must address at some point, I envision a `new` command for dropping `md` files into the source folders, `remove/delete` and `rename` commands, and maybe another one for editing `yaml` front-matter straight from the command line.

I'll also do some more testing with other static site generators and look into what it would take to enable PHP-based ones as well.

And we'll see what else when the time is here..

Anyway, take a look at the [readme on the GitHub page](https://github.com/greg-js/clg) if you want to know more and try it out! Of course comments, feature requests, bug reports and pull requests are all very welcome, just keep the hate-mail to yourself ;-)
