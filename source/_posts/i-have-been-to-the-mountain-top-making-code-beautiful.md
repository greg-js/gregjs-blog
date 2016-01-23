title: "I have been to the mountain top: making code beautiful"
date: 2016-01-22 18:31:42
categories: javascript
tags:
  - async
  - promises
  - js
  - style
---

I decided to refactor my hexo plugin today and one thing that struck me was how useful promises could be to make my code look beautiful and read nicely.

Here's an excerpt to show you what I mean:

{% codeblock lang:javascript %}
loadArticles(query, this.locals).then(function filter(articles) {
  return filterArticles(articles, filters);
}).then(function select(filtered) {
  return selectArticle(filtered);
}).then(function openFn(selected) {
  openFile(selected);
}).catch(function catchAll(err) {
  console.log(chalk.red('Error: '), err);
});
{% endcodeblock %}

<!-- more -->

Before the refactor a lot of that functionality wasn't inside `then` methods and frankly, it read like a bunch of spaghetti code. The truth is that using promises doesn't guarantee readable code. Yes, you might avoid callback hell with them, but when you put all your functionality inside a single `then`, or add multiple layers of indented promises (*promise hell*), what's the point, really?

However, if you make a nice chain by returning more promises, give your [anonymous functions descriptive names](https://github.com/getify/You-Dont-Know-JS/blob/62eb611448fef5cb83e3ba6ac1b95bd2947a456c/scope%20%26%20closures/ch3.md#anonymous-vs-named) (although I admit I wasn't very creative in the example above) and "collect" potential errors for `catch`ing them when you're ready, you can end up with some very nice-looking code.

Anyway, sorry if this all sounds super basic and obvious. It's just that I *thought* I already understood what promises were all about. Looking at my old code though, it's clear that I didn't. There's a big difference between reading about promises and implementing some parts here and there on the one hand, and experiencing the feeling of writing some truly beautiful promisified code on the other.

So if you're still fuzzy on promises, or if you've tried them out before, but wondered aloud why people say they're better than regular old callbacks, then I urge you to spend some time refactoring old code.

But when you do refactor that code, don't just think of promises as a way out of callback hell. Don't even think of them as just a tool for asynchronous control flow. And *definitely* [don't think of them as an unnecessary intermediate step](https://medium.com/@bluepnume/learn-about-promises-before-you-start-using-async-await-eb148164a9c8) on the way to [async/await](http://tc39.github.io/ecmascript-asyncawait/)!

No! Instead, think of promises as those first two things, but also as a way to **make your code beautiful** -- and you shall see **the promised land**!
