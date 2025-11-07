---
layout: post
title: "Graphical Models: Directed Gaussian Graphical Models"
date: 2025-06-26
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Directed Gaussian Graphical Models]
comments: true
---

### Directed Gaussian Graphical Models

**Definition**  
The Gaussian graphical model given by the DAG $G=(V,E)$ is  
$$
\begin{align*}
\mathcal N(G)
&=\bigl\{N_m(\mu,\Sigma)\colon \mu\in\mathbb R^m,\ \Sigma\in\mathrm{PD}_m,\ 
N_m(\mu,\Sigma)\text{ satisfies the global Markov property for }G\bigr\}\\
&=\bigl\{N_m(\mu,\Sigma)\colon \mu\in\mathbb R^m,\ \Sigma\in\mathrm{PD}_m,\ 
\text{density factorizes according to }G\bigr\}.
\end{align*}
$$
**Composition Property**  
For $X\sim N_m(0,\Sigma)$, positivity implies the intersection axiom and moreover
$$
A\perp\!\!\perp B\mid C
\;\Longleftrightarrow\;
u\perp\!\!\perp v\mid C
\quad\text{for all }u\in A,\ v\in B,
$$
since $\Sigma_{A,B}=0\iff\Sigma_{u,v}=0$. 

### Parametrization via Edge Weights

Define  
$$
\R^E
=\{\,B=(\omega_{v u})\in\mathbb R^{m\times m}\colon \omega_{v u}=0\text{ if }u\notin\pa(v)\},
\quad
\mathrm{Diag}^+
=\{\,\Omega=\mathrm{diag}(\epsilon_{vv})\colon \epsilon_{vv}>0\}.
$$
Let $I$ be the $m\times m$ identity.  The map
$$
\phi_G:\R^E\times \mathrm{Diag}^+ \;\to\;\mathrm{PD}_m,
\qquad
\phi_G(B,\Omega)
=(I - B)^{-1}\,\Omega\,(I - B)^{-T}
$$
provides a parametrization of $\mathcal N(G)$.

**Theorem (Parametrization)**  
$N_m(\mu,\Sigma)\in\mathcal N(G)$ if and only if $\Sigma=\phi_G(B,\Omega)$ for some $B\in\mathcal R_E,\ \Omega\in\mathrm{Diag}^+$. 

**Lemma**  
For any DAG $G=(V,E)$ and $B\in\mathcal R_E$, $\det(I - B)=1$ and thus $I - B$ is invertible. 

**Theorem**  
The parametrization map $\phi_G$ is injective.  If $\Sigma=\phi_G(B,\Omega)$, then
$$
B_{v,\pa(v)}=\Sigma_{v,\pa(v)}\,\Sigma_{\pa(v),\pa(v)}^{-1},
\quad
\epsilon_{vv}
=\Sigma_{vv}-\Sigma_{v,\pa(v)}\,\Sigma_{\pa(v),\pa(v)}^{-1}\,\Sigma_{\pa(v),v}.
$$

**Corollary**  
$\{\Sigma\in\mathrm{PD}_m:\,N(0,\Sigma)\in\mathcal N(G)\}$ has dimension $|V|+|E|$. 

### Trek Rule

**Lemma**  
For $B\in\mathcal R_E$, the entries of the path‐matrix $(I - B)^{-1}$ are
$$
[(I - B)^{-1}]_{v u}
=\sum_{\pi\in\Pi(u,v)}w(\pi),
$$
where $\Pi(u,v)$ is the set of directed paths from $u$ to $v$ and
$w(\pi)=\prod_{(x\to y)\in\pi}\omega_{y x}$. 

**Theorem (Trek Rule)**  
If $\Sigma=(I - B)^{-1}\,\Omega\,(I - B)^{-T}$, then
$$
\Sigma_{u v}
=\sum_{\phi\in\mathcal T(u,v)}w(\phi),
$$
where $\mathcal T(u,v)$ are all *treks* from $u$ to $v$, and each trek $\phi$ with top $z$ has weight
$\;w(\phi)=\epsilon_{zz}\,w(\pi_L)\,w(\pi_R)$. 


### Linear Structural Equation Models

In the SEM for $G$, each
$$
X_v
=\omega_{0v}
+\sum_{u\in\pa(v)}\omega_{v u}\,X_u
+\zeta_v,
\quad
\zeta_v\overset{\mathrm{iid}}{\sim}N(0,\epsilon_{vv}).
$$
Vectorizing gives $(I - B)X=\omega_0+\zeta$, so
$$
X=(I - B)^{-1}\omega_0+(I - B)^{-1}\zeta,
\quad
\Var{X}=(I - B)^{-1}\,\Omega\,(I - B)^{-T},
$$
recovering the same family $\mathcal N(G)$.
