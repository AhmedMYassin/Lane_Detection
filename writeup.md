# **Finding Lane Lines on the Road** 

## Lane Detection Project

The project main goal to detect lane lines on the road. To do so we create a pipeline of actions on images that represent the camera output.

## Pipeline Description

* The fllowing steps of the pipeline is used to detect lane lines in images and consequently in videos
1. Convert image to grayscale using grayscale(img)

2. Smoothing the image using **gaussian_blur(image, kernel_size)**. Although this step is optional, it's very important to make it easier to detect edges in next step.

3. Detect edges using canny edge detection algorithm. This algorithm depends on the selected low threshold and high threshold parameters. it was tuned manually to find the best ones.

4. Creating a masking area. This helps a lot to focus on certain area of the image. The selected area vertices are tuned also to find the most important part of the image for lane detection.

5. Creating lines from detected edges using hough transform algorithm. We have here different parameters to set (rho, theta, threshold, min_line_length and max_line_gap). These parameters are tuned to get reasonable generated lines.

6. Lane lines detection: in **draw_lines()**, we need to find the lane lines using the generated lines from hough transform algorithm. 
It's known that the slope is the key to filter the generated lines so we set slope threshold. 
Now we have accepted lines in 2 groups as per the slope orientation. We need only 2 representitive lines so we use regression method; we already have the start and end points for each line so we can use these points to find the best fit lines (y = mx + b). we apply the next formula to find m and b

   m = (mean(x*y) - mean(x)*mean(y)) / (mean(x^2) - mean(x)^2)
   
   b = mean(y) - m*mean(x)
   
   Now we have the parameters of each line so we can draw the lines and show them on the original one using **weighted_img()**

![image2](/test_images_output/solidWhiteCurve.png?raw=true "Output")

7. We apply the same steps for videos as we apply the same pipeline for each frame.

* Check the test images [here](https://github.com/AhmedMYassin/Lane_Detection/tree/master/test_images) then check the output images [here](https://github.com/AhmedMYassin/Lane_Detection/tree/master/test_images_output)
* Check the test videos [here](https://github.com/AhmedMYassin/Lane_Detection/tree/master/test_videos) then check the output vidoes [here](https://github.com/AhmedMYassin/Lane_Detection/tree/master/test_videos_output)


### 2. Potential Shortcoming

Tuned parameters is the main shortcoming of this code. If we have a closer look on these tuned parameters, we will find that the whole process will fail if enviromnement changes:

1. Driving in the morning needs different edge detection parameters to the ones needed for driving in the night.

2. The masked area would need to be changed as it depends on the camera and the car position.

### 3. Suggested possible improvements

1. Tuned parameters are maybe a good and easy solution for problems that can be controlled but for uncontrolled environment it's better to update parameters automatically using deep-learning techniques.

2. While working with videos, we can use filters like kalman to keep tracking of the detected lane lines to make use of previous frames.
