title: Linting code with Neovim and Neomake - ESLint edition
categories: vim
tags:
  - linting
  - js
  - neovim
  - style
  - vim
date: 2015-11-13 17:01:49
---

*Related:*
* [Linting code with Neomake and Neovim](/vim/2015/linting-code-with-neomake-and-neovim)
* [Lint as you type with Neovim and Neomake](/vim/2015/lint-as-you-type-with-neovim-and-neomake/)

{% raw %}
<hr />
{% endraw %}

[Ten days ago, I wrote](/vim/2015/linting-code-with-neomake-and-neovim) about how I set up the [Neomake](https://github.com/benekastah/neomake) plugin for Neovim and how I linted my JavaScript code using the [JSCS linter](http://jscs.info/).

While that worked alright for me, I kept running into weird bugs where JSCS would tell me there was a problem with my code, but the error message would simply display as "undefined". I didn't have the time to figure out what exactly was wrong, so I ended up switching to ESLint instead and it's been smooth sailing ever since. I'll explain how to set it up in this post.

<!-- more -->

My setup does exactly what it should do now, so I thought I'd take a few moments and share how I've set it up and how you can do the same. It might be a little bit of extra work to get it all going, but you'll be glad you did because having a convenient way to lint your code is extremely useful for everyone. Particularly so for people who work in big teams, but even if it's just you working on a hobby project.

### Step 1: Install prerequisites

You need [Neovim](https://github.com/neovim/neovim/wiki/Installing-Neovim), [Neomake](https://github.com/neomake/neomake) and [ESLint](https://github.com/eslint/eslint).

### Step 2: Set up Neomake

Your project-specific configuration is all in the `.eslintrc` file. That means you don't have to run it with special command-line parameters and ESLint will work out of the box just by setting it up like this:

{% codeblock lang:vim init.vim %}
let g:neomake_javascript_enabled_makers = ['eslint']
{% endcodeblock %}

However, you may want to run ESLint on your code with certain command-line options anyway. In that case, continue reading and/or check out [my other article on Neomake](vim/2015/linting-code-with-neomake-and-neovim) for more configuration tips. You'll also find some help there with troubleshooting Neomake in case something does go wrong. In most cases, the default will probably suffice though.

### Step 3: Initialize ESLint

You have a few options here, but all of them involve making an `.eslintrc` file. You can get it made for you and use the recommended ESLint defaults after making a few style decisions interactively:

{% codeblock lang:bash line_number:false %}
eslint --init
{% endcodeblock %}

Or you can use one of the ESLint configuration files that are usually supplied by the popular style guides. [Here's an example](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb).

Or you can mix and match and/or build your own ESLint config. For what it's worth, this is what I'm currently using. It's mostly the AirBnB style guide, but I turned off a few rules that were simply too annoying for me and I also required `use strict`:

{% codeblock lang:json .eslintrc %}
{
    "extends": "airbnb/legacy",
    "rules": {
        "strict": [2, "global"],
        "no-multi-spaces": 0,
        "no-use-before-define": 0,
        "no-console": 0,
        "padded-blocks": 0,
        "no-else-return": 0
    },
    "env": {
        "node": true
    }
}
{% endcodeblock %}

Two more things you probably want to add to your `.eslintrc` are [the environment](http://eslint.org/docs/user-guide/configuring.html#specifying-environments) and perhaps a few globals you need in your project. For the latter:

{% codeblock lang:json .eslintrc %}
{
    // ...
    "globals": {
        "global1": true,    // you're allowed to overwrite this one in your code
        "GLOBAL2": false    // ESLint will complain if you try to overwrite this constant
    }
}
{% endcodeblock %}

### Step 4: Set up some vim keybinds

You don't strictly have to do this, but you'll make things much easier for yourself if you set up convenient keybinds for your in-vim linter. I go into a bit more detail about why you'll want this in [my other Neomake post](vim/2015/linting-code-with-neomake-and-neovim), but here's how I've set mine up:

{% codeblock lang:vim init.vim line_number:false %}
" neomake
nmap <Leader><Space>o :lopen<CR>      " open location window
nmap <Leader><Space>c :lclose<CR>     " close location window
nmap <Leader><Space>, :ll<CR>         " go to current error/warning
nmap <Leader><Space>n :lnext<CR>      " next error/warning
nmap <Leader><Space>p :lprev<CR>      " previous error/warning
{% endcodeblock %}

In my case, I have <Leader> set to comma (comma-as-leader masterrace reporting in).

So, for example, to go to the current error, I just type `, ,` (`<comma><space><comma>`) and so on. Quite convenient.

Now if only there was another plugin to automatically debug my code...
