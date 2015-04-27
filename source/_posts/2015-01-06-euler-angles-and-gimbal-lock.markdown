---
layout: post
title: "Euler Angles and Gimbal Lock"
date: 2015-01-06 13:09:27 -0500
comments: true
categories: 
description: A quick post on gimbal lock and how it relates to Euler angles. This article explores what exactly is gimbal lock, including how and when it happens.
---

A couple months ago, a friend of mine wrote a blog [post](http://luckytoilet.wordpress.com/2014/11/24/visualizing-quaternions-with-unity/) about how quarternions solve the problem of gimbal lock in 3D animations (which happens when you use Euler angles). This got me interested in learning more about Euler angles and exactly how gimbal lock works.

Fundamentally, Euler angles are three angles that are used to describe the orientation of a rigid body in 3D space. The rigid body itself has a local coordinate system that is initially aligned with a fixed global coordinate system. Each Euler angle then represents an elemental rotation of that local coordinate system about one of its three axes. From this, it should be understood that any specific elemental rotation will depend on all prior elemental rotations.

<div style="text-align: center">
    <iframe width="420" height="315" src="//www.youtube.com/embed/Quvb54gttUg" frameborder="0" allowfullscreen></iframe> 
</div>

There are 12 possible sequences of rotation axes for any set of Euler angles; the most popular of which is what's commonly referred to as the 3-2-1 set of Euler angles. In aeronautical terms, this set of Euler angles describes, in order, the yaw, pitch, and roll of an aircraft.

<div style="text-align: center">
    <img src="http://www.allstar.fiu.edu/aero/images/pic5-1.gif">
</div>

## Gimbal lock

<img align="right" src="http://upload.wikimedia.org/wikipedia/commons/d/d5/Gyroscope_operation.gif">

A gimbal is, according to Wikipedia, a "ring that is suspended so [that] it can rotate about an axis" and gimbals are "typically nested one within another to accomodate rotation about multiple axes". As such, a nested three-gimbal system can be used to physically model any 3D rotation based on Euler angles.

Gimbal lock occurs when the middle gimbal rotates in such a way that causes the outer and inner gimbals to align in a plane. In this case, rotating both these gimbals in effect only rotates the rigid body on one axis. The resulting system can only rotate on two axes and it has lost one degree of freedom.

## Gimbal lock and matrices

Any 2D rotation in 3D space can be described using a matrix. For example, the matrix below represents a rotation about the $z$-axis with a counter-clockwise angle of $\theta$ (a positive angle if you use the right-hand rule).

$$\begin{bmatrix}
\cos\theta & -\sin\theta & 0\\
\sin\theta & \cos\theta & 0\\
0 & 0 & 1
\end{bmatrix}$$

The gimbal lock problem can be shown mathematically by representing a 3D rotation based on Euler angles with matrices. For a 3-2-1 rotation, the corresponding mathematical expression would be:

$$
R = 
\begin{bmatrix}1 & 0 & 0 \\ 0 & \cos\alpha & -\sin\alpha\\ 0 & \sin\alpha & \cos\alpha\\\end{bmatrix}
\begin{bmatrix}\cos\beta & 0 & \sin\beta\\ 0 & 1 & 0\\ -\sin\beta & 0 & \cos\beta\end{bmatrix}
\begin{bmatrix}\cos\gamma & -\sin\gamma & 0\\\sin\gamma & \cos\gamma & 0\\0 & 0 & 1\end{bmatrix}
$$

The rightmost matrix represents a rotation about the $z$-axis (yaw), the middle matrix represents a rotation about the $y$-axis (pitch), and the leftmost matrix represents a rotation about the $x$-axis (roll). If the pitch angle, $\beta$, was set to $\frac{\pi}{2}$, which basically means the aircraft is pointing straight up, we get:

$$\begin{aligned}
R & = 
\begin{bmatrix}
1 & 0 & 0 \\ 0 & \cos\alpha & -\sin\alpha\\ 0 & \sin\alpha & \cos\alpha\\
\end{bmatrix}
\begin{bmatrix}
0 & 0 & 1\\0 & 1 & 0\\-1 & 0 & 0
\end{bmatrix}
\begin{bmatrix}
\cos\gamma & -\sin\gamma & 0\\\sin\gamma & \cos\gamma & 0\\0 & 0 & 1
\end{bmatrix} \\

& =
\begin{bmatrix}
0 & 0 & 1 \\ \sin\alpha\cos\gamma + \cos\alpha\sin\gamma & -\sin\alpha\cos\gamma + \cos\alpha\sin\gamma & 0\\ -\cos\alpha\cos\gamma + \sin\alpha\sin\gamma & \cos\alpha\cos\gamma + \sin\alpha\sin\gamma & 0\\
\end{bmatrix} \\

& = 
\begin{bmatrix}
0 & 0 & 1 \\ \sin(\alpha + \gamma) & \cos(\alpha + \gamma) & 0\\ -\cos(\alpha + \gamma) & \sin(\alpha + \gamma) & 0\\
\end{bmatrix}
\end{aligned}$$

From the above matrix, you can tell that in this state, there is no distinction between yaw and pitch. Applying one change to the yaw angle has the exact effect on the orientation of the aircraft as applying the same change to the roll angle. The system has lost one degree of freedom.

## Conclusion

Although I've only mathematically shown how gimbal lock occurs in a 3-2-1 system, it is important to note that there is always the possibility of gimbal lock in any other rotation axes sequence. This is a fundamental problem for Euler angles as the map from Euler angles to rotations is not a covering map. In layman's terms, this means that it is not possible to use Euler angles to represent every 3D rotation. Specifically, for certain values of the second angle, gimbal lock will occur.

Gimbal lock in Euler angles is of practical concern today particularly in the fields of 3D animations and robotics. When it does occur, it leads to weird and undesirable rotation artifacts. Fortunately, there are multiple solutions to this problem. Mechanically, gimbal lock can be solved by adding a fourth gimbal. For 3D animations, a popular solution is to use quarternions instead of Euler angles in calculating rotations.

If, for any reason, you are still confused about what gimbal lock is or how it works, I definitely recommend watching this [video](https://www.youtube.com/watch?v=zc8b2Jo7mno).