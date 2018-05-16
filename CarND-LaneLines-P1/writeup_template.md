# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[image1]: ./masked_image.jpg
[image2]: ./grid.png

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of the following steps:
1. Converting image to HSV format: This allows easy application of color masks in step 2.
2. Isolate the yellow and white lines in the image: The resulting figure is shown below in hsv color format.
![alt text][image1]
3. Grayscale the image
4. Apply Gaussian filter to the image
5. Apply Canny edge detection to the image
6. Define a region of interest to filter out unwanted lines: Instead of hit and try, a grid was drawn on top of the image to figure out appropriate polygon and then the coordinates of the polygon were made a function of the image size. 
![alt text][image2]
7. Run Hough transform on the edge detected image
8. Combine the lines returned by the Hough function appropriately to show only two lines for left and right lanes: it can be seen that left lanes have a negative slope, whereas the right lanes have a positive slope. Going through all the line segments returned in the previous step, we divide it into two categories - negative slope and positive slope. Assuming that the lane detection is working correctly, the number of line segments representing lane lines will be higher than the number of lines representing other things in the environment. For the negative (resp. positive) slopes, the (respective) median slope should represent the slope of the estimated left (resp. right)line segment. Accepting only slopes within a tolerance (= 0.1 for the solution here) of the median, (x, y) coordinates for line segments are collected. Then a best fit line is computed to minimize the least squares error using the stats.linregress() function and corresponding lines are drawn.
9. Final step is to overlay the lines on the original image




### 2. Identify potential shortcomings with your current pipeline


The following are potential shortcomings of this approach:
1. The yellow and white mask applied might not be able to identify all possible shades of yellow and white under different light conditions. This includes performance of algorithm at night, under tree shadows, etc. The solution here fails to adapt to shadows for example.
2. The code actually worked fine without the mask above, as long as the region of interest was correct. The region of interest can change depending on the view from the dashboard of the car, for e.g., uphill/downhill roads. The mask was used to improve performance for the challenge video. Automating the region of interest calculation based on the output of the camera is very important for a real solution.
3. Lines in each image sample from the video are independent, however, real lane lines across images are not independent. Recognizing how the lane lines are and drawing the correct polynomial to represent lane lines is crucial.


### 3. Suggest possible improvements to your pipeline

Possible improvements are:
1. Train the solution over multiple videos to figure out more optimized masks for white and yellow lines.
2. Implement a least squares solution for making lane lines across different images within a video more smooth. This could also help eliminate lane lines which are a significantly off from previous ones.

