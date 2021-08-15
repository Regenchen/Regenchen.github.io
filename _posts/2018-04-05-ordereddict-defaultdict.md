---
layout: post
title: OrderedDict and DefaultDict
tags: Programming
---
## Play with dicts in python

`dict` is a really nice data structure in python but it has limitations. I did some research recently  with the `collections` module in Python and found two niche extentions of `dict`: `OrderedDict` and `DefaultDict`.

## OrderedDict

`OrderedDict` serves under the circumstances that we want to keep the order of keys being added to the dictionary.

{% highlight python %}
from collections import OrderedDict
items = (('apple', 2), ('pear', 1), ('orange', 3))
od = OrderedDict(items)
d = dict(items)
{% endhighlight %}

When we iterate over the dictionary, `OrderedDict` keeps the order while the normal dictionary doesn't.

{% highlight python %}
od.items()
Out[]: odict_items([('apple', 2), ('pear', 1), ('orange', 3)])
d.items()
Out[]: dict_items([('orange', 3), ('pear', 1), ('apple', 2)])
{% endhighlight %}

Or we can make a sorted dictionary with `OrderedDict`:

{% highlight python %}
# sorted by key
sd = OrderedDict(sorted(items, key=lambda t: t[0]))
sd.items()
Out[]: odict_items([('apple', 2), ('orange', 3), ('pear', 1)])
{% endhighlight %}

## DefaultDict

A non-trivial problem with dictionary in python is that when we query an item with `d[key]`, it returns an error if the key doesn't exist. In some implementations with dictionary, we don't want this to happen - it's ok that the key doesn't exist. 

With `DefaultDict`, we can assign a default factory method so we get default value when querying a non-existant key.

{% highlight python %}
# make a defaultdict
from collections import defaultdict
items = (('apple', 2), ('pear', 1), ('orange', 3))

# set default factory method
dd = defaultdict(int)
for k, v in items:
    dd[k] = v

# query a non-existant key
dd['mango']
Out[]: 0
{% endhighlight %}

There's a lot of works that `DefaultDict` could do.

Store key-value pairs in lists:

{% highlight python %}
items = [('GER', 'Bayern'), ('ENG', 'Man City'), ('GER', 'BVB'), ('ESP', 'Barcelona'), ('ENG', 'Liverpool')]
d = defaultdict(list)
for k, v in items:
    d[k].append(v)
d.items()
Out[]: dict_items([('GER', ['Bayern', 'BVB']), ('ESP', ['Barcelona']), ('ENG', ['Man City', 'Liverpool'])])
{% endhighlight %}

Counting:

{% highlight python %}
item = 'abgegangen'
d = defaultdict(int)
for k in item:
    d[k] += 1
d.items()
Out[]: dict_items([('a', 2), ('e', 2), ('b', 1), ('n', 2), ('g', 3)])
{% endhighlight %}

## More with collections module ...

There are a few useful tools in the `collections` module such as `Counter` and `deque`. Find more in the [official documentation](https://docs.python.org/2/library/collections.html#module-collections).
