# **Finding Lane Lines on the Road** 

## Writeup Template
<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />


---
The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

1) read image and convert to gray scale, 
2) blur to blur the edges and remove high frequency content.
3) canny edge detection algorithm to detect all edges, this even includes tree line on the side of the road
4) set an region of interest by using vertices
5) create hough lines to gather all straight line segements
6) To draw a single line on left and right lanes, I created the draw_lines_new() function. Here I split small linesegments into left and right using the slope and finally found a mean value for center and slope for all the lines. Then a line is drawn from the bottom of the image to the start vertices of the region of interest that pass through this center point.
7) Finally I created the weighted lines and flipped r,g,b values to get the required red color overlaid on the image
 

add edge detection, canny lines, hough transform, final lines
![alt text][./test_images/output_solidWhiteCurve.jpg "SolidWhiteCurve"]


### 2. Identify potential shortcomings with your current pipeline

For all the pictures and videos, the current pipeline appears to work well. 

Some potential shortcoming would be what would happen when 

    - the road is curved 
    - when there are mutiple reflective objects and tire marks on the road
    - there are no lane lines marked on the road ( can happen on local streets)

Some major shortcomings were found when using the challenge video which are listed below.

    - Bridge and road gap (transition) is detected as a strong line
    - Tire marks on the road are detected and evaluated as a line
    - While turning, the region of interest also includes neighboring lanes and the slopes of lines in those lanes are included in the calculation

### 3. Suggest possible improvements to your pipeline


Some possible improvements are

1) Near the concrete patch (bridge) picture of the video, there are so many small hough lines that are generated with
small slopes. I tried to filter them out in the draw_lines_new method. This improved the solution to some extent.

2) As road curves ahead, the left line slope direction matched the right line and this causes an error in slope calculation.
To overcome this, I reduced the region of interest by looking less further ahead. This is done by reducing the area of 
the polygon formed by the vertices. 


3) During a turn, the neighboring lanes are also included in the region of interestt which should be avoided


