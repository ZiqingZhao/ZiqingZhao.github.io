---
layout: post
title: "AI in Medicine: Causal Inference"
date: 2025-05-14
excerpt: ""
tags: [AI, Medicine, Causal Inference]
comments: true
---

## 1. Motivation

- **Randomized Controlled Trial (RCT)**: the gold standard for causal effect estimation (e.g. BNT162b2 vaccine vs. placebo)  
- **Observational Studies**: require adjustment for confounders (age, sex, residence, chronic conditions, PCR tests) to estimate treatment effects (e.g. booster effectiveness)  
- **Spurious Correlations**: “correlation ≠ causation” (e.g. Tyler Vigen examples) 
- **Prediction ≠ Causation**: supervised models can mislead (e.g. asthma history and pneumonia mortality) 
- **Confounding Examples**:  
  - Smoking and lung cancer (ethical RCT impossible; observational comparisons confounded)  
  - Simpson’s paradox in kidney stone treatment (aggregate vs. stratified success rates) 

---

## 2. Causality

- **Practical Definition**: $$T$$ causes $$Y$$ if intervening on $$T$$ changes $$Y$$, holding other factors constant  
- **Do-Operator**: $$P(Y \mid \text{do}(T))$$ denotes the distribution of $$Y$$ under an intervention on $$T$$ 
- **Causal Graphs (DAGs)**: nodes = variables; edges = direct causal effects  
  - Example: Aspirin → Stroke, with confounders (e.g. coronary heart disease) 

---

## 3. Conditioning

- **Back-Door Adjustment**:  
  $$
    P(Y \mid \text{do}(T))
    = \sum_x P(Y \mid T, X = x)\,P(X = x)
  $$  
- **Resolving Simpson’s Paradox**: stratify by stone size to recover the true causal effect in kidney stone treatment 
- **Example**: impact of stationary biking on cholesterol is confounded by age; conditioning on age blocks the back-door path 

---

## 4. Confounding, Mediation & Colliding

- **Confounder**: common cause of treatment and outcome; conditioning blocks bias  
- **Mediator**: lies on the causal path; conditioning may remove part of the effect  
- **Collider**: common effect of treatment and outcome; conditioning opens a spurious association (Berkson’s paradox) 

---

## 5. Potential Outcomes

- **Individual Potential Outcomes**: $$Y_i(A)$$, $$Y_i(B)$$ under treatments $$A,B$$  
- **Individual Treatment Effect (ITE)**: $$Y_i(A) - Y_i(B)$$  
- **Average Treatment Effect (ATE)**: $$\mathbb{E}[Y_i(A) - Y_i(B)]$$  
- **Fundamental Problem**: only one potential outcome is observed per unit; counterfactual must be estimated 
$$
\mathbb{E}[Y_i(A)-Y_i(B)] \ne \mathbb{E}[Y_i \mid T=A]- \mathbb{E}[Y_i \mid T=B]
$$
---

## 6. Counterfactual Inference Approaches

1. **Covariate Adjustment**  
   - Regression (model $$Y\sim T + X$$; sensitive to model misspecification, multicollinearity)  
   - Matching (pair treated/untreated on $$X$$)  
	   - $$\text{match}(i)=\min_j \text{Distance}(X_i, X_j)$$
	   - only works well when matched pairs are very similar
   - Stratification (group by $$X$$-levels)
1. **Propensity Score** $$e(X)=P(T=1\mid X)$$  
   - Estimated via logistic regression  
   - Use for matching, stratification, or **Inverse Probability of Treatment Weighting (IPTW)**  
   - Balances covariates across treatment groups; high‐variance if $$e(X)$$ near 0 or 1 
   - Always one dimensional even if X is high-dimensional
   - If conditioning on X suffices to control for all confounding, so does conditioning on estimated PS

---

## 7. Summary

- Two ML‐based causal inference paradigms:  
  1. Covariate‐based outcome modeling  
  2. Propensity‐based reweighting/stratification  
- Valid inference relies on assumptions: unconfoundedness, positivity, consistency, no interference 

---

