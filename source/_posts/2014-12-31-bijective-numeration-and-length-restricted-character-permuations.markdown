---
layout: post
title: "Bijective numeration and length-restricted character permutations"
date: 2014-12-31 21:37:41 -0500
comments: true
categories: 
description: This post explores how to find the total number of character permutations that have length less than or equal to an integer given a set of characters using bijective numeration.
---

Based on this awesome StackOverflow [answer](http://stackoverflow.com/a/15625853)...

Consider the problem where you have a size $N$ set of characters and you need to the generate all possible strings from this set that have length less than or equal to $M$.

Let's represent the set of characters programmatically as a character array. In this array, each character is mapped to an integer index starting from 0 all the way to $N - 1$. Now consider a base-$N$ numeral system. In this system, there are $N$ symbols to represent the $N$ numbers below 10. We also know that in this system, $N^M - 1$ will evaluate to the largest number that has a string representation of length $M$. So to generate all strings with length less than or equal to $M$, we could just enumerate from 0 to $N^M - 1$ in base $N$ and for each base-$N$ integer, we can create a valid string by replacing each digit of the integer with the character that has the same array index.

For example, if the character array was `{'a', 'b', 'c', '3'}` and a base-4 number was 312, the equivalent string is `3bc`.

However, there is a slight problem with this. The strings generated are missing strings with multiple leading characters that is the first character in the array (AKA the character mapped to the 0 index). This is because when you're iterating from 0 to $N^M - 1$, you would never reach $0000$, for example, since this is just 0. From the above example, that means that you're missing the string `aaaa`, even though it is a perfectly valid string if 4 is less than or equal to $M$.

This is where a bijective numeration becomes helpful. A bijective numeration, according to [Wikipedia](http://en.wikipedia.org/wiki/Bijective_numeration) is: "any numeral system in which every non-negative integer can be represented in exactly one way using a finite string of digits".

Conventional numeral systems often have multiple ways to represent the same integer and thus are not bijective. For example, in the numeral system that we're all familiar with, the Hindu-Arabic numeral system, you can represent the integer 1 an infinite number of ways just by tacking any number of 0's in front of it. A bijective base-k numeration system ($k$-adic notation) circumvents this by getting rid of the symbol 0 altogether. The value 0 is represented with the empty string, $\varepsilon$. A digit-set of {1, 2, ..., k} is used to represent the rest of the positive integers. So, for example, 11110 in base-8 would be represented as 8888 in $k$-adic notation.

## From base-10 to $k$-adic notation

So, given the understanding of $k$-adic notation, the problem can be solved with a character array where index 0 is mapped to nothing and indices start at 1. We then iterate from 1 to $N^M + N^{M - 1} + \ldots + N$ and convert each integer from base-10 to the $k$-adic notation equivalent before generating the particular string ($N^M + N^{M - 1} + \ldots + N$ is the largest number that has a string representation of length $M$ in $N$-adic). The only question left is how do we convert from base-10 to $k$-adic.

The algorithm is as complex as the algorithm that converts base-10 to base-$N$. Suppose $x$ is the base-10 number we need to convert from and the $k$-adic equivalent is represented by the digit-string $a_n a_{n_1} \ldots a_1 a_0$, then:

$$\begin{align}
a_0 & = x - q_0 k, q_0 = \lceil \frac{x}{k} \rceil - 1 \\
a_1 & = q_0 - q_1 k, q_1 = \lceil \frac{q_0}{k} \rceil - 1 \\
& \ldots \\
a_n & = q_{n-1} - 0k, q_n = \lceil \frac{q_{n-1}}{k} \rceil - 1 = 0
\end{align}$$