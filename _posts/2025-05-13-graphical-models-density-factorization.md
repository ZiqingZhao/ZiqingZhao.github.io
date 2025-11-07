---
layout: post
title: "Graphical Models: Density Factorization"
date: 2025-05-13
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Density Factorization]
comments: true
---

## Examples

- **Path of length 3** ($1\!-\!2\!-\!3$):  
  $1\perp\!\!\perp3\mid2$ iff  
$$
    f(x_1,x_2,x_3)
    =\psi_{12}(x_1,x_2)\,\psi_{23}(x_2,x_3),
  $$
  e.g. $\psi_{12}=f(x_1,x_2)$, $\psi_{23}=f(x_3\mid x_2)$. 
- **Path of length 4** ($1\!-\!2\!-\!3\!-\!4$):  
  Global MP gives
$$
    f(x_1,x_2,x_3,x_4)
    =f(x_4\mid x_3)\,f(x_3\mid x_2)\,f(x_1,x_2)
    =\psi_{12}\,\psi_{23}\,\psi_{34}.
$$
  Conversely, this factorization implies the global MP. 
- **4-cycle** ($1\!-\!2\!-\!3\!-\!4\!-\!1$):  
  Under the pairwise/global MP assumptions one shows
$$
    f(x_1,x_2,x_3,x_4)
    =\psi_{12}(x_1,x_2)\,\psi_{23}(x_2,x_3)\,\psi_{34}(x_3,x_4)\,\psi_{14}(x_1,x_4).
$$ 
## Density Factorization

- Let $G=(V,E)$ be an undirected graph, and $\mathcal C$ its set of cliques.
- A density $f$ w.r.t. $\lambda=\bigotimes_{v\in V}\lambda_v$ **factorizes** over $G$ if there exist nonnegative potentials $\psi_C:\mathbb R^{|C|}\to[0,\infty)$ such that
  $$
    f(x)
    =\prod_{C\in\mathcal C}\psi_C(x_C)
    \quad(\lambda\text{-a.e.}\quad x\in\mathbb{R}^V).
  $$
- **Proposition:** If $f$ factorizes over $G$, then $P_X$ satisfies the global Markov property w.r.t. $G$. 

## Hammersley–Clifford Theorem

- **Theorem:** If $f>0$ is a positive density w.r.t. $\lambda$, then
$$
\begin{align}
    &f\text{ factorizes over }G \\
    &\Longleftrightarrow \\
    &P_X\text{ satisfies the pairwise (equivalently local/global) Markov property w.r.t.\ }G.
\end{align}
$$ 
- **Proof sketch:**  
  - “$\Rightarrow$” follows from Proposition (5.2) and the equivalence of global/local/pairwise MP under positivity.  
  - “$\Leftarrow$” writes $\log f(x)=\sum_{C\subseteq V}\phi_C(x_C)$ via Möbius inversion on the functions $H_C(x_C)=\log f(x_C,x_{\bar C}=\bar x)$, then uses pairwise MP to show $\phi_C=0$ for non-cliques, yielding the desired factorization. 

## Möbius Inversion

- **Lemma:** For any finite set $V$ and functions $\psi,\phi:2^V\to\mathbb R$,
$$
    \psi(C)=\sum_{B\subseteq C}\phi(B)
    \quad\Longleftrightarrow\quad
    \phi(C)=\sum_{B\subseteq C}(-1)^{|C\setminus B|}\,\psi(B).
$$

- Example: Consider $V=\{1,2\}$
$$
 \begin{align}
 \begin{pmatrix}
 \phi(\emptyset)\\\phi(\{1\})\\\phi(\{2\})\\\phi(\{1,2\})\\
 \end{pmatrix}
 =
 \begin{pmatrix}
 1  & 0  & 0  & 0 \\
 -1 & 1  & 0  & 0 \\
 -1 & 0  & 1  & 0 \\
 1  & -1 & -1 & 1
 \end{pmatrix}
 \begin{pmatrix}
 \psi(\emptyset)\\\psi(\{1\})\\\psi(\{2\})\\\psi(\{1,2\})
 \end{pmatrix}
 \end{align}
$$
Inversion
$$
 \begin{align}
 \begin{pmatrix}
 \psi(\emptyset)\\\psi(\{1\})\\\psi(\{2\})\\\psi(\{1,2\})
 \end{pmatrix}
 =
 \begin{pmatrix}
 1  & 0  & 0  & 0 \\
 1  & 1  & 0  & 0 \\
 1 & 0  & 1  & 0 \\
 1 & 1  & 1  & 1
 \end{pmatrix}
 \begin{pmatrix}
 \phi(\emptyset)\\\phi(\{1\})\\\phi(\{2\})\\\phi(\{1,2\})\\
 \end{pmatrix}
 \end{align}
$$
## Counterexamples & Chordality

- **Counterexamples** on the 4-cycle show distributions can satisfy pairwise MP but fail to factorize (and may or may not be limits of factorizing families). 
- **Summary of relationships:**
$$
    \text{Factorization (F)}\;\implies\;\text{Global MP (G)}\;\implies\;\text{Local MP (L)}\;\implies\;\text{Pairwise MP (P)},
$$
  If the density is positive, the Hammersley-Clifford Theorem yields $\text{(P)}\implies\text{(F)}$.
- **Chordal Graphs:** For a chordal (decomposable) graph $G$, $f$ factorizes over $G$ $\iff$ global MP holds even without assuming positivity. A graph is **chordal** if every cycle of length $\ge4$ has a chord. 