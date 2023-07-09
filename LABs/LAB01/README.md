<div id="top"></div>

<!-- PROJECT SHIELDS -->
<!--
*** I'm using markdown "reference style" links for readability.
*** Reference links are enclosed in brackets [ ] instead of parentheses ( ).
*** See the bottom of this document for the declaration of the reference variables
*** for contributors-url, forks-url, etc. This is an optional, concise syntax you may use.
*** https://www.markdownguide.org/basic-syntax/#reference-style-links
-->

[![Instagram][instagram-shield]][instagram-url]
[![LinkedIn][linkedin-shield]][linkedin-url]
[![Github][github-shield]][github-url]  

<br />

<!-- PROJECT LOGO -->
<div align="center">
  <h3 align="center">LAB01</h3>

![Status][completed-shield]

  <p align="center">
    Digital image processing lesson works. Lesson book is "Digital Image Processing 3rd Edition by Rafael C. Gonzalez and Richard E. Woods"
    <br />
    <a href="https://github.com/arslanalperen/Digital-Image-Processing"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://github.com/arslanalperen/Digital-Image-Processing">View Demo</a>
    ·
    <a href="https://github.com/arslanalperen/Digital-Image-Processing/issues">Report Bug</a>
    ·
    <a href="https://github.com/arslanalperen/Digital-Image-Processing/issues">Request Feature</a>
  </p>
</div>

<br />

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#image-negatives">Image Negatives</a>
    </li>
        <li>
      <a href="#log-transformations">Log Transformations</a>
    </li>
        <li>
      <a href="#power-law-gamma-transformations">Power Law (Gamma) Transformations</a>
    </li>
        <li>
      <a href="#spatial-linear-filters">Spatial Linear Filters</a>
    </li>
        <li>
      <a href="#order-statistic-nonlinear-filters">Order-Statistic (Nonlinear) Filters</a>
    </li>
        </li>
        <li>
      <a href="#references">References</a>
    </li>
  </ol>
</details>

<br />

<div style='text-align: justify;'>

&emsp;LAB01 consists of five sections. 'pgmread' function added in the beggining of the code because of it is using in every function blocks.

# Image Negatives

&emsp;The negative of an image with intensity levels in the range [0, L-1] is obtained by using the negative transformation shown in below equation [1].

$$ s = L - 1 - r $$

&emsp;Reversing the intensity levels of an image in this manner produces the equivalent of a photographic negative. This type of processing is particularly suited for enhancing white or gray detail embedded in dark regions of an image, especially when the black areas are dominant in size. The original image is a digital mammogram showing a small lesion. In spite of the fact that the visual content is the same in both images, note how much easier it is to analyze the breast tissue in the negative image in this particular case [1].

# Log Transformations

&emsp;The general form of the log transformation shown in below equation

$$ s = c * log(1+r) $$

'c' is a constant, and it is assumed that r greater or equal than zero [1].

&emsp;Any curve having the general shape of the log functions would accomplish this spreading/compressing of intensity levels in an image, but the power-law transformations are much more versatile for this purpose. The log function has the important characteristic that it compresses the dynamic range of images with large variations in pixel values. It is not unusual to encounter spectrum values that range from 0 to 10^6 or higher. While processing numbers such as these presents no problems for a computer, image display systems generally will not be able to reproduce faithfully such a wide range of intensity values. The net effect is that a significant degree of intensity detail can be lost in the display of a typical Fourier spectrum [1].

# Power Law (Gamma) Transformations

&emsp;Power-law transformations have the basic form

$$ s = cr^\gamma $$

where c and g are positive constants [1].

&emsp;In addition to gamma correction, power-law transformations are useful for general-purpose contrast manipulation. Figure 3.8(a) shows a magnetic resonance image (MRI) of an upper thoracic human spine with a fracture dislocation and spinal cord impingement. The fracture is visible near the vertical center of the spine, approximately one-fourth of the way down from the top of the picture. Because the given image is predominantly dark, an expansion of intensity levels is desirable. This can be accomplished with a power-law transformation with a fractional exponent. The other images were obtained by processing Fig. 3.8(a) with the power-law transformation function. The values of gamma corresponding to images through are 0.6, 0.4, and 0.3, respectively.
We note that, as gamma decreased from 0.6 to 0.4, more detail became visible. A further decrease of gamma to 0.3 enhanced a little more detail in the background, but began to reduce contrast to the point where the image started to have a very slight “washed-out” appearance, especially in the background. By comparing all results, we see that the best enhancement in terms of contrast and discernable detail was obtained with g = 0.4. A value of g = 0.3 is an approximate limit below which contrast in this particular image would be reduced to an unacceptable level [1].

# Spatial Linear Filters

&emsp;The output (response) of a smoothing, linear spatial filter is simply the average of the pixels contained in the neighborhood of the filter mask. These filters sometimes are called averaging filters. As mentioned in the previous section, they also are referred to a lowpass filters [1].

&emsp;The idea behind smoothing filters is straightforward. By replacing the value of every pixel in an image by the average of the intensity levels in the neighborhood defined by the filter mask, this process results in an image with reduced “sharp” transitions in intensities. Because random noise typically consists of sharp transitions in intensity levels, the most obvious application of smoothing is noise reduction. However, edges (which almost always are desirable features of an image) also are characterized by sharp intensity transitions, so averaging filters have the undesirable side effect that they blur edges. An other application of this type of process includes the smoothing of false contours that result from using an insufficient number of intensity levels. A major use of averaging filters is in the reduction of “irrelevant” detail in an image. By “irrelevant” we mean pixel regions that are small with respect to the size of the filter mask. This latter application is illustrated later in this section [1].

$$ R = \frac{1}{m*n} {\sum_{1}^{m*n} z_i} $$

&emsp;An $m * n$ mask would have a normalizing constant equal to $1/mn$. A spatial averaging filter in which all coefficients are equal sometimes is called a box filter [1].

&emsp;The second mask in Fig. 3.32 is a little more interesting. This mask yields a so called weighted average, terminology used to indicate that pixels are multiplied by different coefficients, thus giving more importance (weight) to some pixels at the expense of others. In the mask shown in Fig. 3.32(b) the pixel at the center of the mask is multiplied by a higher value than any other, thus giving this pixel more importance in the calculation of the average. The other pixels are inversely weighted as a function of their distance from the center of the mask.The diagonal terms are further away from the center than the orthogonal neighbors (by a factor of 12) and, thus, are weighed less than the immediate neighbors of the center pixel. The basic strategy behind weighing the center point the highest and then reducing the value of the coefficients as a function of increasing distance from the origin is simply an attempt to reduce blurring in the smoothing process.We could have chosen other weights to accomplish the same general objective. However, the sum of all the coefficients in the mask of Fig. 3.32(b) is equal to 16, an attractive feature for computer implementation because it is an integer power of 2. In practice, it is difficult in general to see differences between images smoothed by using either of the masks in Fig. 3.32, or similar arrangements, because the area spanned by these masks at any one location in an image is so small. Two 3x3 smoothing (averaging) filter masks shown in below. The constant multiplier in front of each mask is equal to 1 divided by the sum of the values of its coefficients, as is required to compute an average [1].

<center>
<table>
<tr><th> <center> S.F. </center> </th><th> <center> Wei. Av. SF. </center> </th></tr>
<tr><td>

|   |   |   |
|:-:|:-:|:-:|
| 1 | 1 | 1 |
| 1 | 1 | 1 |
| 1 | 1 | 1 |
|   |   |   |

</td><td>

|   |   |   |
|:-:|:-:|:-:|
| 1 | 2 | 1 |
| 2 | 4 | 2 |
| 1 | 2 | 1 |
|   |   |   |


</td></tr> </table>
</center>

&emsp;The second mask (weighted average smoothing filter) is a little more interesting. This mask yields a so called weighted average, terminology used to indicate that pixels are multiplied by different coefficients, thus giving more importance (weight) to some pixels at the expense of others. In the mask the pixel at the center of the mask is multiplied by a higher value than any other, thus giving this pixel more importance in the calculation of the average. The other pixels are inversely weighted as a function of their distance from the center of the mask.The diagonal terms are further away from the center than the orthogonal neighbors (by a factor of 12) and, thus, are weighed less than the immediate neighbors of the center pixel. The basic strategy behind weighing the center point the highest and then reducing the value of the coefficients as a function of increasing distance from the origin is simply an attempt to reduce blurring in the smoothing process.We could have chosen other weights to accomplish the same general objective. However, the sum of all the coefficients in the mask is equal to 16, an attractive feature for computer implementation because it is an integer power of 2. In practice, it is difficult in general to see differences between images smoothed by using either of the masks, or similar arrangements, because the area spanned by these masks at any one location in an image is so small [1].

&emsp;The effects of smoothing as a function of filter size are 5, 9, 15, and 35 pixels, respectively. The principal features of these results are as follows: For we note a general slight blurring throughout the entire image but, as expected, details that are of approximately the same size as the filter mask are affected considerably more. For example, the and black squares in the image,
the small letter “a,” and the fine grain noise show significant blurring when compared to the rest of the image. Note that the noise is less pronounced, and the jagged borders of the characters were pleasingly smoothed. The result for is somewhat similar, with a slight further increase in blurring. For we see considerably more blurring, and the 20% black circle is not nearly as distinct from the background as in the previous three images, illustrating the blending effect that blurring has on objects whose intensities are close to that of its neighboring pixels. Note the significant further smoothing of the noisy rectangles. The results for and 35 are extreme with respect to the sizes of the objects in the image. This type of aggresive blurring generally is used to eliminate small objects from an image. Note also in this figure the pronounced black border. This is a result of padding the border of the original image with 0s (black) and then trimming off the padded area after filtering. Some of the black was blended into all filtered images, but became truly objectionable for the images smoothed with the larger filters [1].

# Order-Statistic (Nonlinear) Filters

&emsp;Order-statistic filters are nonlinear spatial filters whose response is based on ordering (ranking) the pixels contained in the image area encompassed by the filter, and then replacing the value of the center pixel with the value determined by the ranking result. The best-known filter in this category is the median filter, which, as its name implies, replaces the value of a pixel by the median of the intensity values in the neighborhood of that pixel (the original value of the pixel is included in the computation of the median). Median filters are quite popular because, for certain types of random noise, they provide excellent noise-reduction capabilities, with considerably less blurring than linear smoothing filters of similar size. Median filters are particularly effective in the presence of impulse noise, also called salt-and-pepper noise because of its appearance as white and black dots superimposed on an image [1].

&emsp;The median, $\xi$, of a set of values is such that half the values in the set are less than or equal to and half are greater than or equal to In order to perform median filtering at a point in an image, we first sort the values of the pixel in the neighborhood, determine their median, and assign that value to the corresponding pixel in the filtered image. For example, in a neighborhood the median is the 5th largest value, in a neighborhood it is the 13th largest value, and so on. When several values in a neighborhood are the same, all equal values are grouped. For example, suppose that a neighborhood has values (10, 20, 20, 20, 15, 20, 20, 25, 100). These values are sorted as (10, 15, 20, 20, 20, 20, 20, 25, 100), which results in a median of 20. Thus, the principal function of median filters is to force points with distinct intensity levels to be more like their neighbors. In fact, isolated clusters of pixels that are light or dark with respect to their neighbors, and whose area is less than (one-half the filter area), are eliminated by an median filter. In this case “eliminated” means forced to the median intensity of the neighbors. Larger clusters are affected considerably less [1].

&emsp;Although the median filter is by far the most useful order-statistic filter in image processing, it is by no means the only one. The median represents the 50th percentile of a ranked set of numbers, but recall from basic statistics that ranking lends itself to many other possibilities. For example, using the 100th percentile results in the so-called max filter, which is useful for finding the brightest points in an image. The response of a 3x3 max filter is given by $R = max(z_k|k = 1,2,...,9)$. The 0th percentile filter is the min filter, used for the opposite purpose [1].

&emsp;Figure 3.35 shows an X-ray image of a circuit board heavily corrupted by salt-and-pepper noise. To illustrate the point about the superiority of median filtering over average filtering in situations such as this, we show the result of processing the noisy image with a neighborhood averaging mask, and the result of using a median filter. The averaging filter blurred the image and its noise reduction performance was poor. The superiority in all respects of median over average filtering in this case is quite evident. In general, median filtering is much better suited than averaging for the removal of salt-and-pepper noise [1].

# References

[1] Gonzalez R. C.; Woods R. E. (2008). Digital Image Processing, Pearson Prentice Hall of Pearson Education, Inc.

</div>

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[instagram-shield]: https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white
[github-shield]: https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white
[linkedin-shield]: https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white
[ongoing-shield]: https://badgen.net/static/status/on%20going/red
[completed-shield]: https://badgen.net/static/status/completed/green

[instagram-url]: https://www.instagram.com/arslanalperen55/
[github-url]: https://github.com/arslanalperen
[linkedin-url]: https://www.linkedin.com/in/arslanalperen/

[fifo-diagram]: Images/fifo-diagram.png
