---
layout: post
title: "Notes on Binary Indexed Trees (Fenwick Trees)"
date: 2014-12-27 16:52:31 -0500
comments: true
categories: 
description: A brief overview of what a Binary Indexed Tree, or Fenwick Tree is. Quickly and intuitively learn how this data structure works and how it's implemented
---

A binary indexed tree came up in one of the Hackerrank challenges I was working on recently. This is the first time I've ever heard of this data structure and I was interested in learning more. I found this TopCoder [article](http://community.topcoder.com/tc?module=Static&d1=tutorials&d2=binaryIndexedTrees) on binary indexed trees a particularly useful resource and below is a concise summary of what I've learnt.

Suppose we have a size-$n$ array of values. If we wanted to find the summation of all values between two indices, we would need to iterate through the entire interval while maintaining a summation variable. If we then wanted to make $m$ such queries, the time complexity of the whole operation would be $O(mn)$. A binary indexed tree allows us to do this in $O(m\log\ n)$ time. The one trade off is that it takes longer to construct the initial data structure ($O(n \log\ n)$ time instead of $O(n)$ time, which is if we had used an array).

Internally, we will still be using a size-$n$ array to represent the tree. The **main idea** of a binary indexed tree is that indices that are a power of 2 will have the cumulative frequency of all indices before, including itself. Indices that are half way between two indices that are a power of 2 will have the cumulative frequency of all indices before it starting from, but not including, the lower power of 2 index. This pattern continues down a hierarchical structure until you reach an odd index which will only have the frequency of itself. This can be visualized with the following diagram from the aforementioned article:

<img align="right" src="http://community.topcoder.com/i/education/binaryIndexedTrees/bitval.gif" alt="Binary indexed tree visualization">

Another way to put this is that each index $i$ in a binary indexed tree stores the sum of all frequencies of the indices in the range $(i - 2^r, i]$ where $r$ represents the rightmost set bit of $i$. From this definition, it is easier to tell that an index that is a power of 2 will have the sum of all frequencies of the indices before it while an odd index will just contain the frequency of itself.

**Note:** for a binary indexed tree, internally, the starting index is $1$ and not $0$

## Isolating the rightmost set bit of a number

Given the structure of the binary indexed tree, many of its operations will often need to find and isolate the rightmost set bit of a number. As such, we need to be able to do this efficiently.

Any nonzero binary number can be represented in the form $(a)(1)(0..0)$, where $a$ is the rest of the binary number to the left of the rightmost set bit. To isolate the rightmost set bit of one such number (call it $x$), we need to use its two's complement representation.

To get the two's complement representation of a number, all we need to do is invert every bit and add $1$ to the result. So if $x = (a)(1)(0..0)$, then the two's complement representation of $x$ will be $(a^{-})(1)(0..0)$. Bitwise ANDing the two binary numbers together will then result in $(0..0)(1)(0..0)$. This is exactly what we want!

Most computers in the world represent negative integers using two's complement so all we need to do in code is `x & -x`.

## Finding the cumulative frequency for a particular index

Suppose we want to find the cumulative frequency from index $1$ to a particular index $x$. We know that at index $x$, the tree stores the cumulative frequency of the indices in the interval $(x - 2^r, x]$.

So to find the cumulative frequency of the indices in $[1, x]$, we first add `binary_index_tree[x]` to a summation variable and subtract the rightmost set bit of $x$ from itself. Now if we query for `binary_index_tree[x]` again, we get the cumulative frequency of all indices between $(x - 2^r - 2^{r_1}, x - 2^r]$. When we add the new `binary_index_tree[x]` to the summation variable, we get the cumulative frequencies of indices between $(x - 2^r - 2^{r_1}, x]$. We keep repeating this process until the left endpoint of the interval becomes $0$, which is when we have our answer.

```java
int read(int x) {
    int sum = 0;
    while (i > 0) {
        sum += binary_index_tree[i];
        i -= (i & -i);
    }
    return sum;
} 
```

Since the max index you can query for is $n$ (which is the total size of the data set) and it can have a maximum of $\log\ n$ set bits, the `read` function's time complexity is $O(\log\ n)$.

## Changing frequency of a particular index

To change a frequency at a particular index $x$, we also need to change the values of all indices that contain it. All such indices will be greater than $x$.

```java
void update (int x, int val) {
    while (idx <= n) {
        binary_index_tree[idx] += val;
        idx += (idx & -idx);
    }
}
```

Since we are iterating to $n$ and we're adding the rightmost set bit of the current index to itself at each iteration, the `update` function's time complexity is also $O(\log\ n)$.

## Other operations

I've only written about the two most basic operations you can do with a binary indexed tree. If you would like to learn more, the TopCoder article I mentioned above provides indepth explanations into many others.