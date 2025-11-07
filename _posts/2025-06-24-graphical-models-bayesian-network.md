---
layout: post
title: "Graphical Models: Bayesian Network"
date: 2025-06-24
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Bayesian Network]
comments: true
---

**Probabilistic Notation**  

Let $$X_1,\dots,X_m$$ be categorical r.v.’s with $$X_v\in[d_v]=\{1,\dots,d_v\}$$.  Denote the joint state space  
$$
I = \prod_{v=1}^m [d_v], 
\quad
\lvert I\rvert = \prod_{v=1}^m d_v,
$$
and write  
$$
p(i) = P(X_1=i_1,\dots,X_m=i_m),\quad i=(i_1,\dots,i_m)\in I.
$$

### Definition  

Let $$G=(V,E)$$ be a DAG with $$V=[m]$$.  
The **Bayesian network** (discrete graphical model) given by $$G$$ is  
$$
P(G) \;=\;\{\,p\in\Delta^{|I|-1}:\;p\text{ satisfies the global M.P.\ for }G\}
\;=\;\{\,p\in\Delta:\;p\text{ factorizes according to }G\}.
$$  
A valid **factorization** is  
$$
p(i) \;=\;\prod_{v\in V} k_v\bigl(i_v,\;i_{\pa(v)}\bigr),
$$
where each kernel  
$$
k_v(i_v,i_{\pa(v)})=P(X_v=i_v\mid X_{\pa(v)}=i_{\pa(v)})
$$
is a conditional probability table satisfying  $$\;0\le k_v(\cdot)\le1,\;\sum_{i_v=1}^{d_v}k_v(i_v,i_{\pa(v)})=1$$. 

### Proposition  

The dimension of the model $$P(G)$$ is
$$
\dim P(G)
\;=\;
\sum_{v\in V}\Bigl(d_v-1\Bigr)\prod_{w\in\pa(v)}d_w.
$$
In the binary case ($$d_v=2$$), this simplifies to
$$
\dim P(G)
\;=\;
\sum_{v\in V}2^{|\pa(v)|}.
$$


### Example: All Variables Binary  

- **Case (a):**  
  A network on $$V=\{1,2,3,4\}$$ with  
$$
    p(i)
    = p(i_1)\,p(i_2\mid i_1)\,p(i_3\mid i_1)\,p(i_4\mid i_2,i_3)
  $$
  has parameters  
  $$\;k_1(1),\;k_2(1\mid1),k_2(1\mid2),\;k_3(1\mid1),k_3(1\mid2),\;k_4(1\mid i_2,i_3)\;(4\text{ values})$$,  
  totaling $$1+2+2+4=9$$ free parameters. 
- **Case (b):**  
  A different DAG on 4 nodes yields  
  $$\dim P(G)=1+2+4+4=11$$.  If the DAG is *perfect*, $$P(G)$$ coincides with the extended log‐linear model on its skeleton, which also has dimension $$11$$. 


### Likelihood Function  

Given IID data $$\{X^{(j)}\}_{j=1}^n$$ with counts  
$$
N(i)=\#\{\,j:X^{(j)}=i\},\quad\sum_{i\in I}N(i)=n,
$$
the joint likelihood is  
$$
L(p)
=\prod_{i\in I}p(i)^{\,N(i)}.
$$

### Decomposition of the Likelihood  

Under the DAG factorization,
$$
L(p)
=\prod_{i\in I}
\Bigl[\prod_{v\in V}p(i_v\mid i_{\pa(v)})\Bigr]^{N(i)}
=\prod_{v\in V}\;\prod_{i_{\pa(v)}\in I_{\pa(v)}}
\;\prod_{i_v=1}^{d_v}p(i_v\mid i_{\pa(v)})^{N(i_v,i_{\pa(v)})},
$$
where 
$$\;N(i_v,i_{\pa(v)})=\#\{\,j:X^{(j)}_v=i_v,\;X^{(j)}_{\pa(v)}=i_{\pa(v)}\}.$$ 


### Maximum Likelihood Estimation  

By the information inequality, each block  
$$
L_{v,i_{\pa(v)}}(p)
=\prod_{i_v}p(i_v\mid i_{\pa(v)})^{N(i_v,i_{\pa(v)})}
$$
is maximized by  
$$
\hat p(i_v\mid i_{\pa(v)})
=\frac{N(i_v,i_{\pa(v)})}{\displaystyle\sum_{i_v}N(i_v,i_{\pa(v)})}
=\frac{N(i_v,i_{\pa(v)})}{N(i_{\pa(v)})}.
$$
Thus the MLE factorizes as  
$$
\hat p(i)
=\prod_{v\in V}\hat p(i_v\mid i_{\pa(v)}),
$$
with $$\hat p(i_v\mid i_{\pa(v)})$$ as above (and arbitrary if $$N(i_{\pa(v)})=0$$). 
