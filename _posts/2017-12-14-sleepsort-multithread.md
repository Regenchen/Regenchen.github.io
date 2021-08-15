---
layout: post
title: Thread Pool and the Best Sorting Algorithm
tags: Programming
---

The `multiprocessing.dummy` module is a great tool to manage multithread tasks in python. [It replicates the API of `multiprocessing` but is actually a wrapper around the `threading` module.](https://docs.python.org/2/library/multiprocessing.html#module-multiprocessing.dummy) I would create a chart to sum things up.

![chart](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-12-14-chart.png){: .center-image}


## Thread pool

With `multiprocessing.dummy` we can create a thread pool. For example we can build a multithread web crawler:

{% highlight python %}
from multiprocessing.dummy import Pool as ThreadPool

urls = [
    'https://github.com', 
    'https://www.kaggle.com'
]

def fun(i):
    """
    do something.
    """

# set the number of threads
pool = ThreadPool(processes=len(url))

# map the function
result = pool.map(fun, urls)

# manage the threads post mortem
pool.close()
pool.join()
{% endhighlight %}


## Sleepsort

Sleepsort is a **joke** sorting algorithm that [gained popularity on the internet.](https://www.quora.com/What-is-sleep-sort). We can implement the algoirithm with `multiprocessing.dummy` module.

{% highlight python %}
from multiprocessing.dummy import Pool as ThreadPool
from time import sleep

def sleepsort(array):
    """
    sleepsort.
    """
    sorted_list = []
    interval = 0.001

    def task(n):
        sleep(n * interval)
        sorted_list.append(n)

    pool = ThreadPool(len(array))
    pool.map(task, array)
    pool.close()
    pool.join()
    return sorted_list
{% endhighlight %}

## Accuracy

The algorithm relies heavily on the machine itself. Let's see if it **always** works properly.

{% highlight python %}
attempts = 100
inacc = 0

for i in range(attempts):
    sl = sleep_sort([5, 1, 3, 2, 4])
    if sl != sorted(sl):
        inacc += 1

print('Sleepsort is inaccurate {} times out of {} attempts.'.format(inacc, attempts))
{% endhighlight %}

And unfortunately...

{% highlight text %}
Out[ ]: Sleepsort is inaccurate 7 times out of 100 attempts.
{% endhighlight %}

Let's make some plots to see how accuracy shifts with array length and sleep intervals.

![chart](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-12-14-interval.png){: .center-image}

![chart](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-12-14-arraylength.png){: .center-image}

We can see the accuracy skyrockets as intervals become larger or array size decreases. The results support the logic that `multiprocessing.dummy` simply creates **dummy** threads instead of opening **real** CPU threads. 

If we want to sort a lengthy array while giving a trivial sleeping time, those elements at the top of thread assignment queue will wake up from sleeping even before the others getting throught the queue.

![chart](https://raw.githubusercontent.com/Jiaxigu/Jiaxigu.github.io/master/assets/images/2017-12-14-order.png){: .center-image}

Surprisingly, the order of the given array has trivial impact on accuracy. It implies the threads were **not** allocated in the order of array index. (If so, an already sorted array will result in 100% accuracy no matter what.)

## Reference

- [On sleepsort and other spooky sorting algorithms](https://zhuanlan.zhihu.com/p/20644113)
- [On multiprocessing and multithreading](https://mozillazg.github.io/2014/01/python-use-multiprocessing-dummy-run-theading-task.html)

