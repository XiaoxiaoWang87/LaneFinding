# Finding Lane Lines on the Road 

### Goals

1. Create an image processing pipeline that finds lane lines on the road. 
2. Apply the lane finding pipeline to identify lane lines in video. 
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
- Next I applied Canny edge detection to the image with reduced noise level. I chose a low threshold of 50 (supressed pixels with a gradient value below 50) and a high threshold of 150 (pixels with gradient value above 150 are marked as strong edge pixels, while those with gradient value between 50 and 150 and connected to strong edge pixels are marked as weak edge pixels). 
- Then I applied a region selection to only select a polygon area which covers the lane lines. The way the region selection works is that first the selected region (defined by vertices) is colored as white, and elsewhere as black. Then a logic "and"  (using `cv2.bitwise_and`) is used to combine this colored image with the one from Canny edge detection (black and white). 
- After the previous step, only edges nearby the lane lines are kept. Then I implemented the Hough transformation to look for lines in these edges. To get a reasonable lane line detection, I also excluded lines that are shorter than 40 (pixels). If the gap between two points is larger than 20 (pixels), I also don't consider them as in the same line. 
- Next, as a crucial step, I wrote the `draw_lines()` function in the following way in order to draw two smooth lines (one for the left lane, and the other for the right lane) from the lines identified in the presious step.
  - For lines (defined by two x-y pairs: x1, y1 and x2, y2) detected by Hough transformations, if x1 = x2 (horizontal lines) or y1 = y2 (vertical lines), they are excluded as lane lines should have angles between 0 to 90 degree.
  - TO BE DONE.

Below are the image before and after the above processings. 

![Lanes](https://github.com/XiaoxiaoWang87/LaneFinding/blob/master/test_images/solidYellowLeft.jpg =299x168)
![Lanes](https://github.com/XiaoxiaoWang87/LaneFinding/blob/master/test_images_output/solidYellowLeft.jpg =299x168)

#### 2. Potential Shortcomings


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


#### 3. Possible Improvements

A possible improvement would be to ...

Another potential improvement could be to ...
