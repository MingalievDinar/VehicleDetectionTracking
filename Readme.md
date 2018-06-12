# ** Vehicle Detection Project **

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/HOG.png
[image2]: ./examples/heat.png
[video1]: ./project_video_out.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

I created help functions to extract featues from pictures. You can find it in the cell code #2 of the IPython notebook. I have tryed different combinations of paramsto extract HOG features:
* Color map
* Different channels
* Cell size
* Block size
* Orientation

In the cell 10 you can find best 5 results.
Code cells 5-8 for Histogram and Spatial features extraction.

In the cell #16 I selected 5 random Viechel and Non Viechel to show the results of HOG and classifier prediction.

![alt text][image1]

####  2. Explain how you settled on your final choice of HOG parameters.

I tried about 400 combinations of parameters (cell 9) and selected 4 of them to cobine with other features to obtain the best result.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

Among all help functions you can find function `SVM_resultI()`. I trained a linear SVM in cell 12 using spatial, histogram and HOG features. The best 10 results are in cell 13. I used LAB for histogram and spatial features and LUV for HOG. Final version of SVM I got at cell code 14.
Also I biult `GridSearchCV` but it works too much time and I stopped it. 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

In the cell 17 you can find a code with sliding windows implementation. In the next cell (18) I created more simple function with fixed function parameters for convinience. I tried different scales. But a scale equals to 1.5 works the best during car detection on images. Maybe on video I can choose smaller scale because I will exclude False Positive by thresholding heatmap. I overlapped 75% of images - I thinkg it is a reasonable values to have enough windows but not too much in order to speed up the process. That is why I also used not all image channels but only one for each feature type (784 features in total).


#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

I have already described how I choose all features above (selected seceral combinations with highest accuracy for each feature and combined several features to create the final one). In cell 19 I added functions: heatmap, labels and threshold. In the cell 21 I created the final pipline by averaging the results of 7 heatmaps and with threshold equals to 3. Here are some example images:

![alt text][image2]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_out.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Above I provided examples showing the heatmap from a series of frames of video.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

* In the final model here [Self-Driving Car Project Q&A | Vehicle Tracking ](https://www.youtube.com/watch?v=P2zwrTM8ueA&feature=youtu.be&utm_medium=email&utm_campaign=2017-05-24_carnd_projectwalkthroughs&utm_source=blueshift&utm_content=2017-05-24_carnd_projectwalkthroughs&bsft_eid=809c46b1-7b0f-4960-9cc1-459c102110d5&bsft_clkid=c217b86a-0c96-425a-974b-cd00a1328702&bsft_uid=3bf8c026-90a9-45b7-b867-b9eb411d9dd2&bsft_mid=07b9559b-43e4-41ad-9fe5-a8c9436ae936) was used about 8k features for SVM, but I used only 784 - maybe I can male more robsut results if I enrich feature number. On other hand more feature - higher probability of overfitting.
* I think I can play with scale - combine differnt scales.
* Test not only linear kernel for SVM but also other types
