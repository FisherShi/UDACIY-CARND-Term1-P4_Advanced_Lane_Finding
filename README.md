
[//]: # (Image References)

[image1]: ./camera_cal/chessboard_undistorted.png "Undistorted Chessboard"
[image2]: ./camera_cal/test_img_undistorted.png "undistorted test image"
[image3]: ./test_img_binary.png "Binary Example"
[image4]: ./test_img_warped.png "Warp Example"
[image5]: ./test_img_polyfit.png "Fit Visual"
[image6]: ./test_img_output.png "Output"

---
# UDACITY Lane Detection Project

## The Goal of this Project

The goal of this project is to write a software pipeline to identify the lane boundaries in a video from a front-facing camera on a car.

## Summary

In summary, I complete this project by:

1. Calibrate the original video footage
2. Use HLS color and x directional gradient thresholds to generate binary images.
3. Perspective transform the images
4. Identified lane-line pixels and fit their positions with a polynomial
5. Calculated the radius of curvature of the lane and the position of the vehicle with respect to center
6. Use a line class to store the data from previous frames to help identify the lines

The result is shown in the video 'project_output.mp4'

The project repo from Udacity can be found [here](https://github.com/udacity/CarND-Advanced-Lane-Lines)

## Camera Calibration

The code for this step is contained in a IPython notebook located in "./camera_cal/Camera_Calibration.ipynb".  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

The following is an example of a distortion-corrected image:

![alt text][image2]

## Thresholded Binary Image 

I used a combination of HLS color and x directional gradient thresholds to generate a binary image in the helper function section of the IPython notebook located in "./advanced_lane_finding_submission.ipynb".  Here's an example of my output for this step.  

![alt text][image3]

## Perspective Transform

The code for my perspective transform includes a function called `warp()`, which appears in the IPython notebook.  The `warp()` function takes as inputs an image (`img`).  I chose to hardcode the source and destination points in the following manner:

```
    src = np.float32(
        [[140,719],
         [1220,719],
         [780,480],
         [540,480]])
    
    dst = np.float32(
        [[240,719],
         [1040,719],
         [1040,300],
         [240,300]])

```

I verified that my perspective transform was working as expected by verifing that the lines appear parallel in the warped image.

![alt text][image4]

## Polyfit

For more details aoubt how I identified lane-line pixesl and fit their positions with a polynomial, please check the `locate_lines()` function and `polyfit()` function. The following is an example for the fitted line image:

![alt text][image5]

## Radius of Curvature of the lane and the position of the vehicle with respect to center.

In the `polyfit()` function, I calculated the radius of curvature of the lane and the position of the vehicle with respect to center 

## Plot the Lane Area 

I also used a line class to store the data from previous frames to help identify the lines. This is very helpful to identify sudden changes on the images and still provide a good detection result. For more details, please check the `process_frame()` function (the pipeline). The following is an example image of my result plotted back down onto the road such that the lane area is identified clearly:

![alt text][image6]

## Ways to Improve

The pipeline will not produce a good result on videos with poor lighting condition. In order to tackle the challenge, I will need to use more gradiant thresholds and fine-tune the parameteres to get a hihg-quality binary image. Another way to improve my pipline is to use dynamic source points for perspective transform. The hard-coded points sometimes don't always produce good warpped images. 
