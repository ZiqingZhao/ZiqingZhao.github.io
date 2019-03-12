---
layout: post
title: "Best-Buddies Similarity for Robust Template Matching"
date: 2019-03-12
excerpt: "基于Best-Buddies Similarity的鲁棒模板识别"
tags: [Computer Vision, Template Matching]
comments: true
---

## 基于Best-Buddies Similarity 的鲁棒模板识别

[Dekel, Tali & Oron, Shaul & Rubinstein, Michael & Avidan, Shai & T. Freeman, William. (2015). Best-Buddies Similarity for robust template matching. 2021-2029. 10.1109/CVPR.2015.7298813.](https://www.computer.org/csdl/proceedings-article/cvpr/2015/07298813/12OmNxRF76a) 

### 01 Template matching 模板识别

> **Template matching** is a technique in digital image processing for finding small parts of an image which match a template image. It can be used in manufacturing as a part of quality control, a way to navigate a mobile robot, or as a way to detect edges in images.

模板识别可以应用于物体检测、追踪、图像连接和3D重建。而在现实情景中，模板会经历复杂的变形—背景可能改变、目标可能经历非刚性变形和部分遮挡。

模板识别存在许多缺点。

测量相似性时考虑模板和目标图像中的候选窗口中的所有像素（特征点）。当目标背后的背景改变时，这种方法是不可取的。在这种情况下，不同背景下的像素之间的差异可能是任意的，而将这种差异考虑进去可能会导致错误的检测结果。

很多模板识别方法在模板和目标图像之间建立了特定的参数化变形模型（比如刚性变形、仿射变换等），这限制了可以处理的场景类型，并且在考虑复杂模型时可能需要大量参数

### 02 Abstract 摘要

这篇论文主要介绍了在无约束环境下模板匹配的一种新方法，本质是Best-Buddies Similarity（BBS算法）。这是一种在两个点集之间的无参鲁棒相似性测量，基于计算Best-Buddies Pairs（BBPs，原集和目标集中的点对，每个点都是另一个点的最近邻）的数量。BBS有几个关键特性，可以使其抵抗复杂的几何变形和高度异常的点（背景噪声和遮挡）。作者提供了一种统计分析来证明BBS的合理性，并且将BBS应用于复杂的真实数据集。

### 03 Introduction 简介

作者使用了一种新的方法Best-Buddies Similarity来克服模板匹配的局限性，并将其成功应用于template matching in the wild。

BBS测量$$\mathbb{R}^d$$两点集之间的相似性，只依赖于点对中非常小的子集：the Best-Buddies Pairs（BBPs）。如果一对点中，一个点是对应集中另一个点的最近邻，则该对点被视为BBP。BBS是BBP集合所有点的fraction。

这种方法有着重要且非同寻常的特性。BBS由于只考虑BBP，因此对于大部分异常点具有鲁棒性。BBS随两点集的distribution增大而减小，两个distribution相同时取得最大值。也就是说，如果两个点是BBP，它们很可能是从相同的分布中提取的。文章中给出了这种观测的统计公式并且在一维情况下对从不同的高斯分布中提取的点击进行了数值分析。BBS可以在存在异常值的情况下可靠地匹配来自同一分布的特征，在视角变换和集合变形下依然可以进行稳健的模板匹配。

将模板和每个候选图像区域表示为一个联合$$xyRGB$$ 空间。在这个空间里，用BBS测量两个点集之间的相似性，从而进行模板匹配。两个点集（template and the candidate patch）可能源于相同的分布，这样template中的像素点就能在candidate patch中找到best buddies，这将被视为内点（inliers）。反之，来自不同分布的像素点（例如不同背景中的点）找到best buddies的可能性很小因此被视作外点（outliers）。因此BBS不需要显式地为目标外观和变形建模。

这篇论文的主要贡献是：

- 引入一种可以在无约束环境中进行模板匹配的BBS方法；
- 理论分析其主要特征
- 对真实数据进行广泛评估，并与其他常见模板匹配方法进行比较

### 04 Related Work 相关研究

模板匹配与相似性度量息息相关。

目前主要流行的相似性度量方法有： Sum of Squared Differences（SSD），Sum of Absolute Differences（SAD）和 Normalized Cross-Correlation （NCC）。

另一类度量由鲁棒误差函数组成（robust error functions），例如M-estimators、Hamming-based distance，这些与互相关相比受加性噪声和椒盐噪声的应该更小。但以上方法都假定模板和目标图像之间仅存在刚性几何变形，因为它们会对像素差异进行惩罚。

许多方法扩展模板匹配以处理参数转换。Korman等人引入了二维仿射变换下的模板匹配算法，保证了对全局最优解的逼近。同样，Tian和Narasimhan发现了非刚性图像畸变的全局最优估计。但是，这些方法假定模板和底层转换的查询区域之间有一对一的映射。在存在许多异常值（背景噪声和遮挡）的情况下容易出错。此外，这些方法假设了畸变几何的参数模型，而BBS不需要。

测量颜色直方图之间的相似性，称为直方图匹配（Histogram Matching, HM），提供了一种处理变形的非参数技术，通常用于视觉跟踪。 HM完全无视几何，并且能均匀地处理所有像素。 已经有很多跟踪方法可以处理处理环境噪声和部分遮挡。 与跟踪不同，我们只对单个图像中的检测感兴趣，而不需要视频中的冗余时间信息。

Olson根据最大似然估计公式进行模板匹配模板匹配。Oron等人使用$$xyRGB$$空间和简化的模板匹配来测量两个点集之间的EMD。虽然BBS在同一个$$xyRGB$$ 空间中工作，但它不同于EMD，EMD要求1:1匹配，并且不区分内点和外点。

BDS（Bidirectional similarity）被用作两个由图像块的集合表示的图像之间的相似性度量。在一个图像中，BDS求和每个补丁到另一个图像中最近的邻居之间的距离，反之亦然。相比之下，BBS是基于BBP的计数，并且只隐式使用它们的实际距离。此外，BDS不区分输入端和输出端。这些规范使BBS成为一种更为可靠和可靠的测量方法，这一点在后续实验中得到了证明。