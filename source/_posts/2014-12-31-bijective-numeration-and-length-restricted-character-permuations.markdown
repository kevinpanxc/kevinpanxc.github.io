---
layout: post
title: "Bijective numeration and length-restricted character permutations"
date: 2014-12-31 21:37:41 -0500
comments: true
categories: 
---

Based on this awesome StackOverflow [answer](http://stackoverflow.com/a/15625853)...

Consider the problem where you have an array of length $N$ filled with unique characters and you need to generate all the unique string combinations with these characters that have length less than or equal to $M$.

In the character array, each character is mapped to an integer index starting from 0 all the way to $N - 1$. Now consider a base-$N$ numeral system. In this system, there are $N$ symbols to represent the $N$ numbers below 10. We also know that in this system, $N^M - 1$ will evaluate to the largest number that has a string representation of length $M$. So to generate all the string combinations with length less than or equal to $M$, we could just enumerate from 0 to $N^M - 1$ in base $N$ and for each base-$N$ integer we reach, we can create a string combination by replacing each digit of the integer with the character in the array with the same index.

For example, if the character array was `{'a', 'b', 'c', '3'}` and a base-4 number was 312, the equivalent string is `3bc`.

However, there is a slight problem with this. The string combinations generated are missing strings with multiple leading characters that is the first character in the array (AKA the character mapped to the 0 index). This is because when you're iterating from 0 to $N^M - 1$, you would never reach $0000$, for example, since this is just 0.

This is where a bijective numeration becomes helpful. A bijective numeration, according to [Wikipedia](http://en.wikipedia.org/wiki/Bijective_numeration) is: "any numeral system in which every non-negative integer can be represented in exactly one way using a finite string of digits".

Conventional numeral systems often utilize the integer concept of 0 and thus is not bijective since you can have multiple ways to represent an integer just by tacking 0's in front of it. For example, 10 can be represented in an infinite number of ways: 0..010. A bijective base-k numeration system ($k$-adic notation) circumvents this by getting rid of the symbol 0 altogether. The value 0 is represented with the empty string, $\varepsilon$. A digit-set of {1, 2, ..., k} is used to represent the rest of the positive integers. So, for example, 11110 in base-8 would be represented as 8888 in $k$-adic notation.

## From base-10 to $k$-adic notation

So, given the understanding of $k$-adic notation, the problem can be solved with a character array where index 0 is mapped to nothing and indices start at 1. We then iterate from 1 to $N^M$ and convert each integer from base-10 to the $k$-adic notation equivalent before generating the particular string combination ($N^M + N^{M - 1} + \ldots + N$ is the largest number that has a string representation of length $M$ in $N$-adic). The only question left is how do we convert from base-10 to $k$-adic.

The algorithm is as complex as the algorithm that converts base-10 to base-$N$. Suppose $x$ is the decimal number we need to convert from and the $k$-adic equivalent is represented by the digit-string $a_n a_{n_1} \ldots a_1 a_0$, then:

$$\begin{align}
a_0 & = x - q_0 k, q_0 = \lceil \frac{x}{k} \rceil - 1 \\
a_1 & = q_0 - q_1 k, q_1 = \lceil \frac{q_0}{k} \rceil - 1 \\
& \ldots \\
a_n & = q_{n-1} - 0k, q_n = \lceil \frac{q_{n-1}}{k} \rceil - 1 = 0
\end{align}$$