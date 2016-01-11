title: How to rename a published npm module
date: 2016-01-11 14:35:23
categories: javascript
tags:
  - node
  - npm
  - open source
---

Two months ago, I wrote a small plugin for the static site generator [Hexo](http://hexo.io). It allowed users to edit posts more conveniently from the command line so I called it [hexo-easy-edit](https://github.com/greg-js/hexo-easy-edit).

Today I added another feature to the plugin, and I realized that the name no longer reflected all that it does. So I renamed the project to [hexo-cli-extras](https://github.com/greg-js/hexo-cli-extras). However, this turned out to be a little more involved than I originally thought because of the way npm works.

{% asset_img npm-logo.svg npm logo %}

<!-- more -->

## Why just renaming a module causes problems

Let's say you naively just rename your npm module and git repo. Congratulations, you've just broken people's workflows (if npm even allows it in the first place!).

The reason is simple. People who are you using your module have the name hard-coded in their `package.json` and there is no way for them to know that you changed it. You can put all the information in your `README` or GitHub/npm description you want, but that's of no use whatsoever to your users.

{% asset_img broken.jpg everything breaks %}

All they'll see is that npm will fail to fetch your module for them when they next try to install or update.

## Create new instead of rename

The way to go is thus to create a new npm module with the new name. I suppose you could link that to your old renamed GitHub repo, but that might confuse users who are still on your old version and now have a broken GitHub link, so you're probably best off to create a new GitHub repo to go along with the new npm module.

Creating a new repo and module is ideal for new users and doesn't break anything, but now you have one more problem: how do you communicate the name change to the people who have an older version installed?

## Communicating with your existing users

Unfortunately there is no way to way yet to automatically inform users of a name change, but you can do it yourself in two ways.

{% asset_img communicate.jpg communicate with your users %}

The first is obvious: write a message in the `README` for your old repo. Explain that you changed the name, link to the new version and encourage people to move.

The second is less obvious. Unless your module is very popular (in which case you won't be reading this article), most users won't even think to visit the GitHub page for it. But when they update or install your module, they'll access it through npm. Wouldn't it be nice if you could send them a message when they do so, informing them of a name change?

And you guessed it, you can do exactly that. All you need is `npm deprecate` (credit to [Peter Flannery on stackoverflow](http://stackoverflow.com/questions/28371669/renaming-a-published-npm-module) for the idea).

Let's say your old module name is `okay-module`, its latest version is 2.1.0 and your new, renamed module is called `awesome-module`. Then this is the syntax:

{% codeblock lang:bash line_number:false %}
npm deprecate okay-module@2.1.0 "WARNING: This module has been renamed to awesome-module. Please install it instead. See <GitHub page> for more information."
{% endcodeblock %}

Obviously you can put whatever you want inside the message, and whenever someone installs the latest version of your old module, they will receive that message in their terminal.

Now, is this a perfect solution? No, unfortunately not. But until a new feature is added to npm to address the issue of renaming modules, this is the best we've got, and hey, it's pretty good!
