# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

* convert image to grayscale
* use gaussian filter to smooth image
* use canny edge detection to find all edges
* set an area where lines should be included, mask areas outside the area to black
* apply hough transform to find lines
* use draw lines to put line segments together and draw it on the original image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function with below steps.

* use (y2-y1)/(x2-x1) to get slope of each line segments
* abandon line segments with small absolute slope value, which indicate they are unlikely to be actual road lines
* divide line segments into left & right based on the sign of slope value
* use polyfit to fit the line to y = ax + b. (get value of a & b)
* find min & max value of y of left & right line segments, use the formula to calculate left bottom x, left top x, right top x, right bottom x.
* draw the line using (left bottom point, left top point) and (right bottom point, right top point).

If you'd like to include images to show how the pipeline works, here is how to include an image: 

[image2]: ./test_images_output/example.jpg "Pipeline Image"


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the image size is different. I use absolute value of X,Y to mask the image to get interest region. This would be a problem when the image size is different. 

Another shortcoming could be the filter for invalid line segments. Currently I check the slope of line segment, and drop it if the absolute value is smaller than a threshold. It might be a problem when there are line segments with a slope value similar to the slope of actual road lines.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to mask interest regions using the relative scale of X,Y to the image shape, so as to fit an image with any size.

Another potential improvement could be to adjust parameters for gaussian filter, canny edges, and hough transform, so as to prevent or reduce invalid line segments.
