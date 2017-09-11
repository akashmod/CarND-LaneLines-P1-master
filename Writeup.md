
# Finding Lane Lines on the Road - The Writeup

The goal of this project is to make a pipeline that finds lane lines on the road.

## Description of the pipeline used

My pipeline consisted of 5 steps. 1. I converted the images to grayscale, 2. then I used gaussian blur to blur the grayscale images to reduce the image noise. The gaussian blur kernel size of 5 was used as it has shown to provide good results before and is a standard number used in the industry. 3. Next, canny transformation was used to identify the edges. The low threshold of 50 and high threshold of 150 was used in the canny transformation as it has shown to provide good results in the past. 4. Following the canny transformation, the region of interest was selected by defining the mask. The vertical co-ordinate of 320 was used to define the upper limit of the area of interest. 5. Next comes drawing the houghlines in the image. The minimum line length was kept as 5 and the maximum line gap was kept at 1. The two images are then joined, i.e. the original camera image and the lines identified by pipeline in a weighted form and the output was saved.
![solidWhiteCurve1.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidWhiteCurve1.jpg)
![solidWhiteRight1.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidWhiteRight1.jpg)
![solidYellowCurve1.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidYellowCurve1.jpg)
![solidYellowLeft1.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidYellowLeft1.jpg)
![whiteCarLaneSwitch1.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/whiteCarLaneSwitch1.jpg)

In order to draw a single line on the left and right lanes, I created the draw_lines() function in the code such that it performs the following functions: First it segregates the lines into the left and the right lanes. The lines with positive slope are right lanes, since the y-co-ordinate increasing as we go down as opposed to the general trend and vice versa with the left lanes. Once the lines are segregated, we convert the lines identified by the hough transform in the two point form into the slope-intercept form. This gives better handling of the data as now we are using properties of the lines rather than points. We also calculate the lengths of the lines. Now, we take the weighted average of the line properties, i.e. the slope and the intercept along the entire left lane and right lane arrays to get the average line properties in both the cases. However, it has to be weighted average in terms of length, to reduce the effect of line noise in the average calculation. Now that we have the line properties, i.e. the slope and intercept of the line, we use it to calculate the x-co-ordinates of the points in the lowest y-co-ordinate and highest y-co-ordinates of the left and the right lanes and join them together to get the lane lines.

![solidWhiteCurve.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidWhiteCurve.jpg)
![solidWhiteRight.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidWhiteRight.jpg)
![solidYellowCurve.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidYellowCurve.jpg)
![solidYellowLeft.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/solidYellowLeft.jpg)
![whiteCarLaneSwitch.jpg](https://github.com/akashmod/Self_Driving_Cars-LaneLine_Detection/blob/master/test_images_output/whiteCarLaneSwitch.jpg)

The Pipeline was applied to a video captured by a car on a highway driving scenario and it performed satisfactorily in the detection of lane lines on the road for directing the driving of the vehicle. The processed videos can be found in the folder test_videos_output. I really wanted to show the videos here but it turns out the github finds the files too large to open on the site.

## Potential shortcomings with the current pipeline

One potential shortcoming would be what would happen when there are cars in front of the vehicle. When there are objects around the road, the algorithm will take the outline of the object or the car in front and add them to the calculation of the average line showing the lane and hence making it really inaccurate. The algorithm only works when there are only lanes in front of the vehicle or in the area of region. As soon as there is any other object, the algorithm fails. 

The second shortcoming could be when the vehicle is taking a sharp turn, the program can fail to identify the lane lines. This is because when a sharp turn is taken, there are very few lane lines in front of the camera and hence very few in the region of interest. This would make calculation of the average line difficult and hence cause inaccurate lane detection.

## Possible improvements to the pipeline

A possible improvement would be to lower the region of interest to accomodate sharp turns of the vehicle. The lanes right in front of the vehicle can be used to identify the present lanes and the same can be extrapolated to estimate the lanes ahead as even in turns, the lanes follow a pattern or a trend. This would reduce probability of disturbances due to lowered region of interest. 

Another potential improvement could be to use a mask to identify colors like white and yellow in the original picture. The lanes are typically white or yellow in colour and hence a mask that identifies these colors can be created. This mask can now be added to the output of the canny detection using the command bitwise_and to get the lines that were white or yellow in colour. The average can now be taken of these lines and used to get the extrapolated lane lines.
