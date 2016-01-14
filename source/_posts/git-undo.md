title: git undo
date: 2016-01-14 15:39:02
categories: linux
tags:
	- git
  - command line
  - version control
---

In spite of the title of this post, there is no `git undo` command, but I bet many among us wish there was one (given that it could read our minds)! I for one keep having to look stuff up every time I screw up and have to undo something. Was it `reset`? `revert`? `checkout --`??

Luckily, I found this [very handy article](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things) in a book about git the other day. If you did a `git oops` and need to recover, go read the article! But just for my own (and perhaps your) future reference, here are (some of) the different undo commands collected in one small post (*dangerous* commands may result in losing code if used incorrectly):

{% codeblock lang:bash line_number:false %}
# to unstage a file:
git reset HEAD <file>
# to undo changes and go back to the latest commit (dangerous!):
git checkout -- <file>
# to add a file you forgot to stage to the previous commit:
git add <file> && git commit --amend
# to go back x amount of commits (dangerous!):
git reset HEAD~<x>
# to go back x amount of commits and create a new commit:
git revert HEAD~<x>
{% endcodeblock %}

Do read the article I linked above before messing with your git tree and `checkout` [this one](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting/summary/) too for more information about the differences between `reset` and `revert`.
