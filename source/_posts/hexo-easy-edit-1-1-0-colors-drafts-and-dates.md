title: 'hexo-easy-edit 1.1.0: colors, drafts and dates'
categories: javascript
tags:
  - hexo
  - node
  - open source
date: 2015-11-09 16:00:54
---


I just pushed an update to [my hexo plugin](http://gregjs.com/javascript/2015/i-wrote-a-hexo-plugin-hexo-easy-edit/). Changes:

- Menu output now displays the title instead of the slug. Extra information like published status, date and folder has been added as well for convenience.
- Output is now color coded.
- Support added for filtering on before-date (`-b`, `--before`), after-date (`-a`, `--after`) and published status (`-d`, `--draft`).
- Internals have been promisified for much easier reading :-)
- Various little tweaks to make command line editing of your hexo blog posts more convenient.

{% asset_img screen.png hexo-easy-edit 1.1.0 screenshot %}

Still on my todo-list for this project:

- Turn simple title regexing into fuzzy search.
- Add a `remove` command.
- Improve the `publish` command.
