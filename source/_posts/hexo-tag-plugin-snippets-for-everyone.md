title: Hexo tag plugin snippets for everyone
categories: vim
tags:
  - open source
  - vim
  - hexo
date: 2015-11-05 14:52:52
---


As I noted in my last post about Hexo, I'm not a big fan anymore of fancy CMS-style administration panels for blogging. Nowadays, I do pretty much all my writing in vim using markdown.

This is actually a pretty big deal for me because I used to be a professional writer/translator, so I've spent a *lot* of time in various administration panels and WYSIWYG editors. If only I'd known back then what I know now: that writing markdown in vim is so much nicer, so much faster, and so much more powerful -- but, admittedly, not as noob-friendly.

Anyway, one of the many, many reasons why I like vim so much is snippets. Most code editors have snippets, so it's not just a vim thing and there are ways to get similar functionality in traditional word processors I suppose. But be that as it may, snippets are just not something that writers tend to use much, even though they probably should.

Since I started blogging again, I've been making new [UltiSnips](https://github.com/SirVer/ultisnips) snippets whenever I came across new opportunities to use them. They're mostly centered around Hexo's tag plugins and offer a very quick and convenient way to add them to your markdown files. I decided to clean them up, expand them and port them to [Snipmate](https://github.com/garbas/vim-snipmate) (the other popular format for vim snippets).

I had some issues getting Snipmate to play nice with my other plugins so I couldn't test them out. They should work I think, but in case I got some of the snippet syntax wrong, send me a pull request. Same goes for if you have more snippets to add to the mix.

Anyway, [here are the snippets](https://github.com/greg-js/vim-tag-plugin-snippets). Hope they're of some use to someone. If not, oh well, I'll just use them myself, thank you very much.
