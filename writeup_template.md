## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

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


[im01]: ./calibration_undist/undist_calibration2.jpg "Chessboard Calibration"
[im02]: ./projected_lanelines_output/projected_lanelines_test1.jpg "Projected Lane line 1"
[im03]: ./projected_lanelines_output/projected_lanelines_test2.jpg "Projected Lane line 2"
[im04]: ./projected_lanelines_output/projected_lanelines_tes31.jpg "Projected Lane line 3"
[im05]: ./projected_lanelines_output/projected_lanelines_test4.jpg "Projected Lane line 4"
[im06]: ./projected_lanelines_output/projected_lanelines_test5.jpg "Projected Lane line 5"
[im07]: ./projected_lanelines_output/projected_lanelines_test6.jpg "Projected Lane line 6"

[bin01]: ./binary_images/bin_image_1.png "Binary Image1"
[bin02]: ./binary_images/bin_image_2.png "Binary Image2"
[bin03]: ./binary_images/bin_image_3.png "Binary Image3"
[bin04]: ./binary_images/bin_image_4.png "Binary Image4"
[bin05]: ./binary_images/bin_image_5.png "Binary Image5"
[bin06]: ./binary_images/bin_image_6.png "Binary Image6"

[undist01]: ./undistored_warped_images/undistored_warped_image1.png "Undistorted Image 1"
[undist02]: ./undistored_warped_images/undistored_warped_image2.png "Undistorted Image 2"
[undist03]: ./undistored_warped_images/undistored_warped_image3.png "Undistorted Image 3"
[undist04]: ./undistored_warped_images/undistorted_warped_image4.png "Undistorted Image 4"
[undist05]: ./undistored_warped_images/undistorted_warped_image5.png "Undistorted Image 5"
[undist06]: ./undistored_warped_images/undistorted_warped_image6.png "Undistorted Image 6"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second code cell of the IPython notebook located in "./project.ipynb" 

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  This returned the camera caliberation coefficients which I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result. The camera distortion coefficients were saved in the file wide_dist_pickle.p to be read and used later for perspective transform 
The undistored caliberated image is 
![Undistored Caliberated Image][im01]



### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images 

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  

Cell number 4 and 5 has code that does absolute sobel, gradient magnitude threshold and direction gradient threshold
The binary image output for this operation is displayed in the python notebook at the end of cell 5
The input and output binary images are

![bin01]
![bin02]
![bin03]
![bin04]
![bin05]
![bin06]





#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform is in cell 6. I first create a trapezoid region of interest and the cell includes a function called corners_unwarp which takes in the image , the number of corners and camera distortion co effcisnts and does a perpective transform. The transformed images are also displayed in the notebook. 

I randomly chose destination points from source based on offsets



I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

The undistorted warped binary images are

![undist01]
![undist02]
![undist03]
![undist04]
![undist05]
![undist06]


#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I have 2 methods for this, sliding_window_polyfit in cell 7 and prev_frame_polyfit in cell 9







#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I am calculating the radius of curvature in cell 10 and also displaying the curvature of a sample image in cell 11

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.
This is done in cell number 8

![im02]
![im03]
![im04]
![im05]


---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I basically used the pointers given in lecture series and used most of the logic from there
I am basically NOT taking the average of the curvatures. I am  not fully using thr Line Class and also when I ran this on challenge_video , it did not perform well. I could tweak the code to make better use of "State" of the car and lane lines 
and make use of Line class better
