---
layout: post
title: "Best-Buddies Similarity for Robust Template Matching"
date: 2019-03-14
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

BBS测量$$\mathbb{R}^d$$两点集之间的相似性，只依赖于点对中非常小的子集：the Best-Buddies Pairs（BBPs）。如果一对点中，一个点是对应集中另一个点的最近邻，则该对点被视为BBP。BBS是BBP集合所有点的函数。

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

另一个广泛使用的图像匹配测量方法是Hausdorff距离。为了处理遮挡或退化，Huttenlocher等人提出了一个fractional Hausdorff距离，取第K远的点而不是最远点，这个度量高度依赖于需要调整的K。Dubuisson和Jain将max替换为sum。Pomeranz等人在解决拼图问题时使用了Best Buddies。具体来说，他们使用与我们相似的度量来确定if a pair of pieces are compatible with each other。

### 05 Method 方法

该方法旨在存在高水平的异常值（即背景噪声、遮挡）和目标的非刚性变形的情况下，将模板与给定图像匹配。用传统的滑动窗口方法，在模板和图像中每个可能的窗口（模板大小）之间同样计算出BBS。

##### General Defination 定义

下面给出BBS的一般定义。

BBS测量了两个点集$$P=\{p_i\}^N_{i=1}$$和$$Q=\{q_i\}^M_{i=1}$$之间的相似性，这里$$p_i,q_i\in\mathbb{R}^d$$。当一对点$$\{p_i\in P,q_j\in Q\}$$中的$$p_i$$是$$q_j$$的最近邻或$$q_j$$是$$p_i$$的最近邻时，它们是一组BBP。


$$
bb(p_i,q_j,P,Q)=
\left\{
\begin{array}{rcl}
   & 1 & NN(p_i,Q)=q_j\wedge NN(q_j,P)=p_i\\
   & 0 & otherwise
\end{array}
\right.
$$


这里，$$NN(p_i,Q)=\mathop{argmin}\limits_{q \in Q} d(p_i,q)$$，$$d(p_i,q)$$是$$p_i$$和$$Q$$内各点之间距离。

集合$$P$$和集合$$Q$$之间的BBS定义如下：


$$
BBS(P,Q)=\frac{1}{min\{M,N\}}\cdot \sum\limits^{N}_{i=1}\sum\limits^{M}_{j=1}bb(p_i,q_j,P,Q)
$$



##### Key Properties 主要特性

BBS的主要特性为

- 只依赖于匹配点（BBPs）的子集（通常很小），其余的被视为outliers
- BBS可以找出数据中的双向inliers而不需要任何先验知识或者潜在变形
- BBP通过计算BBP的数量进行排名，而不是使用实际的距离值

考虑简单的两个2D点集$$P$$和$$Q$$，集合$$P$$由两个不同的正态分布$$N(\mu_1,\sum_1)$$和$$N(\mu_2,\sum_2)$$所绘制的2d点组成。集合$$P$$中的点是从相同的分布$$N(\mu_1,\sum_1)$$和不同的分布$$N(\mu_3,\sum_3)$$所绘制的。$$N(\mu_1,\sum_1)$$被视为foreground模型，$$N(\mu_2,\sum_2)$$和$$N(\mu_3,\sum_3)$$是两个不同的background模型。

这里我使用了BBP公式对文章中figure 2给出的二维高斯分布结果进行了验证，代码如下

```matlab
mu1 = [2 3];
SIGMA1 = [1 0; 0 2];
r1 = mvnrnd(mu1,SIGMA1,200);
mu2 = [7 8];
SIGMA2 = [1 0; 0 2];
r2 = mvnrnd(mu2,SIGMA2,200);
mu3 = [2 3];
SIGMA3 = [2 0; 0 4];
r3 = mvnrnd(mu3,SIGMA3,200);
subplot(2,2,1);
plot(r1(:,1),r1(:,2),'o',r2(:,1),r2(:,2),'o');
subplot(2,2,2);
plot(r1(:,1),r1(:,2),'o',r3(:,1),r3(:,2),'o');
subplot(2,2,3);
p=[r1;r2];
q=[r1;r3];
idx=knnsearch(p,q);
plot(p(idx,1),p(idx,2),'x');
subplot(2,2,4);
plot(q(idx,1),q(idx,2),'x');
```

得到的结果如下

![BBPs between 2D-Gaussian signals](<https://raw.githubusercontent.com/ZiqingZhao/ZiqingZhao.github.io/master/img/BBPs-between-2D-Gaussian-signals.png>)

与文中所给出的结论基本一致。

上述示例表明了BBS对数据中高级别异常值的鲁棒性。BBS捕获foreground，不强制background匹配，这样就不需要对background和foreground进行参数化建模，或者对它们的分布有一个先验了解。此外，如果$$p$$和$$q$$是从同一分布中抽取的，则$$\{p,q\}$$是BBP的可能性更大。

##### BBS for Template Matching 基于BBS的模板识别

要想将BBS应用于模板匹配，需要将每个image patch转换为$$\mathbb{R}^d$$中的一个点集，这需要将一个image window在spatial-appearance空间中表示。为此，首先把图像region分成若干个$$k\times k$$的patches。每个$$k\times k$$patch由其$$RGB$$值组成的$$k^2$$个矢量和中心像素相对于坐标系的$$xy$$坐标表示。

#### 5.1 Analysis 分析

首先从一维中的一个简单数学模型开始，在这个模型中，图像块被建模为从一般分布中提取的一组点。利用该模型，可以从两个给定的分布$$f_P(p)$$和$$f_Q(q)$$中得出两组点之间的BBS。当$$f_P(p)$$和$$f_Q(q)$$是两种不同的正态分布时，可以利用数值方法分析。最后，将这些结果与多维情况联系起来。由此可以得出，BBS可以明显地捕捉从相似分布中提取的点。也就是说，当两个集合中的点从同一个分布中抽取时，一对点成为BBP的可能性最大，BBS的期望值也最大，并且随着两个正态分布之间的距离的增加而急剧下降。

##### 5.1.1 One-dimentional Case 一维情况分析

根据之前给出的定义，$$BBS(P,Q)$$的期望为


$$
E[BBS(P,Q)]=\frac{1}{min\{M,N\}}\cdot \sum\limits^{N}_{i=1}\sum\limits^{M}_{j=1}E[bb(p_i,q_j,P,Q)]
$$


将$$bb(p_i,q_j,P,Q)$$的期望记作$$E_{BBP}$$，则


$$
E_{BBP}=\iint\limits_{P,Q}bb_{i,j}(P,Q)P_r\{P\}P_r\{Q\}dPdQ
$$


假设每个点独立于其它点，积分式可以进行如下简化


$$
E_{BBP}=\iint\limits_{-\infty}^{\infty}(F_Q(p^-)+1-F_Q(p^+))^{M-1}\cdot (F_P(q^-)+1-F_P(q^+))^{N-1}f_P(p)f_Q(q)dpdq
$$


这里，$$F_P(x)=Pr\{p\le x\}$$，$$F_Q(x)=Pr\{q\le x\}$$，$$p^-=p-d(p,q)$$，$$p^+=p+d(p,q)$$，$$q^-=q-d(p,q)$$，$$q^+=q+d(p,q)$$。

<!--BBS证明过程看不懂-->

##### 5.1.2 多维情况分析

如果d维空间不相关（高斯分布情况下其协方差矩阵是对角矩阵），则一对点是BBP的必要不充分条件是，该对点在各维条件下都是BBP。结合前面所给出的一维空间下的结论，分别在每个维度进行分析。因此在多维情况下，BBS的期望被每个维度的期望值的乘积所限定，即


$$
E_{BBS}\ge \prod\limits_{i=1}^d E^i_{BBS}
$$


这里$$E^i_{BBS}$$表示第$$i$$维空间内的BBS。

BBS随着d的增加下降更快。如果一对点在某个维度不是BBP，则不一定意味着在多维不是BBP。

#### 5.2 应用和算法复杂度

计算两个点集$$P,Q\in\mathbb{R}^d$$之间的BBS值，需要计算每对点之间的距离。也就是说，首先需要构建一个距离矩阵$$D$$，满足$$[D]_{i,j}=d(p_i,q_j)$$。在给定距离矩阵$$D$$的条件下，$$NN(p_i,Q)$$（$$p_i$$点的最近邻）也就是$$D$$矩阵中第$$i$$行的最小值。同样地，$$NN(q_j,P)$$就是$$D$$矩阵中第$$j$$列的最小值。BBS通过计算互为最近邻点的数量得到（除以常量）。

实验中所用到的距离函数如下


$$
d(p_i,q_j)=||p_i^{(A)}-q_j^{(A)}||^2_2+\lambda ||p_i^{(L)}-q_j^{(L)}||^2_2
$$


这里上标$$A$$表示像素的$$RGB$$值，上标$$L$$表示像素的位置（坐标进行了归一化）。$$\lambda$$根据经验选取2，在所有实验中固定。

首先将图像和模板拆分成若干个$$k\times k$$的图像块，为了清晰起见，这里先将分析拆分成单个像素的BBS的复杂度，然后再扩展到$$k\times k$$。

假设|P|=N、|Q|=M，计算距离矩阵$$D$$的复杂度为$$O(dNM)$$。假设图像尺寸为|I|=L，那么构造$$L$$个距离矩阵的算法复杂度为$$O(dNML)$$。因为许多计算可以重用，因此不需要为图像的每个窗口计算距离矩阵$$D$$。

实验中，逐列扫描图像，并对前一列计算的所有距离矩阵进行缓存。这样除第一列外，新列中的所有距离矩阵中只需要计算一个$$Q$$中新像素的距离矩阵，算法复杂度降为$$O(N)$$。由于模板比图像小，复杂度可以看作$$O(dNL)$$，这通常比$$O(dNML)$$小两个数量级。

扩展到$$k\times k$$情况，算法复杂度变为$$O(k^2d\frac{N}{k^2}\frac{L}{k^2})=O(\frac{dNL}{k^2})$$。如果将图像拆分成$$k\times k$$的图像块可以得到更可靠的BBPs并降低复杂度。

使用没有优化的MATLAB代码，$$k=3$$、图像大小为$$360\times 480$$、模板大小为$$40\times 30$$时，运行时间约为4s。

### 6 Results 结果

我们对我们的方法对现实数据进行定性和广泛的定量评估，将BBS与模板匹配常用的六种相似性度量进行了比较：

- SSD
- SAD
- NCC
- HM
- EMD
- BDS

#### 6.1 Qualitative Evaluation 定性评价

从网络上获取四个模板图像对来进行定性评估。在所有的例子中，由于几何变形、遮挡和背景的变化，模板的appearance都发生了巨大的变化。

![BBS for Template Matching](https://raw.githubusercontent.com/ZiqingZhao/ZiqingZhao.github.io/master/img/BBS-for-Template-Matching.jpg)

检测结果表明，在所有这些示例中，BBS是唯一成功匹配模板的方法。

