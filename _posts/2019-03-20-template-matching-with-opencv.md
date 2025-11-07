---
layout: post
title: "Template Matching with OpenCV"
date: 2019-03-20
excerpt: ""
tags: [Computer Vision, Template Matching]
comments: true
---
Template matching looks for the region inside the full image that best matches a given template. The process compares a source image (I) against a template image (T) by sliding T across I and producing a response map (R). The location with the highest (or lowest, depending on the method) response indicates the best match.

### 1. The `matchTemplate()` Function

```c++
void matchTemplate(InputArray img, InputArray temp, OutputArray result, int method)
```

`img` is the source image, `temp` is the template, and `result` is the response map whose size equals `(img.rows - temp.rows + 1) * (img.cols - temp.cols + 1)`.

OpenCV implements six common matching methods:

#### 1. TM_SQDIFF

Computes the squared difference. Smaller responses mean better matches.

$$
R(x,y)=\sum\limits_{x',y'}(T(x',y')-I(x+x',y+y'))^2
$$

#### 2. TM_SQDIFF_NORMED

Computes a normalized squared difference. Values closer to 0 indicate better matches.

$$
R(x,y)=\frac{\sum\limits_{x',y'}(T(x',y')-I(x+x',y+y'))^2}{\sqrt{\sum\limits_{x',y'}T(x',y')^2\cdot\sum\limits_{x',y'}I(x+x',y+y')^2}}
$$

#### 3. TM_CCORR

Computes correlation. Larger responses mean better matches.

$$
R(x,y)=\sum\limits_{x',y'}(T(x',y')\cdot I(x+x',y+y'))
$$

#### 4. TM_CCORR_NORMED

Computes normalized correlation. Values closer to 1 represent better matches.

$$
R(x,y)=\frac{\sum\limits_{x',y'}(T(x',y')\cdot I(x+x',y+y'))}{\sqrt{\sum\limits_{x',y'}T(x',y')^2\cdot\sum\limits_{x',y'}I(x+x',y+y')^2}}
$$

#### 5. TM_CCOEFF

Computes the correlation coefficient. Larger responses indicate higher similarity.

$$
R(x,y)=\sum\limits_{x',y'}(T'(x',y')\cdot I'(x+x',y+y'))
$$

where

$$
T'(x',y')=T(x',y')-\frac{1}{w\cdot h}\sum_{x'',y''}T(x'',y'')
$$

$$
I'(x+x',y+y')=I(x+x',y+y')-\frac{1}{w\cdot h}\sum_{x'',y''}I(x+x'',y+y'')
$$

#### 6. TM_CCOEFF_NORMED

Computes the normalized correlation coefficient. Values closer to 1 indicate better matches.

$$
R(x,y)=\frac{\sum\limits_{x',y'}(T'(x',y')\cdot I'(x+x',y+y'))}{\sqrt{\sum\limits_{x',y'}T'(x',y')^2\cdot\sum\limits_{x',y'}I'(x+x',y+y')^2}}
$$

### 2. The `minMaxLoc()` Function

After computing the response map, `minMaxLoc()` locates the global minimum and maximum values along with their coordinates:

```c++
void minMaxLoc(InputArray img, double* minVal, double* maxVal=0, Point* minLoc=0, Point* maxLoc=0, InputArray mask=noArray())
```

`img` is the input array; `minVal` and `minLoc` receive the minimum value and its location; `maxVal` and `maxLoc` receive the maximum value and its location; `mask` can limit the search region.

If you used a squared-difference method in `matchTemplate()`, lower responses represent better matches, so you should inspect `minVal` and `minLoc`. For the other four methods, look at `maxVal` and `maxLoc` instead.

### 3. Example

```c++
#include <opencv2/core/core.hpp>
#include <opencv2/imgproc/imgproc.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <iostream>
#include <stdio.h>
using namespace std;
using namespace cv;
int main()
{
    Mat img, templ, result;
    img = imread("F://OpenCV//img//image_origin.jpg"); // source image
    templ = imread("F://OpenCV//img//temp_origin.jpg"); // template image
    result.create(img.rows - templ.rows + 1, img.cols - templ.cols + 1, CV_32FC1); // response map
    matchTemplate(img, templ, result, TM_SQDIFF_NORMED);
    // Compare other normalized methods: TM_SQDIFF_NORMED, TM_CCORR_NORMED, TM_CCOEFF_NORMED
    double minVal;
    double maxVal;
    Point minLoc;
    Point maxLoc;
    minMaxLoc(result, &minVal, &maxVal, &minLoc, &maxLoc); // locate extrema
    rectangle(img, maxLoc, Point(maxLoc.x + templ.cols, maxLoc.y + templ.rows), Scalar::all(0), 3); // draw match
    imshow("img", img);
    imshow("result", result);
    waitKey(0);
    return 0;
}
```

The sample demonstrates how to prepare the response map, find the best location, and visualize the matching rectangle.
