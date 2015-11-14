title: Simple promise example
categories: javascript
tags:
  - js
  - node
  - learning
  - async
date: 2015-11-10 14:41:05
---


I generally know what promises are about and have found them to be very convenient on multiple occasions (at least, more so than traditional callbacks). However, for me and undoubtedly for many others, the true nature of the promise still kind of eludes me.

I'm pretty busy nowadays working my way through [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS), building a landing page for this site and hacking on a bunch of other small projects. But I really wanted to get a more theoretical understanding of them now rather than later, rather than just slightly modifying existing code and hoping I understood it somewhat correctly.

So I did what probably every budding and experienced developer does in this kind of situation: I read the [MDN article on promises](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise).

<!-- more -->

Kyle Simpson (the author of You Don't Know JS) will undoubtedly give a fantastic and in-depth review of promises in the (for now) [final volume in his series on Async & Performance](https://github.com/getify/You-Dont-Know-JS/tree/master/async%20%26%20performance). I've still got two books to go before I get there though, but the MDN article enlightened me enough to get me by until then.

I highly recommend giving either the above-linked book (buy it from O'Reilly to support the author or read it for free on github) or the [MDN article on promises](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) a read if you feel in any way fuzzy on what promises are or why you should probably use them.

But just in case you don't believe me, here's a quick and dirty example to wet your appetite:

{% codeblock lang:javascript %}
var getAsyncList = new Promise(function(resolve, reject) {
  setTimeout(function() {
    list = ['a', 'b', 'c'];
    return resolve(list);
  }, 1000)
});

getAsyncList.then(function(list) {
  // the list parameter is the resolved list from above
  list.push('d');
  console.log(list);  // -> ['a', 'b', 'c', 'd']
}).catch(function(reason) {
  // no errors here, but error handling is a breeze with the reject argument on line 1
  console.log('Promise rejected (' + reason + ')');
});
{% endcodeblock %}

Goodbye callback hell! Keep on JS'ing, friends :-)
