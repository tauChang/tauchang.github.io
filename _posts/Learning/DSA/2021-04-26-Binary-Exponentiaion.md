---
title:  "Binary Exponentiation and its Applications"
date:   2021-04-26 17:50:00 +0800
categories: Learning DSA
---
Great thanks to [this post from cp-algorithms](https://cp-algorithms.com/algebra/binary-exp.html).

Binary exponentiation is a technique that is applicable to any operation $$F$$ that possesses the property of **associativity**, i.e. $$F(F(a,b), c) = F(a, F(b,c))$$.

Consider the problem of computing $$a^n$$. Suppose the multiplication operation takes constant time. Naive approach would result in $$O(n)$$ time complexity. To improve this, we observe that any integer $$n$$ could be decomposed into sum of product of two. For example, 

$$25 = 16 + 8 + 1 = 2^4 + 2^3 + 2^0$$

Writing it down in binary number would be more obvious 

$$25 = 11001_{2}$$

Using this observation, we could obtain $$a^n$$ in $$O(\log n)$$ time.

<!-- $$ 
a^n = 
\begin{cases}
1, & \text{if $n=0$}\\
(a^{\lfloor \frac{n}{2} \rfloor})^2,  & \text{if $n$ is even} \\
(a^{\rfloor \frac{n}{2} \rfloor})^2 \times a, & \text{if $n$ is odd}
\end{cases}
$$ -->

{% highlight cpp %}
typedef long long ll;

ll power(ll a, ll n){
    ll res = 1;

    while(n){
        if(n & 1) res = res * a;
        a = a * a;
        n >>= 1;
    }

    return res;
}
{% endhighlight %}

Now we look at some other applications using the binary exponentiation technique.

## Compute $$a^n \text{ mod } k$$
We have the multiplicative property of modular arithmetic:

$$ (A \times B) \text{ mod } k = [(A \text{ mod } k) \times (B \text{ mod } k)] \text{ mod } k$$

To compute $$a^n \text{ mod } k$$, we directly apply binary exponentiation. This results in $$O(\log k)$$ time complexity.

{% highlight cpp %}
typedef long long ll;

ll power_mod(ll a, ll n, ll k){
    ll res = 1;

    while(n){
        if(n & 1) res = (res * a) % k;
        a = (a * a) % k;
        n >>= 1;
    }

    return res;
}
{% endhighlight %}

## Compute Fibonacci Numbers
The main observation is that,

$$
\begin{bmatrix}1 & 1\\1 & 0\end{bmatrix} \begin{bmatrix}F_{i-1} \\ F_{i-2}\end{bmatrix} = \begin{bmatrix}F_{i} \\ F_{i-1}\end{bmatrix}
$$

Let $$A = \begin{bmatrix}1 & 1\\1 & 0\end{bmatrix}$$, and $$x=\begin{bmatrix}1 \\ 0\end{bmatrix}$$. $$F_{n}$$ could be obtained by computing $$ A^{n-1}x $$. This results in $$O(\log n)$$ time complexity.

{% highlight cpp %}
void mmult(int A[2][2], int B[2][2], int res[2][2]){
    int a = A[0][0]*B[0][0] + A[0][1]*B[1][0];
    int b = A[0][0]*B[0][1] + A[0][1]*B[1][1];
    int c = A[1][0]*B[0][0] + A[1][1]*B[1][0];
    int d = A[1][0]*B[0][1] + A[1][1]*B[1][1];

    res[0][0] = a; res[0][1] = b; res[1][0] = c; res[1][1] = d;
}

void pow(int A[2][2], int n, int res[2][2]){
    res[0][0] = res[1][1] = 1; 
    res[1][0] = res[0][1] = 0;

    while(n){
        if(n & 1){
            mmult(A, res, res);
        }
        mmult(A, A, A);
        n >>= 1;
    }
}

int fib(int n){
    if(n < 2) return 1;

    int x[2][2] = { {1, 0}, {0, 0} };
    int A[2][2] = { {1, 1}, {1, 0} };
    int B[2][2];

    pow(A, n-1, B);
    mmult(B, x, x);

    return x[0][0];
}

{% endhighlight %}

## Number of Paths of Length $$k$$
This is one of my favorite examples, even though it is just matrix multiplication in essence.

Given an unweighted directed graph $$G = (E, V)$$, a source vertex $$s$$, and a destination vertex $$t$$, we want to know how many length-$$k$$ paths there are from $$s$$ to $$t$$.

$$E$$ could be represented as an adjacency matrix $$A$$, where $$A_{ij} = 1$$ if there is an edge from vertex $$v_{i}$$ to vertex $$v_{j}$$, and $$0$$ otherwise. As a result, there is a length-$$k$$ path from $$v_{i}$$ to $$v_{j}$$ if and only if $$A^{k}_{ij} > 0$$. This is where binary exponentiation comes into play. The time complexity is thus $$ O(\|V\|^3\log k) $$. Since this is simply matrix multiplication, the code is omitted.

Another problem is to finding the shortest path of length-$$k$$. Interestingly, binary exponentiation could also be used to solve this. See [this article from cp-algorithm](https://www.geeksforgeeks.org/find-the-number-of-paths-of-length-k-in-a-directed-graph/).

## $$AB \text{ mod } k$$ with Large $$A$$ and $$B$$
This is an example of binary exponentiation on addition instead of multiplication, which is also very interesting.

Suppose $$A$$ and $$B$$ are large integers such that the product $$AB$$ could not be stored in any data structures in C++. We want to compute $$AB \text{ mod } k$$.

Back to the basics! We know that $$AB$$ means adding $$A$$ for $$B$$ times. Using binary exponentiation on $$B$$ and the multiplicative property of modular arithmetic, we can obtain $$AB \text{ mod } k$$ in $$O(\log B)$$ time.

{% highlight cpp %}
ll product_mod(ll A, ll B, ll k){
    ll res = 0;

    while(B){
        if(B & 1){
            res = (res + A) % k;
        }

        A = (A << 1) % k; \\ Assuming that A << 1 could be stored in ll
        B >>= 1;
    }

    return res;
}
{% endhighlight %}


## Final Thoughts
Binary representations are really powerful. It also empowers data structures such as binary-indexed tree. More to learn!

## References
<https://cp-algorithms.com/algebra/binary-exp.html>

<https://www.geeksforgeeks.org/program-for-nth-fibonacci-number/>

<https://tutorialspoint.dev/algorithm/mathematical-algorithms/multiply-large-integers-under-large-modulo>
