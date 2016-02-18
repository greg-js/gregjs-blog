title: 'markdown2troff: regular expressions ftw'
date: 2016-02-18 14:14:38
categories: javascript
tags:
- open source
- npm
- js
---

I published another npm module [on GitHub](https://github.com/greg-js/markdown2troff) [and npm](https://www.npmjs.com/package/markdown2troff). It's a simple converter that takes `markdown` as an input and spits out `troff`.

For those not in the know, `troff` (and its GNU implementation, `groff`) is a somewhat arcane text format from the seventies, still used today in linux `man` pages - and maybe other things? It's not quite the most pleasant format to work with, but it does the job well, which is why we still have it around.

`markdown -> troff` conversion was already possible in node with [remark-man](https://github.com/wooorm/remark-man). This package works by first creating an [abstract syntax tree (AST)](https://en.wikipedia.org/wiki/Abstract_syntax_tree) from the `markdown` and using that as a base for manipulation or conversion to other formats.

Now, if you want the best (as in, most accurate) possible conversion from `markdown` to `troff`, I can't think of a better approach than the one taken by `remark-man`, because you are guaranteed to have a perfect representation of the source (inasmuch as it is possible to represent `markdown` syntax in `troff` of course).

However, this approach is rather involved, which I noticed when adding the package as a dependency to my [arch-wiki-man](https://www.npmjs.com/package/arch-wiki-man) project. I wanted a much lighter solution and was willing to give up a little accuracy in conversion to achieve it.

So I got to work and made `markdown2troff`. It uses nothing but regular expressions (well, and JavaScript of course) to go from source to output so it's blazing fast and has no dependencies at all and it currently handles all conversion except for tables and some special characters.

I wrote it in a hurry and with a very specific purpose in mind so right now I admit it has a bit of a spaghetti-code vibe to it and I haven't added any tests. When I get some time off from being ill later, I'll go back and make it nicer to look at.

But as far as I can see, it works and does almost exactly what I want. If you need a `markdown` -> `troff` converter in your NodeJS (though it's easily portable to other languages) project, consider my package. And if you do, also consider contributing and making it better! Thanks for reading :-)
