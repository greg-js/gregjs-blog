title: Asynchronous tests in Mocha using before and after blocks
date: 2015-11-27 14:19:27
categories: javascript
tags:
- js
- testing
- async
- node
---

While writing [my hexo plugin](http://gregjs.com/javascript/2015/hexo-easy-edit-1-2-0-pages-cleanup-rename/), I resolved to switch my coding style for my next project from _whatever works_ to _BDD/TDD_. I'd wasted a bit too much time pushing buggy code and fixing it after the fact. Unit testing was the obvious solution.

Honestly, I still don't understand what exactly the difference is between Test- and Behavior-Driven Development, other than the usage of different syntax (TDD uses `assert` and BDD uses `expect` or `should`, or at least, this _seems_ to be the case). I'll look into it more later, but for now I just wanted to get test on.

Writing tests with Mocha and Chai before writing any code for my new Node project was a bit of a culture-shock, but it was relatively easy and painless. That is, until I ran into asynchronicity issues. Suddenly, my tests started timing out and it took quite some time (despite great documentation) to finally get them right.

{% asset_img screen.png 'Mocha tests screenshot' %}

I suppose others out there could benefit from reading how I structured my async test cases. I'm obviously not saying I'm an expert, since I only just started to seriously use Mocha myself. But this works, and if it so happens that you you are pulling your hair out in despair from being met by failing async unit tests and by some stroke of chance (or through some adept googling) you arrive on this website, this just might be your ticket to sanity.

<!-- more -->

Anyway, what I've got here are some test cases for a `save` function I wrote. The function takes an `article` object as its first parameter (which has an `md` and a `title` property), and an optional second parameter for the path. If the second parameter is not supplied, the article's markdown content (`article.md`) will get saved in the `./content/_content` directory, using `article.title + '.md'` for its title.

This well-commented snippet has a few unit tests to test this function and to help you along with your own. Of course I have more to test the optional parameter etc, but I wanted to keep this short. Take a look, incorporate the ideas in your own projects or comment if you see me making glaring mistakes. Good luck and keep on testing :-)

{% codeblock lang:javascript test.js %}
// I'm using BDD-style expect
var expect = require('chai').expect;

// Pulling in the function I'm testing
var save = require('../lib/save');

// Promisify all the things.. It's just easier to work with .then functions, compared to callbacks
var Promise = require('bluebird');
var fsUnlink = Promise.promisify(require('fs').unlink);
var fsStat = Promise.promisify(require('fs').stat);
var fsReadFile = Promise.promisify(require('fs').readFile);

var path = require('path');

describe('save.js', function() {
  describe('saving files to default directory', function() {

    // Setting up what should be the correct default save path
    var dest = path.join(process.cwd(), 'content', '_content', 'test title.md');

    // Making a fake article to test the function with
    var article = { md: 'some content', title: 'test title' };

    // Everything inside this before block will execute before the two tests in this describe block
    // Note the done parameter. This signifies asynchronous control flow
    // I resolve the promise to save my fake article, then call the done callback
    // Mocha will wait for this done callback before moving on to the next block
    before(function(done) {
      return Promise.resolve(save(article)).then(function() {
        done();
      });
    });

    // Now the fake article has been saved and the tests begin to run
    // I'm using asynchronous fs-functions here, so again I need to use done()
    // Here I'm just making sure that the default path for this fake article exists and is a file
    it('saves files in the right directory', function(done) {
      fsStat(dest).then(function(stats) {
        expect(stats.isFile()).to.be.true;
        done();
      });
    });

    // Same as above, this test will only resolve after the done callback has been called
    // Here I'm checking whether the content of the mock destination file equals what I set it up with
    it('saves the content correctly', function(done) {
      fsReadFile(dest, 'utf8').then(function(content) {
        expect(content).to.equal('some content');
        done();
      });
    });

    // Whatever is in the after block will run after the tests have completed
    // Since I made and saved a fake article, I will now have to delete it again
    // Yet again, the code is asynchronous (though you could use the synchronous fs versions if you prefer..)
    after(function(done) {
      fsUnlink(dest).then(function() {
        done();
      });
    });
  }
});
{% endcodeblock %}




