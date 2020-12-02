# Lane lines detection

The lane detection has been done under three conditions:-
* Normal Condition: In this condition, there image visible lines on the road without any distortion parts. 

![solidWhiteCurve](https://user-images.githubusercontent.com/20774864/100898203-2c39fa00-34c9-11eb-9316-ca95e950cdd9.jpg)

* Under Shadow: In this condition, some lanes are placed in a shadow of an object that might fool Canny detection. 

![shadow](https://user-images.githubusercontent.com/20774864/100898228-322fdb00-34c9-11eb-9240-d75b329c2aef.jpg)


* Road with Bright color closer to the yellow lane: In this condition, the road color is too close to the yellow lane that makes the detection tough, which fools Canny to consider the yellow lane as a background (without edges).

![problem](https://user-images.githubusercontent.com/20774864/100898214-2f34ea80-34c9-11eb-9389-ebd39d6fe047.jpg)



To encounter these conditions, the project has the following steps:-

(1) The entered Image's channels HSV and RGB are separated into individual channels to generate two 1-Dimages, one that contains edges of the yellow lane lines under all mentioned conditions and another one for the white lane lines. 

(2) Canny algorithm is applied on both images and the result will be sum up into one 1-D image of the edges of the present in the image.

(3) The region of interest will take place in this phase when a rectangle region will be taken from the whole image to present the in-front view of the vehicle.

(4) Hough transform will be applied in this region extract lines in the image under a particular threshold.

(5) Ultimately, only two lines will be determined by eliminating some of the incoming lines under particular thresholds for their slopes and lengths.

### Reflection

My pipeline consists of five steps, the first step is to extract almost White and Yellow lane lines from the image under the three conditions. To do so, I have visualized each channel in the RGB & HSV channels for each condition as shown below.

 ![Figure_1 all channels](https://user-images.githubusercontent.com/20774864/100884070-b5493500-34b9-11eb-9c14-d151791a6624.png)
 <br />
 ![Figure_2 under shadow](https://user-images.githubusercontent.com/20774864/100884074-b8442580-34b9-11eb-91c1-2695378b7ed7.png)
  
It's so obvious that the Yellow lane line is detectable in the saturation channel. On the other hand, the White lane line can be recognized under a particular threshold for the blue channel with the Red & Green channels. Thus, these two channels have been taken and added to generate a channel with Yellow and White lane edges as shown below.

![Figure_3 all states](https://user-images.githubusercontent.com/20774864/100884086-bc704300-34b9-11eb-954c-75cf80c1b7a8.png)

Next, the second phase is to apply canny edges on both outcome images, and the outputs will be added to generate combined edges of both White and Yellow lane lines' edges.

 ![download](https://user-images.githubusercontent.com/20774864/100887074-39e98280-34bd-11eb-9d5f-071c24b33b23.png)


In the third phase, the in-front car view is the region of interest where the lane line detection should be done, and the output edges images' background will be cut off. The view region will solely observable in this phase. 

![download (1)](https://user-images.githubusercontent.com/20774864/100887569-c8f69a80-34bd-11eb-88a3-48b2b0baefbc.png)

The Hough transform is done in this phase, the fourth phase, to draw all possible lines from the incoming edges.

![download (3)](https://user-images.githubusercontent.com/20774864/100888145-74075400-34be-11eb-9ac0-bb98cf6a478f.png)

Afterward, the fifth phase begins. This phase is vital where the **only** two lines will be detected on the left and right sides that present the vehicle path. Not only that, but It also contains a simple algorithm to exclude some lines from the output of the Hough lines. The algorithm begins by excluding all lines with slopes smaller than 0.35 in case of positive slopes(The Line on the right side). In contrast, all lines with slopes greater than -0.35 in case of negative slopes(The Line on the left side). Moreover, the line length is also involved as a parameter in this process, the lines with length less than 30 pixels will be excluded as well. The length threshold is essential to remove any distortion lines in the incoming edges.
Then, the slopes and intersections of the remaining lines will be calculated and grouped into two groups. The lines with positive slope and located on the right side of the image are most likely to be the lane lines on the right side of the car, and these lines consequently will be grouped. The same procedures will be done on lines with the negative line slops and located on the left side of the image. The mean of these lines on both sides, right and left, will be the formation of the exact left and right lines that draw the lane lines of the roadsides. 

![download (4)](https://user-images.githubusercontent.com/20774864/100893917-8dab9a00-34c4-11eb-91ff-09d09d811ec5.png)

As shown below, the previous problems have been solved under the previous algorithm in addition to the condition of having no lane lines as showing on the left picture.

![download (5)](https://user-images.githubusercontent.com/20774864/100897427-5808b000-34c8-11eb-8f59-7a14804365e4.png)



