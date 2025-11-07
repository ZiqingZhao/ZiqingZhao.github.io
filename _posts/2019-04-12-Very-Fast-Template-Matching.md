---
layout: post
title: "Very Fast Template Matching"
date: 2019-04-12
excerpt: "Notes on Schweitzer, Bell, and Wu (ECCV 2002)"
tags: [Computer Vision, Template Matching]
comments: true
---

This post summarizes the ECCV 2002 paper *Very Fast Template Matching* by Schweitzer, Bell, and Wu.  
[1] Schweitzer H., Bell J. W., Wu F. (2002). *Computer Vision — ECCV 2002*, LNCS 2353. Springer.

## 0. Abstract

Normalized correlation is a standard way to locate a template inside an image. With $n$ image pixels and $k$ template pixels, naive evaluation costs $O(kn)$, and FFT-based acceleration requires $O(n \log n)$. The paper proposes an integral-image-based method that runs in $O(n)$ by avoiding redundant computations across overlapping windows.

## 1. Introduction

Given template $t$ and image $f$, normalized correlation at location $(u,v)$ is
$$
h(u,v) = \frac{\sum_{x,y} f(u+x,v+y)t(x,y)}{\sqrt{\sum_{x,y} f(u+x,v+y)^2}}.
$$
Large $h(u,v)$ values imply strong matches. Direct computation is $O(kn)$ because both numerator and denominator behave like convolutions. FFT accelerations remain $O(n \log n)$ and are cumbersome to implement. Practical systems therefore rely on hardware tricks, multi-stage filters that evaluate $h(u,v)$ only at promising locations, or coarse-to-fine searches. Viola and Jones showed that, for axis-aligned rectangular templates, integral images can lower the cost to $O(n)$ by reusing summed-area data.

### 1.1 Main Contribution

Integral images can also deliver local algebraic moments in linear time. Those moments define the least-squares polynomial that best approximates the image inside a window, providing accurate surrogates for $h(u,v)$. Approximating with a polynomial of degree $d$ costs $O(d^2 n)$, and experiments show that quadratic (degree-2) approximations already offer high accuracy with much less computation.

### 1.2 Paper Outline

Section 2 reviews integral images. Section 3 shows how to compute local algebraic moments with them. Section 4 derives normalized correlation between the template and the local polynomial approximation. Section 5 describes the full algorithm and its complexity, and Section 6 reports empirical results.

## 2. Integral Images

For an integrable function $f(x,y)$ with $x,y \ge 0$, define the integral image
$$
I(u,v) = \int_{0}^{u}\int_{0}^{v} f(x,y) \, dy \, dx.
$$
The sum over any axis-aligned rectangle is then
$$
\int_{x_a}^{x_b}\int_{y_a}^{y_b} f(x,y) \, dy \, dx = I(x_b,y_b) + I(x_a,y_a) - I(x_a,y_b) - I(x_b,y_a).
$$
In discrete images this becomes
$$
I(x,y)=\sum_{x'=0}^{x}\sum_{y'=0}^{y} f(x',y'),
$$
$$
\sum_{x=x_a}^{x_b}\sum_{y=y_a}^{y_b} f(x,y) = I(x_b,y_b) + I(x_a-1,y_a-1) - I(x_a-1,y_b) - I(x_b,y_a-1).
$$
The recursive definitions
$$
s(x,y) = s(x,y-1) + f(x,y), \qquad I(x,y) = I(x-1,y) + s(x,y)
$$
compute $I$ in a single pass. Viola and Jones used these sums to form Haar-like features consisting of adjacent rectangles whose sums are added or subtracted.

## 3. Fast Computation of Local Algebraic Moments

For any rectangle we can define algebraic moments of order $p+q$ as
$$
m_{pq} = \sum_{x=x_a}^{x_b}\sum_{y=y_a}^{y_b} x^{p}y^{q} f(x,y).
$$
Replacing $f$ by $x^{p}y^{q}f$ in the integral-image formulas yields $m_{pq}$ efficiently. Centered moments with origin $(x_0, y_0)$ are
$$
\mu_{pq} = \sum_{x=x_a}^{x_b}\sum_{y=y_a}^{y_b} (x-x_0)^{p}(y-y_0)^{q} f(x,y),
$$
and can be expressed via
$$
\mu_{pq} =
\sum_{0 \le s \le p \atop 0 \le t \le q}
(-1)^{s+t}
\binom{p}{s}
\binom{q}{t}
x_0^{s} y_0^{t} m_{p-s,q-t}.
$$
Thus each centered moment depends only on equal or lower order raw moments, all of which come from integral images of $x^{p}y^{q}f(x,y)$.

## 4. Fast Template Matching

Let $U_t = (\mu_{00}^t, \ldots, \mu_{pq}^t)$ denote the template moments and $U_f(u,v)$ the image moments at window center $(u,v)$. A naive similarity measure is the normalized dot product
$$
h(u,v) = \frac{\sum_{p,q}\mu_{pq}^f(u,v)\,\mu_{pq}^t}{\sqrt{\sum_{p,q} (\mu_{pq}^f(u,v))^2}}.
$$
However, high-order moments are noise sensitive, so the authors instead approximate $f$ inside the window with a least-squares polynomial, using the moments to solve for the coefficients. Comparing the polynomial approximation with the template yields a closed-form expression analogous to normalized correlation but evaluated on the polynomial coefficients rather than raw pixels. Degree-2 polynomials provide a favorable speed/accuracy trade-off.

## 5. Algorithm and Complexity

1. Precompute integral images for $f$, $xf$, $yf$, $x^2 f$, $xy f$, and $y^2 f$ (sufficient for degree 2).  
2. For each image location $(u,v)$:  
   - Use the integral images to obtain the local raw and centered moments.  
   - Solve the least-squares system for the polynomial coefficients.  
   - Evaluate the closed-form normalized correlation between the template’s coefficients and the local coefficients.  
3. Record the resulting score map and pick maxima as template matches.

All per-location work is constant (given fixed polynomial degree), so the entire procedure is $O(n)$.

## 6. Experimental Findings

The paper reports that quadratic approximations produce accurate matches while reducing runtime substantially compared with direct correlation or FFT-based approaches. The method scales well to multi-scale or multi-orientation searches, making it attractive for detection pipelines that need to scan large images quickly.
