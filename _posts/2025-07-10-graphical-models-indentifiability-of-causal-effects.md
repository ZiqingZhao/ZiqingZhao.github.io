---
layout: post
title: "Graphical Models: Indentifiability Of Causal Effects"
date: 2025-07-10
excerpt: ""
tags: [Graphical Models, Probabilistic Inference, Indentifiability Of Causal Effects]
comments: true
---

### Identifiability

**Definition**  
The causal effect $$P(X_R\in\cdot;\,\doOp(X_T=x_T))$$ is **identifiable** from the marginal distribution of $$X_W$$ if for any two observational distributions $$P^X_1, P^X_2$$ with positive densities that factorize according to $$G$$,
$$
P^X_{W,1} = P^X_{W,2}
\quad\Longrightarrow\quad
P_1\bigl(X_R\in\cdot;\,\doOp(X_T=x_T)\bigr)
= P_2\bigl(X_R\in\cdot;\,\doOp(X_T=x_T)\bigr).
$$
In other words, an identifiable effect is uniquely determined by the marginal distribution of the observed variables $$X_W$$.
### Front–Door Criterion

**Definition**  
Let $$r,t\in V$$, $$r\neq t$$.  A set $$C\subseteq V\setminus\{r,t\}$$ satisfies the **front–door criterion** with respect to the ordered pair $$(t,r)$$ if  
1. every directed path from $$t$$ to $$r$$ contains a node in $$C$$;  
2. there is no back–door path from $$t$$ to $$C$$ that is d–connecting given $$\emptyset$$;  
3. there does not exist a back–door path from $$C$$ to $$r$$ that is d–connecting given $$\{t\}$$.

**Theorem**  
If $$C$$ satisfies the front–door criterion wrt.\ $$(t,r)$$, then
$$
f\bigl(x_r;\,\doOp(X_t=x_t)\bigr)
=
\int
f\bigl(x_C\mid x_t\bigr)\,
\biggl[\int f\bigl(x_r\mid x_t,x_C\bigr)\,f(x_t)\,d\mu_t(x_t)\biggr]
\,d\mu_C(x_C).
$$

### Do–Calculus

**Notation:**  
$$A\perp B\mid C$$ in $$G$$ denotes that $$A$$ and $$B$$ are d-separated by $$C$$ in the DAG $$G$$.

**Theorem (Rules of the Do–Calculus)**  
Let $$A,B,C,D\subseteq V$$ be disjoint.  

- **Rule 1 (Insertion/deletion of observations):**  
  If $$A\perp B\mid (C,D)$$ in $$G_C$$, then  
$$
    f\bigl(x_A\mid x_B,x_D;\,\doOp(X_C=x_C)\bigr)
    =
    f\bigl(x_A\mid x_D;\,\doOp(X_C=x_C)\bigr).
$$
- **Rule 2 (Action/observation exchange):**  
  If $$A\perp B\mid (C,D)$$ in $$G_{C,B}$$, then  
$$
    f\bigl(x_A\mid x_D;\,\doOp(X_B=x_B,\,X_C=x_C)\bigr)
    =
    f\bigl(x_A\mid x_B,x_D;\,\doOp(X_C=x_C)\bigr).
$$
- **Rule 3 (Insertion/deletion of actions):**  
  Let $$B(D)=B\setminus\an_{G_C}(D)$$.  If $$A\perp B\mid (C,D)$$ in $$G_{C\cup B(D)}$$, then  
$$
    f\bigl(x_A\mid x_D;\,\doOp(X_B=x_B),\,\doOp(X_C=x_C)\bigr)
    =
    f\bigl(x_A\mid x_D;\,\doOp(X_C=x_C)\bigr).
$$

### Instrumental Variables

Consider the linear SCM
$$
\begin{aligned}
X_1 &= \omega_{01} + \varepsilon_1,\\
X_2 &= \omega_{02} + \omega_{21}X_1 + \omega_{24}X_4 + \varepsilon_2,\\
X_3 &= \omega_{03} + \omega_{32}X_2 + \omega_{34}X_4 + \varepsilon_3,\\
X_4 &= \omega_{04} + \varepsilon_4,
\end{aligned}
$$
with independent errors.  By the trek rule,
$$
\Cov[X_1,X_2] = \omega_{21}\Var[X_1],
\quad
\Cov[X_1,X_3] = \omega_{32}\,\omega_{21}\Var[X_1].
$$
If $$\Cov[X_1,X_2]\neq 0$$, then
$$
\omega_{32}
=
\frac{\Cov[X_1,X_3]}{\Cov[X_1,X_2]}.
$$
Hence the total effect $$\omega_{32}$$ is (generically) identifiable from the distribution of $$(X_1,X_2,X_3)$$. 
