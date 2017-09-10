# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report



---

### Reflection

### 1. Line Detection Pipe Line

The pipeline consists of five key image operations. An image is converted in to LAB color space and the L (lightness) channel is extracted for computation. Then the image is thresholded to remove all pixels with lightness intensity under 180. Then a region of interest (ROI) is abstracted. This step assumes the camera is mounted on the front of the car near the middle. The ROI is defined to be a parallelagram with a large bottom and small top similar to the expected shape of the lane. From that region canny edges are extracted and hough lines are computed. Having a more restrictive threshold and more permissive hough line parametes seemed to work best. This process returned a set of candidate lines which are then filtered based on slope. Lines with inappropriate slopes (slopes that would not be consistent with a line based on a road lane marking) are removed and the remaining lines are categorized into potential left side markings and potential right side markings with the left side having positive slope. From each of these candidate line sets the longest line is chosen to be the correct line segment. Those two lines are then extended to an approximate vanishing point. 


![Pipeline][pipeline]

This image shows a sample frame going through the pipeline. The right image shows the image after the threshold has been applied, while the center image shows the set of candidate lines after the hough lines have been extracted. The result
image is shown on the left. 


![Result][result]



### 2. Identify potential shortcomings

There are a number of potential short comings with the current solution. First driving in poor lighting conditions (such as low lighting or shawdows) can reduce the number of pixels that pass through the threshold this leads to potentially poor lines. I believe using all three color channels might help solve this problem, lightness was the most effective color channel for white lines in good lighting conditions, but the b color channel appeared to be more effective in shadows with yellow lines. Finding a good color range and a good color space could help with the lighting problems. Another addition is further filtering the candidate line segments based on their x-coordinates at the bottom of the screen. This would ensure that if a line was picked for a lane it would have a higher probablity of being correct. 


### 3. Suggest possible improvements to your pipeline

The pipeline could further by assigning a confidence to each line that is returned and using the confidence and lines of previous results to help inform the decision of current computation. This would help when going through shadowy patches and computations lack confidence but previous lines can help inform the decision. 




[pipeline]: ./pipline.png "Pipeline Image"

[result]: ./result.png "Resulting Image"