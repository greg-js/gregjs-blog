title: 'I wrote a Hexo plugin: hexo-easy-edit'
categories: javascript
tags:
  - hexo
  - open source
  - js
  - node
date: 2015-11-04 17:12:23
---


I'm still pretty new to programming so forgive me if I sound overly enthusiastic, but I'm pretty psyched about this plugin I made for [Hexo](https://hexo.io).

{% asset_img hexo-easy-edit.png hexo-easy-edit screenshot %}

Hexo is a Node.js-powered static site generator and I used it to generate this very blog you're reading right now. I've been trying to be more active on GitHub, but it's pretty tough when you're new and not feeling very confident about your abilities. So last week I rewrote most of the Hexo documentation (the maintainers are Chinese and their docs were understandably not in perfect English) in order to at least do *some* good work in open source.

But as I was proofreading the docs and writing a few blog posts on here, it suddenly occurred to me that a) I could really use a tool to help me blog more efficiently, and b) I actually knew enough JavaScript-fu to do it. So I wrote it, pushed it [to GitHub](https://github.com/greg-js/hexo-easy-edit) and npm, then pushed it a few more times because I kept finding little bugs and updating the readme (lesson learned: test more before pushing), and [here it is](https://www.npmjs.com/package/hexo-easy-edit)!

<!-- more -->

In short, I just got tired of constantly having to navigate to the `source/_posts` and `source/_drafts` folders and open the markdown files in vim. Of course I could use one of the administration plugins to help me administer the site, but I feel that's kind of missing the point of Hexo's simplicity. Plus, I really just want to edit my posts in vim, you know?

So what my plugin does is it adds an extra CLI command to Hexo's suite of commands: `hexo edit`. On its own, it will just look into the folder you're on in the terminal right now, find the source folder, and give you a list of all the markdown files in it. Then you can choose one of them using the arrow or vim-style keys and it will spawn a child process to open the file with whatever you've set as your $EDITOR.

I then made it a little bit more useful by adding the ability to also open it using a GUI editors by adding a `-g` or `--gui` flag. I also added a `-t` or `--target` flag for filtering on subfolders (like `_drafts`, `_posts` or `docs`) and allowed people to filter on a regular expression.

I know it's not the most amazing thing in the world, but I'm happy with it. It's genuinely useful for me and I think at least some other people might have use for it too. Give it a whirl if you're a command-line person and use Hexo. And feel free to complain, open issues or shout at me if you think my code is shit! After all, I'll only know I've really made it as a programmer when I [start receiving hate mail about my code from nerd superstars](http://thenextweb.com/dd/2015/11/02/linux-creator-linus-torvalds-had-a-meltdown-over-a-pull-request-and-it-was-awesome/).
