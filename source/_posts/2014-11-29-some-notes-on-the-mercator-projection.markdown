---
layout: post
title: "Some notes on the Mercator projection"
date: 2014-11-29 22:22:42 -0800
comments: true
categories: 
---

A while back I was investigating on how I would build a Twitter mapping application similar to the [One Million Tweet Map](http://www.onemilliontweetmap.com/) project. One of the first topics I looked into was how I would plot and display each tweet on a map. I was hesitant on using the Google Maps API and wanted to build the project on top of the HTML5 Canvas.

The main challenge here was implementing an algorithm that would map latitude and longitude information into x and y coordinates onto the canvas. I looked into several map projections and decided to settle on the Mercator projection (arguably the most commonly used map projection in the world). I did a little bit of research into Mercator projections with the help of [Wikipedia](http://en.wikipedia.org/wiki/Mercator_projection) and here is a concise summary of what I've learnt.

The Mercator projection is a cylindrical map projection. This means that meridians are mapped to equally spaced vertical lines and circles of latitudes are mapped to equally spaced horizontal lines. The projection assumes that the Earth is a perfect sphere (in truth the Earth is best modelled by an oblate spheroid). Our map will be a Mercator projection of a scaled down version of the perfectly spherical Earth. This scaled down version of the Earth will be called the globe and it has a radius $R$.

The Mercator projection can be understood using small element geometry. The idea is that if we "unwrap" the globe and spread it flat as best we could, very small elements or sections on the globe can be modelled using a rectangle. The diagram below visualizes this idea ($\lambda$ represents the longitude and $\phi$ represents the latitude).

![Alt text](http://upload.wikimedia.org/wikipedia/commons/d/d0/CylProj_infinitesimals2.svg)

The perpendicular distance from the axis of rotation to any point on the globe at latitude $\phi$ is $R \cdot \cos\phi$ and so we get $R \cdot \cos\phi \cdot \delta\lambda$ as the horizontal distance between $P$ and $Q$ using the arc length formula.

The vertical distance between $P$ and $Q$ can also be explained using the same logic. Since latitudes are measured from the center of the globe and the difference in latitude between $P$ and $Q$ is $\delta\phi$, using the arc length formula again gives us $R \cdot \delta\phi$.

Using this information, we define two scale factors from globe to cylinder:

$$k(\phi)=\frac{P'M'}{PM}=\frac{\delta x}{R \cdot \cos\phi \cdot \delta\lambda}\tag{parallel scale factor}$$
$$h(\phi)=\frac{P'K'}{PK}=\frac{\delta y}{R \cdot \delta\phi}\tag{meridian scale factor}$$

## Mapping from longitudes to x coordinates

We note that for small $\delta\lambda$, $\delta x = R \cdot \delta\lambda$. From this equation, we can obtain the mapping function from longitudes to x coordinates (one half of the Mercator projection). $\delta x = R \cdot \delta\lambda$ is a very simple rearranged first order linear differential equation and we can solve it quickly.

$$\begin{align}
\delta x & = R \cdot \delta\lambda \\
\int \delta x & = R \cdot \int \delta\lambda \\
x + c_1 & = R \cdot (\lambda + c_2) \\
x & = (R \cdot \lambda) + (R \cdot c_2) - c_1 \\
& = R(\lambda - \lambda_0)
\end{align}$$

Here $\lambda_0$ is the longitude of an arbitrary central median (this is usually Greenwich).

## Mapping from latitudes to y coordinates

Before starting, it is important to understand that the Mercator projection is conformal. This means that the projection of points from the globe to the cylinder preserves the angles. This property can be defined in two ways:

1. Equality of angles $\beta = \alpha$
2. Isotropy of scale factors: $h = k$

From the above, we know that $\delta x = R \cdot \delta\lambda$ and substituting this into the expression for $k(\phi)$ gets us: 

$$\begin{align}
\frac{1}{\cos\phi} & = k(\phi) \\
& = h(\phi)\tag{from (2)} \\
& = \frac{\delta y}{R \cdot \delta\phi} \\
\frac{\delta y}{\delta\phi} & = R \cdot \sec\phi
\end{align}$$

Solving this differential equation gives us the mapping function from latitudes to y coordinates:

$$y = R \cdot \ln[\tan(\frac{\pi}{4} + \frac{\phi}{2})]$$

## Finding the radius of the globe, $R$

But wait, there's more! All this time we've been working with the radius of the globe, $R$, but what exactly is it's value once we put the projection into practice?

Since the Mercator projection is cylindrical, the scale factor between the globe and the cylinder is one only at the equator. This means that the circumference of the cylinder will equal the circumference of the globe and so the horizontal length of the physical map will equal $2 \cdot \pi \cdot R$.

## Conclusion

Finally, we can define the Mercator projection as with these three equations (where z equals the horizontal length of the physical map):

$$\begin{align}
R & = \frac{z}{2 \cdot \pi} \\
x & = R(\lambda - \lambda_0) \\
y & = R \cdot \ln[\tan(\frac{\pi}{4} + \frac{\phi}{2})]
\end{align}$$

---

##### Sidenote

We can derive the latitude mapping function a second way.

For small elements, we note that $\measuredangle PKQ$ is a right angle and so we get the two equations:

$$\begin{aligned}
\tan\delta = \frac{R \cdot \cos\phi \cdot \delta\lambda}{R \cdot \delta\phi}, \tan\beta = \frac{\delta x}{\delta y}
\end{aligned}$$

From $(1)$ equality of angles, we get the equation $\frac{R \cdot \cos\phi \cdot \delta\lambda}{R \cdot \delta\phi} = \frac{\delta x}{\delta y}$. We also know that $\delta x = R \cdot \delta\lambda$ for small elements. Using the latter fact and rearranging the former equation, we again reach $\frac{\delta y}{\delta\phi} = R \cdot \sec\phi$.