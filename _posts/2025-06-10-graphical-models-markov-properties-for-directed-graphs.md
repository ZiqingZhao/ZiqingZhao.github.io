---
layout: post
title: "Graphical Models: Markov Properties For Directed Graphs"
date: 2025-06-10
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Markov Properties For Directed Graphs]
comments: true
---

## Directed Graphs

**Definition.** A **directed graph** is a pair $G=(V,E)$ consisting of a finite set $V$ and an edge set $E\subseteq V\times V$. We consider only directed graphs without self‐loops, i.e.\  
$$
E\;\cap\;\{(v,v):v\in V\} \;=\;\emptyset.
$$

**Adjacency** 
$v,w\in V$ are adjacent if $(v,w)\in E$ or $(w,v)\in E$.  

**Directed edge** 
We write $v\to w\in E$ to express an edge from tail $v$ to head $w$.  

**Complete graph** 
$G$ is complete if every pair of distinct vertices is adjacent.

**Subgraphs**
A **subgraph** of $G=(V,E)$ is $G'=(V',E')$ such that  
$$
V'\subseteq V,\quad E'\subseteq E\cap (V'\times V').
$$  
The **induced subgraph** by $A\subseteq V$ is  
$$
G_A=(A,\;E\cap(A\times A)).
$$

**Walks and Paths**
- A **walk** is a sequence  $\omega=(v_1,e_1,v_2,\dots,e_{n-1},v_n)$  with $v_i\in V$, $e_i\in E$, and $e_i=(v_i,v_{i+1})$ or $(v_{i+1},v_i)$.  
	- Endpoints: $v_1,v_n$.  
	- Length = number of edges.  
- A **path** is a walk with all vertices distinct.  
- A **directed walk** (or path) has $e_i=(v_i,v_{i+1})$ for all $i$.

**Relations Among Vertices**
For $v,w\in V$:  
- If $v\to w\in E$, then  
  - $v$ is a **parent** of $w$,  
  - $w$ is a **child** of $v$.  
$$
  \mathrm{pa}(v)=\{w: w\to v\in E\},\quad
  \mathrm{ch}(v)=\{w: v\to w\in E\}.
$$
- If there is a directed path from $v$ to $w$, then  
  - $v$ is an **ancestor** of $w$,  
  - $w$ is a **descendant** of $v$.  
$$
  \mathrm{an}(v)=\{w: w\to\cdots\to v\},\quad
  \mathrm{de}(v)=\{w: v\to\cdots\to w\}.
$$
- For $A\subseteq V$,  $\mathrm{an}(A)=\bigcup_{v\in A}\mathrm{an}(v)$,  $\mathrm{de}(A)=\bigcup_{v\in A}\mathrm{de}(v)$.

### Directed Acyclic Graphs (DAGs)

- A **directed cycle** is a directed walk from a vertex to itself.  
- A **DAG** is a directed graph with no directed cycles.  
- In a DAG,  $\mathrm{an}(v)\cap \mathrm{de}(v)=\{v\}.$

### Setup of Graphical Modeling

- Random vector $X=(X_v: v\in V)$ with joint distribution $P_X$.  
- DAG $G=(V,E)$.  
- For $A\subseteq V$, $X_A=(X_v: v\in A)$.  
- Shorthand for conditional independence: for disjoint $A,B,C\subseteq V$,  
  $$
  A\perp\!\!\!\perp B\mid C
  \quad\Longleftrightarrow\quad
  X_A\perp\!\!\!\perp X_B\mid X_C.
  $$

## Local Markov Property

**Definition.** $P_X$ satisfies the **local Markov property** relative to $G$ if  
$$
\forall v\in V:\quad
v\;\perp\!\!\!\perp\;V\setminus\bigl(\mathrm{pa}(v)\cup\mathrm{de}(v)\bigr)
\;\bigm|\;\mathrm{pa}(v).
$$
## Pairwise Markov Property

**Definition.** $P_X$ satisfies the **pairwise Markov property** relative to $G$ if for all non‐adjacent $v,w\in V$ with $w\notin\mathrm{de}(v)$,  
$$
v\;\perp\!\!\!\perp\;w
\;\bigm|\;\bigl(V\setminus\mathrm{de}(v)\bigr)\setminus\{w\}.
$$

#### Relation Between Local and Pairwise

**Lemma 1.**  
If $P_X$ satisfies the local Markov property, then it also satisfies the pairwise Markov property.

**Lemma 2.**  
If the intersection property (C5) of conditional independence holds for $P_X$, then $P_X$ satisfies the local Markov property if and only if it satisfies the pairwise Markov property.

#### Colliders and Non-Colliders on Paths

Let $\omega=(v_1,e_1,v_2,\dots,e_{n-1},v_n)$ be a path in a DAG $G$.  
- A non‐endpoint $v_i$ ($2\le i\le n-1$) is a **collider** if both $e_{i-1}$ and $e_i$ point into $v_i$.  
- Otherwise $v_i$ is a **non‐collider**.

#### d-Separation

**Definition.** Let $v,w\in V$ and $C\subseteq V\setminus\{v,w\}$. Then $v$ and $w$ are **d-connected given** $C$ if there exists a path $\omega$ from $v$ to $w$ such that:
1. every collider on $\omega$ lies in $\mathrm{an}(C)$,  
2. every non-collider on $\omega$ lies outside $C$.
If no such path exists, $v$ and $w$ are **d-separated given** $C$. Extend to sets $A,B,C$ by checking all pairs.

## Global Markov Property

**Definition.** $P_X$ satisfies the **global Markov property** relative to $G$ if for all disjoint $A,B,C\subseteq V$:  
$$
A\;\perp\!\!\!\perp\;B\mid C
\quad\text{whenever $C$ d-separates $A$ and $B$ in $G$.}
$$

**Lemma 3.**  
If $P_X$ satisfies the global Markov property, then it also satisfies the local Markov property.

**Theorem.**  
Let $G$ be a DAG.  The joint distribution $P_X$ satisfies the local Markov property relative to $G$ if and only if $P_X$ satisfies the global Markov property relative to $G$.

**Remark.**
- No assumption of the intersection property (C5) is needed.  
- The global MP for DAGs is *complete*.  
- Proof is given in the next chapter.

