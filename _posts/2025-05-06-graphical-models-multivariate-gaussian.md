---
layout: post
title: "Graphical Models: Multivariate Gaussian"
date: 2025-05-06
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Multivariate Gaussian]
comments: true
---

## Multivariate Normal Distribution

#### Definition

**Standard Normal (univariate)**  
A random variable $X$ is standard normal, $X\sim N(0,1)$, if its density is  
$$
\phi(x)=\frac{1}{\sqrt{2\pi}}e^{-x^2/2},\quad x\in\mathbb R.
$$
For $Y=aX+b$, $a\neq0$,  
$$
Y\sim N\bigl(b,\,a^2\bigr).
$$
**Multivariate Standard Normal**  
A vector $X=(X_1,\dots,X_p)^T$ is standard normal in $\mathbb R^p$ if its entries are iid $N(0,1)$. Its joint density is  
$$
\phi_p(x)=\frac{1}{(2\pi)^{p/2}}\exp\bigl(-\tfrac12\|x\|^2\bigr).
$$ 
**Orthogonal Invariance**  
If $Q\in\mathbb R^{p\times p}$ is orthogonal ($Q^TQ=I$) and $X\sim N_p(0,I)$, then $QX\sim N_p(0,I)$. 

**General Multivariate Normal**  
$Y\in\mathbb R^p$ is multivariate normal if there exist $X\sim N_k(0,I)$, $A\in\mathbb R^{p\times k}$, $b\in\mathbb R^p$ such that  
$$
Y = AX + b.
$$

#### Covariance Matrix

**Definition**  
For $Y=(Y_1,\dots,Y_p)^T$,  
$$
\mu = E[Y], 
\quad
\Sigma = \mathrm{Var}[Y] = E\bigl[(Y-\mu)(Y-\mu)^T\bigr],
$$
a symmetric positive semidefinite matrix.

**Properties**  
- If $X\sim N_p(0,I)$, then $\mathrm{Var}[X]=I$.  
- For $Y=AX+b$,  
  $$
  E[Y] = A\,E[X]+b,\quad
  \mathrm{Var}[Y] = A\,\mathrm{Var}[X]\,A^T = A A^T.
  $$
- $\Sigma$ singular $\iff$ $Y$ lies a.s. in a hyperplane. 

---

#### Theorem: Mean & Covariance Determine Normal Law

If $X,Y$ are both $N_p(\mu,\Sigma)$, they have identical distribution.  

#### Density (for $\Sigma\succ0$)

Define $N_p(\mu,\Sigma)$ for any $\Sigma\succeq0$. 

$$
f_{\mu,\Sigma}(x)
=\frac{1}{\sqrt{(2\pi)^p\det\Sigma}}
\exp\Bigl(-\tfrac12(x-\mu)^T\Sigma^{-1}(x-\mu)\Bigr).
$$

## 3.2 Marginal and Conditional Distributions

**Linear Transformations**  
If $X\sim N_p(\mu,\Sigma)$ and $A\in\mathbb R^{k\times p}, b\in\mathbb R^k$, then  
$$
AX + b \;\sim\; N_k\bigl(A\mu+b,\;A\Sigma A^T\bigr).
$$
**Marginals**  
Partition $X=(X_1,X_2)$, $\mu=(\mu_1,\mu_2)$, $\Sigma=\begin{pmatrix}\Sigma_{11}&\Sigma_{12}\\\Sigma_{21}&\Sigma_{22}\end{pmatrix}$.  
Then  
$$
X_1\sim N\bigl(\mu_1,\Sigma_{11}\bigr).
$$
**Conditional Distribution**  
If $\Sigma\succ0$, then  
$$
X_1\mid X_2=x_2
\;\sim\;
N\!\Bigl(\mu_1 + \Sigma_{12}\Sigma_{22}^{-1}(x_2-\mu_2),\;
\Sigma_{11\cdot2}\Bigr),
$$
where the Schur complement  
$\Sigma_{11\cdot2} = \Sigma_{11} - \Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21}\succ0$. 

The joint density factorizes as  
$$
f_{\mu,\Sigma}(x)
= f_{\mu_1+\Sigma_{12}\Sigma_{22}^{-1}(x_2-\mu_2),\,\Sigma_{11\cdot2}}(x_1)\;\times\;
f_{\mu_2,\Sigma_{22}}(x_2).
$$
## Independence and Conditional Independence

**Independence of Blocks**  
With the above partition, $X_1\perp\!\!\perp X_2\iff\Sigma_{12}=0$. 

**Conditional Independence**  
For indices $i\neq j$, let $C=[p]\setminus\{i,j\}$. The conditional covariance of $(X_i,X_j)\mid X_C$ is  
$$
\Sigma_{\{i,j\}\mid C}
= \Sigma_{\{i,j\},\{i,j\}}
- \Sigma_{\{i,j\},C}\,\Sigma_{C,C}^{-1}\,\Sigma_{C,\{i,j\}}.
$$
Then  
$$
X_i\perp\!\!\perp X_j\mid X_C
\;\iff\;
\bigl(\Sigma_{\{i,j\}\mid C}\bigr)_{12}=0.
$$

**Inverse Covariance (Precision) Matrix**  
Let $K=\Sigma^{-1}$. Then  
$$
X_i\perp\!\!\perp X_j\mid X_{[p]\setminus\{i,j\}}
\;\iff\;
K_{ij}=0.
$$
**Partial Correlation**  
The partial correlation of $X_i$ and $X_j$ given all others is  
$$
\rho_{ij} 
= -\frac{K_{ij}}{\sqrt{K_{ii}K_{jj}}}.
$$
**Determinantal Criterion**  
Equivalently,
$$
X_i\perp\!\!\perp X_j\mid X_C
\;\iff\;
\det\bigl(\Sigma_{\{i\}\cup C,\;\{j\}\cup C}\bigr)=0,
$$
since $\det(\Sigma_{C,C})>0$. 
