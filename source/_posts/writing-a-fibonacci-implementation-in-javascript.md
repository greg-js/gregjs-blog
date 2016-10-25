title: "Writing a Fibonacci implementation in JavaScript"
date: 2016-03-14 16:14:05
categories: javascript
tags:
- js
- tips
- learning
- interview
---

I was just looking through some of the notes I'd taken a few months ago, and was inspired to write a quick post on how to write a *good* JavaScript solution for finding the `n`th number in a Fibonacci sequence.

I'm sure most of this information came right from some other blog or book, so credit really goes to wherever I originally learned these insights, but sadly I didn't include sources in my notes. Just google around if you want to know more though, because it's all over the internet, really.

Just in case you've never heard of the [Fibonacci sequence](https://en.wikipedia.org/wiki/Fibonacci_number), it's a mathematical sequence where every number is the sum of the previous two numbers in the sequence:

{% codeblock lang:plain line_number:false %}
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...
{% endcodeblock %}

Or, equally common:

{% codeblock lang:plain line_number:false %}
1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...
{% endcodeblock %}

Now, I'm not going to go over the importance of this sequence, not only for computer science but for all of the natural world (but trust that it is). All you need to know is that "find the nth Fibonacci number" is a very common interview question for programmers of all persuasions, including the JavaScript kind.

<!-- more -->

## The problem

Write a function that returns the `n`th Fibonacci number.

I will leave the questions of whether the first Fibonacci number is 0 or 1 and whether the `n`th number should be zero- or one-indexed up to you. Adjusting the the following implementations to take these trivial differences into account should be mostly trivial.

I will also assume `n` to always be a positive number. Calling a Fibonacci function on a negative number, or even (depending on your implementation and assumptions) on 0 is meaningless and should really return undefined. Actually implementing this would lead us too far astray though so let us keep it simple and assume these constraints on our input `n`.

(special thanks to **Sahiba Chopra** for helping me realize some inconsistencies addressed above)

## A naive solution

A major reason why this question appears so often in coding interviews is because answering it correctly demonstrates an understanding of a fundamental concept in programming: **recursion**.

In very simple terms, a recursive function is a function that calls itself. Now, if you think about a Fibonacci number as the sum of two previous Fibonacci numbers, each of which are the sums of their two previous numbers, each of which are the sums of, ... You catch my drift, and hopefully it's now somewhat clear to you why recursion and Fibonacci are made for each other.

So here is a recursive, canonical, but naive JavaScript implementation for finding the `n`th number in the sequence:

{% codeblock lang:javascript %}
function fibNaive(n) {
  if (n < 2) {
    return 1;
  } else {
    return fibNaive(n - 2) + fibNaive(n - 1);
  }
}
{% endcodeblock %}

Or, if you want the first number to be 0 and tighten things up a bit:

{% codeblock lang:javascript %}
function fibNaiveZero(n) {
  return (n < 2) ? n : fibNaiveZero(n - 2) + fibNaiveZero(n - 1);
}
{% endcodeblock %}

(Note: I will just pick one of the two common sequences from now on, either starting from 0 or 1, rather than double up on each of them)

## Recursive functions

Every recursive function has a **base case** and a **recursive step**.

* For `fibNaive` the base case is the `(n < 2)` condition of the if-statement resolving to `true` and thus returning `0`
* For `fibNaiveZero` the base case is the `(n < 2)` condition of the ternary resolving to `true` and returning `n`

* For both `fibNaive` and `fibNaiveZero` the recursive step has the function returning a sum of the results of calling itself on `n - 1` and `n - 2`

To drive home the point, let's write out exactly how `fibNaive` performs its recursive step again and again until it reaches its base case:

Suppose `n` is 4. `fibNaive(4)` returns `fibNaive(2) + fibNaive(3)`. Now, the first term resolves to `fibNaive(0) + fibNaive(1)`, which is `1 + 1 == 2`. This 2 is then added to the second term in the original split (`fibNaive(3)`, which of course resolves to `fibNaive(1) + fibNaive(2)`. That 2 from earlier plus `fibNaive(1) == 1` makes 3 and now we add that to `fibNaive(2) == fibNaive(1) + fibNaive(1) == 2`. All in all what you get is `1 + 1 + 1 + 1 + 1 == 5`. Scroll up and verify that this is indeed the correct answer.

So if this solution works, then why is it naive? Well, try finding the 40th number using this function. Takes a little while, doesn't it? And don't even think about running it on higher numbers because it is terribly, terribly, terribly inefficient.

## An iterative solution

I'll get back to explaining why the previous implementation was lacking in a minute (although maybe it's pretty obvious), but first, let's come up with a better solution by eschewing recursion and embracing iteration (note: this uses some ES6, see below for how to make it work in ES5):

{% codeblock lang:javascript %}
function fibIterative(n) {
  let [a, b] = [1, 0];
  while (n-- > 0) {
    [a, b] = [b + a, a];
  }
  return b;
}
{% endcodeblock %}

Instead of a *recursive* loop, here we are using an *iterative* loop to build the Fibonacci sequence. The two numbers `a` and `b` are initialized as 1 and 0, and in every iteration of the loop (counting backwards from n to 0), `a` becomes the sum of the two numbers and the lower number `b` becomes the previous value of the higher number `a`. When `n` reaches 0, the lower of the two numbers is returned and, what do you know, it resolves to the `n`th number in the Fibonacci sequence.

While the naive solution above struggled with finding the solution for `n > ~45`, the iterative solution can easily get you, for example, the 200th number in the sequence in a fraction of a second. Verify for yourself that it's `2.8057117299251016e+41`! Wowza!

Now, the reason why this solution is so fast and the previous one so slow is quite simple if you look closely at what is happening.

Think about what it means for the JavaScript engine to execute `fibIterative(10)`. For every iteration from `n == 10` to `n == 1`, it does nothing more than summing up `a` and `b` and subtracting `1` from `n`. Finally, when `n` is `0`, it returns `b`. All in all, that's pretty simple.

But now think about `fibNaive(10)`. This becomes `fibNaive(8) + fibNaive(9)`. The first term becomes `fibNaive(6) + fibNaive(7)` and the first term of that becomes `fibNaive(4) + fibNaive(5)`, and I'll stop there because this blog post would get really, really long if I don't. And to make matters worse, just to write this out, I would always have to remember where exactly I am in this monstrosity of term duplication and then add every single value up. Even for a computer, that is a pretty, *pretty*, **pretty**  hard problem.

In other words, solving this problem in the naive way results in a **gigantic explosion** of recursive loops, at every level of which the JS engine must create a new execution environment to push onto the stack *and* remember all that came before, because it needs to sum everything up in the end.

But there is hope! Read on for a *good* recursive solution, but first, I promised to give you an ES5 version of the iterative loop, so here it is. As you can see, the only thing that's substantially different is the use of a temporary variable to replace the [array destructuring](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) of the ES6 implementation:

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

Again, this is exactly the same as the ES6 solution above, but it looks a bit more mystifying because we have to use a temporary variable -- or at least, that's how it looks to me but maybe that's just because Array/object destructuring is **definitely** one of the features I absolutely adore about the somewhat mixed bag that is ES6! It just makes certain things a whole lot easier.

## A recursive solution

And now here we are, back to the quest for the `n`th number in the Fibonacci sequence using a recursive function. Before we get to it though, I simply must bore you once again by saying saying a few words about [tail call optimization](http://www.2ality.com/2015/06/tail-call-optimization.html).

*Tail call optimization* or *proper tail calls* is another one of the new features in ES6 that are actually really useful (though I suppose this one isn't quite as sexy as some of the more often talked about ones). Excuse my glossing over of some rather deep and sophisticated concepts, but I'm trying to keep this short, so think about a tail call as what happens when a function returns and calls a function as its *very last* action. It's easier to explain with some examples:

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
  return aFunction() + 2; // this is NOT a tail call, because the very last operation needed here is summing up the result of the function call with 2
}

function tailCall4() {
  // stuff
  return aFunction() + anotherFunction(); // NOT a tail call, again the last operation is a sum
}

function tailCall5(a) {
  // stuff
  return tailCall5(a - 2) + tailCall5(a - 1); // NOT a tail call, last operation is a sum again
}

function tailCall6(n, a, b) {
  // stuff
  return tailCall6(n - 1, a + b, a); // this is a tail call!
}
{% endcodeblock %}

I hope this explains what a tail call is, and why `return fibNaive(n - 2) + fibNaive(n - 1)` is *not* a tail call, but `return fibRecursive(n - 1, a + b, a)` *is* a tail call. But why is it important?

Please do read more about the how and the why on sites like MDN if you're interested, but suffice it to say here that it causes the JS engine (assuming it supports this particular ES6 feature) to essentially treat a recursive function with proper tail calls kind of like an iterative loop **as long as you are using tail calls**. The reason is that stacking huge amounts of execution contexts on top of one another is no longer necessary because all the information is packed into that last function call and nothing from previous iterations needs to be kept in memory.

This means that a well-implemented recursive loop using tail calls will run very quickly indeed. But what does such a proper recursive Fibonacci function look like in JavaScript? Here it is, and do note the tail call, without which it wouldn't work!

{% codeblock lang:javascript %}
function fibRecursive(n, a = 1, b = 0) {
  if (n === 0) {
    return b;
  } else {
    return fibRecursive(n - 1, a + b, a);
  }
}
{% endcodeblock %}

Or, as a sexy oneliner:

{% codeblock lang:javascript %}
const fibSexy = (n, a = 1, b = 0) => (n === 0) ? b : fibSexy(n - 1, a + b, a);
{% endcodeblock %}

Now if you're confused by this, don't worry, it does look a little weird at first sight. But once again, if you look more closely, you'll find that this is actually just like the iterative loop from before. `fibRecursive(n)` initializes the loop with ES6 default arguments `a == 1` and `b == 0`. Then, with every iteration of the loop, `n` becomes `n--`, `a` becomes `a + b` and `b` becomes `a`. Upon reaching the base case (`n == 0` here), the smaller number `b` is returned, which, just like in the iterative function, resolves to the `n`th Fibonacci number. And it all happens in an instant!

ES6 is old news by now, really, but it's still new (or new-*ish*) for a whole lot of people. Case in point: this article used to show an ES5 implementation of `fibRecursive`, until **Emilio Srougo** pointed out to me in the comments that I could drop the initialization function by using [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters).

Just to show you again how ES6 can really help make your code more readable and expressive, here's the old ES5 implementation of the above function. No default parameters means you need an initialization function:

{% codeblock lang:javascript %}
function fibRecursiveES5(n) {
  return fibLoop(n, 1, 0);
}

function fibLoop(n, a, b) {
  return (n === 0) ? b : fibLoop(n - 1, a + b, a);
}
{% endcodeblock %}

Alternatively, you could do away with the initialization function and add this ugliness to the loop:

{% codeblock lang:javascript %}
a = a || 1;
b = b || 0;
{% endcodeblock %}

But you have to admit, the ES6 implementation is so much nicer, isn't it?

## Conclusion

The common Fibonacci interview question may at first sight appear to be a simple question designed to test your basic understanding of recursion, but a more involved answer will take you through a number of sophisticated concepts in computer science in general and JavaScript in particular.

Make sure you know your stuff before you naively answer this question! As for myself, maybe I'll refer people to this post when it inevitably comes up in a future interview? Judging by its word count, it would sure save me a whole lot of talking!
