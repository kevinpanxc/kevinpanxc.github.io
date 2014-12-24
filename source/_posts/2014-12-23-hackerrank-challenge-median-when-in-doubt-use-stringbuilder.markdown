---
layout: post
title: "Hackerrank Challenge: Median - when in doubt, use StringBuilder"
date: 2014-12-23 18:48:51 -0800
comments: true
categories: 
---

In this Hackerrank challenge, you need to calculate a running median of a list of numbers where numbers can be both added and removed. Each time this is done, you need to output the median of the current list. I chose to use Java to solve this challenge.

One way to approach this problem is to maintain a sorted array. You find where you want to insert/delete elements using binary search and the median can be calculated by looking at element(s) at the halfway point of the list. However, the problem with using an array is that insertion/deletion is actually $O(n)$ (where $n$ is the size of the list) since you have to reposition all list elements after the inserted/removed element. It doesn't matter if searching for where to insert/delete is $O(\log\ n)$. However, you can find the median of the list at any time in $O(1)$ because random access operations for an array is $O(1)$.

You can slightly improve upon this solution by using an ordered data structure that is not a list, such as C++'s `std::multiset`. Unfortunately, Java's standard library does not have an equivalent data structure (the closest you get is `TreeSet` but it forbids repeated elements). You can, however, emulate the behaviour of `std::multiset` by using a `TreeMap` where the keys are unique elements in the running list and the values are the number of times they appear. Unfortunately, finding the median still takes $O(n)$.

### Using a completely different approach

The above solution holds promise but there is a completely different and very efficient solution that finds a running median using two data structures instead of one. One data structure holds the bottom half of the **ordered** list (A) while the other holds the top half (B). Each time a number is added to the running list, if it is smaller than the max element of A, then add it to A, else add it to B. The median can be calculated from the two by looking at the max element of A or the min element of B or both if $n$ is even. For this to work, the difference in lengths between A and B must be at most one. If adding or removing a number from the running list causes the lengths to differ by two, you need to balance them by transferring a min/max element between the two structures.

The trick here is to find a type of data structure that allows you to quickly find and remove its max/min element while also allowing you to quickly insert and delete random elements.

One way to do this is to use priority queues. The bottom half of the running list will be stored in a max priority queue while the top half will be stored in a min priority queue. If the priority queues were implemented with a heap, insertion and balancing will have a time complexity of $O(\log\ n)$. Finding the median can be done in constant time. The only problem, though, is that removing a random element from a heap-based priority queue takes $O(n)$. I tried this solution on Hackerrank and sure enough, I get timeout errors.

![Timeout error](http://imgur.com/IqzOBvw.jpg)

### Almost a solution

So, if not priority queues, what do we use?

The answer can be found by revisiting the `TreeMap` solution discussed before. Instead of using two priority queues, we can use two trees! Again, C++'s `std::multiset` datastructure would be perfect for this but since Java's standard library doesn't have the equivalent data structure, we need to use a `TreeMap` where keys are unique elements in the running list and the values are the number of times they appear.

* Find max/min: $O(\log\ n)$
* Insertion/deletion: $O(\log\ n)$

This means that each time an element is inserted or removed into the running list, the algorithm will take $O(\log\ n)$ to compute the median, much faster than the $O(n)$ priority queue solution.

At this point, I was sure I solved the challenge...

![Timeout error again](http://imgur.com/6A7GO1A.jpg)

### StringBuilder to the rescue!

I spent a couple hours mulling over what I could possibly optimize before realizing that I have had substantial success in the past when I stopped using a bunch of `System.out.println` statements and just aggregated all the output in one `StringBuilder` object. Everything could then be printed with just one `println` statement. Exhausted and frustrated from spending so much time on this problem, I tried it.

I still can't believe it's not butter.

![Success!](http://imgur.com/8S76uT2.jpg)

> How much memory does it take to store a dinosaur's DNA? A Pterobyte

> What a pteroble joke.