---
layout: post
title: "Very Fast Template Matching"
date: 2019-04-12
excerpt: "快速模板匹配"
tags: [Computer Vision, Template Matching]
comments: true
---

论文来自European Conference on Computer Vision 2002。

[1] Schweitzer H., Bell J.W., Wu F. (2002) Very Fast Template Matching. In: Heyden A., Sparr G., Nielsen M., Johansen P. (eds) Computer Vision — ECCV 2002. ECCV 2002. Lecture Notes in Computer Science, vol 2353. Springer, Berlin, Heidelberg

## 0 Abstract 摘要

在模板匹配中经常使用归一化相关来检测或计算模板在图像中的位置。对于大小为$$n$$的图像和大小为$$k$$的模板，实现模板匹配的复杂度为$$O(kn)$$，一些快速算法的复杂度为$$O(nlogn)$$。因此程序主要运行时间浪费在模板匹配时的重复计算。本篇论文提出了一个基于**积分图像**的模板匹配算法，其复杂度仅为$$O(n)$$。

## 1 Introduction 简介

通过归一化相关性进行模板匹配是模板匹配最常见的方法。假设$$t$$是图像$$f$$中要检测的模板。通过归一化相关性匹配的模板在图像的每个点$$(u,v)$$处计算以下值：



$$
h(u,v)=\frac{\sum_{x,y}f(u+x,v+y)t(x,y)}{\sqrt{\sum_{x,y}f(u+x,v+y)^2}} \tag{1}
$$



$$h(u,v)$$很大时表示该点可能匹配。

用$$n$$表示图像像素数，用$$k$$表示模板像素数。对上式直接进行计算的复杂度为$$O(kn)$$。由于分子和分母都可以用相关性实现，因此上式直接与计算卷积的复杂度联系在一起。而使用快速卷积（如FFT）对任意$$k$$的复杂度均为$$O(nlogn)$$。

很多计算机视觉系统需要在多个对象、多个尺度和方向上进行搜索，因此模板匹配的计算成本可能主导系统的速度。使用复杂度为$$O(nlogn)​$$算法的系统很少。可能的原因有编码过于复杂，$$O(nlogn)​$$运行时间过长。常用的加速技术有开发专用硬件和仅在图像的一个子集中计算$$h(u,v)​$$值。

一种加速方法是在执行第一阶段将模板像素的一小部分子集与图像进行匹配。在第二阶段仅计算在第一阶段中排名靠前的像素点的$$h(u,v)$$的精确值。另一种加速方法是由粗到细搜索，首先在低分辨率图像中确定匹配的候选位置，然后在高分辨率图像中继续细化。

此前Viola和Jones的研究结果表明，对于特殊形状的模板（轴平行均匀矩形），计算$$h(u,v)$$的算法复杂度可以降为$$O(n)$$。这可以用来建立更加复杂的模板，比如多个矩形的组合。然后，可以利用从多个模板计算出的$$h(u,v)$$值作为特征，用学习算法分析这些特征。

### 1.1 The Main Contribution 主要贡献

Viola和Jones能够将运算速度提高的关键是能够预先计算的积分图像。作者的主要贡献是注意到积分图像可以用来计算线性时间的代数矩，这使得在线性时间内可计算图像每个位置的最佳最小二乘近似多项式，这些近似值可以用来作$$h(u,v)$$值的精确估计。近似时多项式的位置会影响运行时的复杂度，$$d$$阶多项式的复杂度为$$O(d^2n)$$。另一方面，$$h(u,v)$$的估计值的精度随多项式阶数的增大而提高。后续的实验表明，二阶多项式近似能够在显著缩短运行时间的情况下给出足够精确的估计。

### 1.2 Paper Organization 论文结构

论文的组织结构如下。

论文的第二节介绍什么是积分图像。

第三节表明，利用积分图像可以有效地计算出低阶局部代数矩。这些矩用于计算图像的局部最小二乘多项式近似值。

在第四节中，推导了这些多项式与模板之间计算的归一化匹配的封闭式表达式。

第五节描述了算法步骤，并对其复杂性进行了分析。

第六节展示了实验结果。

## 2 Integral Images 积分图像

假设$$f(x,y)$$是非负数$$x$$、$$y$$的可积函数，定义



$$
I(u,v)=\int\limits_{x=0}^u \int\limits_{y=0}^v f(x,y) dydx
$$



那么一个显式积分可以计算为



$$
\int\limits_{x=x_a}^{x_b} \int\limits_{y=y_a}^{y_b} f(x,y) dydx =I(x_b,y_b)+I(x_a,y_a)-I(x_a,y_b)-I(x_b,y_a)
$$



离散情况下，上述两式可以表示成



$$
I(x,y)=\sum_{x'=0}^x\sum_{y'=0}^y f(x',y') \tag{2}
$$

$$
\sum_{x=x_a}^{x_b} \sum_{y=y_a}^{y_b} f(x,y) =I(x_b,y_b)+I(x_a-1,y_a-1)-I(x_a-1,y_b)-I(x_b,y_a-1) \tag{3}
$$



Viola 和 Jones指出，积分图像可以使用如下递归公式进行计算



$$
s(x,y)=s(x,y-1)+f(x,y)
$$

$$
I(x,y)=I(x-1,y)+s(x,y)
$$



这意味着一旦计算出积分图像，任何矩形上的和都可以很快地计算出来。Viola和Jones使用这个方法来计算通过几个相邻矩形之间的相关性创建的“特征”，如下图所示

![](<https://raw.githubusercontent.com/ZiqingZhao/ZiqingZhao.github.io/master/img/Very-Fast-Template-Matching-example1.png>)

双矩形的特征值是两个矩形区域内像素和的差别。该区域的尺寸和形状相同，并且水平或垂直相邻。三矩形的特征值是两个外侧矩形的像素和减去中心矩形的像素和。四矩形的特征值计算了对角矩形对的差别。

## 3 Fast Computation of Local Algebraic Moments 局部代数矩的快速计算

这一节是Viola 和 Jones结论的推广，证明了积分图像可以用来计算低阶局部代数矩。

矩形区域的代数矩定义如下：



$$
m_{pq}=\sum_{x=x_a}^{x_b} \sum_{y=y_a}^{y_b} x^py^qf(x,y) \tag{4}
$$



其阶数为$$p+q$$。下图给出了对应于一阶矩和二阶矩的模板，原点位于矩形的中心。

![](https://raw.githubusercontent.com/ZiqingZhao/ZiqingZhao.github.io/master/img/Very-Fast-Template-Matching-example2.png)

将公式(2)(3)中的$$f$$用$$x^py^qf$$替换，显然代数矩可以用积分图像方法来计算。但由于$$m_{pq}$$的值取决于原点的选择，而公式(2)(3)中的原点是图像原点。可以看出$$m_{pq}$$与矩形中心矩$$\mu_{pq}$$之间有个简单的关系：



$$
\mu_{pq}= \sum_{x=x_a}^{x_b} \sum_{y=y_a}^{y_b} (x-x_0)^p(y-y_0)^qf(x,y)
$$

$$
\mu_{pq}=
\sum\limits_{0{\le}s{\le}p \\ 0{\le}t{\le}q} (-1)^{s+t}
\left(\begin{array}{rcl} p\\s \end{array} \right)
\left(\begin{array}{rcl} q\\t \end{array} \right)
x_0^sy_0^tm_{p-s,q-t}
$$



从中可以得到一个很重要的结论：$$\mu_{pq}$$由相同或更低阶的$$m_{pq}$$得出。

综上所述，本节给出了计算图像中任意给定矩形上局部代数矩$$\mu_ {pq}$$的算法。每个图像只需要计算一次函数为$$x^py^qf(x,y)$$的积分图像，根据公式(3)计算所需矩形的$$m_{pq}$$值，然后利用公式(5)得到$$\mu_{pq}$$的值。

## 4 Fast Template Matching 快速模板匹配

本节将介绍如何使用局部矩来进行模板匹配。首先关注一个单一的矩形，其坐标系的原点位于矩形中心。

设模板t局部矩的矩阵为$$U_t=(\mu_{00}^t,...,u_{pq}^t)$$。用$$U_f(u,v)=(\mu_{00}^f,...,\mu_{pq}^f)$$表示一个矩形的局部矩，其中心在点$$(u,v)$$，尺寸大小与模板$$t$$相同。一种可能的比较方法为直接计算$$U_f$$和$$U_t$$之间的欧式距离作为度量或其它直接比较的方法。其中一种选择是用与公式(1)相同的标准化度量，如下式所示：



$$
h(u,v)=\frac{\sum_{p,q}\mu_{pq}^f(u,v)\mu_{pq}^t}{\sqrt{\sum_{p,q}(\mu_{pq}^f(u,v))^2}} \tag{6}
$$



通过公式(1)我们可以知道，在模板精确匹配时，(6)有最大值，但与(1)式不同的是，它不会degrade gracefully。由于二阶和更高阶矩对噪声非常敏感，因此使用次式进行匹配会产生令人不满意的结果。下面将给出基于矩形上$$f$$的多项式近似的另一种测量方法。

### 4.1 Notation 标记

假设$$A$$为系数向量，$$X$$为变量向量，$$U$$表示中心距向量。

对于下面的二阶多项式



$$
p(x,y)=a_{00}+a_{10}x+a_{01}y+a_{20}x^2+a_{11}xy+a_{02}y^2
$$



其系数向量、变量向量和中心距向量可以表示为



$$
A=
\left(
\begin{array}{}
a_{00}\\a_{10}\\a_{01}\\a_{20}\\a_{11}\\a_{02}
\end{array}
\right)
,
X=
\left(
\begin{array}{}
1\\x\\y\\x^2\\xy\\y^2
\end{array}
\right)
,
U=
\left(
\begin{array}{}
\mu_{00}\\\mu_{10}\\\mu_{01}\\\mu_{20}\\\mu_{11}\\\mu_{02}
\end{array}
\right)
$$



因此$$p(x,y)$$可以表示为



$$
p=A'X
$$



矩形的局部矩$$U_f$$可以表示为



$$
U_f=\overline{Xf}
$$



### 4.2 Approximation by Polynomials 多项式逼近

对于$$f$$的多项式逼近的均方误差为



$$
E=\overline{(p-f)^2}=A'\overline{XX'}A-2A'U_f+\overline{f^2}
$$



$$B=\overline{XX'}$$是一个正定对称矩阵。

​        对于二阶多项式，$$B$$可以表示成



$$
B=\overline{XX'}
=\overline{
    \left(
    \begin{array}{}
    1 & x & y & x^2 & xy & y^2 \\
    x & x^2 & xy & x^3 & x^2y & xy^2\\
    y & xy & y^2 & x^2y & xy^2 & y^3\\
    x^2 & x^3 & x^2y & x^4 & x^3y & x^2y^2\\
    xy & x^2y & xy^2 & x^3y & x^2y^2 & xy^3\\
    y^2 & xy^2 & y^3 & x^2y^2 & xy^3 & y^4
    \end{array}
    \right)
}
\tag{7}
$$



因此，误差$$E$$是$$A$$的二次方程，且$$E$$最小时$$A$$取值为$$A_f=B^{-1}U_f$$。因此，$$f$$的最佳多项式近似$$p_f$$为



$$
p_f=U_f'B^{-1}X\tag{8}
$$



观察得到$$\overline{p_f^2}=U_f'B^{-1}U_f$$。

### 4.3 The Match Measure 匹配方法

对于公式(1)



$$
h(u,v)=\frac{\sum_{x,y}f(u+x,v+y)t(x,y)}{\sqrt{\sum_{x,y}f(u+x,v+y)^2}}
$$



用$$p_f$$替换其中的$$f$$，则公式(1)中分子的近似值为



$$
\overline{p_f^t}=\overline{U_f'B^{-1}Xt}=U_f'B^{-1}\overline{Xt}=U_f'B^{-1}U_t
$$



分母的近似值为



$$
\sqrt{(p_f)^2}=\sqrt{U_f'B^{-1}U_f}
$$



将分子分母结合得到



$$
h(u,v)=\frac{U_f'(u,v)B^{-1}U_t}{\sqrt{U_f'(u,v)B^{-1}U_f(u,v)}}\tag{9}
$$



$$U_f (u,v)$$为中心在点$$(u,v)$$处$$f$$的局部矩。

# 5 The Algorithm 算法

给定模板$$t$$和图像$$f$$，利用$$d$$阶多项式逼近匹配函数$$h(u,v)$$的值。观察得到有$$l=\frac{(d+1)(d+2)}{2}$$个矩$$\mu_{pq}$$满足$$p+q \le d$$。

算法步骤如下：

1. 计算模板的矩$$U_t​$$，$$U_t​$$是一个长度为$$l​$$的向量。
2. 计算$$l$$个矩$$m_{pq}$$的积分图像。
3. 利用公式(3)和公式(5)对图像的每个位置$$(u,v)$$计算其中心矩$$U_f(u,v)$$。根据$$U_t$$和$$U_f(u,v)$$的值计算$$h(u,v)$$。

计算量较大的步骤为第2、3、4步，对于像素数量为$$n​$$的图像，运算复杂度为$$O(nd^2)​$$。当对同一图像匹配不同模板时只需要执行第1、4步即可。

