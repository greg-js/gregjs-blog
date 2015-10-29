title: Normal ternaries vs nested ternaries
categories: javascript
tags:
  - js
  - style
date: 2015-10-29 01:15:16
---

This pretty much sums up my opinion about ternaries:

``` javascript
var Ternary = function(nested) {
  return {
    is: function() {
      return (nested) ? 'the devil' : 'awesome';
    }
  };
};

// something like `size = (a < 10) ? 'small' : 'large'`
var aNormalTernary = new Ternary(false);

// think `size = (a < 10) ? (a < 5) ? (a < 0) ? 'negative' : 'tiny' : 'small' : 'large'`
var aNestedTernary = new Ternary(true);

aNormalTernary.is(); // awesome
aNestedTernary.is(); // the devil
```
