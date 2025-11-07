---
layout: post
title: "Graphical Models: Gaussian Graphical Model"
date: 2025-05-27
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Gaussian Graphical Model]
comments: true
---

## Definition

**Definition**  
The *Gaussian graphical model* given by an undirected graph $$G=(V,E)$$ is the set of (regular) multivariate normal distributions on $$\mathbb{R}^m$$ that satisfy the global Markov property for $$G$$, i.e.
$$
\mathcal{N}(G) \;=\;\{\,N_m(\mu,\Sigma):\;\mu\in\mathbb{R}^m,\;\Sigma\in\mathrm{PD}_m,\;\text{the distribution satisfies the global M.P.\ for }G\}.
$$
Recall: The density of $$N_m(\mu,\Sigma)$$ is positive, so  
$$
\text{global M.P.}\;\Leftrightarrow\;\text{local M.P.}\;\Leftrightarrow\;\text{pairwise M.P.}\;\Leftrightarrow\;\text{factorization according to }G.
$$

**Proposition**  
The Gaussian graphical model $$\mathcal{N}(G)$$ postulates a *sparse inverse covariance matrix* (precision matrix) $$K = (\omega_{v,w}) = \Sigma^{-1}$$.  More precisely,
$$
N_m(\mu,\Sigma)\in\mathcal{N}(G)
\;\Longleftrightarrow\;
\omega_{v,w}=(\Sigma^{-1})_{v,w}=0\quad\text{whenever }v\not\sim w\text{ in }G,\ v\neq w.
$$
The model is of dimension $$\lvert V\rvert+\lvert E\rvert$$. (It is an exponential family.) 
 ## Maximum Likelihood Estimation in Saturated Model

Let  
$$
X^{(1)},\dots,X^{(n)}\;\overset{\mathrm{iid}}{\sim}\;N_m(\mu,\Sigma),\quad
\bar X=\frac1n\sum_{i=1}^n X^{(i)},\quad
S=\frac1n\sum_{i=1}^n\bigl(X^{(i)}-\bar X\bigr)\bigl(X^{(i)}-\bar X\bigr)^T.
$$
Maximizing over $$\mu$$ gives $$\hat\mu_{ML}=\bar X$$.  
Maximizing over $$\Sigma$$ gives $$\hat\Sigma_{ML}=\hat{K}^{-1}$$, where $$\hat{K}$$ maximizes the function
$$
h_S(K)\;=\;\log\det(K)\;-\;\mathrm{tr}(KS).
$$
- **Uniqueness**
	**Lemma 4**  
	(i) If $$S$$ is positive definite, then $$h_S(K)$$ attains its maximum at $$\hat K = S^{-1}$$.  
	(ii) If $$S$$ is singular, then $$h_S(K)$$ is unbounded. 
	
	**Theorem 5**  
	(i) If $$n\ge m+1$$, then $$S$$ is positive definite with probability one and $$\hat\Sigma_{ML}=S$$.  
	(ii) If $$n\le m$$, then $$S$$ is singular and the MLE of $$\Sigma$$ does not exist. 

- **Concavity**
	**Lemma 6**
	The function $$h_S(K)\;=\;\log\det(K)\;-\;\mathrm{tr}(KS)$$ is a strictly concave function of $$K\in {PD}_m$$.

## MLE in Gaussian Graphical Model

Parametrizing by the precision matrix $$K=\Sigma^{-1}$$ subject to the graph $$G$$ yields the feasible set
$$
C(G)\;=\;\{\,K\in\mathrm{PD}_m:\;K_{v,w}=0\text{ if }v\neq w\text{ and }(v,w)\not\in E\}.
$$
MLE of the mean vector is still $$\hat\mu_{ML}=\bar X$$.  
MLE of $$K\in C(G)$$ maximizes the function
$$
\max_{K\in C(G)}h_S(K)= \max_{K\in C(G)} \log\det(K)\;-\;\mathrm{tr}(KS).
\tag{*}
$$

**Theorem 7**  
If $$S$$ is positive definite, the constrained problem in $$(*)$$ has a unique solution $$\hat K_{ML}\in C(G)$$.  This solution satisfies
$$
(\hat K_{ML}^{-1})_{v,w}
=
S_{v,w}
\quad\text{for }v=w\text{ or }(v-w)\in G,
$$
Note: $$(\hat K_{ML})_{v,w}=0$$ if $$v\ne w, (v-w)\notin G,$$
Note: The assumption of S positive definite is not necessary for existence of MLE.

## Estimation by Regression (Pseudo-Likelihood)

For each node $$v\in V$$, the conditional distribution
$$
X_v \mid X_{V\setminus\{v\}}
\;\sim\;
N\!\Bigl(\mu_v
\;-\;\tfrac1{\omega_{vv}}\,
\omega_{v,V\setminus\{v\}}\,(X_{V\setminus\{v\}}-\mu_{V\setminus\{v\}}),\;
\tfrac1{\omega_{vv}}\Bigr).
$$
Thus a linear regression of $$X_v$$ on $$X_{V\setminus\{v\}}$$ consistently estimates $$\beta_{vw}=-\omega_{v,w}/\omega_{vv}$$ and the residual variance $$1/\omega_{vv}$$, from which $$\omega_{v,w}$$ and $$\omega_{vv}$$ can be recovered.
