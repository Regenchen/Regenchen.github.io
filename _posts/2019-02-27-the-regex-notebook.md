---
layout: post
title: Python Regex Notebook
tags: Programming
---

Regex has been useful in my work but it seems I can't always figure things out on site without extensively going through the likes of _StackOverflow_. Recently I could pick up some time to read the official guide for python regex and it's proved to be helpful.

## Repetition

Three metacharacters for repetitive matching function as follows.

| Metacharacter | Equivalence | Example |
|---------------|:-----------:|:-------:|
| `*` | {0, } | `ca*t` -> `ct`, `cat`, `caat`, ... |
| `+` | {1, } | `ca*t` -> `cat`, `caat`, ... |
| `?` | {0, 1} | `ca*t` -> `ct`, `cat` |

## Backslash Plague

Use `r` in the string definition to avoid backslash plague. Avoid excessive use of `\` to escape metacharacters.

| Regular string | Raw string |
|:--------------:|:----------:|
| `"ab*"` | `r"ab*"` | 
| `"\\\\section"` | `r"\\section"` | 
| `"\\w+\\s+\\1"` | `r"\w+\s+\1"` |

## There will be more to come...

blahblahblah...

## Reference

[Official HOWTO](https://docs.python.org/3.3/howto/regex.html)