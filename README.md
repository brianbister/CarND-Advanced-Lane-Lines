## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

The goals / steps of this project are the following:  

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply the distortion correction to the raw image.  
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view"). 
* Detect lane pixels and fit to find lane boundary.
* Determine curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

---

Much of the steps with corresponding images can be found and better explained in the notebook. But the steps
taken to find the lane linds can be summarized as followed.

1) Calculated the calibration matrix and distortion coefficients using the images found in camera_cal.

2) Defined functions for applying a sobel X/Y, magnitiude, and direction threshold, and HLS thresholds. I found
my best results combined a (50, 100) threshold in the X direction and a (120, 255) threshold for the S value
in the HLS image.

3) Calibrated a perspecitve transform by taking the solidYellowLeft image in test_images/. I picked four points
along the lanes and picked four more points on the transformed image such that the line was straight.

4) I applied two mask to the image, the first filtering out the image outside the lane, the second filtering
inside the lane.

5) From here I got all the points along the left side of the image, and all the points along the right side
and got a second-degree polynomial to fit a line for each lane.

6) I then drew the lane lines onto the warped image then unwarped the image.

All the previous are the steps for a single image, when processing a video we do some additional steps.

7) Apply so rules such that the curvature or the two lanes is within a normal range (within 30%) of each
other. If not then we don't consider it a detected line.

8) Only add a new line if the fit is on the same order of magnitude as the previous. I put a huge percentage
50,000% minimum difference. While this was extreme it kept most lines while getting rid of frames with huge
errors.

9) Smoothed each frame by taking the average of the lines in the previous 5 frames. This kept the lines from
jumping around each frame while still allowing it to adapt quickly to new roads/curves.

### Limitations

This would not work under very dark lighting conditions as we might be able to detect lines as well. It also
would not work in the cases with unusual line markings. An example of this could be a street with a curb as a
lane marking. It also would break down in the case where we move the car into a different lane.