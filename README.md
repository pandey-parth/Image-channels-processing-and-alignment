# Image-channels-processing-and-alignment
Sergey Prokudin-Gorsky was the first color photographer in Russia, who made the color portrait of Leo Tolstoy. Each of his photographs is three black-and-white photo plates, corresponding to red, green, and blue color channels. Currently, the collection of his pictures is situated in the U.S. Library of Congress, the altered versions have proliferated online. In this task, I made a programme which will align the images from the Prokudin-Gorsky plates and learn the basic image processing methods.

### Input Image Loading
The input image is the set of 3 plates, corresponding to B, G, and R channels (top-down). You should implement the function 𝚕𝚘𝚊𝚍_𝚍𝚊𝚝𝚊 that reads the data and returns the list of images of plates. 𝚍𝚒𝚛_𝚗𝚊𝚖𝚎 is the path to the directory with plate images. If this directory is located in the same directory as this notebook, then default arguments can be used.

![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/plates/1.png) ![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/plates/2.png)
![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/plates/3.png) ![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/plates/4.png)

### The borders removal 

It is worth noting that there is a framing from all sides in most of the images. This framing can appreciably worsen the quality of channels alignment. Here, we suggest that you find the borders on the plates using Canny edge detector, and crop the images according to these edges. The example of using Canny detector implemented in skimage library can be found here.

The borders can be removed in the following way:

* Apply Canny edge detector to the image.
* Find the rows and columns of the frame pixels. For example, in case of upper bound we will search for the row in the neighborhood of the upper edge of the image (e.g. 5% of its height). For each row let us count the number of edge pixels (obtained using Canny detector) it contains. Having these number let us find two maximums among them. Two rows corresponding to these maximums are edge rows. As there are two color changes in the frame (firstly, from light scanner background to the dark tape and then from the tape to the image), we need the second maximum that is further from the image border. The row corresponding to this maximum is the crop border. In order not to find two neighboring peaks, non-maximum suppression should be implemented: the rows next to the first maximum are set to zero, and after that, the second maximum is searched for.

![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/Screenshot/Screenshot_2021-05-20%20image%20-%20Jupyter%20Notebook.png)

### Channels separation 

The next step is to separate the image into three channels (B, G, R) and make one colored picture. To get channels, you can divide each plate into three equal parts.

![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/Screenshot/Screenshot_2021-05-20%20image%20-%20Jupyter%20Notebook(3).png)

## Search for the best shift for channel alignment
In order to align two images, we will shift one image relative to another within some limits (e.g. from −15 to 15

pixels). For each shift, we can calculate some metrics in the overlap of the images. Depending on the metrics, the best shift is the one the metrics achieves the greatest or the smallest value for. We suggest that you implement two metrics and choose the one that allows to obtain the better alignment quality:

    Mean squared error (MSE):

![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/Screenshot/Screenshot_2021-05-20%20image%20-%20Jupyter%20Notebook(6).png)


where w, h are width and height of the images, respectively. To find the optimal shift you should find the minimum MSE over all the shift values.

    Normalized cross-correlation (CC):

![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/Screenshot/Screenshot_2021-05-20%20image%20-%20Jupyter%20Notebook(7).png)

To find the optimal shift you should find the maximum CC over all the shift values.

Final Image will look like -

![](https://github.com/pandey-parth/Image-channels-processing-and-alignment/blob/master/Screenshot/Screenshot_2021-05-20%20image%20-%20Jupyter%20Notebook(5).png)
