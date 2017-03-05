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

#### Step 1

I started by separating the image into color channels, I noticed that the red channel provides the higher contrast between the lines and the road, so I use it as my base. Here are the 3 channels for comparison:

Red
![alt text][image1]
Blue
![alt text][image2]
Green
![alt text][image3]

#### Step 2

Then I apply a gaussian blur with a kernel value of 5 to smooth the image, this removes noise than can cause false lines.

#### Step 3 

After that, I apply canny algorithm with a min threhsold of 100 and a max thresold of 200 to find strong edges, as a ratio 1:2 or 1:3 is recommended.

![alt text][edges]

#### Step 4

Then I clip the image with the region of interest to avoid doing Hough Lines transform on lines that are not interesting

#### Step 5

Then I apply hough lines with a threshold of 50, a minimum line legth of 20 and a max line gap of 10. Instead of using a big line gap value to find a long line that crosses the dashed line, I detect the small sections of the line and process it later in the draw_lines method.

#### Step 6

Then I modified draw_lines to get a single line on each side like this:

I get the slope of the line, and if it is smaller than -0.4 I consider it to be in the left side, and if it is bigger than 0.4 then it is on the right side. I also check if the line is in the first half or the second half of the image to avoid strange lines interfering.

Then for each side I store each point in an array and I get the average value for x1,y1,x2,y2, then I get the slope and y-intercept of these lines to extend the line to the border of the image and the horizon.

![][lines]

#### Step 6

Then I apply a mask again with the region of interest to cut the final lines. This mask is propotional to the size of the image.


###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when there is no lines on the road, the system can't detect the lanes.

Another shortcoming is that the contrast between the line and the asphalt is not good enough, sometimes there are patches on the road which are very light and compared to the white line, is difficult to detect.

Another shortcoming is when the car enters a tunnel, the lighting conditions change and this is very bad for the camera and the detection algorithm, also cameras tend to take few seconds to adapt to the correct exposure and in the mean time, the algorithm will not find any lines.

Another shortcoming could be if there are cars in front hiding the lines, or construction works, there might no be enough votes to constitute a line.

And also depending on weather conditions, the lines might be more or less visible, such as rain or mist, that would make it more difficult, even at night, with the front lights on, the lines can't be seen far away.

The polygonal mask is fixed so might not always contain the lines, for instance, if there are curves or hills, the lines might not be detected far away from the car.

###3. Suggest possible improvements to your pipeline

A possible improvement would be to convert the image to HSV and then find the lanes using color ranges for yellow and white and remove the rest, then blend the lines detected into one image and work with that image, there should be less lines that don't belong to the lane.

Other improvements might be to use some kind of prediction to guess where the lines should be based on the previous frames, until lines are detected again.

It would also be better if the parameters were more dynamic so they could adapt to different lighting conditions or angles between the camera and the road.
