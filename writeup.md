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

The steps I used to process the image can be found in `process_image(img_in)` function:
1. Convert to grayscale
2. Guassian blur with kernel size of 5
3. Detect edges using Canny, thresholds are (50,150)
4. Cut off a region of interest (trapezoidal)
5. Transform the image to hough space and detect lines
6. Filter zero length lines, low slope (nearly horizontal) lines
7. Split the lines into left and right lanes
8. Filter out low quality lines by their length percentile in the lane
9. Average the remaining lines per lane
10. Draw the average lines on the original

I didn't see the need to modify draw_lines, but rather to limit it to the minimal function that its name implies - draw a list of lines on an image.
Instead I do averaging of the lines using a Line and LineAverager class I wrote. 


### 2. Identify potential shortcomings with your current pipeline

First problem is that the current pipeline assumes straight lines, which would lead to jumps in estimated slope and curvature 
if the algorithm detects only part of the lane (due to shadows, hiding by another car, etc.).

Also, by limiting the field of view to a narrow trapeze, I am missing out the option of identifying turns and of 
the algorithm performing during slope changes.

If the lane is obstructed by another vehicle making a maneuver, the pipeline will likely miss the lane. 
If road crack patches or old and erased lane lines are still visible, the pipeline might be confused and identify them as lane lines.

Another issue is that since parameters (thresholds for example) are fixed, it is unclear how the algorithm will deal with night driving, 
with wet road, with worn-off markings, etc.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to try some parameters to detect the lane lines, and if this fails, 
search over other parameters or features, color spaces - until a confident enough lane is detected. 
