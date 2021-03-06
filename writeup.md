

## Vehicle Detection Project

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/hog.png
[image2]: ./output_images/grid.png
[image3]: ./output_images/bounding.png
[image4]: ./output_images/heatmap.png
[video1]: ./project_video_ouput.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first code cell of the IPython notebook in the function get_hog_features. I used the implementation of the web lesson.  

I started by reading in some of the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example

![alt text][image1]


#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters but finally I´ve used the default parameters that were in the web lesson.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using hog features using all the channels in the YCrCb color space, the histogram features and the spatial binning of color features.

I´ve made some tests and I get the best results using all the features. I tried to improve the speed of the classifier using only one channel but I got worse results and a lot of false positives in the video.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

The sliding window algorithm is in the find_cars function in the notebook. I used the implementation given in the web lesson with some modifications to reduce the number of false positives.

After making a lot of tests, I´m using a 1.5 scale sliding window with a value of 2 cells per step. It´s not the fastest configuration but It´s the only I tested that didn´t show false positives

![alt text][image2]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on one scale using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  

![alt text][image3]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video and I created an array of the last 24 frames to smooth the drawing of the bounding boxes. From the positive detections of the last 24 frames I created a heatmap and then thresholded that map to identify vehicle positions. I use a high threshold because I´m using 24 frames to build the heatmap .I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result of the heatmap of one of the test images 

![alt text][image4]

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

This was a hard project with a lot of new concepts. The hardest part was to delete the false positives without sacrificing the speed but I was not able to acheive a fast algorithm without false positives.

I think I can improve the project in many ways:
* I think that I will obtain better results with a neural network. I´m watching the Object detection and segmentation lecture from Stanford
* I will try to mix Project 4 and Project 5

