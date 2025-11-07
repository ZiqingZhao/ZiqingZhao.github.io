---
layout: post
title: "Graphical Models: Learning Undirected Graphs"
date: 2025-07-27
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Learning Undirected Graphs]
comments: true
---

### Testing Conditional Independence

**Conditional Independence Graph**  
Let $$X=(X_1,\dots,X_m)$$ have distribution $$P_X$$.  
The **conditional independence graph** $$G_X=(V,E)$$ has
$$
V=\{1,\dots,m\},
\quad
\{v,w\}\in E\;\Longleftrightarrow\;X_v\;\perp\!\!\!\perp\;X_w\;\bigm|\;X_{V\setminus\{v,w\}}.
$$
In the Gaussian case this is also called the **concentration graph** and—assuming the intersection axiom holds—$$G_X$$ is the smallest undirected graph whose Markov model $$M(G_X)$$ contains $$P_X$$. 

**Structure Learning via Hypothesis Testing**  
For each pair $$\{v,w\}$$, test
$$
H^0_{vw}: X_v\indep  X_w\mid X_{V\setminus\{v,w\}}
\quad\text{vs.}\quad
H^1_{vw}: X_v\not\indep  X_w\mid X_{V\setminus\{v,w\}}.
$$
Rejecting $$H^0_{vw}$$ leads to inclusion of edge $$\{v,w\}$$ in $$\hat G$$; multiple‐testing adjustments control overall error rates.

**Gaussian Conditional Independence Test**  
Assume $$X^{(1)},\dots,X^{(n)}\overset{iid}{\sim}N_m(\mu,\Sigma)$$ with empirical covariance $$S$$.  Then
$$
X_v\perp\!\!\!\perp X_w\mid X_{V\setminus\{v,w\}}
\;\Longleftrightarrow\;
\omega_{vw}=0,
\quad
\omega_{vw}=-\frac{(\Sigma^{-1})_{vw}}{\sqrt{(\Sigma^{-1})_{vv}(\Sigma^{-1})_{ww}}}.
$$
A consistent estimator is the sample partial correlation
$$
r_{vw}
=\frac{(S^{-1})_{vw}}{\sqrt{(S^{-1})_{vv}(S^{-1})_{ww}}},
$$
for which under $$H^0$$ one may use either a $$t$$-test $$r_{vw}\sqrt{(n-m)/(1-r_{vw}^2)}\sim t_{n-m}$$ or a Fisher $$z$$-transform $$z(r_{vw})\approx N(0,1)$$ 

### Information Criteria

Instead of testing, one may select $$G$$ by optimizing a penalized‐likelihood score.  For model $$M(G)$$ and data $$X$$,
$$
\mathrm{BIC}(G)
=2\sup_{P\in M(G)}\log L(P)
\;-\;\dim(M(G))\log n.
$$
Maximizing BIC over all $$G$$ is feasible for small $$m$$; for larger $$m$$, greedy or stepwise search (or stronger penalties like extended BIC) is used 

###  Neighborhood Selection

**Pseudo‐Likelihood Approach**  
For each node $$v$$, regress $$X_v$$ on the other variables $$X_{V\setminus\{v\}}$$ (e.g. linear for Gaussian, logistic for binary).  Use $$\ell_1$$‐penalized variable selection (Lasso) to estimate $$\widehat{\mathrm{ne}}(v)$$.  Combine local neighborhoods via AND/OR rules:
$$
\widehat{\mathrm{ne}}_{\mathrm{AND}}(v)
=\{w: w\in\widehat{\mathrm{ne}}(v)\;\wedge\;v\in\widehat{\mathrm{ne}}(w)\},
\quad
\widehat{\mathrm{ne}}_{\mathrm{OR}}(v)
=\{w: w\in\widehat{\mathrm{ne}}(v)\;\vee\;v\in\widehat{\mathrm{ne}}(w)\}.
$$
### Chow–Liu Algorithm

**Tree‐Structured Model:**  
When $$G$$ is a tree, the joint density factorizes as
$$
f(x)
=\prod_{\{v,w\}\in E}\frac{f(x_v,x_w)}{f(x_v)f(x_w)}
\prod_{v\in V}f(x_v).
$$
The MLE tree maximizes
$$
\sum_{\{v,w\}\in E} \widehat I(v,w),
\quad
\widehat I(v,w)
=\sum_{i_v,i_w}\hat p(i_v,i_w)\,\log\frac{\hat p(i_v,i_w)}{\hat p(i_v)\,\hat p(i_w)},
$$
i.e. a maximum‐weight spanning tree with weights $$\widehat I(v,w)$$.  By Kruskal’s or Prim’s algorithm this is $$O(m^2\log m)$$. 

**Gaussian Case:**  
For Gaussian data one shows $$\widehat I(v,w)=\tfrac12\log(1-r_{vw}^2)$$, so the same spanning‐tree approach with edge weight $$\lvert r_{vw}\rvert$$ recovers the MLE tree. 

### Bayesian Approaches

Bayesian methods place priors on $$G$$ and/or parameters and compute posterior probabilities for edges or graphs.  