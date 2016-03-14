---
layout: post
title: "My Newbie Experience with SEO"
date: 2014-12-21 01:52:36 -0800
comments: true
published: false
categories: 
use_content_as_description: true
---

[TwitchQuotes](http://www.twitchquotes.com) is a website I built slightly under a year ago and which I am currently still maintaining. It has some decent statistics. Over the past few months, the average session duration is at 3 minutes and 45 seconds, with 5.44 pages visited per session. The overall bounce rate is at 45.93%.

I started building this website as a way to introduce myself to the popular web application framework, Ruby on Rails. After finishing the basic features with a decent looking UI, I deployed it to Heroku and left a few links on Reddit. I thought nothing of it until a couple days later when this happened...

![Crazy stats](http://i.imgur.com/RvuHKWh.png)

When I saw this I knew that I had to keep building this website up. 

### The Idea

Twitch.tv is a live streaming video platform that is dedicated to video games. This means that most of the time you'll find people streaming themselves playing video games. This is a relatively new form of entertainment that has already garnered a large following.

![Twitch TV stats](http://images.eurogamer.net/2014/usgamer/Twitch-News-02.png)

These statistics are impressive enough that it's no surprise that there were rumours both Google and Amazon were vying to buy Twitch earlier this year. In the end, Amazon was able to [acquire Twitch.tv](http://arstechnica.com/gaming/2014/08/amazon-not-google-reportedly-buying-twitch-for-1-billion/) for a whopping $970 million.

Part of a stream on Twitch is an IRC chat channel for viewers. On particularly popular streams with thousands of people watching, you can imagine how crazy chat gets. What typically ends up happening is loads and loads of spam. People especially like to copy and paste ridiculous passages of text (AKA **copypastas**) that make no sense but which is exactly why they are funny.

> Hello, I am currently 15 years old and I want to become a walrus. I know there's a million people out there just like me, but I promise you I'm different. On December 14th, I'm moving to Antarctica; home of the greatest walri. I've already cut off my arms, and now slide on my stomach everywhere I go as training. I may not be a walrus yet, but I promise you if you give me a chance and the support I need, I will become the greatest walrus ever. Thank you all.

I happened to watch Twitch often at the start of this year, especially while working out, and I was witness to spam consisting of very creative and absolutely hilaroius copypastas. This gave me the idea of creating a [bash.org](www.bash.org)-like website for Twitch chat. At that time I was still exploring the magical world of Ruby on Rails and TwitchQuotes fit perfectly as a beginner project for the framework. 

### Growth in Organic Search Traffic

From the initial burst of visits from Reddit, site traffic quieted down for about 2 months hovering around 30-40 visits per day. Traffic from organic search was almost non-existent. From a total of about 2700 visits during that time, only 381 came from organic search. 

About halfway through May, the site started to rise in search rankings and organic search visits started rolling in. From a modest start of 40-50 visits from Google each day, the number grew to around 70-100.

![Organic search traffic](http://imgur.com/iAnuWCh.jpg)

### Google... why?

At around the end of October, TwitchQuotes was getting around 2000 search impressions and around 120-140 visits a day from organic search. This all abruptly ended on October 22nd when TwitchQuotes surprisingly plummeted in search rankings for many important keywords. From 2000 search impressions a day to 200, from 140 organic search visits a day to 20. To say the least, I was surprised and very curious as to why the site was so drastically affected. It is highly probable that this drop in search rankings was related to the rollout of Google Penguin 6 which began on October 17th. Google Penguin is a search engine algorithm that is aimed at decreasing search rankings of websites that utilize black-hat SEO techniques such as link spamming and keyword stuffing to rise in search rankings. I honestly haven't done anything remotely close to what Google Penguin is looking for so why TwitchQuotes was affected, I have no idea (this is only assuming Penguin 6 was the cause of the search ranking drops).

![Dip in traffic](http://i.imgur.com/tbrikSc.jpg)

I started seriously looking into what is often referred to as SEO or search engine optimization and I did several things to try to get the site back on track.

* **Change the meta description tag**

The very first thing I looked at was TwitchQuotes's meta description tag. I realized that the old description was basically the title of the site verbatim. And that description was the same for every, single, page. This resulted in Google completely ignoring the contents of the tag and using other words on the site as the search preview snippets. Not surprisingly, TwitchQuotes didn't look too good on Google.

![Old TwitchQuotes search preview snippet](http://i.imgur.com/tN7U0Lr.jpg)

After changing it, it looks much better. In addition, I provided a custom meta description for every page that warranted one.

![New TwitchQuotes search preview snippet](http://imgur.com/OEHrA0a.jpg)

* **Get a custom domain**

I hosted TwitchQuotes on Heroku (it's really good!) and so the address to the site was a subdomain of herokuapp.com (twitchquotes.herokuapp.com). Given the relatively low visibility twitchquotes.herokuapp.com had on Google anyways, I decided to quickly snatch up a custom domain name for TwitchQuotes (www.twitchquotes.com).

* **Censor profane language**

Given the nature of copypastas, it was common to have profanities in them. Although I'm not entirely sure how this affects TwitchQuotes's search rankings, it definitely makes the website appear less professional. So I implemented a profanity filter that hid detected quotes behind an Ajax request.

![Explicit quote](http://i.imgur.com/EqGDKNe.jpg)

### Climbing Back

I barely scratched the surface of SEO but the current changes to TwitchQuotes seem to be working out. In a little over one month, the new domain name was able to rank higher and on more keywords than the old herokuapp subdomain. Total search impressions of TwitchQuotes is now hovering around 3 000 per day which is about 500 more than it was before.

![New stats](http://imgur.com/tKM52BF.jpg)