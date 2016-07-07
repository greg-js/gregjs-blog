title: Relative wdn warp points and clg new command
date: 2016-07-06T23:55:07.782Z
category: javascript
tags:
- open source
- command line
- node
- wdn
- clg
---

Right, here's another quick update for those blessed few among you who are, for some reason or another, interested in my quest to use Node.js to make the command line as convenient as it can be for bloggers and content creators.

## `wdn v3.2.0`: relative warp points & tests

For my [warp directory package](https://github.com/greg-js/wdn), I've just added a relative paths feature, allowing you to jump to paths *relative to a warp point*. What does that mean? Here's an example:

{% codeblock lang:bash line_number:false highlight:false %}
[~]$ wdn ls
  warp1  =>  /home/user/my/project
  warp2  =>  /home/user/another/project
[~]$ wdn warp1
[~/my/project]$ wdn warp2/relative/path
[~/another/project/relative/path]$
{% endcodeblock %}

Simple, right? You can use Windows-style backslash separators too, but you'll have to double them up (as in `wdn warp2\\relative\\path`). I guess I could address this, but since exactly zero people are using this package on Windows, I'm not going to bother right now and will use my time to do other things -- unless someone reports an issue, in which case I'll get right to it!

## `clg v0.2.0`: the `new` command

As for my [`clg` package](https://github.com/greg-js/clg) for bloggers and maintainers of statically generated websites, I just added the `new` command. This means you can now use it not just for editing existing content, but for conveniently making new content as well.

So no more navigating to the right directory (`cd ../../../source/_posts`, amirite ppl?) and dropping a markdown file there with some bootstrapped `yaml` info. Just write a little `.clg.json` config file to set up your directories and you're good to go. As an example, this is the `.clg.json` I'm using on this very Hexo blog:

{% codeblock lang:json ./.clg.json %}
{
  "sourceDirs": "source/_posts",
  "newDirs": {
    "post": {
      "dir": "source/_posts",
      "metadata": {
        "category": "javascript",
        "tags": ""
      },
      "assetDir": true
    },
    "draft": "source/_drafts"
  }
}
{% endcodeblock %}

With this in the root dir of my blog, I can go anywhere within it and do `clg new post` to automatically drop a new markdown file with some `title`, `date` (defaults), `category` and `tags` metadata in the `./source/_posts` folder and open said markdown file in my `$EDITOR`. It will also automatically [create an asset folder for images](https://hexo.io/docs/asset-folders.html). Likewise, `clg new draft` will drop a simple markdown draft in the `./source/_drafts` folder. Once written and saved, you can edit your content using `clg edit`. Check the readme on GitHub for more info.

I've also been doing some of the more boring but necessary work on both projects -- adding tests, comments etc. At some point I'll feel the projects will have matured enough to share them with the world, but for now, if you're reading this, you're one of the less than 20 people using it ;-) No matter, I'm really just building it for myself anyway.

Anyway, hope everyone is doing well. I promise I'll write some more (neo)vim and javascript tutorials soon (since those articles are **by far** the most popular on this blog), but I've been busy doing these projects and building a business site for a buddy and it might take just a little while longer. But hey, good things come to those who wait so bear with me.
