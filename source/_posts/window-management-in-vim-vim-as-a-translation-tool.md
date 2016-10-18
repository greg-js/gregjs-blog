title: "Window management in vim (+ vim as a translation tool)"
date: 2016-10-18T14:12:23.984Z
category: javascript
tags:
- neovim
- tips
- tutorial
---

Everyone's favorite text editor (vim) adheres nicely to the [UNIX philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) of doing one thing well. That one thing that vim does exceedingly well is of course editing plaintext. A corollary of this is that vim users tend to keep things comparatively simple when editing text (the key word here is *comparatively*, as in compared to users of other editors and IDEs -- I know your vim config is hundreds of lines long, but you're still just editing plaintext).

For example, 90% of my own time in a vim instance is spent inside a single window, as I edit just a few buffers, then open and close more as I go along. Of course, I tend to have at least half a dozen such vim instances open at any given time and have delegated a lot of what IDEs usually do to my window manager and UNIX environment, but this is par for the course and I digress.

Still, if 90% of my vim time is "vanilla", that leaves 10% of it for occasional fanciful flights into the spectacular. Sometimes you want to do a little more than just opening a buffer, editing it and writing to it. Sometimes it just doesn't cut it to have several terminal windows open, each running vim.

{% asset_img scrollbind.gif scroll-locking two buffer windows %}

It doesn't happen terribly often, but sometimes you need multiple windows. And vim actually has pretty good window management, allow me to show you.

<!-- more -->

## What are windows?

People who are newer to vim sometimes have a little difficulty dealing with the terminology. What is referred to as a tab in most editors is actually a buffer in vim, while a vim tab is really more like an instance (I think? I admit I never actually use tabs). A window however is pretty much what you'd expect.

The idea is that, much like in [terminal multiplexers](https://en.wikipedia.org/wiki/Terminal_multiplexer) like `screen` or `tmux`, or [tiling window managers](https://www.maketecheasier.com/tiling-window-managers-linux/) like `i3`, `awm` or `dwm`, users can split their available screen real estate into multiple windows. Each of these can then display different (or the same) buffers and you can of course use the registers (among other things) across them.

In fact, even if you're new to vim, you may have come across a few special windows already:
* the `:help` command splits the screen automatically, giving you a new window to read help text in (by the way, you can get docs on any subject including third-party plugins with a simple `:help subject`).
* task runners like [`Neomake`](/vim/2015/linting-code-with-neovim-and-neomake-eslint-edition/) and `Syntactic` pop open the special location window at the bottom
* the autocompletion plugin [`Deoplete`](https://github.com/Shougo/deoplete.nvim) can, when [coupled with something like `tern.js`](/vim/2016/neovim-deoplete-jspc-ultisnips-and-tern-a-config-for-kickass-autocompletion/), pop up handy preview windows with quick function info at the top.

Still, it's doubtful that the uninitiated spend a lot of time managing multiple windows in the same vim instance. It's admittedly a little clunky and the default shortcuts can be somewhat unintuitive.

As you'll see in the next section though, there are definitely some valid use cases for vim windows. Even if you're not immediately going to start using this feature all the time, it'll at least be good to know that you can. And with a little extra configuration and/or a plugin or two, you can really make vim window management a breeze.

## A possible use case: vim for translators

I was inspired to write this short guide after answering someone's question about using vim as a translation tool on the [vim subreddit](https://www.reddit.com/r/vim). Well, before I was a programmer, I actually worked as a professional writer and translator, and in my final year of doing so, I often used vim for it.

The specific question the Reddit user asked was in fact a twofold one:
* how to split text into sentences so they visually stand out and can be easily edited
* how to do the translations for these sentences side by side in a new file

For the first problem, I suggested a simple regex substitution: `:%s/[\.!?][\s\n]*/&\r\r\r\r\r\r/g`

{% asset_img split.gif Splitting text into sentences using a regex %}

Run that on a buffer with some text in it and it will put six newlines after each sentence ending with a period, exclamation mark or question mark. Now, this was just off the top of my head and it won't be perfect (things like "Mr. Gregjs" are an edge case that was obvious right after I made the gif, quotation marks is another now I think about it), but it's probably good enough and it's simple.

For the second problem, I suggested opening a new vertically split window and linking them together. In other words: window management.

## Creating, closing and managing vim windows

You can create a new window by splitting your window either horizontally or vertically. As mentioned earlier, this is of course very similar to `screen`, `tmux` or tiling window managers. Here are some of the default key bindings:

{% codeblock lang:vim %}
" split the screen horizontally and open the current buffer
:split
" split the screen horizontally and open `./newfile`
:split newfile
" split the screen horizontally with a new empty buffer
:new

" split the screen vertically and open the current buffer
:vsplit
" split the screen vertically and open `./newfile`
:vsplit newfile
" split the screen vertically with a new empty buffer
:vnew
{% endcodeblock %}

As aliases for `:split` and `:vsplit`, you can also use the key combination `CTRL+w`, followed by `s` or `v` respectively (I wish they'd made it `h` rather than `s`, but it is what it is).

To cycle through your windows, hold `CTRL` and press `w` twice. This will make vim focus on the next window. Alternatively, you can use `CTRL+w`, followed by one of the directional `hjkl` keys (or *horror* the arrow keys) to switch to a specific window to the left, bottom, top or right of the current one.

To close a  window, `:hide` or `:close` it. If you're still following along, you can also maximize a window with `CTRL+w`, followed by `_` and make them equal size again with `CTRL+w`, followed by `=`. And there are quite a few additional commands to change window size and more, but I personally recommend just using the mouse for that.

I do recommend setting up some of your bindings and maybe use a plugin, so make sure to read the last section of this article before you commit these key bindings to memory.

## Linking two buffer windows

So with basic window management out of the way, let's now use it for translation. First, we open some file with text in it, then run that regex on it to split it into sentences.

Next, we open a new file using a a vertical split with `:vsplit translation.txt` (and let's fill it up with some empty lines or yank-put the original content, just to better appreciate the step after this one).

{% asset_img vertical-split.gif vertically splitting a vim window %}

Now, let's link the two buffer windows together by running `:set scrollbind` (or `:set scrollbind!` if you want to toggle it using the same command later) in each of the windows. Remember, you can cycle to the next window with `CTRL-w` `CTRL-w` (or hold the `CTRL` and tap `w` multiple times).

There you have it. When you scroll the left window, the right one will scroll along with it and vice versa (note: in the gif below, I copied and pasted everything so you can better see what's going on).

{% asset_img scrollbind.gif scroll-locking two buffer windows %}

Again, this is not going to be useful 100% of the time, but 100% of the times you do need it, you'll be very glad you know how to do it.

## A few suggestions for custom key bindings

I don't know about you, but I think the default bindings are a little awkward. Here are my current custom key bindings for window management, but if you think you have a better setup, I'd love to hear it in the comments:

{% codeblock lang:vim %}
" window keys
nnoremap <Leader>wh :new<CR>
nnoremap <Leader>wv :vnew<CR>
nnoremap <Leader>wH :split<CR>
nnoremap <Leader>wV :vsplit<CR>
nnoremap <Leader>wn :wincmd w<CR>
nnoremap <Leader>wc :close<CR>
{% endcodeblock %}

(by the way, my `<Leader>` is `,` (comma), so in practice my keys are prefaced by `,w`)

As for window navigation (going from one to the other), I have gotten very used to [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator), which is useful even if you don't use `tmux` because it gives you `CTRL+[hjkl]` bindings for switching to windows directionally with `hjkl`. But of course you could very easily just make these bindings yourself if you never use `tmux` (hint: `:wincmd` is the command for what happens when you do `CTRL+w`).

I simply don't bother with any of the other commands (and there are a lot of them, just [check the docs](http://vimdoc.sourceforge.net/htmldoc/windows.html)) because I either prefer to use the mouse or just recreate my windows, rather than messing around with trying to resize or rearrange them using the keyboard. Truthfully, I just don't want to have to remember more key bindings.

Anyway, I hope I've inspired at least one person out there to make a little more use of vim windows. Catch you all on the flip side!

{% raw %}
<p style="font-size: smaller">
<strong>P.S.</strong> I've been asked about my dotfiles a few times recently. Sorry, the [version I have up on GitHub](https://github.com/greg-js/dotfiles) is very much outdated as I have both a private and a public git repository tracking it and need to clean out the private stuff before pushing changes to the public repo. I keep forgetting to actually do it though, but eventually I will, sorry to those asking for my recent dotfiles, I'll get it to soon enough.
</p>
{% endraw %}


