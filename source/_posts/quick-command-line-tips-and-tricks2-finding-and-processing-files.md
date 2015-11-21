title: 'Quick command-line tips and tricks 2: Finding and processing files'
categories: linux
tags:
  - command line
  - tips
date: 2015-11-21 16:07:47
---


Here are some more quick tips and tricks to improve your command-line user experience and improve your productivity. [Part one here](linux/2015/a-few-quick-command-line-tips-and-tricks/).

{% asset_img commandline.png "bash prompt" %}

<!-- more -->

### locate

The fastest way to find files on your filesystem is by way of the `locate` command:

{% codeblock lang:bash line_number:false %}
locate MYFILE
{% endcodeblock %}

`locate` is fast because it reads a periodically updated database instead of querying the file system. The downside is that newer files may not have been indexed yet and may therefore fail to appear in your search results. You can however force an update with the `updatedb` command.

### find

`locate` is great for locating a specific file, but it's not a very versatile tool. If you want to find files and then do things with them, you'd probably prefer to use `find` instead. Here's an example of a typical find command:

{% codeblock lang:bash line_number:false %}
find ./ -iname "*FILENAME_PART*"
{% endcodeblock %}

This will do a recursive search in your current directory (`./`), looking for a case-insensitive (`-iname`) file that has the string `filename_part` appear somewhere in the title (note the shell expanding syntax using the asterisks).

### -exec

`find` is an extremely useful tool and explaining all that it can do here would fill many posts. Here are just a few more examples of its versatility.

To find all log files in the current directory and copy them to a folder:

{% codeblock lang:bash line_number:false %}
find ./ -iname "*.log" -exec cp '{}' ~/logs \;
{% endcodeblock %}

For every log file found, the `cp` command will run, with `'{}'` replaced by the path to the file.

To do a grep search within each file in the current directory, looking for a particular string:

{% codeblock lang:bash line_number:false %}
find ./ -type f -exec grep -H "special_string" '{}' \;
{% endcodeblock %}

`-type f` restricts the search results to files only and `grep -H` searches inside the files instead of just in their filenames. Obviously this command can take a while to complete if you have a lot of large files in your directory.

### -print0

One thing that often tends to trip up shell magic is whitespace in filenames. The above commands should usually work fine, but if you're having issues, there's a quick way to do certain things without the risk of whitespace ruining your day. Here's an example:

{% codeblock lang:bash line_number:false %}
find /downloads -type f -size +1G -print0 | xargs -0 ls -l
{% endcodeblock %}

First of all, check out the `-size +1G` option, which will filter out all files under one gigabyte. There are a lot more filters like this (see `man find` for more).

Next, the `print0` option separates the different results by a zero byte NUL character and the `-0` option for `xargs` will then process inputs separated by that same character. This way whitespace in your filenames in all its forms won't disturb your output. The result of the command is that an `ls -l` will be done for every found file.

### for-loop

One more way to process files is with a quick for loop:

{% codeblock lang:bash line_number:false %}
for i in *.txt; do cp "$i" "${i/.txt/.txt.bak}"; done
{% endcodeblock %}

Note how variables are wrapped with double quotes to prevent whitespace from spoiling the party. This gets very tedious after a while, which is while the NUL character mentioned above is great for quick and easy file processing.

This command will look in the current directory (non-recursively), find all files ending in `.txt` and make a copy of it as `*.txt.bak`. Of course you could use a command substitution instead of a simple glob expansion, or you could do much more complicated things.

But I guess this is probably enough for another quick update. As I was writing this, I kept coming to the realization that there is just so much more to talk about. I want to keep these posts as easily digestible as possible though, so I'll just stop abruptly here. Stay tuned for more and I'll catch you on the flip side, you command-line ninja, you!
