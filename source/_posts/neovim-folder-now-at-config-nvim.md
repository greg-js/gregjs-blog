title: No more ~/.nvimrc - Neovim folder now at ~/.config/nvim
categories: vim
tags:
  - neovim
date: 2015-11-08 20:39:28
---


Just in case you missed [the announcement](https://github.com/neovim/neovim/wiki/Following-HEAD#20151026), Neovim now supports [XDG configuration](http://www.gregjs.com/linux/2015/fixing-the-reappearing-folders-problem-in-your-home-directory/) and has moved to a new configuration folder.

That means that your old `~/.nvimrc` and `~/.nvim` paths won't work anymore by default. It's possible this came as a shock to you when you updated and realized none of your configuration or plugins work anymore.

The fix is very easy though.

- `mv ~/.nvim ~/.config/nvim`
- `mv ~/.nvimrc ~/.config/nvim/init.vim`
