title: Do yourself a favor and modularize your .vimrc/init.vim
date: 2016-01-08 15:31:26
categories: vim
tags:
  - neovim
  - style
  - dotfiles
---

I spend quite a bit of time reading various blogs and forums frequented by vim/neovim users. Of course the topic of the `.vimrc` (or `init.vim` [if you've switched to the new neovim style](/vim/2015/no-more-nvimrc-neovim-folder-now-at-config-nvim/)) file comes up all the time in such circles and I've learned a huge deal from looking at (and stealing parts of) other people's configuration files.

{% asset_img tools.jpg Organized tools %}

That said, I am often baffled by the lack of organization in people's config files. More often than not it's a total mess, but even when the file has been carefully commented and organized, it's almost always **just one file**.

<!-- more -->

It's actually even worse than that. I often see people *boasting* about how many lines their `.vimrc`/`init.vim` has, as if it that's somehow a sign of how advanced their setup is. But why? Modularization is not only everywhere, it is universally accepted as a very good thing, so why is it so rare in vim configs?

Mind you, my own setup isn't quite the shining example of impeccable organization I would like it to be. Mine too is the result of years-long piecemeal expansion, of adding something here, copy-pasting something there, tweaking something elsewhere. But at least mine isn't a gigantic file with key bindings, plugin initializations, color schemes, UI settings and what not, all mixed in with one another!

Most of us probably tweak our vim config files once every couple of weeks and when that happens, you'll be so glad you've taken taken ten minutes out of your busy schedule to split up your `.vimrc`/`init.vim` into some logical categories. As an example, this is what mine looks like:

{% codeblock lang:vim init.vim %}
source $HOME/.config/nvim/config/init.vimrc
source $HOME/.config/nvim/config/general.vimrc
source $HOME/.config/nvim/config/plugins.vimrc
source $HOME/.config/nvim/config/keys.vimrc
source $HOME/.config/nvim/config/line.vimrc
{% endcodeblock %}

Doesn't that make a whole lot more sense?

Now, I'm not saying you should organize yours like mine. Maybe you can come up with something much better than this. In fact, I wouldn't be surprised if you did! But for what it's worth, here's what's in each of these configuration files:

- `init.vimrc` holds my `vim-plug` ([my favorite plugin manager](https://github.com/junegunn/vim-plug)) section, which initializes all my other plugins.
- `general.vimrc` holds a bunch of general settings which didn't fit in the other categories.
- `plugins.vimrc` holds all my plugin-specific settings.
- `keys.vimrc` holds all my custom key-bindings.
- `line.vimrc` holds my statusline configuration.

In conclusion, sorry for this maybe somewhat ranting post. It's just that I keep seeing these enormous vim config files everywhere and I just don't get it. I mean, hey, whatever works for you of course, but.. Well, my way is better! ;-)

Oh, and just in case you're interested in the content of my vim config files, [feel free to have a look](https://github.com/greg-js/dotfiles/tree/master/.config/nvim).
