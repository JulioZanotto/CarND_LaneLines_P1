# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/gray_image.jpg "Grayscale"
[image2]: ./examples/blur_gray_image.jpg "Gaussian Blur"
[image3]: ./examples/edges_image.jpg "Canny Edge"
[image4]: ./examples/masked_im_image.jpg "Region Of Interest"
[image5]: ./examples/drawed_line_img_image.jpg "Draw Lines"
[image6]: ./examples/weighted_image.jpg "Weighted Mixtured"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps:

1.	First, I converted the images to grayscale as below using the simple convertion from OpenCV, COLOR_RGB2GRAY:

![alt text][image1]

2.	Then I applied a Gaussian Blur to smooth the image, to make the Canny Edge filter job, easier, as shown below:

![alt text][image2]

The best threshold, after several tries, was a ```kernel_size = 7```

3.	With the output of the Gaussian Blur I applied The Canny Edge filter to get the image gradients, so we can detect the edges within the image. An example is shown below, as output with the following parameters: ```low_threshold = 50``` and a ```high_threshold = 150```

![alt text][image3]

4.	Now it is time to find some lines, so for that was used the HoughLinesP from OpenCV. But in order to avoid finding lines in the whole image, I have narrowed down the Region Of Interest, by masking the image and working with only the piece of it which is more important to the job. An example of the masked region is shown below made on the Canny Edge output:

![alt text][image4]

On the masked image, I draw the lines found by the function. But to achieve it was not an easy path, so for that I wrote two more helper functions. 
	
	1.	In order to draw the right and left lane, was necessary after fiding the lines, to check the position. As we know we wanted to find the almost diagonal lane lines which has a peculiar slope. So a checker function would get the coordinates and for each one check the slope, if it falls within the parameters it would be stored and averaged to be either the left lanes or the right lanes.

	2.	With the mean coordinates of the right lane and left lane, I created an extrapolate function, which would get the image and draw the line for the whole image.

The final result is shown below:

![alt text][image5]


5.	To finish the job and check the result uppon the actual image, both the drawed and the actual were merged using the provided function weighted_img, and the final result is shown below:

![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


The potentials shortcomings would be on images with different kind of shades and paviment color. As it was trick to narrow down the best threshold for each function, it was tested on the provided video clips. It is not certain that this would still work on different roads, on different time of day, and worse of all with more light paviment, so the Canny edge would find more edges or no edge at all.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to try to find better and more general parameters, or try to foresee different shades or the appearence of shadows. It was particularly hard to set the Gaussian Blur kernel to work either on the with and yellow lanes and also in the Challenge, as the edge lines would disapper or appear too much to generate too many lines by the hough lines.

Another potential improvement could be to to tweak the HoughLinesP parameters as there are a lot of combinations for different of lanes, as if it is continuous or segmented.
