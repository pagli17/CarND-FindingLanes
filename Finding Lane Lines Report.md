**Finding Lane Lines on the Road**

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of several steps. 
First of all, I loaded all the images in a vector. After that I started drawing a suitable ROI for all the sample images by trial and error. 
Once found the ROI vertices I tried a brute force approach using nested for loop for the following values: 
- *Kernel size* for the Gaussian Blur (range going from 1 to 9)
- *Canny Threshold* (I set a ratio of 1/3 for low and high ones)
- *Hough Threshold* (range going from 1 to 30)

So I applied in order: Gaussian Blur, ROI to the image, and then on the massed image I computed the hough segments and placed these n top of the original image.
Finally for the first part of the task I carefully tuned min_line_len and max_line_gap on the first video "solidWhiteRight.mp4" to obtain the best result.

In order to draw a single line on the left and right lanes, the first step required was to modify the draw_lines() function by creating two arrays, one for the Hough segments identifying the left lane line and another for the Hough segments identifying the right lane line. I've done this separation according to the sign of *m* given the rect equation *y = mx + b* 
At last for each lane line I computed the average *m* and *b* of all the respective side segments (m_left and b_left for left lane line, m_right and b_right for right lane line). When selecting the *m* and *b* factors I prevented some of them entering in the respectives arraysin order to remove some noisy lane markings which were giving an overall bad result when met in the video. This together with an additional tailored tuning of *min_line_len* and *max_line_gap* led to satisfying results from my point of view.


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming is the case when the ROI doesn't match new kind of recording setups because it is now fit to the sample images and videos.

Another shortcoming is the case in which the pipelin is applied to curved roads (like the optional challenge case). It presents many issues in the way it is now.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to try to make the ROI vertices change according to the size of the image in order to fit it even to new unseen images.

Another potential improvement could be to adapt the pipeline to make it work for the optional challenge.
Two quick changes would be to refit the ROI in order to correct two problems:
- remove the front part of our car recorded by the camera.
- reduce the ROI to have a narrower look at the road as the Canny and Hough algorithms are now segmenting also the trees at the edge of the road.
