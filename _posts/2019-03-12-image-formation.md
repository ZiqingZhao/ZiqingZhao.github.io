---
layout: post
title: "Image Formation"
date: 2019-03-12
excerpt: ""
tags: [Computer Vision]
comments: true
---

[1] R. Szeliski, *Computer Vision: Algorithms and Applications*, 2010.  
[2] R. Szeliski, A. Hai-zhou, *Computer Vision: Algorithms and Applications*, Tsinghua University Press, 2012.

## 1. Geometric Primitives and Transformations

### 1.1 Geometric Primitives

**2D points.** A 2D point can be represented in Cartesian form $\mathbf{x} = (x, y) \in \mathbb{R}^2$ or in homogeneous coordinates $\tilde{\mathbf{x}} = (\tilde{x}, \tilde{y}, \tilde{w}) \in \mathbb{P}^2$ with $\mathbb{P}^2 = \mathbb{R}^3 \setminus \{(0,0,0)\}$. The homogeneous vector relates to its Cartesian counterpart through
$$
\tilde{\mathbf{x}} = (\tilde{x}, \tilde{y}, \tilde{w}) = \tilde{w}(x, y, 1) = \tilde{w}\,\bar{\mathbf{x}},
$$
where $\bar{\mathbf{x}} = (x, y, 1)$ is the augmented vector.

**2D lines.** A line expressed in homogeneous coordinates $\tilde{\mathbf{l}} = (a, b, c)$ satisfies $\bar{\mathbf{x}} \cdot \tilde{\mathbf{l}} = ax + by + c = 0$. The intersection of two lines is $\tilde{\mathbf{x}} = \tilde{\mathbf{l}}_1 \times \tilde{\mathbf{l}}_2$, whereas the line through two points is $\tilde{\mathbf{l}} = \tilde{\mathbf{x}}_1 \times \tilde{\mathbf{x}}_2$. Least-squares fitting recovers a line from noisy points or the best intersection of almost-concurrent lines.

**2D conics.** Conics are described by $\tilde{\mathbf{x}}^{T} Q \tilde{\mathbf{x}} = 0$ and play a key role in multi-view geometry and camera calibration.

**3D points.** Analogously, a 3D point can be written as $\mathbf{x} = (x, y, z) \in \mathbb{R}^3$ or $\tilde{\mathbf{x}} = (\tilde{x}, \tilde{y}, \tilde{z}, \tilde{w}) \in \mathbb{P}^3$ with augmented form $\bar{\mathbf{x}} = (x, y, z, 1)$ so that $\tilde{\mathbf{x}} = \tilde{w}\,\bar{\mathbf{x}}$.

**3D planes.** A plane with homogeneous parameters $\tilde{\mathbf{m}} = (a, b, c, d)$ satisfies $\bar{\mathbf{x}} \cdot \tilde{\mathbf{m}} = ax + by + cz + d = 0$.

**3D lines.** A line defined by two points $(\mathbf{p}, \mathbf{q})$ can be parameterized as $\mathbf{r} = (1-\lambda)\mathbf{p} + \lambda \mathbf{q}$ with $0 \le \lambda \le 1$. Homogeneously, $\tilde{\mathbf{r}} = \mu \tilde{\mathbf{p}} + \lambda \tilde{\mathbf{q}}$, which has excess degrees of freedom. Plücker coordinates remedy this with
$$
\mathbf{L} = \tilde{\mathbf{p}}\,\tilde{\mathbf{q}}^{T} - \tilde{\mathbf{q}}\,\tilde{\mathbf{p}}^{T},
$$
subject to $\det(\mathbf{L}) = 0$, leaving four degrees of freedom.

**3D quadrics.** Quadrics mirror conics via $\tilde{\mathbf{x}}^{T} Q \tilde{\mathbf{x}} = 0$.

### 1.2 Two-Dimensional Transformations

Planar transformations include translation, Euclidean transforms (rotation plus translation), similarity transforms (uniform scale + Euclidean), affine transforms, and projective transforms.

**Translation.**
$$
x' = x + t, \qquad x' = [\,\mathbf{I} \ \ t\,]\bar{x},
$$
or in homogeneous form,
$$
\bar{x}' = \begin{bmatrix} \mathbf{I} & t \\ 0^{T} & 1 \end{bmatrix} \bar{x},
$$
which enables chaining via $3\times3$ matrices.

**Euclidean transformation (rotation + translation).**
$$
x' = \mathbf{R}x + t = [\,\mathbf{R} \ \ t\,]\bar{x},
$$
where
$$
\mathbf{R} = \begin{bmatrix}
\cos\theta & -\sin\theta \\
\sin\theta & \cos\theta
\end{bmatrix}
$$
is an orthogonal rotation matrix satisfying $\mathbf{R}\mathbf{R}^{T} = \mathbf{I}$ and $|\mathbf{R}|=1$.

**Similarity transformation (scale + rotation + translation).**
$$
x' = s\mathbf{R}x + t = [\,s\mathbf{R} \ \ t\,]\bar{x} = 
\begin{bmatrix}
a & -b & t_x \\
b & a & t_y
\end{bmatrix}\bar{x},
$$
with $a^2 + b^2 = s^2$. Similarities preserve angles between lines.

**Affine transformation.**
$$
x' = \mathbf{A}\bar{\mathbf{x}}, \qquad
x' = \begin{bmatrix}
a_{00} & a_{01} & a_{02} \\
a_{10} & a_{11} & a_{12}
\end{bmatrix}\bar{x},
$$
where $\mathbf{A}$ is $2\times3$. Affine maps preserve parallelism.

Projective transforms extend this framework by allowing arbitrary $3 \times 3$ homogeneous matrices and are handled similarly, though they are not detailed further here.
