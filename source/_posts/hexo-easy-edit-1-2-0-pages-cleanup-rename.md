title: 'hexo-easy-edit 1.2.0 : pages, cleanup & rename'
categories: javascript
tags:
  - hexo
  - node
  - open source
date: 2015-11-11 16:55:40
---


So I pushed another minor version (quickly followed by a patch) to my little `hexo-easy-edit` plugin.

`npm install --save hexo-easy-edit` to install it.

`npm update hexo-easy-edit` to update if you already had a previous version installed.

I've noticed people are actually starting to use it, so in the off-chance people will visit this blog to see what changed, here's what's new:

<!-- more -->

- Support added for editing pages. Use the `-p`, `--page` or `--pages` option to search your blog's pages rather than its posts. I actually didn't know earlier that hexo made such a distinction in its internal database, so this is an important update.

- `rename` command has been added. You can now rename posts and pages from the command line using `hexo rename <search terms> <-n "new title">` (without the tags obviously). Besides `-n`, `--new` is also accepted, and you'll want to wrap the new title with quotes (single or double) if you want your title to include spaces.

- `rename` will give you a similar search feature and menu selection to find the post you want to modify as with `edit`. You will also get the choice to rename the *filename*, the *title* of the post, or both. I'm guessing some bugs will appear here as I test everything out more over the coming days/weeks.

- Title regexing has been improved significantly. Multiple words are no longer taken literally. Now they're made into an array, mapped to separate regexes and tested one after the other on both the post's title and its slug. This should make your posts and pages easier to find.

- I cleaned up the code and did some refactoring. Everything's now logically split into separate files and the callback pyramid of doom has been reduced thanks to promisification.

Still on the todo-list:

- Add a remove/delete command
- Improve `publish` (though this looks tough as I'm not sure I can modify the publish command without changing the hexo source, might be that I can only add an event listener after the publishing has already happened)

I'll probably do this tomorrow or the day after and then I'll consider this little project mostly done.
