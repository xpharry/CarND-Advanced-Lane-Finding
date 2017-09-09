# Advanced Lane Finding Project

## Project Overview

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./output_images/thresholded_output.png "Binary Example"
[image4]: ./output_images/warped_output.png "Warp Example"
[image5]: ./output_images/fit_lane_lines.png "Fit Visual"
[image6]: ./output_images/image_pipeline_output.png "Output"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

Here I will consider the rubric points individually and describe how I addressed each point in my implementation.

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the 3rd code cell of the IPython notebook located in "./advanced_lane_finding.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image (thresholding steps in a series of cells titled "Step 3: Color & Gradient Threshold" in "./advanced_lane_finding.ipynb").  Here's an example of my output for this step.  (note: this is not actually from one of the test images)

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `perspective_transform()`, which appears in a cell titled "Perspective Transformation" in "./advanced_lane_finding.ipynb".  The `perspective_transform()` function takes as inputs an image (`img`), as well as a transform matrix which is computed from a function called "compute_perspective_M(src, dst)".  I chose the hardcode the source and destination points and the coordinate values are shown in the table below:

The source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 580, 460      | 440, 0        | 
| 700, 460      | 950, 0        |
| 1100, 720     | 950, 720      |
| 200, 720      | 440, 720      |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I used the method of sliding windows to fit my lane lines with a 2nd order polynomial and the code are shown in the cell titled "Concised Verison of Fitting a Polynomial using Sliding Windows". An example output is shown as below:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the cell titled "repeat the calculation of radius of curvature after correcting for scale in x and y" in my Jupyter notebook.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the cell titled "Single Image Processing Pipeline" in the function `image_processing_pipepine()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Oh, in the very beginning, I want to say this one is much tougher than I expected. I devote so much time to absorb the code and restyle it to be my own. Even now, several problems such as applying threshold and automatically redo the sliding windows, haven't be addressed perfectly but only mostly. I am submitting this only to meet the basic requirement and hoping to get some feedback from the reviewers.

In this project, I practiced the techniques commonly used in detecting lanes, including calibrating cameras, distortion correction, applying thresholds with sobel information and different color channels and fitting lane lines using sliding windows.

The biggest issue came to me when I was trying to find the best combinations of thresholds. Despite of the my fresh knowledge and limited experience, I cannot use any theoretical reasoning to navigate the parameter adjusting. Finally I only got a so so result.

The required video generated from "project_video.mp4", which I named "project_video_outut.mp4", was showing perfect lane detecting ignoring some failures for a small while.

Below are two aspects I will try to improve from:

1) Optimising the threshold combinations to get a more clear lane lines;

2) Designing a mechanism to automatically redo sliding windows when necessary instead of every time.