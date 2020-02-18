# **Finding Lane Lines on the Road** 

## This is a P1 project write-up

In this write-up to describe Finding Lane Lines on the Road.
The goals of the project is the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report (this)

### Reflection

#### 1. Pipeline description 

Pipeline for line identification is implemented. It takes road images and accepts video stream. 
The function process_image(image) takes images as input and returns an image with annotated lanes as output. Also video processing is provided taking images from the video, putting them through processing pipeline and provides video as an output.

Pipeline has following steps: 
 - conversion of images to grayscale
 - blurring using Gaussian filter
 - edge detection using Canny algorithm
 - image masking with the region of interest
 - feature extraction using Hough transform
 - lanes detection algorithm
 - drawing lanes
 - blending original image with lanes image

Region of interest is is adjusted after witnessed how pipeline behaves for "challenge.mp4". In that video car hood is visible on the bottom of the video. When Canny is applied to that area multiple edges are detected that are obviously from lanes marking. So region is adjusted not to include car hood.

Lane detection is implemented in in detect_lanes() function. It receives list of lines extracted by Hough transform and image dimension. First Boundaries are defining vertical field of interest. In theory it can be different then top and bottom of a masking region of interest. In demonstrated case same values are used - 0.6.
Lanes are found in these steps:
 - Hough features (lines) are filtered based on it slope
 - lines are then grouped with the ones that are positive and negative
 - mean slope and offset is then calculated.
 - cross section of detected mean lines and bottom the picture and top of the region of interest gives 

In this implementation draw_lines() function is not significantly modified. It receives list of lines that are coming from find_lanes(lines, width, height) algorithm. So it is used only as a step to display detected (found) lanes.

#### 2. Shortcomings with your current pipeline:

This is a very simplified approach that has many shortcomings:
1. Processing works only on limited lighting conditions that are tested with provided examples.
2. Transitions from dart to light segments it not handled very well.
3. Shadows from the trees and different asphalt colors are not taken enough i to account.
4. Lanes curvature is not estimated (considered). Lanes are modeled with straight lines

#### 3. Suggest possible improvements to your pipeline

For further improvements more testing is necessary. New test cases should cover:
1. Different lighting conditions including night drive and different weather conditions.
2. Different orientations and angles of view - this should be important while turning.
3. Lane changes - overtaking, advanced path patterns and similar.
4. Traffic densities - other vehicles and objects on the road
5. Road works, constructions sites and similar.

To improve this pipeline all shortcomings should be addressed and well tested on a big test suite as described above.
Algorithm improvements should be done in following directions:
 - utilizing different color spaces
 - curvature of the lanes modeling 
 - robustness improvements by crosschecking of different (parallel running) pipelines
