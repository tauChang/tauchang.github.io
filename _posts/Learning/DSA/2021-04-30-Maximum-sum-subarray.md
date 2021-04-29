---
title:  "Maximum Sum Subarray"
date:   2021-04-30 01:47:00 +0800
categories: Learning DSA
---

Inspired by [Codeforces 578C. Weakness and Poorness](https://codeforces.com/contest/578/problem/C), I took a moment to review the classical maximum subarray problem. Let's take a look at two linear time DP algorithms.

## Kadane's Algorithm
The well-known algorithm to the maximum subarray problem. Beautiful and concise.

{% highlight cpp %}
int n;
int arr[1000];

int kadane(){
    int max_so_far = 0, max_end_here = 0;
    
    for(int i = 0; i < n; ++i){
        max_end_here = max(max_end_here + arr[i], 0);
        max_so_far = max(max_so_far, max_end_here);
    }

    return max_so_far;
}
{% endhighlight %}

## Prefix Sum
As we pass through the array, we record prefix sum `pre[0..i]`. We also maintain another variable `minpre` that denotes the minimum in `pre[0..i]`. We update `ans` to see if we could obtain a larger sum by summing up a subarray ending with `arr[i]`. To improve space complexity, we observe that we need not maintain the whole `pre[0..i]`, but just the prefix sum until `arr[i]` in each iteration. So instead of `pre[0..i]`, we just need a variable `pre`.

{% highlight cpp %}
int n;
int arr[1000];

int prefix_sum(){
    int pre = 0, minpre = 0, ans = 0;
    
    for(int i = 0; i < n; ++i){
        pre += arr[i];
        ans = max(ans, pre - minpre);
        minpre = min(minpre, pre);
    }

    return ans;
}
{% endhighlight %}

## Maximum Absolute Value of Sum of Subarray
This is the case encountered in [Codeforces 578C. Weakness and Poorness](https://codeforces.com/contest/578/problem/C). 

In fact, we only need to compute the maximum subarray and the minimum subarray, and compare their absolute value. So Kadane's algorithm could be easily modified (maintain `max_so_far`, `max_end_here`, `min_so_far`, `min_end_here`).

But in my opinion, using prefix sum is easier in this case. The maximum absolute value of sum of subarray is simply the difference between the maximum value and the minimum value in `pre[0..n-1]`. Wow!

{% highlight cpp %}
int n;
int arr[1000];

int prefix_sum_abs(){
    int pre = 0, mx = 0, mn = 0;
    
    for(int i = 0; i < n; ++i){
        pre += arr[i];
        mx = max(mx, pre);
        mn = min(mn, pre);
    }

    return mx - mn;
}
{% endhighlight %}

## Some Thoughts
There are so many variations of the maximum subarray problem. I should take a look at [this link](http://web.ntnu.edu.tw/~algo/MaximumSubarray.html#4).

## References
<https://www.tutorialspoint.com/maximum-subarray-sum-in-o-n-using-prefix-sum-in-cplusplus>