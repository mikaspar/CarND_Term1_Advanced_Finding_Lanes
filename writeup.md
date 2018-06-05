## Advanced Lane Finding



---



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

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./examples/Sobel_X.jpg "Sobel X"
[image3]: ./examples/Sobel_Mag.jpg "Sobel Magnitude"
[image4]: ./examples/S_Channel.jpg "S-Channel"
[image5]: ./examples/warped_line_lanes.png "Warped Lane Lines"
[image6]: ./examples/Result.jpg "Result"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./P4_Advanced_Lane_Finding.ipynb" .  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate the results of this step, I saved the undistortion corrected images in the "./output_images" folder. 

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I proved results of all in the lesson provided techniques to obtain a proper binary image to identify the lane lines. At the end I winded up using the combination of Sobel X, Magnitude Sobel and S-channel of the HLS space. The result ouputs the sobel_m_S function.

[image1]
[image2]
[image3]
[image4]
[image5]
[image6]
[video1]





#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

Followingly I used the perspectice transform in order to identify the "real" curvature of the lane (supposed to be perfectly flat). 

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 260, 700      | 260, 0        | 
| 1060, 70      | 1060, 720      |
| 685, 450      | 1060, 720      |
| 595, 450      | 260, 0        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.



#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I used the histogram function along the x-axis to find the pixel peaks representing the lane lines. After that I used the brute search function to identify the pixels of the left and right lane line. Once lane found and polynom can be fitted, previous knowledge search can be started in order to search for lane pixels in surrounding area of the found lane only.



#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the function curve_pos_calc of the provided notebook, using the calculation provided in the lesson, considering the pixel -> meter conversion.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines in the function final of the notebook.


---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

In order to improve the performace a better sanity check function would have to be developed. Up to now I use only line distance and curvature similarity test. If this one fails, the lines are identified as corrupted and the brute search function starts over again. The technique of obtaining the binary images could be improved using more channel and finetuning the treshold parameters. 
