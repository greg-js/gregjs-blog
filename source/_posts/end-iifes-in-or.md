title: 'End IIFEs in })(); or }()); ?'
date: 2015-12-09 18:16:32
categories: javascript
tags:
- js
- style
- crockford
---

Before I deleted my previous blog, I took a look at my pageview statistics and it turned out that the post that got by far the most views was one about stylistic differences in immediately invoked function expressions (IIFEs). That means that quite a few people are still a little confused about this whole thing, so allow me to clarify it once more.

An IIFE is an extremely common JavaScript design pattern. It's how you create modules, define private variables and methods and safeguard the global namespace from pollution. Here's what an IIFE looks like:

{% codeblock lang:javascript %}
(function IIFE() {
  // ...
}());
{% endcodeblock %}

If you've done any work involving JavaScript relatively recently, you've most likely seen these all over the place. The subject of this post though is the fact that an IIFE can also look like this (note the last line):

{% codeblock lang:javascript %}
(function IIFE() {
  // ...
})();
{% endcodeblock %}

<!-- more -->

An IIFE is literally an _immediately invoked function expression_. Or, in other words, it's basically short for this:

{% codeblock lang:javascript %}
function IIFE() {
  // ...
}
IIFE();
{% endcodeblock %}

Unfortunately, the JavaScript engine doesn't allow immediate execution of a function expression:

{% codeblock lang:javascript %}
function IIFE() {
  // ...
}();
// --> SyntaxError
{% endcodeblock %}

Hence, it gets wrapped in parentheses. It doesn't matter whether you wrap the function expression and then invoke it, or wrap the function expression along with its evocation. The result is exactly the same:

{% codeblock lang:javascript %}
(function IIFE() {
  // ...
}());

// is equivalent to

(function IIFE() {
  // ...
})();
{% endcodeblock %}

So, which one should you use? Being (almost) functionally equivalent (for one potential difference which will most likely not affect you, read the third post [in this github thread](https://github.com/airbnb/javascript/issues/21)), the choice is a stylistic one. Here are a few arguments for one or the other:

- The AirBnB style guide [recommends outside](https://github.com/airbnb/javascript#7.2).
- ESLint prescribes the same [by default](http://eslint.org/docs/rules/wrap-iife.html).
- JSLint [recommends "inside" invocation](https://jslinterrors.com/do-not-wrap-function-literals-in-parens), reasoning that doing otherwise produces potentially confusing code.
- In typically crockfordian fashion, JSLint's Douglas Crockford denigrates outside invocation [using colorful language](https://www.youtube.com/watch?v=eGArABpLy0k).

It _seems_ that the majority of developers (and most large project, among which Angular and jQuery) prefer outside invocation nowadays, but a lot of people still use the alternative.

So, again, which one should you use? Whichever you prefer. There are some good arguments on both sides, but in the end one is not objectively better than the other, so take your pick! Me, I put them outside. Dog balls be damned!
