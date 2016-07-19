title: How to get a shiny coverage badge for your GitHub project
date: 2016-07-19T14:14:59.020Z
category: javascript
tags:
- node
- open source
- testing
---

I know the internet's great at all, but you know what's not great? An internet connection that doesn't work and keeps kicking you off every five seconds! That's basically what my life has been like the past week and a half, so you can imagine how frustrated I've been.

[![Coverage Status](https://coveralls.io/repos/github/greg-js/wdn/badge.svg?branch=master)](https://coveralls.io/github/greg-js/wdn?branch=master)

Time to let off some steam then by writing up this simple tutorial on how to give your (Node) project a nice coverage badge.

<!-- more -->

## What is coverage?

Code coverage is a measure of how well you are testing your project. If a project has 99% coverage, that means that 99% of the lines, functions and/or branches of the files included in your tests are *covered* by tests. A line, function or branch is covered when said line, function or branch is executed in one or more of your tests.

Now what does that tell us? First of all, it tells you something about how well maintained and stable the project is, and to what extent the maintainer(s) is testing the project.

Note I said in the above paragraph that it tells you *something*. It doesn't tell you **everything**. Coverage only takes into account the files actually included in the tests. So even though a project has 100% coverage, it might still be badly tested if it doesn't include some key files at all! It also doesn't tell you anything about the quality of the tests. All it measures is the proportion of executed code of included files.

Bearing in mind those caveats, code coverage is still a very useful measure, and while [you shouldn't necessarily aim for 100% coverage](https://www.thoughtworks.com/insights/blog/are-test-coverage-metrics-overrated), getting it to a decent level and displaying it proudly in your project's `readme` is something a lot of devs like to do.

## What to use?

For Node.js projects, you have some options, but in my experience the most straightforward setup uses [mocha](https://github.com/mochajs/mocha), [istanbul](https://github.com/gotwarlost/istanbul), [travis-ci](https://github.com/travis-ci/travis-ci) and [coveralls](https://github.com/nickmerwin/node-coveralls). Yes, I know that looks like a lot of work, but it isn't really.

```
npm install --save-dev mocha chai istanbul coveralls
```

(travis doesn't have to be installed and `chai` is an assertion library for `mocha`)

## Step 1: Write some tests

With `mocha` installed as your test suite and `chai` as your assertion library, you can now start putting test files in `./test`. Here's a quick example of a simple unit test (ES5-style):

{% codeblock lang:javascript %}
var expect = require('chai').expect;
var myFile = require('../lib/my-file'); // the file you're testing

describe('myFile', function() {
  it('should be a function', function() {
    expect(myFile).to.be.a('function');
  });
});
{% endcodeblock %}

Go and read more [about testing](http://chaijs.com/) if you're not sure how this works, but basically, it just checks if whatever is exported in `my-file` is a function. If it is, the test passes, if it isn't, it fails.

## Step 2: Set up a test script

Now, you *could* install `mocha` globally and type `mocha` in your project's root folder to execute the tests, but that would be crazy. Instead, you should install it locally (as shown above) and add the command to the `scripts` object in your `package.json`:

{% codeblock lang:json package.json %}
// ...
"scripts": {
  "test": "node node_modules/.bin/mocha"
}
// ...
{% endcodeblock %}

In more recent versions of `npm`, it's actually not necessary to refer to the path, as it will automatically set up the commands in `node_modules/.bin`. However, it can't hurt, so I personally prefer to just refer to it directly. You also don't need the `node` in there, but it also can't hurt and will make absolutely sure the script is executed with node.

With this setup, you can run `npm test` somewhere in your project dir and `mocha` will execute all the tests in the `./test` folder.

## Step 3: Set up a code coverage script

The good news is you're now testing your code. The bad news is you're not generating any coverage reports yet. Again, there are a couple of ways you can go about this. Here's how I like to set it up:

{% codeblock lang:json package.json %}
"scripts": {
  // ...
  "coverage": "node node_modules/.bin/istanbul cover _mocha -- -R spec"
  // ...
}
{% endcodeblock %}

This sets up a `npm run coverage` script for you. It will cause `istanbul` to run *all* your tests set up with `mocha` and generate a coverage report in `./coverage`.

Now is probably a good time to add some things to your `.gitignore`, as you don't want to be committing all this stuff:

{% codeblock .gitignore %}
coverage
.coveralls.yml
{% endcodeblock %}

You probably don't have a `.coveralls.yml` file yet, but we'll make one very soon.

## Step 4: Set up travis-ci

[Travis CI](https://docs.travis-ci.com/user/for-beginners) is a continuous integration tool. This post is already getting long so I won't discuss it any further here. I'll keep it simple and just tell you how to get it running. First you should sign in at [travis-ci.org](https://travis-ci.org) (it's free and you can just use your GitHub account).

Once logged in, you should see a list of all your GitHub repos. Switch on the repo(s) you want to track and create a `.travis.yml` file in your project's root directory. Assuming you want to test your project on Node v6, v5, v4 and v0.12, here's what you should put in it:

{% codeblock lang:yaml .travis.yml %}
language: node_js
node_js:
  - "6"
  - "5"
  - "4"
  - "0.12"
script: npm run coverage
after_success: 'npm run coveralls'
{% endcodeblock %}

You can commit this now, but it won't work just yet because you haven't yet set up `coveralls`.

## Step 5: Set up a coveralls script

[coveralls](https://coveralls.io/) is what will generate that cool badge you're working towards here. What it does is it takes the `istanbul` output and feeds it into a fancy web application. You'll have to [go to the site](https://coveralls.io), sign in with your GitHub account and enable the repo you want to track.

When you've enabled the repo, go to the detailed view and look for your token. Copy it and add it to a `./.coveralls.yml` file like this (don't forget to add it to your `.gitignore`):

{% codeblock lang:yaml .coveralls.yml %}
repo_token: PASTE_YOUR_TOKEN_HERE
{% endcodeblock %}

This is just so you can run the script and send coverage reports locally, but you'll probably use the automated way more often.

Now add one more script to your `package.json` to tie it all together:

{% codeblock lang:json package.json %}
"scripts": {
  // ...
  "coveralls": "cat ./coverage/lcov.info | node node_modules/.bin/coveralls"
  // ...
}
{% endcodeblock %}

## Step 6: Add the badges to your readme, commit and push

You can get the markdown for the travis build and coveralls status badge from the repo detailed view of respectively travis-ci and coveralls.io. Simply add it to your `Readme`, `commit` your changes and `push` to GitHub.

And we're done. From now on, every time you push changes to your GitHub project, `Travis CI` will build the project on the different node versions. If successful, it will run the `coverage` script, which will have `Istanbul` generate a coverage report. Finally, it will run the `coveralls` script, which takes the coverage report and submits it to `coveralls`.

The result is your badges will update automatically and you're now free to chase that mythical 100% coverage in a desperate effort to impress your peers!
