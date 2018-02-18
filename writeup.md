Self-Driving Car Project 1 Writeup
# **Finding Lane Lines on the Road** 
---

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

## Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
The pipeline consisted of 6 steps: 
- Step1: Convert the images to grayscale. 
- Step2: Apply Gaussian Blurring to smooth out noise and spurious gradient.
- Step3: Apply Canny Edge detection with low and high thresholds.
- Step4: Apply masking on the image to select a region of interest.
- Step5: Apply hough transformation to detect line segments.
- Step6: Stack the detected line segments onto the original image.

This produces the result with lane segments instead of a single line on the left and right lanes. In order to plot a single lane line, further modifications are made to the draw_lines() function.

According to the comments in the function definition, 
- Step 1: We group line segments into left or right according their slope.  
- Step 2: Then we take an average of the grouped line segments to get an averaged line segments, represented by two averaged points (x1_avg, y1_avg) and (x2_avg, y2_avg). 
- Step 3: Then we can calculate the averaged slope of the left and right lane.
- Step 4: Given the slope, we can calculate the bias of the lane lines. Once we get both slope and bias, we can draw the full lane line.

However, the average slope value can sometimes be skewed by line segments detected in the region of interest that have invalid slope values. Therefore, we have to remove them before taking the average in step 2.

### 2. Identify potential shortcomings with your current pipeline
The major issue here is that the region of interest is defined manually and hardcoded in the pipeline. For most test cases it works fine but for the last challenge, it failed on some parts of the road. The road conditions can vary so the region of interest should do that too. 

Another issue with this computer vision approach as a whole is that in northen countries, the snow in winter can easily cover the lane line and make it hard even for human eyes to detect. I wonder how we can solve this problem.

### 3. Suggest possible improvements to your pipeline
If there is a way to dynamically determine the region of interest or allow passing in different region of interest instead of hardcoding, it could be helpful to handle images of various road conditions.