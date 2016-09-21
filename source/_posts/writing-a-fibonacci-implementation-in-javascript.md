title: "Writing a Fibonacci implementation in JavaScript"
date: 2016-03-14 16:14:05
categories: javascript
tags:
- js
- tips
- learning
- interview
---

I was just looking through some of the notes I'd taken a few months ago, and became inspired to write a quick post on how to write a *good* JavaScript solution for finding the `n`th number in a Fibonacci sequence.

I'm sure most of this information came right from some other blog or book, so credit really goes to wherever I originally learned these insights from, but sadly I did not include a source in my notes. Just google around if you want to know more however, because it's all over the place really.

Just in case you've never heard of the [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number), it's a mathematical sequence where every number is the sum of the previous two numbers in the sequence:

{% codeblock lang:plain line_number:false %}
1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...
{% endcodeblock %}

Now, I'm not going to go over what exactly the sequence is or why it's important, not only in computer science but in all of the natural world (but trust that it is). All you need to know is that "find the nth Fibonacci number" is a very common - or so I'm told - interview question for programmers of all persuasions, including the JavaScript kind.

The thing with implementing this in JavaScript though, is that it is a little more involved than one might expect..

<!-- more -->

## The problem

Write a function that returns the `n`th Fibonacci number, (assuming that the first two numbers are 1).

## A naive solution

A major reason why this question appears so often in coding interviews is because answering it correctly demonstrates an understanding of a fundamental concept in programming: recursion.

In very simple terms, a recursive function is a function that calls itself, and a Fibonacci number is always the sum of the two previous Fibonacci numbers, each of which are the sums of their two previous numbers. As you can see, recursion and Fibonacci are made for each other, so here is a canonical, but naive JavaScript implementation for finding the `n`th number in the sequence:

{% codeblock lang:javascript %}
function fibNaive(n) {
  if (n <= 1) {
    return n;
  } else {
    return fibNaive(n - 2) + fibNaive(n - 1);
  }
}
{% endcodeblock %}

Every recursive function has a **base case** (in this case, the contents of the `if (n <= 1)` block) and a **recursive step** (the contents of the `else` block). Basically, the recursive step is performed again and again, until the base case is reached. For example, if `n` is 3, then this function will return `fibNaive(2) + fibNaive(1)`, which resolves to `fibNaive(0) + fibNaive(1) + fibNaive(1)`, or `0 + 1 + 1`, which is 2. Scroll up and verify that this is indeed the correct answer.

So if this solution works, then why is it naive? Well, try finding the 40th number using this function. Takes a little while, doesn't it? Don't even think about running it on high numbers because it is terribly, terribly, terribly inefficient.

## An iterative solution

I'll get back to explaining why the previous implementation was lacking in a minute, but first, let's come up with a better solution by eschewing recursion and embracing iteration (note: this uses some ES6, see below for how to make it work with ES5):

{% codeblock lang:javascript %}
function fibIterative(n) {
  var a = 1;
  var b = 0;
  while (n > 0) {
    [a, b] = [b + a, a];
    n--;
  }
  return b;
}
{% endcodeblock %}

Instead of a recursive loop, here we are using an iterative loop to build the Fibonacci sequence. Two numbers are initialized as 1 and 0, and in every iteration of the loop, counting backwards from n to 0, the sum of the two numbers is calculated. When n reaches 0, the lower of the two numbers is returned, which resolves to the `n`th number in the sequence.

While the naive solution above struggled with finding the solution for `n > ~45`, the iterative solution can easily get you, for example, the 200th number in the sequence in a fraction of a second. Verify for yourself that it's `2.8057117299251016e+41`! Wowzah!

Now, the reason why this solution is so fast and the previous one so slow is quite simple if you think about it. Well no, in fact, it's rather complicated, but I'm just going to explain it as simple as I can.

`fibIterative(5)` is a simple operation for the JS engine. For every iteration from `n == 5` to `n == 1`, it sums up `a` and `b` and subtracts 1 from `n`. But now think about `fibNaive(5)`. This becomes `fibNaive(3) + fibNaive(4)`. What the JS engine is going to do is it's going to first calculate `fibNaive(3)` by running it recursively (see above how). Then it will take that number and add it to `fibNaive(4)`.

In other words, solving this problem in the naive way results in a **giant monstrosity** of recursive loops, at every level of which the JS engine must create a new execution environment *and* remember what number to sum with what. Try visualizing the recursive tree for solving `fibNaive(10)`, and you'll immediately see how difficult it really is.

But there is hope! Read on for a **good** recursive solution, but first, I promised to give you an ES5 version of the iterative loop, so here it is. As you can see, the only thing that's different is the use of a temporary variable to replace the [array destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) of the ES6 implementation:

{% codeblock lang:javascript %}
function fibIterativeES5(n) {
  var a = 1;
  var b = 0;
  var c = null;
  while (n > 0) {
    c = a;
    a = b + a;
    b = c;
    n--;
  }
  return b;
}
{% endcodeblock %}

Again, this is exactly the same as the ES6 solution above, but it looks more complicated because of the temporary variable. I'm actually not a huge fan of many aspects of ES6 myself, but array/object destructuring is **definitely** one of the features I absolutely adore! It just makes things a lot easier.

## A recursive solution

And now here we are, back to the quest for the `n`th number in the Fibonacci sequence using a recursive function. Before we get to it though, I need to say a few words about [tail call optimization](http://www.2ality.com/2015/06/tail-call-optimization.html).

*Tail call optimization* or *proper tail calls* is another one of the new features in ES6 that are actually really useful (though I suppose this one isn't quite as sexy as some of the more often talked about ones). Excuse my glossing over of some rather deep and sophisticated concepts, but I'm trying to keep this short, so think about a tail call as what happens when a function calls a function as its *very last* action. It's easier to explain with some examples:

{% codeblock lang:javascript %}
function tailCall() {
  // stuff
  return aFunction(); // this is a tail call
}

function tailCall2(a) {
  // stuff
  return aFunction(a); // this is a tail call
}

function tailCall3() {
  // stuff
  return aFunction() + 2; // this is NOT a tail call, because the last operation is actually adding 2
}

function tailCall4() {
  // stuff
  return aFunction() + anotherFunction(); // NOT a tail call, again the last operation is a sum
}

function tailCall5(a) {
  // stuff
  return tailCall5(a - 2) + tailCall5(a - 1); // NOT a tail call, last operation is a sum
}

function tailCall6(n, a, b) {
  // stuff
  return tailCall6(n - 1, a + b, a); // this is a tail call
}
{% endcodeblock %}

I hope this explains what a tail call is, and why `return fibNaive(n - 2) + fibNaive(n - 1)` is not a tail call, but `return fibRecursive(n - 1, a + b, a)` *is* a tail call. But why is it important?

Well, as noted above, ES6 introduced *proper tail calls* or *tail call optimization*. Go read more about the how and the why elsewhere if you're interested, but suffice it to say here on this blog that it causes the JS engine (assuming it supports this particular ES6 feature) to essentially treat a recursive function more like an iterative loop **as long as you are using tail calls**. So no more creating huge amounts of execution contexts, which means that a well-implemented recursive loop using tail calls will run very quickly indeed.

So what does this proper recursive Fibonacci function look like in JavaScript? Here it is, and do note the tail call, without which it wouldn't work!

{% codeblock lang:javascript %}
function fibRecursive(n, a = 1, b = 0) {
  if (n === 0) {
    return b;
  } else {
    return fibRecursive(n - 1, a + b, a);
  }
}
{% endcodeblock %}

Maybe this looks a little weird at first sight, but if you look more closely, you'll find that it's just like the loop from before. `fibRecursive(n)` initializes the loop with ES6 default arguments `a = 1` and `b = 0`, and then with every iteration of the loop, `n` becomes `n--`, `a` becomes `a + b` and `b` becomes `a`. Upon reaching the base case (`n === 0` here), `b` is returned, which, just like in the iterative function, resolves to the `n`th Fibonacci number. And it all happens in an instant!

ES6 is old news by now, really, but it's still new (or new-*ish*) for a whole lot of people. Case in point: this article used to show an ES5 implementation of `fibRecursive`, until **Emilio Srougo** pointed out to me in the comments that I could drop the initialization function by using [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters).

Just to show you again how ES6 can really help make your code more readable and expressive, here's the ES5 implementation of the above function. No default parameters means you need an initialization function:

{% codeblock lang:javascript %}
function fibRecursiveES5(n) {
  return fibLoop(n, 1, 0);
}

function fibLoop(n, a, b) {
  if (n === 0) {
    return b;
  } else {
    return fibLoop(n - 1, a + b, a);
  }
}
{% endcodeblock %}

Alternatively, you could do away with the initialization function and add this ugliness to the loop:

{% codeblock lang:javascript %}
a = a || 1;
b = b || 0;
{% endcodeblock %}

But you have to admit, default parameters are so much nicer, aren't they?

## Conclusion

The common Fibonacci interview question may at first sight appear to be a simple question designed to test your basic understanding of recursion, but a more involved answer will take you through a number of sophisticated concepts in computer science in general and JavaScript in particular.

Make sure you know your stuff before you naively answer this question! As for myself, maybe I'll refer people to this post when it inevitably comes up in a future interview? Judging by its word count, it would sure save me a whole lot of talking!
