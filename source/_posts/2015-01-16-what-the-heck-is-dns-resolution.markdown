---
layout: post
title: "What the heck is the Domain Name System (DNS)?"
date: 2015-01-16 15:20:09 -0500
comments: true
categories: 
---

> This post was inspired by Monica Dinculescu's [speech](http://meowni.ca/posts/cat-dns-cascadia/) at CUSEC 2015, "The Internet Needs More Cats".

The Internet is so prevalent in our everyday lives but the majority of Internet users have a very limited understanding of how the whole system works. Admittedly, I am one of them. Recently, I've decided to tackle a very small fragment of my Internet ignorance; I wanted to understand how DNS or the Domain Name System works.

DNS is basically the system responsible for translating website addresses such as `google.com` to IP addresses. These addresses are what is used internally to identify specific computers among the billions of computers connected over the Internet. We use domain names instead of IP addresses since the former has a lot more meaning to us as humans and thus is a lot easier to memorize (`www.google.com` vs `http://74.125.224.72/`).

What exactly is this system and where is it stored? The DNS service is not run by an one computer. Instead, the service is offered through an entire network of interconnected computers. But before I get into the details of that, there are some other things I need to go over.

## Domain name syntax

A domain name essentially consists of labels concatenated and delimited by dots. This is just a fancy way to describe the general structure of all the URLs we know and love, like `www.facebook.com` and `www.reddit.com`. Each label of a domain name, from right to left, corresponds to a particular level in DNS. For example, the label `com` is what is generally known as a top level domain or TLD. Other top level domains include `ca` and `cn`. Then, the labels `facebook` or `reddit` correspond to a lower level in the DNS.

As a side note, there is actually one level above TLDs. This is called the root domain and is represented as a `.` at the end of all URLs. However, this `.` is implicit so we usually don't include it when displaying URLs.

## Domain name space (what does all these level stuff mean?)

<img align="right" style="width: 50%" src="http://upload.wikimedia.org/wikipedia/commons/b/b1/Domain_name_space.svg">

The domain name space can be represented as a tree-like structure. Each node represents one particular label in a domain name. The root node would thus correspond to the aforementioned `.` that is implicitly tagged on to all URLs. TLDs correspond to child nodes of the root node as expected.

<div style="clear: both; margin-bottom: 20px"></div>

## Practically...

A domain name space tree is represented as a distributed database system. DNS name servers, specifically authoritative name servers, assume responsibility over a particular zone in the name space. A zone consists of one or more connected nodes. Each node contains a resource record holding information associated with the current domain name as well as all its child nodes.

As such, to determine an IP address from a domain name, one needs to parse the domain name into the respective labels and traverse the domain name space tree to reach the particular node with the correct IP address.

## Where are all these name servers?

There are currently [13 root name servers](http://en.wikipedia.org/wiki/Root_name_server#Root_server_addresses) located around the world with the majority in the United States. Responsibility of TLD domains is delegated to specific organizations by the Internet Corporation for Assigned Names and Numbers (ICANN). For example, the `com` TLD was originally administered by the United States Department of Defense but is now administered by Verisign. Lower level domains are managed by various other organizations and private entities.

## Speeding things up

Having to traverse through the entire domain name space tree everytime you type `www.google.com` into your browser address bar is super slow. Again, an age old technique called caching is used to speed things up. Typically, alongside the authoritative name servers making up the domain name space tree are recursive name servers. These servers are often provided by Internet Service Providers (ISP) or other organizations such as Google. These servers are the ones your browser will most likely talk to first when it needs to resolve a domain name.

Recursive name servers provide two essential services. First, they are able to perform a DNS recursion over the domain name space tree to resolve a domain name to an IP address. A DNS recursion is basically a recursive traversal over the domain name space tree using information from the domain name to reach the right authoritative name server and obtain the right IP address. A recursive name server only performs a DNS recursion if its local cache does not contain the IP address mapped to a requested domain name. The second service provided by recursive name servers is caching of successful DNS recursion results so that future queries will not require another expensive DNS recursion. Finally, it is also important to note that IP address caching can also happen at the client level AKA on your computer.

## Useful links

* [How DNS query works](https://technet.microsoft.com/en-ca/library/cc775637%28v=ws.10%29.aspx)
* [Mechanics Behind the Internet: Authoritative vs. Recursive DNS Servers: What’s The Difference?](http://www.dnsmadeeasy.com/authoritative-vs-recursive-dns-servers-whats-the-difference/)
* [What Is Authoritative Name Server?](http://www.dnsknowledge.com/whatis/authoritative-name-server/)