title: "Friends don't let friends `sudo npm install -g` packages"
date: 2016-09-26T11:56:24.353Z
category: javascript
tags:
- node
- npm
---

A friend of mine was installing his first npm package the other day (in case you're wondering, the package in question was [vtop](https://github.com/MrRio/vtop)). But while everything went fine and my buddy is enjoying his node-powered monitoring now, I was a little disheartened to learn that, as per the `readme`, he had `sudo npm install -g`'d this global package.

{% asset_img sudo.jpg super user %}

Call me crazy, but I just don't like the idea of needing `sudo` to install global `npm` packages. The good news though is that you don't really need it. Let me show you how.

<!-- more -->

## Why?

If you're on Mac OSX or a Linux distribution, a fresh Node.js install will most likely put your Node stuff somewhere in `/usr/local` and your global installs in `/usr/local/lib/node_modules`.

Since those directories have root permissions and ownership, any global package install will require `sudo`.

Now, this isn't inherently bad. `npm` itself won't automatically run build scripts as root and global `npm` packages won't just get root-access to your system directories. But hey, it's just not very nice I think. And judging from the amount of times questions about this pop up on StackOverflow and Reddit, a lot of people feel the same way.

## `nvm`

The easiest solution comes in the form of [nvm](https://github.com/creationix/nvm), the Node Version Manager. This Bash script will manage multiple `node` and `npm` versions for you and as an added bonus will put your globally installed packages in your home directory, removing the need for `sudo`.

Installation is simple:

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash

# OR

wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
```

And then install the latest node:

```
nvm install node
```

Read the `readme` or do an `nvm -h` to see what else you can do. `nvm` is an invaluable tool, especially if you're a dev.

However, it's probably not for everyone. If you know you'll only ever need one version of `node` at the same time, using `nvm` is kind of overkill and might slow down the initial start up time of your terminal by a few hundred milliseconds. Luckily you have other options.

## Change the prefix

If `nvm` is too much for you, maybe [npm-g_nosudo](https://github.com/glenpike/npm-g_nosudo) is more up your alley. This bash-script will change the prefix, responsible for setting the globally installed packages dir, and fix your `PATH`, so your global packages will go straight somewhere in your home dir (`~/.npm-packages` for example).

The script is specific for Ubuntu, but I don't see why it wouldn't work on other distros or OSX. You could also very easily just change the prefix and `PATH` yourself, using this excellent [quick guide on GitHub](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md).

## Change ownership and permissions

The third option is to change the ownership and permissions of the default global package location so you don't need `sudo`. This is by far the most invasive and dangerous solution to this problem though.

I very much discourage you from doing this and urge you to either let `nvm` manage `node` and `nvm` for you, or to change the prefix, either manually or with the `npm-g_nosudo` script.

If you really, really want to go this more dangerous route, it'd be as simple as entering a few `chown` and `chmod` commands. But seriously, don't do it. Friends don't let friends `sudo npm install -g packages` but they sure as hell don't let them appropriate system dirs to a particular user!
