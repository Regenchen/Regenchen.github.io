---
layout: post
title: deque(), islice() & more
tags: Programming
---

It feels really cool to eventually kick off my career! Here I want to discuss two small pieces of code which provides an efficient solution to assembling a [sliding window](https://www.geeksforgeeks.org/window-sliding-technique/) over a sequence. 

## The sliding window function with deque()

{% highlight python %}
from collections import deque

def sliding_window(seq, W):
  # deque initialization
  it = iter(seq)
  window = deque((next(it, None) for _ in range(W)), maxlen=W)
  yield tuple(window)
  # loop
  for e in it:
    window.append(e)
    yield tuple(window)
{% endhighlight %}

This function provides a generator with `yield` function. The generator returns a tuple of elements in the window in each iteration.

Several notes should be kept for this function:
- During initialization, `next()` is processed for `W` times to form a deque of `W` elements.
- `_` in the `range` iterator means the variable is not important by itself but serves for grammatical purpose.
- Into the `for` loop, iterator `it` continues from where `next()` ends.
- In the deque, due to the restriction of `maxlen` handler, each time a new element is appended to the deque, an element would be discarded from the **opposite end**.
- The function returns an iterable in which the values are sequentially given by `yield`. 

## islice()

`islice()` enables picking up specific elements from the iterable.

{% highlight python %}
from itertools import islice
# get first k items
it = iter('abracagabra')
list(islice(it, 3))
>>> Out[]: ['a', 'b', 'r']
# get the kth item (0-based)
it = iter('abracagabra')
next(islice(it, 3, 3))
>>> Out[]: 'a'
{% endhighlight %}

`islice()` could be useful in the round-robin scheduling. Another post will be dedicated to the algorithm.

## References

- [The sliding window function](https://stackoverflow.com/questions/6822725/rolling-or-sliding-window-iterator)
- [Doc on deque()](https://docs.python.org/3.7/library/collections.html#collections.deque)

