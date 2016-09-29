title: Upgrade your terminal with bash functions for matching text in filenames or contents
date: 2016-09-29T12:10:27.593Z
category: linux
tags:
- bash
- command line
- shell
- tips
---

Everyone knows about aliases, but what about bash functions? In this article, I'll explain how to make and source your own bash functions and show you how they can increase the versatility of your terminal.

The problem we'll solve here is a threefold one and concerns finding files in a certain directory, and doing things with said files. In particular, we would like to:

1. get a list of files, the filenames of which match a given text pattern
2. get a list of files, the contents of which match a given text pattern
3. do things with the above lists of files

Now, if you're `bash` (or `zsh` or `fish` or whatever) savvy, you might know how to do these things on the command line already using the tools that come with any `bash` or `bash`-compatible shell environment:

{% codeblock lang:bash line_number:false %}
# 1. match "potato" (case insensitive) in filenames within the current dir
find ./ -iname "*potato*" -print
# 2. match "potato" (case insensitive regex) in file contents within the current dir
grep -Ri ./ -e "potato"
# 3. match and run `ls -l` on results
find ./ -iname "*potato*" -exec ls -l '{}' +
find ./ -name "*potato*" -print0 | xargs -0 ls -l # a very common variation of the above, works just in case your `find` doesn't have the `-exec` option
grep -Ri "$1" -e "potato" -lZ | xargs -0 ls -l
{% endcodeblock %}

<!-- more -->

Note that these are not the only possible solutions. Variations are most definitely possible and easily found if you do some searching.

Personally, I use these commands (or similar ones) all the time, but there's a big problem: I keep forgetting the exact syntax and often get one of the options wrong, or I forget to add the null operator, or whether the search pattern is globbed or a regex. I'm sure you know how it goes.

All too often this ends with me having to waste a few seconds a-googlin`, which is fine, really. Or is it? What if I told you you could easily turn these three useful but somewhat bulky commands into bash functions? Said functions would enable you to do this:

1. `match_filename ./ potato`
2. `match_contents ./ potato`
3. `matchrun_filename ./ potato ls -l`
4. `matchrun_contents ./ potato ls -l`

As you can see, the result is something similar to an alias or a script. Like an alias, you declare your bash functions by sourcing them in your `.bashrc` or whatever startup script you prefer (`.zshrc` in my case). But like a script, you have a lot more control over what exactly your function does and have access to the parameters you pass it.

Here is the code to make the four functions noted above work in your terminal. Put them in a startup script and source it:

{% codeblock lang:bash .bashrc %}
function match_filename() {
  find "$1" -iname "*$2*" -print
}

function match_contents() {
  grep -Ri "$1" -e "$2"
}

function matchrun_filename() {
  find "$1" -iname "*$2*" -print0 | xargs -0 ls -l
}

function matchrun_contents() {
  grep -Ri "$1" -e "$2" -lZ | xargs -0 "${@:3}"
}
{% endcodeblock %}

This post isn't really about how this code works (check out the man pages for `find`, `grep` and `xargs` or blog posts about these subjects for that), but rather about writing the functions themselves. The main thing is that you can refer to positional arguments in your bash functions:

* `$1`, `$2`, `$3` refer to the first, second and third arguments
* `$#` is the number of passed arguments and `$@` all parameters
* `${@:3}` refers to all parameters, beginning from the third
* wrap parameters into double quotes to deal with spaces in filenames -- using null-separated lists (as with `print0` and `-0`) helps too

There you go, as you can see it's very easy to extend your shell environment with handy bash functions to achieve the exact kind of functionality you need or want.

Next time you need something more powerful than an alias, but not as involved as a script, write yourself a little bash function. I'll share some more of mine later, but have you made any nice ones? Feel free to share in the comments, I'd love to steal them!
