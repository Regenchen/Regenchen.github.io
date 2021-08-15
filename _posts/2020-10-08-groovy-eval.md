---
layout: post
title: eval() in Groovy
tags: Programming
---

As a python fanatic, `eval()` and `literal_eval()` walked straight into my coding life and would never ever leave again. For me, this pair of beauty usually works as interpreter in data pipelines, enabling rule configuration from other systems. The `eval()` function exists in Groovy as well, and it deserves a mini intro.

## `Eval.me()`

`Eval.me()` allows script evaluation without arguments. The script can be compound, i.e. a collection of multiple expressions:

	def exp = "def a = 1; a + 2"
	println(Eval.me(exp)) // 3

When arguments are provided, it's possible to customize binding namespaces to  values:

	def values = ['a': 1, 'b': 2]
	def exp1 = 'd.a + d.b'
	def exp2 = '"$d.a + $d.b"'
	
	println(Eval.me("d", values, exp1)) // 3
	println(Eval.me("d", values, exp2)) // 1 + 2

On a side note, the dollar sign `$` is a placeholder in string for variables.

## `Eval.x()`

`Eval.x()`, `Eval.xy()` and `Eval.xyz()` are simple variations of `Eval.me()`. The binding of arguments are fixed to `x`, `y` and `z`. The number of arguments are restricted to 1, 2 and 3 respectively.

	def val1 = 1.3
	def val2 = 1.5
	def val3 = 1.9
	def exp1 = "x > 1.5"
	def exp2 = "x + y > 2"
	def exp3 = "x + y > z"
	
	println(Eval.x(val1, exp1)) // false
	println(Eval.xy(val1, val2, exp2)) // true
	println(Eval.xyz(val1, val2, val3, exp3)) // true

## `GroovyShell` and pipeline caching

Here is the Java source code of `Eval.me()`:

	public static Object me(String symbol, Object object, String expression) throws CompilationFailedException {
		Binding b = new Binding();
		b.setVariable(symbol, object);
		GroovyShell sh = new GroovyShell(b);
		return sh.evaluate(expression);
	}

The function first binds arguments to values and then execute the script. In parallel jobs, problems could emerge as the binding process is exhaustive.

Luckily, `GroovyShell` provides with other methods which allow the parsing pipeline to be cached:

	def exp = 'x < 0.5'
	def value = 0.35
	
	Binding b = new Binding()
	GroovyShell sh = new GroovyShell(b)

	def script = sh.parse(exp) // `script` is the parsing pipeline
	b.setVariable("x", value)
	boolean result = script.run() // true

This allows different value to be passed to the same pipeline, which enables a minimal configurable pipeline in parallel, stream computing. Below is the time efficiency of script parsing with and without caching.

![](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2020-10-08-evaluate.png){: .center-image}

## References

- [https://mrhaki.blogspot.com/2009/11/groovy-goodness-simple-evaluation-of.html]()
- [https://docs.groovy-lang.org/latest/html/api/groovy/util/Eval.html]()