title: Configuring the deoplete asynchronous keyword completion plugin with tern-for-vim
date: 2016-01-21 15:00:07
categories: vim
tags:
  - neovim
  - autocompletion
  - dotfiles
---

*The information contained within this article is not necessarily wrong, but please note that I wrote an update to this article in September 2016:*
* [Neovim, Deoplete, JSPC, UltiSnips and Tern: A config for kickass autocompletion](vim/2016/neovim-deoplete-jspc-ultisnips-and-tern-a-config-for-kickass-autocompletion/)

{% raw %}
<hr />
{% endraw %}

If you're a vim-user and you have at least a couple of vim plugins installed, chances are you've heard of [Shougo](https://github.com/shougo). While [Tim Pope](https://github.com/tpope) is still probably the most prolific and well-known author of widely used vim plugins, Shougo is way up there along with him.

Why am I talking about Shougo? Well, like many vim-lovers I have made the move to Neovim, and Shougo had announced over half a year ago that he would create something the people in the Neovim-camp have been waiting for: a high-performing autocompletion plugin that takes advantage of Neovim's built-in asynchronous job control.

{% asset_img autotern.png Autocompletion with tern for vim %}

Well, I'd been using good old [SuperTab](https://github.com/ervandew/supertab) for basic autocompletion while Shougo worked on his plugin (sadly I'm not skilled enough to help him.. yet) but I just checked the other day and it turns out [the wait is over](https://github.com/Shougo/deoplete.nvim)!

<!-- more -->

## Why Deoplete and tern-for-vim?

[Deoplete](https://github.com/Shougo/deoplete.nvim) - *dark powered neo-completion* - is Shougo's new asynchronous keyword completion system and it's pretty darn awesome if you ask me! Pair it up with [Tern for Vim](https://github.com/ternjs/tern_for_vim) and you get some really nifty functionality (along with classic autocompletion, asynchronous-style). Take the screenshot above as a very simple example.

Configuring Deoplete and tern-for-vim isn't hard. But hey, what's a blogger like me to do? Talk about really hard stuff? Nah, I'll just help you out with the easy stuff is what I'll do! It might save you some time and give me some exposure - everybody wins! ;-)

Anyway, you know what autocompletion is for and you know why it's a great candidate for making use of asynchronous job control, so I'm not going to waste your time belaboring its usefulness. Some of you may be wondering what tern-for-vim is about though.

Well, Tern offers JavaScript code-analysis and brings non-IDE editors like vim and Sublime Text a little closer to what you might expect from actual IDE's like WebStorm, but without the bloat that comes with something like WebStorm (I say this as a former WebStorm license holder who had this program constantly grind his poor pc to a halt). I'll refer you to [the official website](http://ternjs.net/) if you want to know more, but suffice it to say that the tern-for-vim plugin provides some very nice autocompletion and other features to everyone's favorite editor (vim, just in case that wasn't obvious).

{% asset_img autocompletion.png Autocompletion example %}

## Initializing Deoplete and tern-for-vim

You should use one of the many plugin managers to install the plugins. Shougo made one (of course he did) called [neobundle](https://github.com/Shougo/neobundle.vim), but personally I use [vim-plug](https://github.com/junegunn/vim-plug), so I'll be using that syntax.

Installing is a breeze with a manager like vim-plug:

{% codeblock lang:vimscript init.vim %}
call plug#begin('$HOME/.config/nvim/plugged')

Plug 'Shougo/deoplete.vim'
Plug 'ternjs/tern_for_vim', { 'do': 'npm install' }

call plug#end()
{% endcodeblock %}

Note the `npm install` line there. If your plugin manager doesn't support something like this, you'll have to manually go into the installation folder and do an `npm install` yourself. The reason is that Tern actually spins up a NodeJS server to analyze your code on the fly.

By the way, to install plugins using vim-plug, source your config files and simply run `:PlugInstall`. Later on, run `:PlugClean` to get rid of deleted plugins, `:PlugUpdate` to update your installed ones and `:PlugUpgrade` to update vim-plug itself.

## Configuration

Now let's configure deoplete. Note that I've made some personal choices here with regards to how I like to do things. Run `:help deoplete` to get the manual for the plugin in case you want to look up how to do things differently. Also, I recommend [splitting up your .vimrc/init.vim file](/vim/2016/do-yourself-a-favor-and-modularize-your-vimrc-init-vim) into multiple configuration files in order to maintain your own sanity in the long run.

{% codeblock lang:vim plugins.vimrc %}
let g:deoplete#enable_at_startup = 1
if !exists('g:deoplete#omni#input_patterns')
  let g:deoplete#omni#input_patterns = {}
endif
" let g:deoplete#disable_auto_complete = 1
autocmd InsertLeave,CompleteDone * if pumvisible() == 0 | pclose | endif
{% endcodeblock %}

Lines 1-4 enable deoplete at startup and make sure the autocompletion will actually trigger using the omnifuncs set later on.

The commented line 5 would actually *disable* **auto**completion, forcing me to press the trigger to see the autocomplete possibilities. I've been playing with it on and off because I'm not sure I like having my text blocked with lots of possible autocompletes while I type.. YMMV

Line 6 is for automatically closing the scratch window at the top of the vim window on finishing a complete or leaving insert. If you don't have something like this set, you'll have to manually close it with `:pc`, `:pclose` or `:q` with the preview window active.

Now for setting some local omnifunc settings. Of course this depends on what you have installed (if anything).

{% codeblock lang:vim plugins.vimrc %}
" omnifuncs
augroup omnifuncs
  autocmd!
  autocmd FileType css setlocal omnifunc=csscomplete#CompleteCSS
  autocmd FileType html,markdown setlocal omnifunc=htmlcomplete#CompleteTags
  autocmd FileType javascript setlocal omnifunc=javascriptcomplete#CompleteJS
  autocmd FileType python setlocal omnifunc=pythoncomplete#Complete
  autocmd FileType xml setlocal omnifunc=xmlcomplete#CompleteTags
augroup end

" tern
if exists('g:plugs["tern_for_vim"]')
  let g:tern_show_argument_hints = 'on_hold'
  let g:tern_show_signature_in_pum = 1

  autocmd FileType javascript setlocal omnifunc=tern#Complete
endif
{% endcodeblock %}

The last lines under `" tern` set the omnifunc for JavaScript to `tern#Complete`, which enables tern *and* makes it fall back to whatever you have set otherwise (in my case set on line 6 here).

As for the settings on line 13 and 14, check the tern manual (`:help tern`) for more information. There are actually many more things you can do and I have not spent a lot of time there myself, so I may end up writing another post later about other features of this plugin.

Also note that I wrapped the tern settings in an if-statement. The reason is that I don't have it installed on my server but prefer using the same configuration files. This way I can just copy the config over without it breaking my setup.

## Setting up deoplete to use `<Tab>`

The default key for autocompletion is `<Ctrl-x><Ctrl-o>` and `<Ctrl-P>` for going up in the list and `<Ctrl-N>` for going down. At least, I think it is, as I changed it to exclusively use `<Tab>` pretty much immediately.

I originally had a pretty ugly hack here since remapping `<Tab>` in insert mode breaks actual tabs, but commenter `TabComplete` (I doubt that's their real name though) came up with a much better solution so here's how to do it the *right* way:

{% codeblock lang:vim keys.vimrc %}
" deoplete tab-complete
inoremap <expr><tab> pumvisible() ? "\<c-n>" : "\<tab>"

" tern
autocmd FileType javascript nnoremap <silent> <buffer> gb :TernDef<CR>
{% endcodeblock %}

Line 2 sets `<Tab>` up to do what you most likely want it to do -- autocomplete and cycle from top to bottom.

Lastly, I also set up a `gb` keybinding for moving the cursor straight to a variable definition using Tern's JS code analysis engine.

## Done

I haven't talked about introducing your own sources in this article as I haven't done so myself yet, but I might write another post about that if or when I get to it. But either way, if you followed this, you should have deoplete and tern-for-vim working quite nicely (and speedily) together. Isn't it beautiful?

{% asset_img beautiful.png Another autocompletion example %}
