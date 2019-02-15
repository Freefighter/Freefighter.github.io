---
layout:     post
title:      Review | Random Features for Large-Scale Kernel Machines
subtitle:   
date:       2019-02-08
author:     Yifan
header-img: img/post-bg-blackboard.jpg
catalog: true
tags:
    - Machine Learning
    - Computation
---

## Goal

Accelerate the computation of kernel matrix by directly approximating the kernel function.

Map x into a low-dimension random feature space, $z: \mathcal { R } ^ { d } \rightarrow \mathcal { R } ^ { D }$

$$
k ( \mathbf { x } , \mathbf { y } ) = \langle \phi ( \mathbf { x } ) , \phi ( \mathbf { y } ) \rangle \approx \mathbf { z } ( \mathbf { x } ) ^ { \prime } \mathbf { z } ( \mathbf { y } )
$$

## Basic Thought

Rewrite k(x, y) as an expectation over a certain probability space, and hence we can use **sample mean** to approximate it.

## Method: Random Fourier Features

Some kernels depend only on $\Delta = |x-y|$, for example, Gaussian kernel, $e ^ { - \frac { \| \Delta \| _ { 2 } ^ { 2 } } { 2 } }$

There is a theorem by Bochner: A continuous kernel $k ( \mathrm { x } , \mathrm { y } ) = k ( \mathrm { x } - \mathrm { y } )$ on $\mathcal { R } ^ { d }$ is positive definite if and only if $k ( \delta )$ is the Fourier transform of a **non-negative measure**.

And recall the Fourier Transform, $X ( j \omega ) = \int _ { - \infty } ^ { + \infty } x ( t ) e ^ { - j \omega t } d t$, we let

$$
p ( \omega ) = \frac { 1 } { 2 \pi } \int e ^ { - j \omega ^ { \prime } \delta } k ( \delta ) d \delta
$$

And use the inverse formula:

$$
k ( \mathbf { x } - \mathbf { y } ) = \int _ { \mathcal { R } ^ { d } } p ( \omega ) e ^ { j \omega ^ { \prime } ( \mathbf { x } - \mathbf { y } ) } d \omega = E _ { \omega } \left[ \zeta _ { \omega } ( \mathbf { x } ) \zeta _ { \omega } ( \mathbf { y } ) ^ { * } \right]
$$

By sampling different $\omega$ from $p(\omega)$, z(x) is a vector of D $\zeta _ { \omega } ( \mathbf { x } )$, and 

$$
k ( \mathbf { x } , \mathbf { y } ) = k ( \mathbf { x } - \mathbf { y } ) \approx \mathbf { z } ( \mathbf { x } ) ^ { \prime } \mathbf { z } ( \mathbf { y } )
$$

Plus, note that both $k()$ and $p(\omega)$ are real-valued, so we could ignore the imaginary part,

$$
k ( x - y ) = \int _ { R ^ { d } } p ( \omega ) \cos \left( \omega ^ { T } ( x - y ) \right) d \omega
$$

After adding a random variable uniformly drawn from $[0, 2\pi]$, (**to avoid additionally calculating $sin(\omega^{ T } x)$, since $cos(\omega^{T} (x-y)) = cos(\omega^{T} x) cos(\omega^{T} y) + sin(\omega^{T} x) sin(\omega^{T} y)$**)

$$
= \int _ { 0 } ^ { 2 \pi } \frac { 1 } { 2 \pi } d b \int _ { R ^ { d } } p ( \omega ) 2 \cos \left( \omega ^ { T } x + b \right) \cos \left( \omega ^ { T } y + b \right) d \omega = E \left[ z _ { \omega } ( x ) z _ { \omega } ( y ) \right]
$$

So the final map is $z _ { \omega } ( x ) = \sqrt { 2 } \cos \left( \omega ^ { T } x + b \right)$


### How close is the approximation to the original kernel function?

By Hoeffding inequality, (since the rv, i.e. the integral, is bounded)

$$
\operatorname { Pr } \left[ \left| z ( x ) ^ { T } z ( y ) - k ( x , y ) \right| \geq \epsilon \right] \leq 2 e ^ { - D \left( \frac { \epsilon } { 2 } \right) ^ { 2 } }
$$

## Method: Random binning Features

### First try to approximate a special "hat" kernel

$k _ { h a t } ( x , y ; \delta ) = \max \left( 0,1 - \frac { | x - y | } { \delta } \right)$

Partition the real number line with a grid of pitch δ, and shift this grid randomly by an amount u drawn uniformly at random from [0,δ]. This grid partitions the real number line into intervals [u + nδ,u + (n + 1)δ] for all integers n.

### Extend it to more general kernel function

And we can rewrite a kernel function depending only on the L1 distance between pairs of points,

\int _ { 0 } ^ { \infty } k _ { h a t } ( x , y ; \delta ) p ( \delta ) d \delta

the distribution p could be set as, $p ( \delta ) = \delta \ddot { k } ( \delta )$, which is guarenteed by the lemma 1 in this paper.

Random maps for separable multivariate shift-invariant kernels of the form $k(x − y) = \prod _ { m = 1 } ^ { d } k _ { m } ( | x ^ { m } - y ^ { m } | )$, can also utilize this method.

