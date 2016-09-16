title: "Neovim, Deoplete, JSPC, UltiSnips and Tern: A config for kickass autocompletion"
date: 2016-09-16T12:32:33.746Z
category: vim
tags:
- autocompletion
- dotfiles
- neovim
- async
---

First of all, sorry for the delay, I promised to write this earlier but life got in the way.. Hopefully I'll be writing more regularly again now though :-)

{% asset_img screen.png Tern autocomplete example %}

Anyway, earlier this year I wrote [a series of Neovim articles](/tags/neovim/) on how to set up some of the more popular plugins. The one I did on [Deoplete](https://github.com/Shougo/deoplete.nvim) - [Shougo](https://www.github.com/Shougo)'s asynchronous keyword completion plugin - is still one of the most often-visited articles on this blog.

I had recently been made aware though that maybe [my original article](/vim/2016/configuring-the-deoplete-asynchronous-keyword-completion-plugin-with-tern-for-vim) was a little simplistic as some people were still experiencing problems setting Deoplete up and make it play nice with multiple completion plugins. As I still believe in Deoplete as a fantastic productivity-booster, I decided to write an update and hopefully help some people make their kickass Neovim setup just that little more kickass!

In this case, I will be setting up Neovim so the Deoplete autocompletion engine pops up automagically and pulls in from a few different sources: words from the file itself, [UltiSnips snippets](https://github.com/SirVer/ultisnips), [JSPC parameter completion](https://github.com/othree/jspc.vim) and [the Tern code-analysis engine for JavaScript](http://ternjs.net/). If you want to use different autocompletion plugins or a different language, then that's fine, you can still use this guide, just change the plugins, sources and omnifuncs and check your plugin's readme if you need extra help.

Let's get started!

<!-- more -->

## First things first

I'll assume for the rest of this guide that you're using some kind of plugin manager. I recommend [vim-plug](https://github.com/junegunn/vim-plug) but there a number of good ones, pick whichever you prefer. I will be using `vim-plug` syntax in the rest of this article.

I'll also assume you've either got a clean config, or at least a config without any autocompletion-related plugins already set up. Many of the potential issues you may run into when setting this up are probably related to conflicting plugins and/or conflicting settings. So try to get rid of (or backup/`git commit`) those settings so you can start with a clean slate.

## Install Deoplete and enable it

[Deoplete](https://github.com/Shougo/deoplete.nvim) is of course the one plugin everything else relies on, so install it by adding this to your vim config and executing a `:PlugInstall` (if you're using `vim-plug`):

``` vim ~/.config/nvim/init.vim
...
Plug 'Shougo/deoplete.nvim', { 'do': ':UpdateRemotePlugins' }
Plug 'ervandew/supertab'
...
```

If you're using a different plugin manager, you may have to run `:UpdateRemotePlugins` manually. Remember you do need Python3 enabled as well, but this shouldn't be an issue. Just in case it is, you'll have to run one or both of these on the command-line first:

``` bash
pip3 install neovim
pip3 install --upgrade neovim
```

And to enable Deoplete, add this to your vim config:

``` vim ~/.config/nvim/init.vim
let g:deoplete#enable_at_startup = 1
```

Also, note I've also installed good old [SuperTab](https://github.com/ervandew/supertab). This will make it much easier later on to use `<Tab>` for everything.

## Install your autocompletion plugins

Again, if you have different plugins, then install those. For this guide I'll stick with a few of my personal favorites for writing JavaScript in vim:

``` vim ~/.config/nvim/init.vim
Plug 'SirVer/ultisnips'
Plug 'honza/vim-snippets'
Plug 'ternjs/tern_for_vim', { 'for': ['javascript', 'javascript.jsx'] }
Plug 'carlitux/deoplete-ternjs', { 'for': ['javascript', 'javascript.jsx'] }
Plug 'othree/jspc.vim', { 'for': ['javascript', 'javascript.jsx'] }
```

[UltiSnips](https://github.com/SirVer/ultisnips) is a great snippet engine and [vim-snippets](https://github.com/honza/vim-snippets) is a large collection of snippets ready for you to use. Of course you can (and should) write your own snippets as well. I tend to mostly use my own, but the ready-made ones are great too! You just have to remember the shortcuts.

[tern_for_vim](https://github.com/ternjs/tern_for_vim) is actually not strictly necessary, but it's good to have, as it gives you access to extra Tern features from inside vim (such as `:TernDefPreview`). To complete installation for `tern_for_vim`, you *must* go into the folder (if you're using `vim-plug`, it'll be the folder set up at the top of your plugins after `call plug#begin`) and do an `npm install`:

``` bash ~/.config/nvim/plugged/tern_for_vim/
npm install
```

[deoplete-ternjs](https://github.com/carlitux/deoplete-ternjs) now provides an easier way to access Tern-completions. Lastly, [JSPC](https://github.com/othree/jspc.vim) gives you better function parameter completion.

Don't forget to actually install the plugins with `:PlugInstall` or the equivalent thereof for your plugin manager.

Note that Tern may not work out of the box just like this. These plugins merely provide a way to access the information spit out by the Tern server. If you don't have it installed yet, you'll have to manually get it on your machine first as well:

``` bash
npm install -g tern
```

And one more thing just in case some people don't know: to actually use Tern, you need a `.tern-project` file in your project directory. Read about the file in question [here](http://ternjs.net/doc/manual.html#configuration) or try this sample config:

``` json path/to/your/project/.tern-project
{
  "plugins": {
    "es_modules": {},
    "node": {}
  },
  "libs": [
    "ecma5",
    "ecma6",
    "browser",
    "jquery"
  ],
  "ecmaVersion": 6
}
```

## Set up your omnifuncs and sources

This can be a little confusing, especially since there is more than one way to set this up. You can simply overwrite omnifuncs for specific filetypes like this:

``` vim ~/.config/nvim/init.vim
autocmd FileType css setlocal omnifunc=csscomplete#CompleteCSS
```

However, if you want Deoplete to play nice with multiple omnifunctions provided by third-party plugins, you should use these Deoplete-specific settings:

``` vim ~/.config/nvim/init.vim
let g:deoplete#omni#functions = {}
let g:deoplete#omni#functions.javascript = [
  \ 'tern#Complete',
  \ 'jspc#omni'
\]
```

Okay, first of all, the first line is really just a bit of a VimL quirk. Omitting it will cause an error because the variable won't be defined when you try to set it in line 2-5.

For those among you with different plugins, what exactly you need to put here will vary depending on the plugins you've chosen. In this case, JSPC takes the default JS omnifunc and makes *additions* to it, so you don't need the default anymore. Tern on the other hand is a wholly separate source, hence you need to add it as part of an array, so Deoplete has access to everything. Refer to your plugins' Readmes to learn how to set yours up.

Finally, you'll probably want to adjust which sources Deoplete pulls from when showing completions:

``` ~/.config/nvim/init.vim
set completeopt=longest,menuone,preview
let g:deoplete#sources = {}
let g:deoplete#sources['javascript.jsx'] = ['file', 'ultisnips', 'ternjs']
let g:tern#command = ['tern']
let g:tern#arguments = ['--persistent']
```

The `ultisnips` and `ternjs` sources speak for themselves, but the `file` source grabs words from the current file, which may or may not be useful for you. Another commonly used one is `buffer`, which will grab words from the currently open buffers. Personally, I find myself often having so many buffers open that this causes spam in my autocompletion menu, so I don't always use it.

The two last `tern` settings are only necessary (or recommended) if you're also using `tern_for_vim`. If you're just using `deoplete-ternjs`, I'm pretty sure you won't need these.

## Use Tab for everything (except UltiSnips)!

Here's how I put it all together:

``` vim ~/.config/nvim/init.vim
autocmd FileType javascript let g:SuperTabDefaultCompletionType = "<c-x><c-o>"
let g:UltiSnipsExpandTrigger="<C-j>"
inoremap <expr><TAB>  pumvisible() ? "\<C-n>" : "\<TAB>"
```

In this JavaScript-specific case, I want `SuperTab` to help me use `<Tab>` for everything *except expanding UltiSnips snippets* (trust me, you don't want this because it's incredibly annoying). This way, you can tab through all your completion suggestions and use `Ctrl+j` to expand and walk through your snippets. But your mileage may vary. You might prefer it set up a different way but this works great for me.

One last thing. You might be loving Tern's autocompletion, but not so much the fact that it automatically pops up preview windows with extra info. Here are two possible ways to address that (a third way might be to set up your own key binding for `:pclose`):

``` vim ~/.config/nvim/init.vim
" close the preview window when you're not using it
let g:SuperTabClosePreviewOnPopupClose = 1

" or just disable the preview entirely
set completeopt-=preview
```

## Done

**Phew**! Well, I hope this is useful for some of you out there! I know setting up (neo)vim to work more like an IDE can be a bit of work. But you get so much in return it's worth a little tinkering in my opinion.

Now, enjoy, feel free to use the comment section if you're still experiencing issues, and here's to hoping my next article will hit the virtual presses a little sooner than this one. Keep on coding, friends!
