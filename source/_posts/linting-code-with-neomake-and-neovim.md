title: Linting code with Neomake and Neovim
categories: vim
tags:
  - vim
  - linting
  - neovim
  - style
  - js
date: 2015-11-03 17:09:05
---

*Related:*
* [Linting code with Neovim and Neomake, ESLint edition](/vim/2015/linting-code-with-neovim-and-neomake-eslint-edition/)
* [Lint as you type with Neovim and Neomake](/vim/2015/lint-as-you-type-with-neovim-and-neomake/)

{% raw %}
<hr />
{% endraw %}


I've been using Neovim for a while now and absolutely love it. One of the major advantages it has over regular vim is its asynchronous job processing functionality.

While not a lot of plugins leverage async job-control yet, the few that do offer significant performance increases compared to synchronous alternatives. One such plugin is [Neomake](https://github.com/benekastah/neomake), which is heavily inspired by the wildly popular [Syntastic](https://github.com/scrooloose/syntastic). By the way, Neomake also works in regular vim, but then it won't be async.

{% asset_img neomake_example.png Example of Neomake linting JavaScript code with JSCS %}

Setting it up isn't hard, but I have to admit I spent an hour or two to sort of get it right. The reason is that I was so used to Syntastic's setup that I didn't realize what exactly was happening behind the scenes. So I had to do some reading and experimenting to figure out how to use it right. Information about this is scarce on the web right now, so if you're in a similar jam or just want to know more about Neomake and what it can do for you, read on.

<!-- more -->

## Syntastic and Neomake
You can use both Syntastic and Neomake for all kinds of purposes, but the main thing they're used for is to lint your files while you edit them. The linting output gets sent to vim's locations or quickfix window and notifications are placed in the gutter at the relevant lines. You can then jump from one error/warning to the other and fix the issues.

So why use Neomake over Syntastic if Syntastic is so popular and beloved (5,685 stars on Github a the time of writing)? Well, because Neomake uses asychronous job processing, the linting commands are fired off in the background, resulting in the elimination of those annoying wait-times in between saves you will experience with Syntastic in traditional vim.

## How to set it up
Right now I'm just using Neomake to lint my JavaScript code with [JSCS](http://jscs.info/), so that's what I'll use here. Other linters for all language are pretty much identical to set up and use though.

First, you'll need to install JSCS globally:

{% codeblock lang:bash line_number:false %}
  npm install -g jscs
{% endcodeblock %}

(Optional), generate a `.jscsrc` for your project:

{% codeblock lang:bash line_number:false %}
  cd myProject
  jscs --auto-configure ./
{% endcodeblock %}

Next, install Neomake, using whatever plugin manager you prefer (I like [vim-plug](https://github.com/junegunn/vim-plug)). Neomake should already be able to lint a JS file you've got open in the current buffer if you run `:Neomake`.

However, here's what to add to your vim config file so Neomake runs automatically whenever you enter or save a buffer containing a JavaScript file (delete the BufEnter if you only want it to run upon writing a file, which is recommended):

{% codeblock lang:vimscript line_number:false init.vim/.nvimrc %}
autocmd! BufWritePost,BufEnter * Neomake
{% endcodeblock %}

Neomake *should* work now (if it doesn't, do some debugging by setting `let g:neomake_verbose=3` and/or `let g:neomake_logfile='/tmp/error.log'` and inspecting the output). Warning and error symbols should appear in your gutter if errors are found. But where's the error window?

Well, you have to call it yourself. If you set it up like explained above, you'll get them in the locations window, which you can call with `:lopen` and close with `:lclose`. You can also go to a specific error by calling `:ll #` with # being the error number. But an easier solution would likely be to add this to your vim config:

{% codeblock line_number:false init.vim/.nvimrc %}
let g:neomake_open_list = 2
{% endcodeblock %}

This will open the window automatically when Neomake is run, but without moving your cursor. Now you can run `:ll #` to move to errors or just `:ll` to go to the first one. When you've fixed all errors and save, the window will close automatically. Neat :-)

We're not done yet though. What if you want to pass extra arguments to your linter program? For example, I like to write my JS using Babel, JSX and ES6, so I want to use the `--esnext` flag. I might also want to change my default preset (which will be used if there is no `.jscsrc` present in the root folder of the project).

The answer to those issues is to redefine the maker. Here's how I set mine up. Note how the `args` array contains all the arguments in order. The name of the file to be linted will be added automatically at the end, but if you want to put it somewhere else, use `%:p`. The reporter you should use and the errorformat string will depend on the output of your linter but you don't have to look it up yourself, just look it up [here](https://github.com/benekastah/neomake/tree/master/autoload/neomake/makers/ft). Anyway, here's my setup for my jscs linter:

{% codeblock line_number:false init.vim/.nvimrc %}
let g:neomake_javascript_jscs_maker = {
    \ 'exe': 'jscs',
    \ 'args': ['--no-color', '--preset', 'airbnb', '--reporter', 'inline', '--esnext'],
    \ 'errorformat': '%f: line %l\, col %c\, %m',
    \ }

let g:neomake_javascript_enabled_makers = ['jscs']
{% endcodeblock %}

Lastly, the signs inserted into your gutter by Neomake may or may not look nice depending on what font you've got installed. Unfortunately, it didn't look all that great on mine, so I changed the symbols to a simple `W` for warnings and `E` for errors:

{% codeblock line_number:false init.vim/.nvimrc %}
let g:neomake_warning_sign = {
  \ 'text': 'W',
  \ 'texthl': 'WarningMsg',
  \ }

let g:neomake_error_sign = {
  \ 'text': 'E',
  \ 'texthl': 'ErrorMsg',
  \ }
{% endcodeblock %}

(note: now you can style the vim highlighting using the values in `texthl`)

And that's it. It's a little bit more work to set up maybe, but thanks to Neomake's async job-control, you'll never have to wait for Syntastic to finish linting your code ever again. Enjoy.
