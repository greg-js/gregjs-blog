title: How to scrape the web gently with Node.js
date: 2015-12-03 03:37:34
categories: javascript
tags:
- node
- js
- scraping
---

I was scraping a few thousand links from a single domain just a little too aggressively yesterday and kept getting `ECONNRESET` and `socket hang up` errors instead of that sweet, sweet data I longed for. Like usual, it took quite a bit of searching, testing, googling and experimenting until I finally identified the correct solution to the problem.

If you (yes, _you_) are getting `request error: read ECONNRESET` and/or `request error: socket hang up` errors in Node when scraping websites for data, chances are it's because you're essentially bombarding the server with thousands of requests. The server at some point stops responding to your suspicious behavior and all those requests you launched start coming back empty-handed.

The correct way to address this issue depends on your code of course. In my case for example, it was no use limiting my promise concurrency for the promise array I was mapping over, but for you that might be the easier solution. I'll quickly go over two alternative solutions which may work for you before saying a few words about the one that did for me.

<!-- more -->

## Limiting maximum concurrent sockets in Node

Since version 0.12, Node.js puts no limit whatsoever on the default maximum amount of concurrent sockets allowed for the global http agent. In some cases this is great, in others - like when you're scraping the web - it can cause problems.

No matter which library you're using to make your requests (I use [request-promise](https://github.com/request/request-promise)), as long as it's using the `http` module under the hood (which is virtually always the case), you can change `maxSockets` setting by calling it early in your script - ie before you make the actual requests or even load your request library.

So here's the code, put this in your init or config script, or right near the top of your file:

{% codeblock lang:javascript %}
var http = require('http');
var https = require('https');

http.globalAgent.maxSockets = 1;
https.globalAgent.maxSockets = 1;
{% endcodeblock %}

As you can see, I set a very low hard limit to the amount of sockets for both the `http` and the `https` globalAgent. This is because changing the `http` settings won't make any difference if all you're making is `https` requests! I should have been sleeping, but instead I spent about an hour and a half debugging and despairing at 3 AM in the morning by overlooking this simple and somewhat obvious fact, so make sure to double-check this.

Note that you can probably get away with a few more sockets, but hey, I was fine with it taking a bit longer, as long as I got to the data. And I like the site I was scraping, so I wanted to be as nice as possible!

Finally, you can change the settings on a specific agent for some more fine-grained control, but if you do that, you'll have to manually make your requests using the `http` module. So if you want to use `request`, `superagent`, `fetch` or whatever, you'll only be able to manipulate the maxSockets setting of the globalAgent, unless maybe the library in question exposes this setting to you.

## Other solutions

Quickly, here are a few alternative solutions to the problem of requests going out way too fast:

### Batching & promise concurrency

The most straightforward solution for you might actually be to change your code so you're not firing off all your requests at the same time. To make sure that doesn't happen, you could make a queue or split up your big array full of requests into ten small ones and process them in turn.

It's hard to write out specific code snippets for this, as this obviously depends on a multitude of factors. However, one thing I'd like to mention in a bit more detail is promise concurrency.

Promise concurrency is the idea that when you're processing an array full of promises, you can only have a set number of them resolving at the same time. For example, if you have an array of 150 promises which each resolve with data from a server and the concurrency is set to 2, the third promise will only begin to resolve when the first comes back with the data. Of course whether this is helpful for you or not depends on how you've set up your promise array.

I use `Bluebird` so here's how you set a concurrency limit when using the `.map` or the `.filter` method (at the time of writing the only ones that have implemented the limit), using two different styles:

{% codeblock lang:javascript %}
Promise.map(promiseArray, mapFunction, { concurrency: 2 });
promiseArray.filter(filterFunction, { concurrency: 2 });
{% endcodeblock %}

I don't use other promise libraries, but I assume they probably have a somewhat similar feature.

### async

Wherever there's a problem in Node involving timing or asynchronicity, there is the [async module](https://github.com/caolan/async) with a solution. Now, `async` uses callbacks and not promises so again this may not be what you're looking for.

Then again, it may be _exactly_ what you are looking for. Here are two methods straight from the docs you will want to look at if you want to solve your concurrency problem using `async`:

- [async.queue](https://github.com/caolan/async#queue)
- [async.parallelLimit](https://github.com/caolan/async#parallel)
