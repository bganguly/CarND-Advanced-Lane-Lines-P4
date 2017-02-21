##Writeup Template
---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
###Writeup / README

####1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.

This is the required README.  
All code cells mentioned here are in the IPython notebook located in "advanced-lane-finding.ipynb".  
For brevity, i will just refer to the IPython notebook as simply 'notebook'.  
Where needed, cells have embedded images in them, and i will skip mentioning that repeatedly in the discussion of rubric points below.
###Camera Calibration

####1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the code cell 2 and 3 of the  notebook.

In code cell 2 - I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

In code cell 3 - I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()`. 

At the end of this step the camera calibration cooefficients are made available to the remainder of the notebook.

###Pipeline (single images)

####1. Provide an example of a distortion-corrected image.

The code for this step is contained in the code cell 4 of the  notebook.  
We simply call cv2.undistort() with appropriate distorted image and calibration coefficients.  
The undistorted image is then made available to the remainder of the workbook.

####2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

The code for this step is contained in the code cells 5-7 of the  notebook.

In code cell 5, i define a set of convenience functions that i found useful via trail and error.  
These include yellw_white masking/ several types of sobel transforms and a few HLS transforms as well.

In code cell 6, i run the undistorted image (previously made available) through these transforms and compute a combined image. I then run the combined image through a cv2 getPerspectiveTransform followed by a cv2.warpPerspective(). This yields a binary_birdseye which is then made available to the remainder of the workbook. Finally i display both the combined and the binary_birdseye image.

In code cell 7, i process the undistorted image through various sobel transforms and display the results to get some insight into the transformed images.

####3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for this step is contained in the code cell 6 of the  notebook.

As described previously, I ran the undistorted image through several transforms and generate a combined image. I then run the combined image through a cv2 getPerspectiveTransform followed by a cv2.warpPerspective(). This yields a binary_birdseye which is then made available to the remainder of the workbook. Finally i display both the combined and the binary_birdseye image.

####4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

The code for this step is contained in the code cell 8 and 13 of the  notebook.

In code cell 8, i am essentially researching/ getting a feel for the np2.polyfit() function.  
In code cell 13, i sample the relevant part (bottom histogram) of the image in a series of small rectangles and then calculate the lane line polynomials. In retrospect, i should have created a separate helper function for this, rather than the lenghty step n cell 13.

####5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

The code for this step is contained in the code cell 12 of the  notebook.  
In addition to computing the curvature and the position of vehicle (offset), it also creates the lane onto a blank image, then does an inverse perspectiveTransform , then paints the region on to the udistorted image, then adds annotations on the undistorted image and finally returns the updated image with annotations.

####6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The code for this step is contained in the code cell 15 of the  notebook.  

---

###Pipeline (video)

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./movie.mp4) and the youtube link is this - https://www.youtube.com/watch?v=kGhNtidvWso.  
Sometimes when the youtube initially loads , some frames appear to be black, however that appears to be only a temporary issue as the physical video has no such 'drop offs' and the issue can't be consistently reproduced in the browser.

---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The video is still wobbly in some places and in some other places the left edge appears to separate from the left lane. I will reserach why this is being caused.
