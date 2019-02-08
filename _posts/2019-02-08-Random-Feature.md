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

Some kernels depend only on $\Delta = |x-y|$, eg: Gaussian kernel, $e ^ { - \frac { \| \Delta \| _ { 2 } ^ { 2 } } { 2 } }$

There is a theorem by Bochner: A continuous kernel $k ( \mathrm { x } , \mathrm { y } ) = k ( \mathrm { x } - \mathrm { y } )$ on $\mathcal { R } ^ { d }$ is positive definite if and only if $k ( \delta )$ is the Fourier transform of a **non-negative measure**.

And recall the Fourier Transform, $X ( j \omega ) = \int _ { - \infty } ^ { + \infty } x ( t ) e ^ { - j \omega t } d t$, we let

$$
p ( \omega ) = \frac { 1 } { 2 \pi } \int e ^ { - j \omega ^ { \prime } \delta } k ( \delta ) d \Delta
$$

And use the inverse formula:

$$
k ( \mathbf { x } - \mathbf { y } ) = \int _ { \mathcal { R } ^ { d } } p ( \omega ) e ^ { j \omega ^ { \prime } ( \mathbf { x } - \mathbf { y } ) } d \omega = E _ { \omega } \left[ \zeta _ { \omega } ( \mathbf { x } ) \zeta _ { \omega } ( \mathbf { y } ) ^ { * } \right]
$$

By sampling different $\omega$ from $p(\omega)$, z(x) is a vector of D $\zeta _ { \omega } ( \mathbf { x } )$, and 

$$
k ( \mathbf { x } , \mathbf { y } ) = k ( \mathbf { x } - \mathbf { y } ) \approx \mathbf { z } ( \mathbf { x } ) ^ { \prime } \mathbf { z } ( \mathbf { y } )
$$
