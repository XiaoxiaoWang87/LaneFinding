# Finding Lane Lines on the Road

### Goals

1. Create an image processing pipeline that finds lane lines on the road.
2. Apply the lane finding pipeline to identify lane lines in a video.
3. Be familiar with simple image processing and edge detection techniques and their shortcomings in practice.


---

### Files for submission

- Project: https://github.com/XiaoxiaoWang87/LaneFinding/
- Code: [LaneFinding/P1.ipynb](https://github.com/XiaoxiaoWang87/LaneFinding/blob/master/P1.ipynb)
- Image output: [LaneFinding/test_images_output/](https://github.com/XiaoxiaoWang87/LaneFinding/tree/master/test_images_output)
- Video output: https://www.youtube.com/playlist?list=PLtwuyAB8W1dgcRNujw-ShOy6UVMt03yEa

---

### Reflection

#### 1. Method Description

My pipeline consisted of seven steps. I implemented these steps in a class called `LaneFindingPipeline()`.

- First, I converted the images to grayscale.
- Then, to reduce image noise I applied a Gaussian smoothing with `kernal size = 5` (the larger the kernal size, the blurrer the image becomes) to the grey-scaled image.
- Next I applied Canny edge detection to the image with reduced noise level. I chose a low threshold of 50 (suppressing pixels with a gradient value below 50) and a high threshold of 150 (marking pixels with a gradient value above 150 as strong edge pixels, while those with a value between 50 and 150 and connected to strong edge pixels as weak edge pixels).
- Then I applied a region selection to only select a polygon area covering the lane lines. First the selected region (defined by vertices) is colored as white, and elsewhere colored as black. Then a logic "and"  (using `cv2.bitwise_and`) is used to combine this colored image with the one from Canny edge detection (black and white).
- After the previous step, only edges nearby the lane lines are kept. Then I implemented the Hough transformation to look for lines in these edges. To get a reasonable lane line detection, I excluded lines that are shorter than 40 (pixels). If the gap between two points is larger than 20 (pixels), I don't consider them to be in the same line.
- Next, I wrote the `draw_lines()` function to draw two smooth lines (one for the left lane, and the other for the right lane) from the lines identified in the previous step.
  - For lines detected by Hough transformations and defined by two x-y pairs: x1, y1 and x2, y2, if x1 = x2 (horizontal lines) or y1 = y2 (vertical lines), they are excluded as lane lines should have angles between 0 to 90 degree.
  - Categorize points into those belong to the left lane and right lane. This is done by calculating the slope of the line connecting point (x1, y1) and (x2, y2). Also, require that if a point belongs to the left (right) lane, its x-location must be smaller (bigger) than the x-location of the upper left (right) corner of the polygon region.
  - Fit separate linear models using the points that belong to the left lane and right lane, respectively.
- Add the fitted lines to the original image.

Below are the image before and after the above processing steps.

Original image:

<img src="https://github.com/XiaoxiaoWang87/LaneFinding/blob/master/test_images/solidYellowCurve.jpg" width="400">

Processed image:

<img src="https://github.com/XiaoxiaoWang87/LaneFinding/blob/master/test_images_output/solidYellowCurve.jpg" width="400">


#### 2. Potential Shortcomings

1. Currently the algorithm determines if a line belong to the left or right lane by calculating the line angle and constraining it to a pre-defined range. This range may vary for different camera positions or different road geometry.



Another shortcoming could be ...


#### 3. Possible Improvements

A possible improvement would be to ...

Another potential improvement could be to ...
