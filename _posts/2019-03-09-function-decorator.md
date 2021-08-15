---
layout: post
title: Python Decorators
tags: Programming
---

I was looking into the [`kdtree`](https://github.com/stefankoegl/kdtree/blob/master/kdtree.py#L187) repository this week and came up with the `require_axis` decorator function. I have used some of the built-in decorators in python, but function decorators seem pretty powerful at the first glance so I decided to dig it a bit.

## Built-in decorators 

> Decorators don't differ from ordinary functions, you only call them in a fancier way.
> 
> -- *stackoverflow*

However, some are often used in the *fancier way* -- namely [`@property`](https://docs.python.org/3/library/functions.html#property), [`@classmethod`](https://docs.python.org/3/library/functions.html#classmethod) and [`@staticmethod`](https://docs.python.org/3/library/functions.html#staticmethod).

## Function Decorators

Into the world of user-defined decorators, we need to understand first the most substantial utility of this syntactic sugar: **to wrap a function and modify (usually enhance) its behavior.** A snippet of a simple decorator is as follows.

{% highlight python %}
"""define decorator"""
def p_decorate(f):
  def wrapper(name):
    return "<p>{0}</p>".format(f(name))
  return wrapper
"""use decorator"""
@p_decorate
def get_victor(name):
  return "the victor is {0}".format(name)
# print(get_victor("Me"))
# output - <p>the victor is Me</p>
{% endhighlight %}

## Decorator with arguments

There is room for better extensibility with the decorators. First, it's possible to pass arguments to them. 

{% highlight python %}
"""define decorator"""
def tag(sign):
  def tag_decorate(f):
    def wrapper(name):
      return "<{0}>{1}</{0}>".format(sign, f(name))
    return wrapper
  return tag_decorate
"""use decorator"""
@tag("b")
def get_victor(name):
  return "the victor is {0}".format(name)
# print(get_victor("Me"))
# output - <b>the victor is Me</b>
{% endhighlight %}

## Decorator for class methods

In a class, methods are expected to reference the current object as their first parameter. It brings confusion when designing a decorator for functions and class methods alike. The solution is to build the wrapper function so that it accepts arbitrary number of parameters.

{% highlight python %}
"""define decorator"""
def p_decorate(f):
  def wrapper(*args, **kwargs):
    return "<p>{0}</p>".format(f(*args, **kwargs))
  return wrapper
"""use decorator in class methods"""
class Person(object):
  def __init__(self):
    self.name = "John"
    self.family = "Doe"
  @p_decorate
  def get_fullname(self):
    return self.name + " " + self.family
# Person().get_fullname()
# output - <p>John Doe</p>
{% endhighlight %}

## Signature overloading

Decorators will overload the name, module and docstring of the original function:

{% highlight python %}
print(get_victor.__name__)
# Outputs wrapper
{% endhighlight %}

We can use [functools.wraps](https://docs.python.org/2/library/functools.html#functools.wraps) to reset these signatures to the original state.

{% highlight python %}
from functools import wraps
"""define decorator"""
def tag(sign):
  def tag_decorate(f):
    @wraps(f)
    def wrapper(name):
      return "<{0}>{1}</{0}>".format(sign, f(name))
    return wrapper
  return tag_decorate
"""use decorator"""
@tag("b")
def get_victor(name):
  """return the name of the victor"""
  return "the victor is {0}".format(name)
# print(get_victor.__name__) # get_victor
# print(get_victor.__doc__) # return the name of the victor
# print(get_victor.__module__) # __main__
{% endhighlight %}

## References

- [Python - what are all the built-in decorators?](https://stackoverflow.com/questions/480178/python-what-are-all-the-built-in-decorators)
- [A guide to Python's function decorators](https://www.thecodeship.com/patterns/guide-to-python-function-decorators/)
- [Primer on Python decorators](https://realpython.com/primer-on-python-decorators/)
