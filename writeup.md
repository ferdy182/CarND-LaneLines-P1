#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./red.jpg "Red"
[image2]: ./green.jpg "Green"
[image3]: ./blue.jpg "Blue"
[edges]: ./edges.png "Edges"
[lines]: ./lines.png "Lines"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I start by separating the image into color channels, I noticed that the red channel provides the higher contrast between the lines and the road, so I use it as my base.

Red
![alt text][image1]
Blue
![alt text][image2]
Green
![alt text][image3]

Then I apply a gaussian blur with a kernel value of 5 to smooth the image.

After that, I apply canny algorithm with a min thresold of 100 and a max thresold of 300

![alt text][edges]

Then I apply hough lines with a threshold of 50, a minimum line legth of 5 and a max line gap of 30.

Then I modified draw_lines to get a single line on each side like this:

I get the slope of the line, and if it is smaller than -0.4 I consider it to be in the left side, and if it is bigger than 0.4 then it is on the right side. 

Then for each side I store each point in an array and I get the average value for x1,y1,x2,y2, then I get the slope and intercept of these lines to extend the line to the border of the image and the horizon.

![][lines]

Then I apply a mask with the region of interest to eliminate lines that are not relevant. This mask is propotional to the size of the image.


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there is no lines or the contrast between the line and the asphalt is not good enough 

Another shortcoming could be if there are cars in front hiding the lines, there might no be enough votes to constitute a line.


###3. Suggest possible improvements to your pipeline

A possible improvement would be to use color ranges after converting the image to HSV to properly remove the background
