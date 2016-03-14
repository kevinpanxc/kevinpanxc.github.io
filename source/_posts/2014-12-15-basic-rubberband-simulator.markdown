---
layout: post
title: "Basic Rubberband Simulation"
date: 2014-12-15 22:10:07 -0800
comments: true
published: false
categories: 
description: This post goes over how to implement a basic rubberband simulation using the HTML5 Canvas. Find out the general ideas behind the simulation and how to implement one yourself.
---

I was part of a hackathon recently where my group wanted to build a game with the main game mechanic being an elastic tether that ties two co-operating players together.

We initially planned to build our own physics engine which included coding the tether ourselves. 6 hours into the hackathon, we realized that everything would be a lot easier if we just used Box2D. So we did. Despite the late technology switch, the hackathon turned out really well and you can see the code [here](https://github.com/alvinlao/charm).

Now that everything's over, I decided to revisit the original incomplete tether code and finish it.

The idea is actually very simple, the rubberband/tether can be simulated as a series of linked nodes connected by springs which are themselves simulated using Hooke's law. So for each "step in time" of the rubberband, we iterate through every node and apply Hooke's law between it's two immediately adjacent nodes to determine the net force and thus the acceleration of the node. We add the acceleration of the node to its current velocity and then we add the velocity to its current position. At this point we are almost done, we just need to apply damping forces so that nodes will not remain in constant motion after a force is applied to it. There are a few ways to model the damping force but since this is supposed to be a "basic" simulation, we will use the simplest, which is linear damping.

Linear damping can be represented mathetically using the equation $F = -cv$ (where $F$ is force, $c$ is the damping coefficient, and $v$ is the velocity of the node).

Here is the [link](http://www.kevinpan.ca/projects/rubber-band-simulator/) to the actual simulation being run on the HTML 5 canvas and here is the [link](https://github.com/kevinpanxc/rubber-band-simulator) to the GitHub repo.

![Screenshot](http://imgur.com/1z8pH5x.jpg)

One definite thing I learnt from building this simulation is that using vectors makes everything easier. For my electric field simulation project, I naïvely chose to calculate forces using magnitudes and angles. This method was several folds more cumbersome to deal with compared to simple vector algebra.