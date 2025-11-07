---
layout: post
title: "Graphical Models: Causal Models"
date: 2025-07-01
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Causal Models]
comments: true
---

### Interventional Distributions  

A **family of interventional distributions** is a collection  
$$
\bigl\{\,P_{A,x_A}\colon A\subseteq V,\ x_A\in\mathbb R^A\bigr\},
$$
where each $$P_{A,x_A}$$ is a joint law for $$X=(X_v)_{v\in V}$$ such that under $$P_{A,x_A}$$,  
$$
X_A = x_A\quad\text{with probability }1.
$$
Notation:  
$$
P\bigl(X\in\cdot\,;\,\doOp(X_A=x_A)\bigr)=P_{A,x_A}.
$$
### Causal Markov Property  

**Definition**  
A family of interventional distributions satisfies the **causal Markov property** for the DAG $$G=(V,E)$$ if for every $$A\subseteq V$$ and $$x_A\in\mathbb R^A$$:  
1. $$P\bigl(X\in\cdot\,;\,\doOp(X_A=x_A)\bigr)$$ factorizes according to $$G$$.  
2. For all $$v\notin A$$, the interventional conditional law equals the observational one:
$$
P\bigl(X_v\in\cdot\mid X_{\pa(v)}=x_{\pa(v)};\doOp(X_A=x_A)\bigr)
=
P\bigl(X_v\in\cdot\mid X_{\pa(v)}=x_{\pa(v)}\bigr),
$$
whenever the conditioning values agree with the intervention. 

### Truncated Factorization  

Let $$f$$ be the observational joint density (w.r.t.\ $$\mu=\bigotimes_{v}\mu_v$$), and write  
$$f(x_v\mid x_{\pa(v)})$$ for the conditional densities.  

**Proposition**  
A family of interventional distributions satisfies the causal Markov property if and only if  
$$
f\bigl(x;\,\doOp(x_A=x_A)\bigr)
=
\prod_{v\notin A} f(x_v\mid x_{\pa(v)})
\;\times\;
\prod_{v\in A} \mathbf{1}\{x_v = x_A\},
$$
with the dominating measure $$\mu_{V\setminus A}\times$$ counting on $$\mathbb R^A$$. 

### Mutilated DAG  

**Definition**  
For a DAG $$G=(V,E)$$ and $$A\subseteq V$$, the **mutilated DAG** $$G_{\doOp(A)}=(V,E_{\doOp(A)})$$ has  
$$
E_{\doOp(A)} \;=\; E\setminus\{\,w\to v: v\in A\}.
$$

**Proposition**  
Under the intervention $$\doOp(X_A=x_A)$$, the interventional distribution factorizes according to the mutilated DAG:
$$
P\bigl(X\in\cdot\,;\,\doOp(X_A=x_A)\bigr)
\quad\text{factorizes on }G_{\doOp(A)}.
$$

### Structural Equation Models  

A **structural equation model** (SEM) on $$G=(V,E)$$ assumes
$$
X_v \;=\; g_v\bigl(X_{\pa(v)},\,\omega_v\bigr),\quad v=1,\dots,m,
$$
where the “noise” variables $$\omega_1,\dots,\omega_m$$ are random.  

**Proposition**  
If $$\omega_1,\dots,\omega_m$$ are independent, then the joint law of the unique solution $$X$$ satisfies the local Markov property for $$G$$. 

**Fact**  
Conversely, every distribution $$P_X$$ satisfying the local Markov property for $$G$$ arises from some SEM with independent errors (e.g.\ one may take $$\omega_v\sim\mathrm{Unif}(0,1)$$ and choose $$g_v$$ as the quantile map of $$P(X_v\mid X_{\pa(v)})$$). 

### Structural Causal Models  

Interpreting SEMs causally, one writes assignments  
$$
X_v := g_v\bigl(X_{\pa(v)},\omega_v\bigr)
$$
and under $$\doOp(X_A=x_A)$$ replaces each equation for $$v\in A$$ by  
$$
X_v := x_A.
$$
The resulting variables $$X(\doOp(X_A=x_A))$$ are called **potential outcomes**. 
