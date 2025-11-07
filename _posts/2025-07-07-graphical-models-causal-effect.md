---
layout: post
title: "Graphical Models: Causal Effect"
date: 2025-07-07
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Causal Effect]
comments: true
---

### Setup  

Let $$G=(V,E)$$ be a DAG and assume the causal Markov property so that
$$
f(x)=\prod_{v\in V}f(x_v\mid x_{\pa(v)}),\quad f(x)>0.
$$

### Interventional Distributions  

For $$A\subseteq V$$, the interventional density under $$\doOp(X_A=x_A)$$ is
$$
f\bigl(x;\,\doOp(X_A=x_A)\bigr)
=\prod_{v\notin A}f(x_v\mid x_{\pa(v)})\;\times\;\prod_{v\in A}\mathbf1\{x_v=x_A\}.
$$


### Causal Effect

**Definition**  
Let $$T,R\subseteq V$$.  The *causal effect* of $$X_T$$ on $$X_R$$ is the map
$$
x_T\;\mapsto\;P\bigl(X_R\in\cdot\;;\,\doOp(X_T=x_T)\bigr).
$$
Often for $$\lvert T\rvert=\lvert R\rvert=1$$ one considers:
- If $$X_T$$ is binary, the *average treatment effect*
  $$\;E[X_R;\doOp(X_T=1)] - E[X_R;\doOp(X_T=0)].$$
- In a linear SCM, $$E[X_R;\doOp(X_T=x_T)]=\omega_0+\omega x_T$$, and $$\omega$$ is the *total effect*.



### Total Effects in Linear Structural Causal Models

Consider the linear SEM
$$
X_v = \omega_{0v} + \sum_{u\in\pa(v)}\omega_{v u}\,X_u + \vartheta_v,\quad v\in V,
$$
where $$\{\vartheta_v\}$$ are independent mean-zero errors.  Set $$B=(\omega_{v u})$$ and let the *path matrix* be $$(I-B)^{-1}$$.

**Example**  
For $$V=\{1,2,3\}$$ with
$$
X_1=\omega_{01}+\vartheta_1,\quad
X_2=\omega_{02}+\omega_{21}X_1+\vartheta_2,\quad
X_3=\omega_{03}+\omega_{31}X_1+\omega_{32}X_2+\vartheta_3,
$$
the total effects $$C(t,r)$$ are:
$$
C(1,2)=\omega_{21},\;
C(1,3)=\omega_{31}+\omega_{32}\,\omega_{21},\;
C(2,3)=\omega_{32},
$$
and zero for reverse ordering.

**Proposition**  
In this linear SCM, the total effect of $$X_t$$ on $$X_r$$ is
$$
C(t,r)\;=\;(I-B)^{-1}_{r t}
\;=\;
\sum_{\pi\in\Pi(t,r)}\;\prod_{(v\to w)\in\pi}\beta_{w v},
$$
where $$\Pi(t,r)$$ is the set of directed paths from $$t$$ to $$r$$.

### Adjust for Source Nodes


Remark
Causal effect of Xt on XR depends on the conditional distribution of X_R\mid X_t

### Adjusting for Parents

**Theorem**  
Let $$t\in V$$ and $$R\subseteq V\setminus(\{t\}\cup\pa(t))$$.  Then
$$
f\bigl(x_R;\,\doOp(X_t=x_t)\bigr)
=\int 
f\bigl(x_R\mid x_t,x_{\pa(t)}\bigr)\,
f\bigl(x_{\pa(t)}\bigr)\,
d\mu_{\pa(t)}(x_{\pa(t)}).
$$

**Remark**  
This shows the causal effect of $$X_t$$ on $$X_R$$ is identified by the marginal law of $$(X_t,X_{\pa(t)},X_R)$$.

### Identification of Causal Effects

Suppose we observe the vairables $$X_T$$, $$X_R$$ and $$X_C$$ for a set $$C \subseteq V \setminus [T \cup R].$$
1. When does covariate adjustment work? That is, when do we have
$$
f(x_R; \doOp(X_T = x_T^*)) = \int f(x_R \mid x_T^*, x_C) f(x_C) \, \mathrm{d} \mu_C(x_C)?
$$
2. Is the causal effect of $$X_T$$ on $$X_R$$ identifiable? That is, is $$P(X_R \in \cdot \, ; \doOp(X_T = x_T^*))$$ uniquely determined by the marginal distribution of $$(X_T, X_R, X_C)$$?