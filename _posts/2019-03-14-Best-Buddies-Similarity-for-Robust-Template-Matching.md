---
layout: post
title: "Best-Buddies Similarity for Robust Template Matching"
date: 2019-03-14
excerpt: "Robust template matching with BBS"
tags: [Computer Vision, Template Matching]
comments: true
---
### Robust Template Matching with Best-Buddies Similarity

The paper “Best-Buddies Similarity for Robust Template Matching” (Dekel et al., CVPR 2015) introduces the Best-Buddies Similarity (BBS) measure for unconstrained template matching tasks such as detection, tracking, stitching, and 3D reconstruction, where templates often undergo severe appearance changes, nonrigid deformation, and partial occlusion.

### 1. Template Matching

Classical template matching compares all pixels within the template and search window, making it brittle when backgrounds differ or when only rigid transformations are modeled. Parameterized warps (rigid, affine, etc.) limit the range of supported scenarios and demand many parameters for complex deformations.

### 2. Abstract

BBS measures similarity between two point sets by counting Best-Buddies Pairs (BBPs)—pairs of points that are mutual nearest neighbors. Because it relies only on this subset, BBS is robust to outliers caused by background clutter or occlusions. The paper provides statistical analysis to justify BBS and demonstrates state-of-the-art performance on challenging benchmarks.

### 3. Introduction

BBS operates in $$\mathbb{R}^d$$ by representing each candidate patch and template as point sets in joint $$xyRGB$$ space. If two sets originate from the same distribution, their points form BBPs (inliers); mismatched backgrounds yield few BBPs (outliers), avoiding the need to explicitly model foreground/background appearance or deformation.

### 4. Related Work

Traditional similarity measures include SSD, SAD, and NCC. Robust error functions (M-estimators, Hamming distance) tolerate certain noise types but still assume rigid deformations. Other approaches model parameterized transformations (e.g., affine, nonrigid) or rely on feature-based matching and learning-based descriptors, yet they either impose strict assumptions or require significant computation.

### 5. Best-Buddies Similarity

Given point sets $$P$$ and $$Q$$, BBS counts pairs $$(p_i, q_j)$$ such that $$p_i$$ is the nearest neighbor of $$q_j$$ in $$P$$ and vice versa. Section 5.1 analyzes BBS statistically:

- **1D case:** The expected BBS value is maximized when $$P$$ and $$Q$$ are drawn from the same distribution and falls quickly as their distributions diverge.
- **Multi-dimensional case:** If dimensions are independent (e.g., diagonal covariance), the expectation lower-bounds by the product of per-dimension expectations, so higher dimensionality sharpens discrimination.

To apply BBS in template matching, an image region is partitioned into $$k \times k$$ patches. Each patch contributes vectors containing RGB appearance ($$A$$) and normalized coordinates ($$L$$), and distance is measured by

$$
d(p_i,q_j)=\|p_i^{(A)}-q_j^{(A)}\|_2^2+\lambda\|p_i^{(L)}-q_j^{(L)}\|_2^2,\quad \lambda=2.
$$

#### Complexity

Constructing the distance matrix $$D$$ for sets of size $$|P|=N$$, $$|Q|=M$$ costs $$O(dNM)$$. For an image with $$|I|=L$$ windows, naive evaluation is $$O(dNML)$$, but caching distance information column by column reduces it to roughly $$O(dNL)$$ since only the new column’s contributions must be recomputed. Extending to $$k \times k$$ patches yields $$O(dNL/k^2)$$ complexity. In MATLAB, using $$k=3$$, a $$360\times480$$ image, and a $$40\times30$$ template takes about 4 s without heavy optimization.

### 6. Results

The paper benchmarks BBS against SSD, SAD, NCC, Hamming distance, EMD, and BDS on challenging Internet images that exhibit large geometric changes, occlusion, and background variation. Qualitatively, BBS is the only method that consistently retrieves the correct matches under these conditions, highlighting its robustness to severe appearance changes.
