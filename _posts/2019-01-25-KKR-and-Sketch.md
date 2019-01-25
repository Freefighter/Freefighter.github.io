---
layout:     post
title:      Review | KRR and Sketch
subtitle:   Randomized sketches for kernels - Fast and optimal nonparametric regression
date:       2019-01-25
author:     Yifan
header-img: img/post-bg-blackboard.jpg
catalog: true
tags:
    - Machine Learning
    - Computation
---

## Kernel Ridge Regression

First map x to kernel space

$$\mathbf { x } _ { i } \rightarrow \Phi _ { i } = \Phi \left( \mathbf { x } _ { i } \right) = K(x, .)$$

It’s a function, but in Hilbert Space, it can be represented as an infinite vector.

And $f(x_i) = \left( f _ { 1 } , f _ { 2 } , \ldots \right) _ { \mathcal { H } }  \Phi(x_i)$ (**reproducing property**, indeed H is called reproducing kernel Hilbert space (RKHS))

Change the proplem to guess $\hat{y} = f^T \Phi(x)$

Then make use of the following property:

$\left( P ^ { - 1 } + B ^ { T } R ^ { - 1 } B \right) ^ { - 1 } B ^ { T } R ^ { - 1 } = P B ^ { T } \left( B P B ^ { T } + R \right) ^ { - 1 }$

we can get:

$\mathbf { w } = \left( \lambda \mathbf { I } _ { d } + \Phi \Phi ^ { T } \right) ^ { - 1 } \Phi \mathbf { y } = \Phi \left( \Phi ^ { T } \Phi + \lambda \mathbf { I } _ { n } \right) ^ { - 1 } \mathbf { y }$

and indeed we never actually need access to the feature vectors

$y = \mathbf { w } ^ { T } \Phi ( \mathbf { x } ) = \mathbf { y }^T \left( \Phi ^ { T } \Phi + \lambda \mathbf { I } _ { n } \right) ^ { - 1 } \Phi ^ { T } \Phi ( x ) = \mathbf { y }^T \left( K + \lambda \mathbf { I } _ { n } \right) ^ { - 1 } K (X, \mathbf { x } )$

Which is the same as **GP Regression**.

### objective function

$$ \frac { 1 } { 2 n } \sum _ { i = 1 } ^ { n } \left( y _ { i } - f \left( x _ { i } \right) \right) ^ { 2 } + \lambda _ { n } \| f \| _ { \mathcal { H } } ^ { 2 } $$

### Quadratic Form

$f ^ { \diamond } : = \arg \min _ { f \in \mathcal { H } } \lbrace \frac { 1 } { 2 } \sum _ { i = 1 } ^ { n } \left( y _ { i } - f \left( x _ { i } \right) \right) ^ { 2 } + \lambda _ { n } \| f \| _ { \mathcal { H } } ^ { 2 } \rbrace$ could be written as

$$
\frac { 1 } { 2 } (Y - K \omega)^T (Y - K \omega) + \lambda _ { n } \omega ^ { T } K \omega \\
\Rightarrow \arg \min _ { \omega \in \mathbb { R } ^ { n } } \left\{ \frac { 1 } { 2 } \omega ^ { T } K ^ { 2 } \omega - \omega ^ { T } \frac { K y } { \sqrt { n } } + \lambda _ { n } \omega ^ { T } K \omega \right\}
$$

## Sketch Method

To decrease the cost of computation (inverse), we reduce the dimension of the problem above from n to m.

$\widehat { \alpha } = \arg \min _ { \theta \in \mathbb { R } ^ { m } } \lbrace \frac { 1 } { 2 } \alpha ^ { T } ( S K ) \left( K S ^ { T } \right) \alpha - \alpha ^ { T } S \frac { K y } { \sqrt { n } } + \lambda _ { n } \alpha ^ { T } S K S ^ { T } \alpha \rbrace$

### Choosing a Suitable Sketch Matrix

> For suitably chosen orthonormal matrices, including the DFT and Hadamard cases among others, a matrix-vector product (say of the form $S u$ for some vector $u \in \mathbb { R } ^ { n }$ ) can be computed in $\mathcal { O } ( n \log m )$ time, as opposed to $\mathcal { O } ( n m )$ time required for the same operation with generic dense sketches.

Indeed, we have the following Johnson-Lindenstrauss Lemma to guarantee keeping the effect.

> JOHNSON-LINDENSTRAUSS TRANSFORMATION AND RANDOM PROJECTION

For any set $V$ of n points in $\mathbb { R } ^ { d }$ , there exists a map $f : \mathbb { R } ^ { d } \rightarrow \mathbb { R } ^ { k }$ such that for all $u , v \in V ,$

$$\quad ( 1 - \epsilon ) \| u - v \| ^ { 2 } \leq \| f ( u ) - f ( v ) \| ^ { 2 } \leq ( 1 + \epsilon ) \| u - v \| ^ { 2 }$$

> Fast Dimension Reduction Using Rademacher Series on Dual BCH Codes

And the S could be chosen in this way to have the property above:

$$S = B D \Phi ^ { ( r ) }$$

Let $B$ be a $m \times n$ matrix with Euclidean unit length columns, and $D$ a random $\{ \pm 1 \}$ diagonal matrix.

$$\Phi _ { n } ^ { ( r ) } = H D ^ { ( r ) } H D ^ { ( r - 1 ) } \dots H D ^ { ( 1 ) }$$

$H = H _ { d }$ is the Walsh-Hadamard matrix, and $r = \lceil 1 / 2 \delta \rceil$

Combining the above, the random transformation $S = B D \Phi ^ { ( r ) }$ has Euclidean JLP for $m < n ^ { 1 / 2 - \delta }$ , and can be applied to a vector in time $O ( d \log d ) .$

That's caused by the recursive definition of H,

$$H _ { m } = \frac { 1 } { \sqrt { 2 } } \left( \begin{array} { c c } { H _ { m - 1 } } & { H _ { m - 1 } } \\ { H _ { m - 1 } } & { - H _ { m - 1 } } \end{array} \right)$$

### Further Decreassing the Cost

Instead of using $\Phi ^ { ( r ) } = \Phi _ { n } ^ { ( r ) }$ as above, we use a block-diagonal $d \times d$ matrix comprised of $n / \beta (\beta \times \beta$) blocks each drawn from the same distribution as $\Phi _ { \beta } ^ { ( r ) } .$

The running time of the block-diagonal matrix is $O ( d \log k )$.

By applying the Walsh transform independently on each block (recall that $\beta = f _ { \mathrm { BCH } } ( k ) k ^ { \delta } = O \left( k ^ { 2 + \delta } \right) ) .$