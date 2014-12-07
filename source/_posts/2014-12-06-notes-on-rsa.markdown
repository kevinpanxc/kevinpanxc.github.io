---
layout: post
title: "Notes on RSA"
date: 2014-12-06 16:10:31 -0800
comments: true
categories: 
---

I first learnt about RSA cryptography during an introductory number theory course in my second study term in university. Like many things you learn in class, the details tend to quickly be forgotten once the exam's over. RSA was no exception. However, I've always been strangely annoyed for not fully understanding how this cryptosystem works and so I intend for this post to act as a "reference sheet" for RSA.

In RSA cryptography, there are two keys: a public key and a private key. The sender of a message uses the public key to encrypt the message and the idea is that you can only decrypt the message if you have the private key associated with that public key.

# Generating keys

1. Select two **large** distinct primes $p$ and $q$. Let $n = pq$
2. Select $e \in \mathbb{Z}$ such that $GCD(e, \phi(n)) = 1$ and $1 \lt e \lt \phi(n)$, where $GCD$ is the greatest common denominator function
3. Solve congruence $ed \equiv 1 (\textrm{mod}\ \phi(n))$ for $d$ where $d \in \mathbb{Z}, 1 < d < \phi(n)$

The public key will be the tuple $(e, n)$ and the private key will be the tuple $(d, n)$. Note, $\phi(n)$ here refers to [Euler's totient function](http://en.wikipedia.org/wiki/Euler%27s_totient_function), which is the number of positive integers less than or equal to n that are coprime with n. It also has the property that if two integers, $m$ and $n$ are coprime, then $\phi(mn) = \phi(m)\phi(n)$. Using this property and the fact that $p$ and $q$ are coprimes (since they are both primes), we note that $\phi(n) = \phi(pq) = \phi(p)\phi(q) = (p - 1)(q - 1)$.

# Encrypting and decrypting messages

Let $M$ be the message we want to encrypt. $M$ is an integer such that $0 \leq M \lt n$. The encrypted message will be $C$ where $C \equiv M^{e} (\textrm{mod}\ n), 0 \leq C \lt n \$.

To decrypt the message, the receiver needs to compute integer $R$ where $R \equiv C^{d} (\textrm{mod}\ n), 0 \leq R \lt n$. It is actually the case that $R \equiv M (\textrm{mod}\ n)$ and since $0 \leq R \lt n$, $R = M$.

Overall, we need to prove that $(M^{e})^{d} \equiv M (\textrm{mod}\ n), \forall M \in \mathbb{Z}$.

#Proof

(This proof is really just my version of [this](http://crypto.stackexchange.com/questions/2884/rsa-proof-of-correctness))

We can break the proof into two cases, $GCD(M, n) = 1$ and $GCD(M, n) \neq 1$.

---

**Case 1 ($GCD(M, n) = 1$):**

We start off by using Euler's totient theorem which states that:

> If $GCD(A, B) = 1$ then $A^{\phi(B)} \equiv 1 (\textrm{mod}\ B)$. In our case, since $GCD(M, n) = 1$, then $M^{\phi(n)} \equiv 1 (\textrm{mod}\ n)$.

Since $ed \equiv 1 (\textrm{mod}\ \phi(n))$, then let $ed = 1 + k\phi(n)$ where $k \in \mathbb{Z}$. From this, we get that: 

$$(M^{e})^{d} = M^{ed} = M^{1 + k\phi(n)} = M(M^{k\phi(n)})$$

Using the above fact:

$$\begin{align}
M^{\phi(n)} & \equiv 1 (\textrm{mod}\ n) \\
M^{k\phi(n)} & \equiv 1^{k} (\textrm{mod}\ n) \\
M^{k\phi(n)} & \equiv 1 (\textrm{mod}\ n) \\
M(M^{k\phi(n)}) & \equiv M (\textrm{mod}\ n) \\
M^{ed} & \equiv M (\textrm{mod}\ n)
\end{align}$$

---

**Case 2 ($GCD(M, n) \neq 1$):**

We will use the Chinese remainder theorem to prove this. 

> A more specific version of the theorem states that:

> $$\begin{align}
> x & \equiv y (\textrm{mod}\ A_1) \\
> & ... \\
> x & \equiv y (\textrm{mod}\ A_k) \\
> \\
> \text{if and only if...} \\
> \\
> x & \equiv y (\textrm{mod}\ A)
> \end{align}$$

> where $A_1, ..., A_k$ are **pairwise** coprime and $A = A_1...A_k$.

By the Chinese remainder theorem, we only need to prove the two congruences below to prove $(M^{e})^{d} \equiv M (\textrm{mod}\ n)$: 

$$\begin{align}
(M^{e})^{d} \equiv M (\textrm{mod}\ p)\tag{1} \\
(M^{e})^{d} \equiv M (\textrm{mod}\ q)\tag{2}
\end{align}$$

Since $GCD(M, n) \neq 1$, it must be true that either $GCD(M, n) = p$ or $GCD(M, n) = q$.

Without loss of generality, let's assume that $GCD(M, n) = p$. Since $M$ is a multiple of $p$, let $M = kp, k \in \mathbb{Z}, k \gt 0$.

To prove (1):

$$\begin{align}
M & \equiv kp (\textrm{mod}\ p) \\
M & \equiv 0 (\textrm{mod}\ p) \\
\\
\text{Using the above fact...} \\
\\
(M^{e})^{d} & \equiv M (\textrm{mod}\ p) \\
((kp)^{e})^{d} & \equiv M (\textrm{mod}\ p) \\
k^{ed}p^{ed} & \equiv 0 (\textrm{mod}\ p) \\
0 & \equiv 0 (\textrm{mod}\ p)
\end{align}$$

For (2), we note that since $M = kp$, if $q$ divides $M$, then $q$ must divide either $k$ or $p$. $q$ cannot divide $p$ since they are both primes and $q$ cannot divide $k$. If $q$ does divide $k$, then it implies $M \geq n$ which $M$ cannot be. We reach the conclusion that $GCD(M, q) = 1$. By Euler's totient theorem, $M^{\phi(q)} = M^{q - 1} \equiv 1 (\textrm{mod}\ q)$.

$$\begin{align}
(M^{e})^{d} & = M^{ed} \\
& = M^{ed - 1}M \\
& = M^{k\phi(n)}M\tag{from before} \\
& = (M^{q - 1})^{k(p - 1)}M \\
& \equiv 1^{k(p - 1)}M (\textrm{mod}\ q) \\
& \equiv M (\textrm{mod}\ q)\tag*{$\square$}
\end{align}$$

# Why is RSA secure?

The answer to this question lies in the fact that it is nearly impossible (by our current computing capabilities) to determine the private key from its corresponding public key, especially if the primes chosen are sufficiently large.

If the public key was $(e, n)$, then remember that to determine the private key you need to solve the congruence $ed \equiv 1 (\textrm{mod}\ \phi(n)), 1 \lt d \lt \phi(n)\ \text{and}\ \phi(n) = (p - 1)(q - 1)$. The best way to do so, without actually knowing what $\phi(n)$ is, is to prime factorize $n$ into its only two prime factors. This, as it turns out, is a very very hard thing to do. There [doesn't exist an algorithm](http://en.wikipedia.org/wiki/Integer_factorization) (with the exception of quantum algorithms) that can solve the problem in polynomial time.

> I made a movie about cosmic cookware. It was universally panned.