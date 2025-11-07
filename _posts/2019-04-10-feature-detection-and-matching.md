---
layout: post
title: "Feature Detection and Matching"
date: 2019-04-10
excerpt: ""
tags: [Computer Vision, Template Matching, Feature Detection]
comments: true
---

## 1. Feature Extraction Overview

### 1.1 Image Features

Classical features fall into three families: corners, edges, and regions. Corners (often called interest points, keypoints, or simply features) are especially popular because small motions in any direction trigger large intensity changes, making them reliable landmarks.

### 1.2 Feature Detection and Matching

A feature consists of a keypoint location and a descriptor that encodes the appearance around that location. Two features match when their descriptors lie close to one another in descriptor space. Widely used detectors include SIFT, SURF, and ORB.

#### 1.2.1 SIFT

Scale-Invariant Feature Transform (SIFT), introduced by David Lowe in 1999, proceeds in four stages:

1. **Scale-space extrema detection.** Construct a scale space with Difference-of-Gaussian (DoG) filters and find potential scale- and orientation-invariant interest points.  
2. **Keypoint localization.** Fit a quadratic model around each candidate to refine the position and reject unstable points.  
3. **Orientation assignment.** Use local gradient orientations to assign one or more canonical directions so that subsequent processing is rotation invariant.  
4. **Descriptor construction.** Measure local gradients at the selected scale and pool them into histograms that tolerate local deformations and illumination changes.

SIFT is robust to rotation, uniform scaling, and global brightness changes, and it tolerates a certain amount of viewpoint variation, affine distortion, and noise.

#### 1.2.2 SURF

Speeded Up Robust Features (SURF), proposed by Herbert Bay et al. in 2006, accelerates SIFT by using integral images, approximated Gaussian derivatives, and simplified descriptors, enabling real-time deployment.

#### 1.2.3 ORB

Oriented FAST and Rotated BRIEF (ORB) combines a FAST keypoint detector with a BRIEF descriptor, augmenting each with orientation and rotation invariance. Proposed in 2011 by Rublee et al., ORB’s optimizations yield roughly 100× the speed of SIFT and 10× the speed of SURF while remaining competitive in accuracy.

## 2. ORB Feature Detection and Matching

### 2.1 oFAST: FAST Keypoint Orientation

ORB inherits the FAST detector but augments it with an orientation estimate, producing oFAST keypoints.

#### 2.1.1 FAST Keypoint Detection

FAST tests the intensity differences between a candidate pixel $p$ and the pixels on a circle of radius 3. If a contiguous set of pixels exceeds a brightness threshold relative to $p$ (typically at least three of the samples at $90^{\circ}$ increments and at least three quarters of the full 16 samples), $p$ is declared a corner:
$$
N = \sum_x |I(x) - I(p)| > \varepsilon_d.
$$
Early rejection—checking the four pixels at $0^{\circ}$, $90^{\circ}$, $180^{\circ}$, $270^{\circ}$ first—further speeds up the test.

#### 2.1.2 Intensity-Centroid Orientation

Given the image moments
$$
m_{pq} = \sum_{x,y} x^p y^q I(x,y),
$$
the intensity centroid is
$$
C = \left(\frac{m_{10}}{m_{00}}, \frac{m_{01}}{m_{00}}\right),
$$
and the orientation of keypoint $p$ is obtained from the vector pointing from $p$ to $C$:
$$
\theta = \tan^{-1}(m_{01}, m_{10}).
$$
This centroid approach is stable under noise and gives each FAST corner a dominant direction.

### 2.2 rBRIEF: Rotation-Aware BRIEF

ORB uses BRIEF descriptors but adapts them for rotation and improved robustness.

#### 2.2.1 BRIEF Descriptor Construction

BRIEF randomly samples $n$ point pairs $\{(x_i, y_i)\}$ inside a patch centered at keypoint $p$ (typically a $31 \times 31$ window). For each pair, it compares intensities and forms a binary test:
$$
\tau(p; x, y) =
\begin{cases}
1 & p(x) < p(y) \\
0 & \text{otherwise}
\end{cases}
$$
The descriptor is the concatenation of these bits,
$$
f_n(p) = \sum_{i=1}^{n} 2^{i-1} \tau(p; x_i, y_i),
$$
producing a compact string such as `1011` for $n=4$. BRIEF is fast but not rotation invariant and is sensitive to noise, so Gaussian smoothing is often applied first.

#### 2.2.2 Steered BRIEF

By stacking the point pairs into a $2 \times n$ matrix
$$
S =
\begin{pmatrix}
x_1 & \ldots & x_n \\
y_1 & \ldots & y_n
\end{pmatrix},
$$
and rotating it with $R_\theta$, the sampling pattern can be aligned with the keypoint orientation, producing a rotation-steered descriptor
$$
S_\theta = R_\theta S, \qquad g_n(p, \theta) = f_n(p) \mid (x_i, y_i) \in S_\theta.
$$
However, steering alone reduces descriptor variance and thus discriminability.

#### 2.2.3 rBRIEF

rBRIEF restores high variance and low correlation by learning the sampling pattern:

1. Build a dataset of 300k keypoints.  
2. Enumerate all possible binary tests formed by pairs of $5 \times 5$ subwindows inside the $31 \times 31$ patch ($M = 205{,}590$ candidates after removing duplicates).  
3. For each test, evaluate its mean (distance from 0.5) and rank them.  
4. Perform a greedy selection: add the best test to the result set $R$, remove it from the candidate list $T$, and keep adding tests whose correlation with already-selected tests stays below a threshold. Increase the threshold if fewer than 256 tests survive.

The resulting 256-bit descriptor maintains high variance around 0.5 and low inter-bit correlation, outperforming steered BRIEF. Using $5 \times 5$ subwindows (average intensities instead of single pixels) also improves noise robustness. Experiments show that rBRIEF remains accurate even as SNR drops, whereas SIFT deteriorates quickly.

### 2.3 Summary

ORB achieves much faster runtimes than SIFT or SURF while providing rotation invariance. Although it lacks inherent scale invariance, practical implementations (e.g., OpenCV) pair ORB with image pyramids and geometric verification to handle moderate scale changes.

## 3. Examples

### 3.1 SURF (MATLAB)

```matlab
%% Read Images
Temp = rgb2gray(imread('temp.jpg'));
I = rgb2gray(imread('image.jpg'));
TPoints = detectSURFFeatures(Temp);
IPoints = detectSURFFeatures(I);
%% Detect Feature Points
figure;
imshow(Temp);
title('Feature Points from Template');
hold on;
plot(TPoints);
figure;
imshow(I);
title('1000 Strongest Feature Points from Image');
hold on;
plot(selectStrongest(IPoints, 1000));
%% Extract Feature Descriptors
[TFeatures, TPoints] = extractFeatures(Temp, TPoints);
[IFeatures, IPoints] = extractFeatures(I, IPoints);
%% Find Putative Point Matches
Pairs = matchFeatures(TFeatures, IFeatures);
matchedTPoints = TPoints(Pairs(:, 1), :);
matchedIPoints = IPoints(Pairs(:, 2), :);
figure;
showMatchedFeatures(Temp, I, matchedTPoints, ...
    matchedIPoints, 'montage');
title('Putatively Matched Points (Including Outliers)');
%% Locate the Object in the Scene Using Putative Matches
[tform, inlierTPoints, inlierIPoints] = ...
    estimateGeometricTransform(matchedTPoints, matchedIPoints, 'affine');
figure;
showMatchedFeatures(Temp, I, inlierTPoints, ...
    inlierIPoints, 'montage');
title('Matched Points (Inliers Only)');
%%
TPolygon = [1, 1;...
        size(Temp, 2), 1;...
        size(Temp, 2), size(Temp, 1);...
        1, size(Temp, 1);...
        1, 1];
newTPolygon = transformPointsForward(tform, TPolygon);
figure;
imshow(I);
hold on;
line(newTPolygon(:, 1), newTPolygon(:, 2), 'Color', 'y');
title('Detected Point');
```

### 3.2 ORB (C++)

```c++
#include <iostream>
#include <opencv2/core/core.hpp>
#include <opencv2/features2d/features2d.hpp>
#include <opencv2/highgui/highgui.hpp>
using namespace std;
using namespace cv;
int main(int argc, char** argv)
{
    int pairN = 1;
    char buf[100];
    sprintf(buf, "F:\\OpenCV\\ORB\\img\\pair%04d_frm1.jpg", pairN);
    Mat I = imread(buf);
    sprintf(buf, "F:\\OpenCV\\ORB\\img\\pair%04d_frm2.jpg", pairN);
    Mat T = imread(buf);
    std::vector<KeyPoint> keypoints_I, keypoints_T;
    Mat descriptors_I, descriptors_T;
    Ptr<FeatureDetector> detector = ORB::create();
    Ptr<DescriptorExtractor> descriptor = ORB::create();
    Ptr<DescriptorMatcher> matcher = DescriptorMatcher::create("BruteForce-Hamming");
    detector->detect(I, keypoints_I);
    detector->detect(T, keypoints_T);
    descriptor->compute(I, keypoints_I, descriptors_I);
    descriptor->compute(T, keypoints_T, descriptors_T);
    Mat ShowKeypointsI, ShowKeypointsT;
    drawKeypoints(I, keypoints_I, ShowKeypointsI, Scalar::all(-1), DrawMatchesFlags::DEFAULT);
    drawKeypoints(T, keypoints_T, ShowKeypointsT, Scalar::all(-1), DrawMatchesFlags::DEFAULT);
    imshow("KeypointsI", ShowKeypointsI);
    imshow("KeypointsT", ShowKeypointsT);
    imwrite("F:\\OpenCV\\ORB\\img\\KeypointsI.jpg", ShowKeypointsI);
    imwrite("F:\\OpenCV\\ORB\\img\\KeypointsT.jpg", ShowKeypointsT);
    vector<DMatch> matches;
    matcher->match(descriptors_I, descriptors_T, matches);
    double min_dist = 10000, max_dist = 0;
    for (int i = 0; i < descriptors_I.rows; i++)
    {
        double dist = matches[i].distance;
        if (dist < min_dist) min_dist = dist;
        if (dist > max_dist) max_dist = dist;
    }
    cout << "max dist: " << max_dist << endl;
    cout << "min dist: " << min_dist << endl;
    std::vector<DMatch> good_matches;
    for (int i = 0; i < descriptors_I.rows; i++)
    {
        if (matches[i].distance <= max(1.5 * min_dist, 30.0))
        {
            good_matches.push_back(matches[i]);
        }
    }
    Mat img_goodmatch;
    drawMatches(I, keypoints_I, T, keypoints_T, good_matches, img_goodmatch);
    imshow("goodmatch ", img_goodmatch);
    imwrite("F:\\OpenCV\\ORB\\img\\goodmatch.jpg", img_goodmatch);
    waitKey(0);
    return 0;
}
```
