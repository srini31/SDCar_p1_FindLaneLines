# **Finding Lane Lines on the Road** 

## Project Writeup
<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project we will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

Here are the project specifications listed in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)
---
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"


[//]: # (Image References)

[image1]: ./test_images/output_solidWhiteCurve.jpg "SolidWhiteCurve"
[image2]: ./test_images/output_solidYellowCurve.jpg "SolidYellowCurve"
---

### Reflection

### 1. Description of the pipeline and explanation of how the draw_lines() function was modified.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

1) read image and convert to gray scale, 
2) blur to blur the edges and remove high frequency content.
3) canny edge detection algorithm to detect all edges, this even includes tree line on the side of the road
4) set an region of interest by using vertices
5) create hough lines to gather all straight line segements
6) To draw a single line on left and right lanes, I created the draw_lines_new() function. Here I split small linesegments into left and right using the slope and finally found a mean value for center and slope for all the lines. Then a line is drawn from the bottom of the image to the start vertices of the region of interest that pass through this center point.
7) Finally I created the weighted lines and flipped r,g,b values to get the required red color overlaid on the image
 
![solidwhitecurve image][image1]

Add edge detection, canny lines, hough transform to get the final output lines


sample output
![solidyellowcurve image][image2]


### 2. Identification of potential shortcomings with the current pipeline

For all the pictures and videos, the current pipeline appears to work well. 

Some potential shortcoming would be what would happen when 

    - the road is curved 
    - when there are mutiple reflective objects and tire marks on the road
    - there are no lane lines marked on the road ( can happen on local streets)

Some major shortcomings were found when using the challenge video which are listed below.

    - Bridge and road gap (transition) is detected as a strong line
    - Tire marks on the road are detected and evaluated as a line
    - While turning, the region of interest also includes neighboring lanes and the slopes of lines in those lanes are included in the calculation

### 3. Possible improvements to the pipeline


Some possible improvements are

1) Near the concrete patch (bridge) picture of the video, there are so many small hough lines that are generated with
small slopes. I tried to filter them out in the draw_lines_new method. This improved the solution to some extent.

2) As road curves ahead, the left line slope direction matched the right line and this causes an error in slope calculation.
To overcome this, I reduced the region of interest by looking less further ahead. This is done by reducing the area of 
the polygon formed by the vertices. 


3) During a turn, the neighboring lanes are also included in the region of interestt which should be avoided


