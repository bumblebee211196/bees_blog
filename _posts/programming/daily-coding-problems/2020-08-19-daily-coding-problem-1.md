---
title: Daily Coding Problem 1
date: 2020-08-19 11:00:00 +05:30
tags: [programming, daily-coding-problems]
---

## Problem

Given a list of numbers and a number k, return whether any two numbers from the list add up to k.

For example, given [10, 15, 3, 7] and k of 17, return true since 10 + 7 is 17.

**Bonus**: Can you do this in one pass?

***

The most straightforward approach that comes to our mind is to run two loops and check if the elements add up to the given target. If you arrived at the same solution at first glance, it’s okay!. That is the answer.

## Solution 1: Using two loops

**Python:**
{% highlight python %}
from typing import List


def solution(arr: List[int], k: int) -> bool:
    n = len(arr)
    for i in range(n):
        diff = abs(k - arr[i])
        for j in range(i+1, n):
            if i != j and arr[j] == diff:
                return True
    return False
{% endhighlight %}
**Go:**
{% highlight go %}
func Solution(arr []int, k int) bool {
   n := len(arr)
   for i := 0; i < n; i++ {
      diff := abs(k - arr[i])
      for j := i+1; j < n; j++ {
         if i != j && arr[j] == diff {
            return true
         }
      }
   }
   return false
}

func abs(v int) int {
   if v < 0 {
      return -v
   }
   return v
}
{% endhighlight %}

**Time Complexity:** O(n²)

**Space Complexity:** O(1)

If we observe closely, we are checking if the difference between the value k and the element exists in the array or not. Doing so makes us iterate the array once again hence our time complexity becomes O(n²). What if we can check if the element exists in the array in O(1) time. Yes, we can! using a **hash-table**. Every time we compute the difference, we check if the difference is present in the hash-table. If the difference is present, we can return True and end the computation. If not, we have not come across such element and add the current element to the hash-table. At the end we will return False if no elements match the target sum **k**.

## Solution 2: Using hash-table

**Python:**
{% highlight python %}
def solution1(arr: List[int], k: int) -> bool:
    hash = {}
    for v in arr:
        diff = abs(k - v)
        if diff in hash:
            return True
        hash[v] = True
    return False
{% endhighlight %}
**Go:**
{% highlight go %}
func Solution(arr []int, k int) bool {
	hash := make(map[int]bool)
	for _, v := range arr {
		diff := abs(k - v)
		if _, ok := hash[diff]; ok {
			return true
		}
		hash[v] = true
	}
	return false
}
{% endhighlight %}
**Time Complexity:** O(n)

**Space Complexity:** O(n)

I hope you guys enjoyed this post. If you find it helpful, please share and claps are much appreciated. Feel to ask your queries in the comments section!.