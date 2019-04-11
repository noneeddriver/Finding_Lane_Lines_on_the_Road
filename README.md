[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

# **Finding Lane Lines on the Road** 

#### Author: Pengmian Yan

#### Last modified: April 11th, 2019

#### Version: 1.0



## Overview

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project, I designed a pipeline which can detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

The requirements from Udacity are in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)



### List of important files:

#### Codes:
1. Finding_Lane_Lines_in_image_Yan.ipynb
2. Finding_Lane_Lines_in_video_Yan.ipynb
#### Writeup:
1. README.md (You are reading it now.)
#### test_images_output: (My Pipline worked at all images well.)
1. solidWhiteCurve_result.jpg
2. solidWhiteRight_result.jpg
3. solidYellowCurve2_result.jpg
4. solidYellowCurve_result.jpg
5. solidYellowLeft_result.jpg
6. whiteCarLaneSwitch_result.jpg
#### test_videos_output:
1. solidYellowLeft_output.mp4 (My Pipline worked well)   
2. solidWhiteRight_output.mp4 (An error occurred between the third and fourth second.)
3. challenge_output.mp4 (My current pipeline didn't work, because it's mainly for straight lane lines without disturbing lines. )

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # "Image References"
[image1]: ./test_images_output/solidYellowCurve2_gray.jpg "Grayscale"
[image2]: ./test_images_output/solidYellowCurve2_edges.jpg "Edges"
[image3]: ./test_images_output/solidYellowCurve2_linesegments.jpg "Linesegments"
[image4]: ./test_images_output/right_lines_before_filter.PNG "right_lines_before_filter"
[image5]: ./test_images_output/right_lines_after_filter.PNG "right_lines_after_filter"
[image6]: ./test_images_output/solidYellowCurve2_result.jpg "result"
[image7]: ./test_images_output/Error.PNG "error"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. First, I converted the images to grayscale.

![alt text][image1]

Then I use the Canny algorithm to detect edges in grayscaled images. The Gaussian smoothing was then used to smooth the edges. 

![alt text][image2]

After that, I defined a regio of interest (ROI), which was applied as a mask in the image. I detected line segments in edges point images with the Hough tranform, but only in the ROI. 

![alt text][image3]

Next were the detected line segments devided to the left group and the right groutp, depending on their solops. Additionally with the X-positon of the line segments were the plausibility also checked. For instance, the right line segments schouldn't have a X-Position smaller than 500. Besides, the slope of each left line segment should be within the reasonable tolerance arround the average slope of left line segments. This can help filter out the disturb and noise, especially for the image solidYellowCurve2.jpg. 

right line segments before filter:
![alt text][image4]
                                   
  right line segments after filter:
![alt text][image5]
                                   

I generate then points with an increment at the detected line segments. For each line segment could their end points be choosed, no matter the length of line segments. 

With the generated points would a left line and a right line fitted.

At last, the two lines would with the original image merged, with weights. The result looks well. 

![alt text][image6]

Other images were also tested, they were all fine. 



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be limited accuracy when the lane line are not so straight.

Another issue is my pipeline get following error at the video solidWhiteRight (between third and fourth second):

![alt text][image7]

I will fix it in next version.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to fit the points in polynom instead of straight line. 

Another potential improvement could be to make the video annotation robuster.
