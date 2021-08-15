---
layout: post
title: Groovy Method Pointer
tags: Programming
---

My recent work involves a lot of scripting and SWEng with Groovy. Groovy lives under the solemn structure of Java while providing syntactic sugars for those lazy coders. For a python guy like me, Groovy is full of goodness so far.

## Closure and Method Pointer `.&` 

An important concept in Groovy is `Closure`. While being important, it's a complicated mechanic so that I want to save it for later. Today I would love to elaborate a bit on the method pointer operator `.&`.

As the title goes, the method pointer operator `.&` allows you to store a reference to a method in a variable, in order to call it later:

{% highlight groovy %}
def str = 'example of method reference'
def fun = str.&toUpperCase
def upper = fun()
assert upper == str.toUpperCase()
{% endhighlight %}

The method pointer has the type `Closure` so it can be used wherever a `Closure` could be used. 


## Strategy Pattern

[Strategy Pattern](https://www.tutorialspoint.com/design_pattern/strategy_pattern.htm) can make good use of the method pointer converting an existing method.

{% highlight groovy %}
def transform(List elements, Closure action) {
    def result = []
    elements.each {
        result << action(it)
    }
    result
}
String describe(Person p) {
    "$p.name is $p.age"
}
def action = this.&describe
def list = [
    new Person(name: 'Bob',   age: 42),
    new Person(name: 'Julia', age: 35)]
assert transform(list, action) == ['Bob is 42', 'Julia is 35']
{% endhighlight %}


## Overloading

Arguments are resolved at runtime so overloading is possible.

{% highlight groovy %}
def doSomething(String str) { str.toUpperCase() }
def doSomething(Integer x) { 2*x }
def reference = this.&doSomething
assert reference('foo') == 'FOO'
assert reference(123)   == 246
{% endhighlight %}


## References

- [http://docs.groovy-lang.org/latest/html/documentation/index.html#method-pointer-operator]()
- [https://www.jianshu.com/p/0fedb67ab236]()
- [https://www.tutorialspoint.com/design_pattern/strategy_pattern.htm]()