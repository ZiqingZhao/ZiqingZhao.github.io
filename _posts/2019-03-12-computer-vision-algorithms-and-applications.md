---
layout: post
title: "Computer Vision: Algorithms and Applications"
date: 2019-03-12
excerpt: ""
tags: [Computer Vision]
comments: true
---
[1] R. Szeliski, *Computer Vision: Algorithms and Applications*, 2010.  
[2] R. Szeliski, A. Hai-zhou, *Computer Vision: Algorithms and Applications*, Tsinghua University Press, 2012.

### Book Overview

Szeliski uses a chapter map that moves from image-based (2D) topics on the left, through geometry (3D), to photometric and appearance-based topics on the right. As you go downward the abstraction level increases: lower levels build on algorithms introduced earlier, although the dependencies are not strictly linear.

Chapter 2 surveys **image formation**. Section 2.1 covers geometric image formation, i.e., how points, lines, and planes are projected using projective geometry—including radial lens distortion. Section 2.2 focuses on **radiometry** and **optics**, while Section 2.3 explains how sensors work, covering sampling, aliasing, color perception, and on-camera compression.

Chapter 3 is about **image processing**, such as linear and nonlinear filtering (3.3), Fourier transforms (3.4), image pyramids and wavelets (3.5), image warping (3.6), and global optimization methods including **regularization** and **Markov random fields** (3.7). The chapter closes with applications like seamless stitching and image restoration.

Chapter 4 introduces **feature detection and matching**—the basis for many 3D reconstruction and recognition pipelines used again in Chapters 6, 7, 9, and 14. The chapter also reviews edge and line detection.

Chapter 5 covers **region segmentation** techniques such as active contours and tracking. Methods include split-and-merge, mean shift, and graph-based segmentation, with applications in performance-driven animation, interactive editing, and recognition.

Chapter 6 explains **geometric alignment** and **camera calibration**. Section 6.1 solves feature-based alignment with linear or nonlinear least squares plus uncertainty weighting and robust regression. Section 6.2 applies these ideas to **3D pose estimation**, while Section 6.3 shows how alignment feeds into intrinsic calibration, with examples such as photo registration for flipbooks, handheld 3D pose estimation, and single-view modeling of architecture.

Chapter 7 focuses on **structure from motion (SfM)**. Section 7.1 starts with **3D point triangulation** when camera poses are known, then reviews algebraic techniques and RANSAC-style robust sampling for two-frame SfM. The second half of the chapter studies **multi-frame SfM**, including factorization (7.3), **bundle adjustment** (7.4), and constrained motion/structure models (7.5), plus applications like view morphing, sparse 3D modeling, and match moving.

Chapter 8 returns to intensity information with **dense, intensity-based motion estimation** (optical flow). Section 8.1 introduces translational motion, hierarchical schemes, Fourier methods, and iterative refinement. Section 8.2 generalizes to **parametric motion models** for camera rotation and zoom, Section 8.3 describes spline-based models, and Section 8.4 moves to general per-pixel optical flow, including layered and learned models in Section 8.5. Applications include automated morphing, frame interpolation, and motion-driven interfaces.

Chapter 9 discusses **image stitching** for panoramic mosaics. Section 9.1 catalogues motion models such as planar motion and pure camera rotation. Section 9.2 presents **global alignment** (a special case of bundle adjustment), Section 9.3 covers **panorama recognition**, and the chapter closes with compositing/blending strategies that hide exposure differences. Stitching leverages image warping and feature matching for tasks like whiteboard scanning, video summarization, 360-degree panoramas, and interactive photo montages.

Chapter 10 surveys other **computational photography** techniques. Section 10.1 emphasizes precise modeling/calibration of image formation, followed by high dynamic range imaging from multiple exposures (10.2), **deblurring and super-resolution** (10.3), and **image editing/compositing** (10.4). Section 10.5 adds **texture analysis**, **synthesis**, **inpainting**, and **non-photorealistic rendering**.

Chapter 11 focuses on **stereo correspondence**, a constrained instance of motion estimation with known camera poses. Stereo searches a smaller space, yielding dense depth estimates that form **visible surface models** (11.3). Section 11.6 extends to **multi-view stereo** for reconstructing full 3D surfaces, enabling applications like head and gaze tracking and depth-based background replacement.

Chapter 12 introduces additional **3D shape and appearance modeling** techniques, from classic shape-from-X methods (shading, texture, focus, occluding contours, silhouettes) to **active range finding** via structured light. The chapter also discusses interpolation, geometric simplification, **surface point sets**, specialized modeling for buildings/faces/bodies, and **appearance modeling** methods for estimating texture maps, albedo, and BRDFs.

Chapter 13 reviews **image-based rendering** approaches such as **view interpolation**, **layered depth images**, sprites/layers, and higher-order light field or Lumigraph representations, along with applications in video-based rendering: video denoising, morphing, video textures, and 360-degree tours.

Chapter 14 covers **recognition**: Sections 14.1–14.2 discuss face detection/recognition, Section 14.3 targets specific object instances, Section 14.4 broad categories, and Section 14.5 explains how **scene context** guides recognition.
