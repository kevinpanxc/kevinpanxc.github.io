---
layout: post
title: "Today I used Fermat's little theorem outside of class - Hackerrank Challenge: Game of Thrones II"
date: 2014-11-28 02:05:51 -0800
comments: true
categories: 
---

This challenge asks you to find the number of possible palindrome anagrams in a word $\bmod 10^9 + 7$.

To answer this question, we need to know two facts:

* We know that a set of characters can be a palindrome if there exists a complete bipartite graph where each vertex represents a character in the set and an edge exists between vertices if two characters are the same (the exception is for a string with an odd number of characters. In this case, it must be true that there exists one character which you can remove from the set so that a complete bipartite graph can be constructed from the rest of the characters in the set).

**TLDR:** A palindrome anagram of a string exists if: (1) for strings of even length, every unique character in the string can only occur an even number of times (2) for strings of odd length, there must only be one unique character that occurs in the string an odd number of times

* We also note that we only need to know the first half of a palindromic string to know the entire string.

With these two facts in mind, the answer to the challenge will *almost* be as simple as finding the number of unique permutations of all characters in one of the bipartitions of the aforementioned graph. However, since the question wants the answer to be returned $\bmod 10^9 + 7$ (perfectly reasonable as the actual number can grow insanely large), we will need to use modular arithmetic.

## Calculating the number of unique permutations of a string (without modular arithmetic)

There are two formulas you can use to do this (note $n$ represents string length and $n_i$ represents number of occurences for one unique character):

$$x = \frac{n!}{n_1!n_2!n_3!...n_n!}\tag{w/ factorials}$$

$$x = {n \choose n_1}{n - n_1 \choose n_2}{n - n_1 - n_2 \choose n_3}...\tag{w/ binomial coefficients}$$

Explanation for the factorial formula can be found [here](http://cs.stackexchange.com/a/2445). I can try to explain the latter formula briefly. For the first unique character in string, there are ${n \choose n_1}$ to place $n_1$ characters in $n$ spaces. For the next unique character in string, since $n_1$ places are already taken up, there are ${n - n_1 \choose n_2}$ ways to distribute the next $n_2$ characters, and so on.

For my algorithm, I chose to go with the factorial equation.

## Now apply modular arithmetic

There are two primary properties of modular arithmetic that is commonly used in programming. They are:

$$(a + b) \% m = ((a \% m) + (b \% m)) \% m\tag{addition}$$
$$(a * b) \% m = ((a \% m) * (b \% m)) \% m\tag{multiplication}$$

We will only need the multiplication property. Using this, we can define a modular multiplication method:
    
```java
static long M = ((long)Math.pow(10, 9)) + 7;

static long modularMultiplication(long a, long b) {
    return ((a % M) * (b % M)) % M;
}
```

We can also define a factorial method using modular arithmetic. For the purposes of the question, $x$ will always be less than $M$ and $factorial$ will always return values less than $M$. As such, we don't need to use modular multiplication here:

```java
static long factorial(long x){
    if(x==0||x==1)
        return 1;
    else return (x * factorial(x - 1)) % M; 
}
```

You might've noticed that I left out something crucial that's needed to solve this question: how do you perform modular division? * gasp * This is where Fermat's little theorem comes into play (yay more math!). The theorem states that:

$$a^{p - 1} \equiv 1 \bmod p, a \in \mathbb{R}\tag{where p is a prime number}$$

Did you also notice that $10^9 + 7$ is a prime number? This is the reason why we can apply the theorem to this question.

Starting from the theorem, we multiply both sides by $a^{-1}$ to get us:

$$\begin{aligned}
a^{p - 2} \equiv a^{-1} \bmod p\tag \\
a^{-1} \equiv a^{p - 2} \bmod p \\
\frac{1}{a} \equiv a^{p - 2} \bmod p
\end{aligned}$$

This means that all we need to do perform modular division is to find $a^{p - 2} \bmod p$. Then we can modular multiply the result with the dividend. The last final piece to the puzzle is how do we actually find that value quickly and efficiently? Well, fortunately, there is a modular exponentiation technique called binary exponentiation (or exponentiation by squaring) that can do this in logarithmic time (finds $a^n \bmod p$ in $\mathcal{O}(\log n)$).

Here's the implementation of the algorithm in Java:

```java
static long modularPow(long base, long exponent, long modulus) {
    long result = 1;
    base = base % modulus;
    while (exponent > 0) {
        if (exponent % 2 == 1) {
            result = (result * base) % modulus;
        }
        exponent = exponent >> 1;
        base = (base * base) % modulus;
    }
    return result;
}
```

Finally, here's the implementation of modular division:

```java
static long modularDivision(long a, long b) {
    return modularMultiplication(a, modularPow(b, M - 2, M));
}
```

Knowing all these, we can quickly find the result of $\frac{n!}{n_1!n_2!n_3!...n_n!} \bmod (10^9 + 7)$ and solve the problem.

> Mathematical puns are the first sine of madness