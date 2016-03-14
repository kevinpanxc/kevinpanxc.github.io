---
layout: post
title: "Hackerrank challenge: Sherlock and MiniMax"
date: 2014-11-25 01:07:46 -0800
comments: true
published: false
categories: 
description: Essentially, you sort A and find the overlap between elements in A and integers within [P, Q]. Next, you iterate through elements of A within the overlap and determine the distance of the halfway point between two adjacent elements.
---

I have spent my previous co-op workterms mainly working on numerous side projects such as my moderately successful [TwitchQuotes](http://www.twitchquotes.com) website, a multiplayer Flappy Bird game aptly named [Flappy BirdS](http://www.github.com/kevinpanxc/flappy-birds), and a [2D electric field simulator](http://http://kevinpan.ca/projects/electric-field-vectors/) built on the HTML5 canvas.

Working on side projects is fun but they can often be unstimulating. To remedy this, I have adopted the habit of solving one or two [Hackerrank](http://www.hackerrank.com) problems when I had time (for those who don't know what Hackerrank is, I highly recommend you to check it out). One of the most recent problems I've completed falls under the Arrays and Sorting category called **Sherlock and MiniMax**.

The problem itself is fairly straightforward: you are Watson, and Sherlock (for whatever reason) gives you an array $A$ of size $N$ and integers $P$ and $Q$ and he wants you to find the smallest integer $M$ between $P$ and $Q$ (both inclusive) such that the minimum distance between any array element in A ( $min\\{ \| A_i - M \| \mid 1 \leq i \leq N\\} $) is **maximised**.

The solution I developed has a computational complexity of $\Theta(n \log n)$. Essentially, you sort $A$ and find the overlap between elements in $A$ and integers within $\[P, Q\]$. Next, you iterate through elements of $A$ within the overlap and determine the distance of the halfway point between two adjacent elements. The maximum "halfway distance" is your answer.

The array can be sorted in $\Theta(n \log n)$ time while the overlap can be determined by binary searching integers $P$ and $Q$ within the sorted array $A$ which is $\mathcal{O}(\log n)$. Iterating through the elements of A within the overlap is $\mathcal{O}(n)$. 

> What do you call a tiny collection of galaxies? A puny-verse