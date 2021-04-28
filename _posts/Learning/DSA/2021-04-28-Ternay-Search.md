---
title:  "Ternary Search"
date:   2021-04-29 00:50:00 +0800
categories: Learning DSA
---
Great thanks to [this post from cp-algorithms](https://cp-algorithms.com/num_methods/ternary_search.html).

Given an [unimodal function](https://en.wikipedia.org/wiki/Unimodality) $$f(x)$$ defined over an interval $$[l, r]$$, we want to find its maximum or minimum. Here we assume finding its maximum, i.e. its peak.

We use ternary search, which is similar to binary search, but we look at two points $$ml$$ and $$mr$$ instead of just one point, where $$l \leq ml \leq mr \leq r$$. 

![](/img/ternary_search.png){:class="img-responsive"}

If $$f(ml) < f(mr)$$, then we know the peak would not be in $$[l, ml)$$, so we inspect $$[ml, r]$$ in the next iteration. Similarly, if $$f(ml) > f(mr)$$, we inspect $$[l, mr]$$ in the next iteration. If $$f(ml) = f(mr)$$, the peak is in $$[ml, mr]$$, but to simplify code, we inspect either $$[ml, r]$$ or $$[l, mr]$$ in the next iteration.

The time complexity could be expressed as

$$
T(n) = T(\frac{2n}{3}) + O(1)
$$

which is $$O(\log n)$$.

The code is quite self explanatory.

{% highlight cpp %}
double f(double x){
    return 3-(x-2)*(x-2);
}

double ternary_search(double l, double r){
    while(r - l > 1e-6){
        double ml = l+(r-l)/3, mr = r-(r-l)/3;
        if(f(mr) > f(ml)){
            l = ml;
        } else{
            r = mr;
        }
    }
    return f(l);
}
{% endhighlight %}

Now take a look at [Codeforces 578C. Weakness and Poorness](https://codeforces.com/contest/578/problem/C). Really interesting. [Here](https://yuihuang.com/cf-578c/) is an wonderful explanation with extremely helpful visualization. The solution is $$O(n \log n)$$ I hope to write my own explanation of this problem later.

## References
<https://cp-algorithms.com/num_methods/ternary_search.html>\\
<https://yuihuang.com/ternary-search/>\\
<https://yuihuang.com/cf-578c/>