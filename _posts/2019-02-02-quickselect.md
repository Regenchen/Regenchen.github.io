---
layout: post
title: The Quickselect Algorithm
tags: Programming
---

The last three months have been really hasty but I'm fortunate enough to make past the first three months at Alibaba Cloud. Before the lunar year 2018 concludes I would like to discuss a bit about a powerful algorithm recently used in my work - the `Quickselect` algorithm.


## Randomization at its best

Given an unsorted, numeric array, `Quickselect` finds the k-th largest or smallest element in the array. This algorithm has an average time complexity at `O(n)` to find, let's say, the median of an array. Using an exhaustive approach, it takes at least `O(nlogn)`. `Quickselect` is, however, a randomized approach and the most efficient median-finder available. The worst case time complexity is `O(n^2)` though.

## Description

Following are the steps of `Quickselect` to find the k-th largest or smallest element:

- (Randomly) choose an element from the array as `pivot`.
- Partition the array into 2 parts: left subarray contains elements smaller than or equal to the `pivot`; right subarray contains elements larger than the `pivot`.
- Form a new array: `[left subarray + pivot + right subarray]`
- Compare `k` with `len(left subarray)`:
	- `k == len + 1`: pivot is the target.
	- `k <= len`: repeat step 1-3 on left subarray
	- `k > len + 1`: repeat step 1-3 on right subarray with `k = k - len`.

## Sample code

{% highlight python %}
import random

def quickselect(a, k):
  """
  :param a: unsorted array
  :param k: k-th element to find
  """
  (left, pivot, right) = partition(a)
  if len(left) == k - 1:
    result = pivot
  elif len(left) >= k:
    result = quickselect(left, k)
  else:
    result = quickselect(right, k - 1 -len(left))
  return result

def partition(a):
  """
  :param a: array or subarray to partition
  """
  s = len(a)
  # base cases
  if s==1:
    return([], a[0], [])
  if s==2:
    if a[0] < a[1]:
      return([], a[0], a[1])
    else:
      return([], a[1], a[0])
  # choose pivot
  p = random.randint(0, len(a) - 1)
  pivot = a[p]
  left = []
  right = []
  for i in range(len(a)):
    if not i == p:
      if a[i] > pivot:
        right.append(a[i])
      else:
       left.append(a[i])
  return (left, pivot, right)
{% endhighlight %}

## Improvements

As mentioned above, the performance of `quicksort` is good in average but sensitive to the chosen pivot. the worst case time complexity of `Quickselect` is `O(n^2)`. It leaves room for caveat in a robust system. However, with some techniques, the worst case time complexity can be improved to `O(n)`. Refer to [median of medians](https://en.wikipedia.org/wiki/Median_of_medians), [Introselect](https://en.wikipedia.org/wiki/Introselect) for more information.


## References

- [Introduction to quickselect](https://www.jianshu.com/p/52f90fe2b141)
- [Implementation of quickselect](https://github.com/azabet/Python/blob/master/QuickSelect.py)

