title: 'Lint as you type with Neovim and Neomake'
categories: vim
tags:
  - neovim
  - async
  - js
  - style
date: 2015-12-15 18:13:37
---

*also:* **[Linting code with Neomake and Neovim](/vim/2015/linting-code-with-neomake-and-neovim)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Linting code with Neovim and Neomake, ESLint edition](/vim/2015/linting-code-with-neovim-and-neomake-eslint-edition)**

An exchange with commenter flndr8 on [my first Neomake post](/vim/2015/linting-code-with-neomake-and-neovim) inspired me to talk a little bit about 'lint-as-you-type' functionality in Neovim.

I'm not sure what the exact term for it is, but what I mean by lint-as-you-type is a kind of IDE-like feature that would run a linter on your code while you are editing the file. This in contrast with the 'lint-when-you-save' approach, which I covered briefly in my previous two articles on this subject.

Now, if this feature is high on your wishlist, I've got good news and bad news. The bad news is that, as far as I know, there is no optimal way (yet) to get this in vim. So if you consider this a killer feature, you may want to consider using a real IDE like [WebStorm](https://www.jetbrains.com/webstorm/).

However, the good news is that you can get very close indeed. So if lint-when-you-save just doesn't cut it for you, and you are willing to change the way you use vim a little bit, you might still get what you want.

<!-- more -->

### Autocommands and events

When you want to run a particular command, plugin or function when something happens in vim, the normal way to do it is with an autocommand or `autocmd`. The syntax is pretty simple, take for example this line, which nets you lint-when-you-save functionality:

{% codeblock lang:vimscript line_number:false init.vim/.nvimrc %}
autocmd BufWritePost,BufEnter * Neomake
{% endcodeblock %}

- `autocmd` tells vim that this line is an autocommand
- `BufWritePost,BufEnter` are two events to listen for
- `*` is a selector (in this case a wildcard) - for example to make the command only run on javascript files, replace this with `*.js`
- `Neomake` is the command that runs when the event triggers

Conclusion: with this line, Neomake will run on the current file whenever the `BufWritePost` or `BufEnter` event triggers. `BufWritePost` signifies that it will happen after writing the buffer to the file and `BufEnter` means it will run upon switching to a buffer.

{% asset_img autocmd.png autocmd events screenshot %}

You can get a list of supported events by opening vim and typing `:h autocmd-events`. There are quite a few of them as you see, but the ones that look particularly interesting if you want to enable lint-as-you-type functionality are probably `InsertChange`, `InsertLeave`, `TextChanged` and `TextChangedI`. The story doesn't end here though, because of the way vim handles text editing.

### Buffers and Files

After reading the previous paragraphs, you might suppose that this should net you at least some of the desired lint-as-you-type functionality:

{% codeblock lang:vimscript line_number:false init.vim/.nvimrc %}
autocmd InsertChange * Neomake
{% endcodeblock %}

But it won't, and the reason is that whenever you edit a file using vim, it creates a buffer with the contents of that file. Only when you save (`:write`) the buffer to the file does the underlying file get changed. Until then it remains untouched.

This means that the above line *will* cause Neomake to lint your code using whatever linters you have enabled. However, it will run it on the *file*, which is untouched, and not on the buffer, which holds your changes.

### Lint-as-you-type

This brings me to the crux of this post. As far as I know, there is no quick and easy way to run shell commands (like linters) on your buffer contents instead of on your file. There may be some plugins out there that do this, but I personally don't know of any.

The logical workaround is simple: if you want lint-as-you-type, you'll have to write the buffer before running the linter. Fortunately this is easy, as in vimscript you can chain commands with the pipe (`|`) character:

{% codeblock lang:vimscript line_number:false init.vim/.nvimrc %}
autocmd InsertChange * update | Neomake
{% endcodeblock %}

_(note: `update` is just like `write`, only it only writes when the buffer has been modified)_

If you try this out, you'll see that the linter will run every time you change something in insert mode. But you'll also see that this is probably not what you want. As vim-users, we're used to making edits in small logical chunks, so it makes more sense to use `InsertLeave`. You'll also want to run the linter on changes from normal mode, so try this:

{% codeblock lang:vimscript line_number:false init.vim/.nvimrc %}
autocmd InsertChange,TextChanged * update | Neomake
{% endcodeblock %}

In lieu of operating on buffers instead of files, this is in my opinion probably the closest you can get to lint-as-you-type for now.

### Notes

Adding this code to your vim config file has a serious side-effect: every change you make will be written to the file automatically, and this may or may not clash with your editing style. I personally wouldn't be able to handle the paradigm change, but hey, it might fit your way of getting things done perfectly.

One thing I can think of off the top of my head that you really must do if you want to work like this is enabling vim's persistent undo. By default, you will lose your undo history when you quit vim. With the above setting enabled, you won't be able to go back ever again if you accidentally close vim or if there's a unexpected crash or power outage.

Here's what to add to your config to enable persistent undos, which will allow you to go back in such cases:

{% codeblock lang:vimscript line_number:false init.vim/.nvimrc %}
set undodir=~/.config/nvim/undodir
set undofile
{% endcodeblock %}

You can set the amount of undos to remember with `set undolevels=X` (default is 1000) and of course you can set the undodir to anything you want, but make sure the directory exists! Read `:h undo-persistence` to learn more.

That's it. I hope this lint-as-you-type style works out for some of you out there ;-)
