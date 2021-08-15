---
layout: post
title: Ternary Conditional Operator
tags: Programming
---

So today I came across a situation where I need to use **ternary conditional operator** in list comprehensions. My friend worked it out for me and I feel like it's better to note it down.

## List Comprehensions

List comprehensions provide a concise way to create lists in python. Instead of a `for`  loop, we can write simply one line of code like this:

{% highlight python %}
l = [-1, 2, 3, 5, 10]
a = [e+1 for e in l]
{% endhighlight %}

Then we get:

{% highlight python %}
a = [0, 3, 4, 6, 11]
{% endhighlight %}


## Iterator filter

On top of it, we can even add an `if` statement to the list comprehension. 

{% highlight python %}
b = [e for e in l if e>2]
{% endhighlight %}

Then we get:

{% highlight python %}
a = [3, 5, 10]
{% endhighlight %}

`if` here is an **iterator filter**. This is cool. But the problem is: what if you want to keep elements that are larger than 2, **and**, for example, multiply the other elements by 2 and make a new list?

## Ternary conditional operator

Now what we need is a **ternary conditional operator**.

{% highlight python %}
c = [e if e>2 else e*2 for e in l]
{% endhighlight %}

Then we get:

{% highlight python %}
c = [-2, 4, 3, 5, 10]
{% endhighlight %}

The ternary conditional operator writes in general:

{% highlight python %}
<expression1> if <condition> else <expression2>
{% endhighlight %}

And you can go further with nested `if`-`elif`-`else`-like statements:

{% highlight python %}
c = [e if e>2 else e*2 if e>0 else e+10 for e in l]
{% endhighlight %}

The result should be:

{% highlight python %}
c = [9, 4, 3, 5, 10]
{% endhighlight %}

## Why position matters

As you may notice, when we use only the `if` statement, it's positioned after the `for`-`in` iterator. It's because the **iterator filter** `if` can be only used on iterators or iteration generators, which is a list here. But a **ternary conditional operator** can't be used on iterators since an iterator can't be evaluated.

## Performance

My friend also told me that it's quicker to iterate over a set instead of a list. Iterating over a set needs constant time `O(1)` per step only. Good catch!

## Reference

- [Ternary conditional operator](http://www.pythoncentral.io/one-line-if-statement-in-python-ternary-conditional-operator/)
- [List comprehension](http://www.pythonforbeginners.com/basics/list-comprehensions-in-python)

