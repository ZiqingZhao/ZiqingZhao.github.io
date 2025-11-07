---
layout: post
title: "Graphical Models: Markov Properties Undirected Graphs"
date: 2025-05-06
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Markov Properties Undirected Graphs]
comments: true
---

## Undirected Graphs and Graphical Modeling

- **Undirected graph** $$G=(V,E)$$:  
  • $$V$$ a finite vertex set,  
  • $$E\subseteq\binom V2$$ a set of edges. 
- **Adjacency:** $$v,w\in V$$ are adjacent ($$v\!-\!w\in E$$) denoted $$v\sim_G w$$.
- **Complete graph:** $$E=\binom V2$$.  
  **Clique:** $$A\subseteq V$$ induces a complete subgraph $$G_A$$.  
  **Maximal clique:** a clique not strictly contained in any larger clique. 
- **Neighborhood:**  
  $$
    \mathrm{ne}(v)=\{w\in V: v\sim_G w\},\quad
    \mathrm{ne}(A)=\bigl(\bigcup_{v\in A}\mathrm{ne}(v)\bigr)\setminus A.
  $$
  **Closure:** $$\mathrm{cl}(A)=A\cup\mathrm{ne}(A)$$. 
- **Graphical model setup:**  
	- Random vector $$X=(X_v)_{v\in V}$$ with joint law $$P_X$$.  
	- For $$A\subseteq V$$, $$X_A=(X_v)_{v\in A}$$.  
	- Shorthand for conditional independence:  
    $$
      A\indep B\mid C\quad\iff\quad X_A\indep X_B\mid X_C.
    $$
## Markov Properties

1. **Pairwise Markov property:**  Two non-adjacent variables are conditionally independent given all other variables
   $$
     \forall\,v-w\notin G: \quad v\indep w\mid V\setminus\{v,w\}.
   $$

2. **Local Markov property:**  A node is independent of its non-neighbors given its neighbors
   $$
     \forall\,v\in V:\quad
     v\indep \;V\setminus\mathrm{cl}(v)\;\mid\mathrm{ne}(v).
   $$
3. **Global Markov property:**  Two subsets of variables are conditionally independent given a separating subset.
   For disjoint $$A,B,C\subseteq V$$, if $$C$$ separates $$A$$ and $$B$$ in $$G$$ (every path from $$A$$ to $$B$$ meets $$C$$), then 
$$
	A \indep B\mid C\quad\forall A,B,C\subseteq V \text{ disjoint, } A,B\ne\emptyset,\text{ s.t. } C\text{ seperates } A \text{ and }B
$$
- **Separation:** A set $$C$$ separates $$A$$ and $$B$$ if every path in $$G$$ from a vertex in $$A$$ to one in $$B$$ 
- **Implications:**  
  - Local $$\implies$$ Pairwise (via weak union) 
  - Global $$\implies$$ Local (since $$\mathrm{ne}(v)$$ separates $$v$$ from $$V\setminus\mathrm{cl}(v)$$) 

## Equivalence of Markov Properties

- **Intersection property (C5):**  
  If positive density of B, C, D $$f(X_B, X_C, X_D)>0$$, then  
$$
A\indep B\mid C\cup D \text{ and } A\indep C\mid B\cup D\iff A\indep (B\cup C)\mid D
$$ 
- **Theorem (Lauritzen 1996):**  
  If $$(C5)$$ holds (e.g. $$P_X$$ has a positive density), then the Pairwise, Local and Global Markov properties are equivalent

## Failure and Completeness

- **Non-equivalence when (C5) fails:**  
  Binary examples on small graphs satisfy Pairwise but not Local/Global MP.

- **Completeness of Global MP:**  
  If $$C$$ does _not_ separate $$A,B$$, there exists a Gaussian $$N(0,K^{-1})$$ with  
$$
    K_{vv}=1,\quad
    K_{v_iv_{i+1}}=\varepsilon\ (i=1,\dots,n-1),\quad
    K_{vw}=0\ \text{otherwise},
  $$
  which obeys Global MP yet _not_ $$A\indep B\mid C$$. 
## Further Concepts

- **Independence model:**  
  $$\mathcal I(P_X)=\{(A,B,C):X_A\perp X_B\mid X_C\}$$.

- **I‐map:** Graph $$G$$ is an I‐map of $$P_X$$ if $$\mathcal I(P_X)\supseteq\mathcal I_{\mathrm{global}}(G)$$.

- **Perfect map:** $$G$$ is perfect for $$P_X$$ if $$\mathcal I(P_X)=\mathcal I_{\mathrm{global}}(G)$$. 
