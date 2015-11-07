title: 'How to use shell variables in awk'
date: 2015-11-07 18:15:53
categories: linux
tags:
- awk
- command line
- shell
---

If you ever find yourself in a life-or-death kind of thing where you're forced to use shell variable in your awk scripts, then fear not!

First, here's what **doesn't** work (warning: contrived example):

{% codeblock lang:bash %}
statement=$(echo "something is not cool" | sed -e 's/cool/awesome/')
# $statement is now "something is not awesome"

first=$(echo "$statement" | awk '{ print $1 }')
# $first is now "something"

# now let's pipe $statement into awk to print the first, second and
# fourth column, but only if the first column matches $first!
truth=$(echo $statement | awk '$1 ~ $first { print $1, $2, $4 }')

echo $truth
# nothing happens
{% endcodeblock %}

The problem is that awk doesn't know about `$first_word`.

The solution (or one of the possible solutions) is to teach it in the BEGIN block. Here's the right code:

<!-- more -->

{% codeblock lang:bash %}
statement=$(echo "something is not cool" | sed -e 's/cool/awesome/')
# $statement is now "something is not awesome"

first=$(echo "$statement" | awk '{ print $1 }')
# $first is now "something"

# now let's pipe $statement into awk to print the first, second and
# fourth column, but only if the first column is equal to $first!
truth=$(echo $statement | awk 'BEGIN { first=$first } $1 ~ first { print $1, $2, $4 }')

echo $truth
# -> "something is awesome"! but wait, let's fix that..

echo $(echo $truth | sed -e 's/something/everything/')
# done and done
{% endcodeblock %}

In the begin block, set `first` (no `$` necessary in awk) to the shell variable `$first`. Now you can use it.

Happy `awk`ing!
