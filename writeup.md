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
1.	I convert the images to grayscale
2.	I apply a Gaussian blurring (with a kernel size of 5) to suppress most of noise. Note that this step is not necessary since the Canny filter function already includes Gaussian blurring with a kernel size of 5. 
3.	I apply the canny filter to the gray blurred image to extract the edges. I choose a low threshold of 50 and a high threshold of 150 (the max is 255 pixels)
4.	Then I select a region of interest including the lane lines we want to detect. I suppose that the region of interest is always located at the same place since the camera is at a fixed place. The region of interest is a percentage of the image width and height
5.	Finally, I apply a Hough transform to found the lane lines.


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by using the nb.polyfit function.

The Draw lines function takes as input the hough lines. Each Hough lines is defined by a start and an end point.

First we need to know if a point is part of the right or the left lane line. For that we calculate the slope of the hough line ((y2-y1)/(x2-x1)). If the slope is > 0 the points are part of the right line. If not, they are part of the left line.

Once we defined if the points are part of the right or left line, we use the nb.polyfit function to find a polynomial of degree 1 (y = mx + b) fitting to the right line points and to the left line points. This give us two m and two b coefficients

Finally to draw the extrapolated left and right line I use the following x and y coordinates:
start point : 	y = img_height; x = (y - b)/m
end point :   	y = img_height*0.6  (it corresponds to the top of the region of interest); x = (y - b)/m

Found hereafter some images to show how the new draw_lines function in action:



![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


I identify the following shortcomings:
-	What would happen when contrast between the road and the lane lines is modified by the weather or the night
-	What happen if another car is in the region of interest?
-	Big curves can modify the hardcoded region of interest. The region of interest I have defined is not working with the “challenge” video
-	The function parameters (canny, hough lines, ...) are hardcoded. I imagine that they must change per road, weather and a lot of other conditions. Making this kind of algorithm to work in all conditions should be a real nightmare.



### 3. Suggest possible improvements to your pipeline

-	All the hardcoded values (functions parameters, region of interest) should be defined dynamically 
-	Add more filtering to better detect line edges.
-	Why just doing the filtering on the gray image. It should also be done to the yellow, blue and red image to get back more information.

