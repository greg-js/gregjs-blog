title: "Checking whether a file/directory exists without using fs.exists"
date: 2016-03-17 18:06:52
categories: javascript
tags:
- js
- node
- fs
- async
---

Anyone who has spent any time writing Node has most likely used the [file system module](https://nodejs.org/api/fs.html). One of the methods in this core module though is not only different from all the others, but according to most experts is not even useful at all, despite its popularity with a host of other developers. Because of this, the method has been deprecated for years now. I talk, of course, about [fs.exists](https://nodejs.org/api/fs.html#fs_fs_exists_path_callback) (and [fs.existsSync](https://nodejs.org/api/fs.html#fs_fs_existssync_path)).

For some people, it's a very contentious issue - see for example [this issue thread](https://github.com/nodejs/node/issues/1592) or [this one](https://github.com/nodejs/node/issues/103). The reason for its deprecation (**way** back in the iojs days of yore) is that, unlike all other file system methods, it doesn't use the canonical nodeback API. Furthermore, its main use case scenario, checking whether a file exists before opening, is actually an antipattern and should be replaced by just trying to open the file and possibly handling the error.

{% asset_img nodejs-new-pantone-black.png node logo %}

But despite its long-standing (in JS terms at least) deprecation, tons of people still continue to use `fs.exists`, and some newcomers or relative newcomers are surely confused about what to use instead. At least, I myself was when I first ran into this issue.

While I agree that the method is often superfluous, and sometimes entirely pointless (checking whether a file exists before doing something with that file is not helpful when race conditions may occur and the file may be deleted in the interim), I and I am sure many others would argue that there **are** legitimate use cases for this method, or at least for what this method *should* have been, had it been designed better.

So if you are looking for a replacement for `fs.exists(Sync)`, read on, for what follows is a short discussion of how to check whether a file or directory exists without using this deprecated method.

<!-- more -->

## Synchronous exists

Far be it from me to recommend synchronously manipulating the file system, but hey, it *is* more straightforward so let's start with the easy stuff here.

The official `fs` documentation recommends using [fs.statSync](https://nodejs.org/api/fs.html#fs_fs_statsync_path) or [fs.accessSync](https://nodejs.org/api/fs.html#fs_fs_accesssync_path_mode) instead, so let's go over both.

### fs.accessSync

This method checks the accessibility of a file and nothing more or less. It throws an error if the file is not accessible and does absolutely nothing if it is.

So, let's say you want to check for the existence of `myDir`, representing the directory `/path/to/myDir`, before attempting to create it:

{% codeblock lang:javascript %}
try {
  fs.accessSync(myDir);
} catch (e) {
  fs.mkdirSync(myDir);
}
{% endcodeblock %}

Or, if you prefer to recreate `fs.existsSync` instead:

{% codeblock lang:javascript %}
function fsExistsSync(myDir) {
  try {
    fs.accessSync(myDir);
    return true;
  } catch (e) {
    return false;
  }
}

if (!fsExists(myDir)) {
  fs.mkdirSync(myDir);
}
{% endcodeblock %}

### fs.statSync

The code above has a problem: what if `myDir` exists, but is not a directory but a file which happens to have the same name? In that case, the path would be accessible, but it wouldn't be a folder, and you might end up screaming at your monitor during a nasty debugging session later on when other things start to fail because of that.

The solution is to use `fs.statSync` and the `isDirectory` or `isFile` method on the returned `stats` variable. Here's a synchronous function that checks for whether a given path exists **and** is a directory:

{% codeblock lang:javascript %}
function isDirSync(aPath) {
  try {
    return fs.statSync(aPath).isDirectory();
  } catch (e) {
    if (e.code === 'ENOENT') {
      return false;
    } else {
      throw e;
    }
  }
}

if (!isDirSync(myDir)) {
  fs.mkdirSync(myDir);
}
{% endcodeblock %}

`isDirSync` will return `true` if the given path exists and is a directory, and `false` in all other cases. Note that it also throws the error in case a different error is encountered. This is actually not necessary because as far as I know, there really are no other possible errors, but hey, you never know!

The `ENOENT` error code is what you get when `fs.statSync` throws a `no such file or directory` error. You need to check for this because unlike what you may expect, checking the stats of an non-existing path will **not** give you an empty `stats` object, but rather it will cause the function to fail and throw that error.

You might also sometimes see people check for the error *number* rather than the error *code*. You're probably better off not doing that because the numbers may be different based on the version of Node you're running and/or the operating system you're running it on.

## Asynchronous exists

In async world (AKA the real world for us Node people), your alternatives to `fs.exists` are `fs.access` or `fs.stat`. Here we go.

### fs.access

As per the nodeback API, the first (and in this case only) parameter of `fs.access` is an error object. Let's again check whether a directory already exists before making it:

{% codeblock lang:javascript %}
fs.access(myDir, function(err) {
  if (err && err.code === 'ENOENT') {
    fs.mkdir(myDir);
  }
});
{% endcodeblock %}

Note that I combined checking for the error and the code. An alternative to this particular solution would have been to try to make the dir with `fs.mkdir` first, and then check for an `EEXIST` error code which would have been thrown if the directory was already there.

### fs.stat

And of course we can also check for a path's stats and decide what to do based on the results. For example, let's make an asynchronous `checkFile` function, which returns true only if the given path exists and is a file, and false in all other cases:

{% codeblock lang:javascript %}
function checkIfFile(file, cb) {
  fs.stat(file, function fsStat(err, stats) {
    if (err) {
      if (err.code === 'ENOENT') {
        return cb(null, false);
      } else {
        return cb(err);
      }
    }
    return cb(null, stats.isFile());
  });
}

checkIfFile(aPath, function(err, isFile) {
  if (isFile) {
    // handle the file
  }
});
{% endcodeblock %}

Or, using promises:

{% codeblock lang:javascript %}
function checkIfFile(file) {
  return new Promise(function(resolve, reject) {
    return fsStat(file).then(function(stats) {
      resolve(stats.isFile());
    }).catch(function(err) {
      if (err.code === 'ENOENT') {
        resolve(false);
      } else {
        reject(err);
      }
    });
  });
}

checkIfFile(aPath).then(function(isFile) {
  if (isFile) {
    // handle the file
  }
}).catch(function(e) { throw e; });
{% endcodeblock %}

The point of all this is not to get hung up on the details of making a perfect replacement for `fs.exists`, but rather to show that you can accomplish the goals you would have used `exists` for with better, non-deprecated methods.

## But I really want fs.exists!!!

If you want it, you can keep using it! Just write your own replacement or require/import one of the many user-created packages that have it. [Here's a good one](https://github.com/sindresorhus/path-exists), but feel free to take your pick!
