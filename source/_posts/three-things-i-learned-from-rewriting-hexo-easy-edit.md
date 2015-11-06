title: Three things I learned from rewriting hexo-easy-edit
categories: javascript
tags:
  - hexo
  - learning
  - node
  - open source
date: 2015-11-06 01:42:43
---


Earlier today I ended up rewriting [the Hexo plugin](https://github.com/greg-js/vim-tag-plugin-snippets) I wrote a couple of days ago. The reason was that I suddenly realized a better approach. And what's more: after rewriting it, it was almost child's play to add a couple of extra features and general niceties.

Besides being able to edit any blog post of a Hexo blog in your editor of choice from the command line using a regular expression on the title and/or subfolder, you can now also filter on tags and plugins and open newly created posts automatically. Also, the little menu now displays your posts in chronological order :-)

Now, I'm by no means an expert of course, but I'd like to jot down a couple of thoughts, because I feel like I did learn some valuable lessons from writing this [modest but actually useful plugin](https://www.npmjs.com/package/hexo-easy-edit).

<!-- more -->

## A few things I learned:

- **Think before coding.**
Before the rewrite, I was constantly using the filesystem module to check folders, filter files and extensions, compare stats, etc. Then I realized I could use database queries instead! That made it so much easier to do powerful things (and using fewer callbacks and promises). In retrospect, I really should have thought things trough more before sitting down and starting to code!

- **Test before pushing.**
I know about the importance of testing, but I figured it just wasn't necessary for something as simple as this. Well, I was wrong, because I accidentally pushed out buggy versions (as well as publishing them to npm) several times. I'll definitely try to write my next project using a TDD or BDD test suite, even if it's something small.

- **Just do it.**
Those who know me may disagree but I am at heart a very insecure person. I'd been putting off putting my code online, preferring to focus on studying theory and doing practice projects. But while theory and practice are important too, I feel like the past week I've spent on GitHub may have been more educational for me than the past four months I spent studying math, Linux and algorithms.. There's a time for everything, but at some point, you really need to **just do it**.

{% youtube ZXsQAXx_ao0 %}
