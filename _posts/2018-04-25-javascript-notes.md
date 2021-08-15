---
layout: post
title: JS notes - ternary, const function
tags: Programming
---

Recently I'm attacking the world of JavaScript - a treasure for the front-end devs but a pain for the data scientists. Here are some notes from my experience working with JavaScript...

## Ternary conditional operator

I explained [here](https://jiaxigu.github.io/2017-11-27/ternary-operator-python) how to do ternary operator in Python but things get more complicated in JavaScript. In the latest ES6 coding standards, JavaScript is much more simplified than its previous versions. Here is an example of ternary conditional operator:

{% highlight javascript %}
var price = 25;
var isExpensive = price > 20 ? true : false;
console.log(isExpensive) // >>> true
{% endhighlight %}

It replaces:

{% highlight javascript %}
var price = 25;
var isExpensive;
if (price > 20) {
  isExpensive = true;	
} else {
  isExpensive = false;
}
console.log(isExpensive) // >>> true
{% endhighlight %}

For multiple ternary operator, the coding format looks like:

{% highlight javascript %}
var price = 25;
var origin = 'sweden';
var car = 
  price > 50 
    ? 'porsche'
    : price < 10
      ? 'seat'
      : origin === 'sweden'
        ? 'volvo'
        : 'bmw'
console.log(car) // >>> 'volvo'
{% endhighlight %}

## Const function

This is a new feature in ES5 and sexy af:

{% highlight javascript %}
const helloWorld = () => 'Hello World!';
{% endhighlight %}

The const function ensures **simplicity** and **immutability**. This line of code replaces:

{% highlight javascript %}
function helloWorld() {
  return 'Hello World!';
}
{% endhighlight %}

There are also good reasons to keep using the old way to define functions. Temporal dead zone(TDZ) could be an issue, readability as well.

## References

- [Ternary conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
- [On const function](https://stackoverflow.com/questions/33040703/proper-use-of-const-for-defining-functions-in-javascript)
- [Pros and cons](https://medium.freecodecamp.org/constant-confusion-why-i-still-use-javascript-function-statements-984ece0b72fd)
