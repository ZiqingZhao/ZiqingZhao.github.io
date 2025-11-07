---
layout: post
title: "Graphical Models: Back Door Criterion"
date: 2025-07-07
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Back Door Criterion]
comments: true
---

### Setup  

- Let $$G=(V,E)$$ be a DAG with vertex set $$V=\{1,\dots,m\}$$.  
- Let $$X=(X_1,\dots,X_m)$$ have joint density $$f$$ w.r.t.\ product measure $$\mu=\bigotimes_{v\in V}\mu_v$$.  

Assume the interventional distributions satisfy the causal Markov property for $$G$$.  
- **Goal:** Estimate the causal effect of $$X_T$$ on $$X_R$$ from observational data on $$(X_T,X_R,X_C)$$ with $$T,R,C\subseteq V$$, $$T\cap R=\emptyset$$ 

### Identification of Causal Effects  

Suppose we observe $$(X_T,X_R,X_C)$$ for $$C\subseteq V\setminus(T\cup R)$$.  
1. **When does covariate adjustment work?**  That is, when does  
$$
   f(x_R;\doOp(X_T=x_T))
   \;=\;
   \int f(x_R\mid x_T,x_C)\,f(x_C)\,d\mu_C(x_C)
   \tag{A}
$$
2. **Is the causal effect identifiable?**  I.e.\ is $$f(x_R;\do(X_T=x_T))$$ uniquely determined by the marginal of $$(X_T,X_R,X_C)$$? :

### Pearl’s Back-Door Criterion  

**Definition**  
A **back-door path** from $$t$$ to $$r$$ is any path beginning $$t\leftarrow \cdots \to r$$.  
A set $$C\subseteq V\setminus\{t,r\}$$ satisfies the **back-door criterion** for $$(t,r)$$ if  
1. $$C\cap \de_G(t)=\emptyset$$,  
2. every back-door path from $$t$$ to $$r$$ is d-blocked by $$C$$.  

**Theorem 2**  
If $$C$$ satisfies the back-door criterion for $$(t,r)$$, then the adjustment formula (A) holds.  
Moreover, if $$C=\emptyset$$,  
$$
f(x_r;\doOp(X_t=x_t)) \;=\; f(x_r\mid x_t).
$$
The back-door criterion is sufficient but not necessary for the adjustment formula to hold. A simple counter example is $$c\leftarrow t\rightarrow r$$.


**Theorem 3 (Adjustment criterion of Shpitser et al., 2012)**
Let $$C \subseteq V \setminus \{r, t\}$$. The covariate adjustment formula (A) holds for all densities $$f$$ that factorize according to the considered DAG $$G$$ if and only if
- for all $$v \neq t$$ that lie on a directed path from $$t$$ to $$r$$, we have $$C \cap \operatorname{de}(v) = \emptyset$$;
- every path from $$t$$ to $$r$$ that is d-connecting given $$C$$ is a directed path from $$t$$ to $$r$$.


### Intervention Graphs

**Definition**  
For each $$v\in A\subseteq V$$, introduce an **intervention node** $$F_v$$.  The **intervention graph** has vertex set $$V\cup\{F_v:v\in A\}$$ and edges $$E\cup\{F_v\to v:v\in A\}$$.  
Define the augmented conditionals  
$$
f'(x_v\mid x_{\pa(v)},F_v=i_v)
=
\begin{cases}
f(x_v\mid x_{\pa(v)}),&i_v=\star,\\
\mathbf1\{x_v=i_v\},&i_v=x_v.
\end{cases}
$$  
**Key Fact:**  For any $$B\subseteq A$$,  
$$
f(x;\do(X_B=x_B))
=
f'\bigl(x\mid F_B=x_B^*,\;F_v=\emptyset, \; v\in A\setminus B \bigr).
$$

### Adjustment Criterion for Joint Interventions  

**Theorem 5 (Adjustment criterion)**  
Let $$T,R,C\subseteq V$$ be pairwise disjoint.  The covariate adjustment formula (A) holds for **all** densities $$f$$ that factorize w.r.t. $$G$$ if and only if:  
1. For every $$v\notin T$$ on a $$T$$-proper directed path to $$R$$, no element of $$C$$ is a descendant of $$v$$ in the mutilated graph $$G_{\do(T)}$$.  
2. Every path from any $$t\in T$$ to any $$r\in R$$ that is d-connecting given $$C$$ is either directed from $$T$$ to $$R$$ or is not $$T$$-proper.  

*Definition:* A path is **$$T$$-proper** if its only node in $$T$$ is its starting node. 

With joint interventions, even when all variables in the given DAG are jointly observed, there might be no set, for which the adjustment formula works.

###  Efficiency of Estimators  

In **linear Gaussian** SCMs, whenever (A) applies one can estimate the total effect of $$X_t$$ on $$X_r$$ by the regression coefficient of $$X_t$$ in the linear regression of $$X_r$$ on $$(X_t,X_C)$$.  Different valid adjustment sets $$C$$ yield estimators with different asymptotic variances; choosing a minimal or parent set often improves efficiency. 
