---
layout: post
title: gc.collect()
tags: Programming
---

Boosting algorithms, such as LightGBM and XGBoost, are taking over now in Kaggle machine learning competitions and industrial tasks at scale. It's now really simple to perform a machine learning study powered by these advanced algorithms, but it comes with significant memory cost.

<!-- more -->

## Introduction

{% highlight python %}
import gc
gc.collect()
{% endhighlight %}

The aforementioned `gc.collect()` command **enforces memory dump**. Normally we use `del(i)` to clean up an object, here for example `i`, to release memory for future use. However, in python, `del(i)` only puts an object in a state of **limbo** - the memory signals to be cleared but is not emptied until the python thread is killed, or `gc.collect()` is called.



## When to use in Machine Learning

In a large-scale, offline machine learning case, we can use `gc.collect()` after model training and prediction as follows.

{% highlight python %}
from sklearn.model_selection import StratifiedKFold
import xgboost as xgb
skf = StratifiedKFold(...)
for i, (train_index, test_index) in enumerate(skf.split(X, y)):
	...
    xgb_model = xgb.train(...)
    xgb_model.predict(...)
gc.collect()
{% endhighlight %}

I haven't implement a test to check if the `gc.collect()` call is necessary after an XGBoost (or LightGBM) session, but it could't go wrong either, right?

## Reference

- [kaggle kernel](https://www.kaggle.com/rshally/porto-xgb-lgb-kfold-lb-0-282)
- [csdn](http://blog.csdn.net/nirendao/article/details/44426201/)

