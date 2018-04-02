# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project we are detecting lane lines in images using Python and OpenCV. 

[//]: # (Image References)

[image1]: ./test_images_output/solidWhiteRight.jpg "SolidWhiteRight"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 major steps. 

First, I decided to use color filtering on the raw image to distinguish the colored lanes. The white colored objects were segregated and then I repeated this procedure for finding yellow objects after converting the raw image to HSV space.

After obtaining a grayscale image with yellow and white objects highlighted, it was smoothed using a Gaussian kernel before performing Canny Edge detection.

A Region of Interest (ROI) mask was defined and applied on the edge-highlighted image from the previous step.

On this ROI bounded edge-highlighted image, a Hough transform was applied to detect all the candidate lane lines' end-points.  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by segregating the end-points of candidate lines based on their slopes. I applied Least squares linear regression (numpy polyfit) to find the  fitting line parameters. A thresholding was applied on the slope for the fitting line to reject outlier cases. I then drew these fitting lines for the left and right lane in my ROI.

Here is an example image demonstrating how the pipeline works: 

![Example][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the road turns sharply and the detected lane's slope is not within the threshold. This pipeline would simply reject it as outlier and not detect the lane. 

Another shortcoming could be that shadows of trees,signboards etc. would interfere with the lane detection.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use a weighted moving average of slopes from recent frames to reject outliers instead of using fixed thresholds.

Another potential improvement could be to use a color space like HSL that can handle shadows/lightness of lanes better.

