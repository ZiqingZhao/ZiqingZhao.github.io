---
layout: post
title: "Graphical Models: Learning Dags"
date: 2025-07-27
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Learning Dags]
comments: true
---

### Learning Markov Equivalence Classes  
- Let $$X=(X_1,\dots,X_m)$$ be a random vector with distribution $$P_X$$.  
- Assume $$P_X\in\mathcal{M}(G_X)$$ for some DAG $$G_X$$.  
- $$G_X$$ is **minimal** for $$P_X$$:  
$$
    P_X\in\mathcal{M}(G_X),\quad
    P_X\notin\mathcal{M}(G)\;\forall\,G\supsetneq G_X.
  $$
- **Goal**: Estimate $$G_X$$ from i.i.d.\ data $$X^{(1)},\dots,X^{(n)}$$.  
- **Issue 1**: There may be other DAGs $$\widetilde G_X$$ with  
  $$\mathcal{M}(\widetilde G_X)=\mathcal{M}(G_X)$$.  
- **Conclusion 1**: Without extra assumptions, one can only recover the Markov equivalence class $$[G_X]$$, represented by a CPDAG (§13). 

*Example*  
- True DAG and its CPDAG (diagrams).  
- Algorithms to infer a CPDAG from observational data:  
  - **Constraint-based**: PC algorithm (tests conditional independencies)  
  - **Score-based**: Greedy Equivalence Search (optimizes an information criterion)  
  - **Hybrid** methods 

### Family of DAG Models Not Closed Under Intersection  
- **Issue 2**: A single $$P_X$$ may lie in $$\mathcal{M}(G_1)\cap\mathcal{M}(G_2)$$ with $$[G_1]\neq[G_2]$$, yet no smaller DAG $$G$$ satisfies  $$\mathcal{M}(G)=\mathcal{M}(G_1)\cap\mathcal{M}(G_2)$$.  
- Undirected graphical models do admit such intersections.  
- **Conclusion 2**: Uniqueness of the minimal DAG model must be assumed to guarantee consistency of any estimator. 

*Counterexample*  
- DAGs $$G_1,G_2$$ with local MP requiring:  
$$
    1\indep 4,\quad
    2\indep 4\mid1,\quad
    4\indep \{1,2\}
    \quad\text{vs.}\quad
    1\indep \{3,4\},\;
    3\indep 4\mid1,\;
    4\indep 1.
$$
- Claim: ∃ distribution $$P_X$$ such that  
$$
    X_1\indep  X_2\mid X_{3,4},\quad
    X_2\indep  X_3\mid X_{1,4},\quad
    X_3\indep  X_4\mid X_{1,2},
$$
  with $$P_X\in\mathcal{M}(G_1)\cap\mathcal{M}(G_2)$$ but in no smaller model.  
- **Proof sketch**: Embed into a larger DAG $$G_L$$ with an extra node 5 so that those independencies correspond exactly to d-separations in $$G_L$$; by completeness there is $$P\in\mathcal{M}(G_L)$$; marginalize out node 5 to get $$P_X$$. 
### Score-Based Search  
- Learn a DAG by optimizing an information criterion (e.g., BIC).  
- **Exact** global search is feasible for small/moderate $$m$$; **greedy** search for larger $$m$$.
- **Decomposable BIC**:  
  $$
    \mathrm{BIC}
    = \sum_{v\in V} \mathrm{BIC}_v\bigl(X_v,\,X_{\mathrm{pa}(v)}\bigr),
  $$
  since both log-likelihood and dimension decompose over the local conditionals. 

#### PC Algorithm  
- **Faithfulness assumption**:  
$$
    X_v\indep  X_w\mid X_C
    \;\Longrightarrow\;
    C\text{ d-separates }v,w\text{ in }G_X.
$$
- **Skeleton recovery**: Start with the complete graph; for each adjacent pair $$(v,w)$$ and increasing $$\lvert C\rvert$$, remove $$v–w$$ if $$X_v\indep  X_w\mid X_C$$. 
- **Edge orientation**:  
  1. **Unshielded colliders**: Orient $$u–v–w$$ as $$u\to v\leftarrow w$$ whenever $$v\notin C(u,w)$$.  
  2. **Meek’s rules** (no new colliders, no cycles):  
     - **R1**: If $$i\to j$$ and $$j–k$$ with $$i,k$$ nonadjacent, orient $$j–k$$ as $$j\to k$$.  
     - **R2**: If a directed path $$i\to k\to j$$ exists, orient $$i–j$$ as $$i\to j$$.  
     - **R3**: If chains $$i–k\to j$$ and $$i–l\to j$$ exist with $$k,l$$ nonadjacent, orient $$i–j$$.  
     - **R4**: If chains $$i–k\to l$$ and $$k\to l\to j$$ exist with $$k,j$$ nonadjacent, orient $$i–j$$. 
- **Consistency**: PC is consistent given correct CI tests and faithfulness.  
- **Caveats & extensions**: Faithfulness may fail; hybrid/FCI algorithms handle late
