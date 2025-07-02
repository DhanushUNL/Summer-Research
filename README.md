---
author: Dhanushka
output: prettydoc::html_pretty
date: 2025-06-17
---

# Detecting Dust Particles on the Scans of Screwdriver striations - Project overview

This is a small study mainly circles around the main objective, **"Identify the dust particles on the scans obtained to evaluate the striation patterns obtained by running (either pulling or pushing) screw drivers (flat headed) with various angles (60, 70, 80) using both sides (A, B) on lead plates"**

![Experimental setup](setup.png){fig-align="center" width="412" height="269"}

Here we are ignoring the experiment totally that collected the data and solely focus on detecting the coordinates of dust particles. And, these images are high resolution images and scales are also very small (basically in nano meters). We can use the graphs (scatter-plots) drawn for `nearby` **(lags)** cross sections (both vertical and horizontal) to identify the existing dust particles using the property of the uniformity of the nearby vertical and horizontal cross-sections . but that is tedious task. Notice that according to the *figure below*, it can be clearly observable to a naked eye that the surface is pretty much uniform.

And initially i believed by using a classification technique, we can easily detect the peaks in the image by simply training a suitable ML algorithm. But, due to the fact that the images are taken at very small scales, it was not easy to come up with a threshold that ables to recognize the peaks by examining the transformed data using the `x3p_to_df` function which gives x and y coordinates with a value assigned to each an every pixel coordinate.

![A horizontal cross section of a scan](images/clipboard-567757959.png){#fig-horizontal-cross-section fig-align="center" width="320"}

Hence, it is vital to use focal maps. So we will be using an appropriate focal matrix `(Spacial Data analysis)` in order to find these peaks in the scanned images, rather than the previous approach. For the analysis, we will be using `"terra"` package.

## Objectives of this study:

-   Identify the location of the peaks where the dust particles are located.

-   Identify the locations around the peaks with in a certain radius to detect the size of the dust particle in pixels.

## Tasks to do

1.  install git-lfs `(done)`

2.  get familiar with working with x3p formatted data. (Just for one gel type)

    i.  Horizontal surfaces

    ii. vertical surfaces

3.  find a suitable a focal matrix to work with these image data.

4.  get familiar to working with raster data.

5.  Later extend this to other gels.

## June 11 - 18

I was able to clear most of my doubts regarding the project. So I started working with the scans (obtained through a particular gel), which I was provided with. Since all the images were with higher resolution, the file sizes were quite big. Hence, I had to install `git lfs` that helps tracking down `.x3p` files in my data.

-   I am still learning about the spacial data handling with `terra`.

-   I got a bit familiar with working with x3p formatted data. (Just for one gel type)

    -   Horizontal surfaces (horizontal cross sections)

    -   Vertical surfaces (vertical cross sections)

-   Also in the process of reviewing R materials.

## June 19 - 24

-   Applied the `Gaussian, Maximum, Sum` filters on images to see if it is possible to distinguish the dust from the striation marks. ===\> Didn't go well. couldn't identify any.

    -   Gaussian filter = *A **Gaussian filter** is a linear, low-pass filter commonly used in image processing for **smoothing** and **noise reduction**. there are several parameters, specially, `d = distance parameter` (under the Gaussian* filter it is referred as the **standard deviation** (`in microns)` of the Gaussian kernel function. Smaller the d value, less blurry the image. **Note :** *When you increase the d-value, we significantly lose dimension (some pixels) from the original image as well.*

-   Applied the diff() operation on images (basically, to the surface matrix) at different lags and ended up deciding lag 5 is suitable as it recreate the scans with out the striations. `(Because the main objective is to identify the dust)`

    -   This diff() operator, essentially calculates how much each pixel differs from the one lag ( lag =1 if we want to see the differences of adjacent cells in pixels ) units apart it.

-   Then tried to apply the filters (focal maps defined above) to these `differenced` images at `lag 5`.

-   I was able to come up with a method to detect the dust particles on a image by obtaining the positive outliers of the values of the images after taking lagged differences at lag = 5.

    -   That is, use the Q3 + 1.5 IQR as the cut off value in detecting the positive outliers that could be dust. (higher values indicates spikes/bumps)

    -   Then by taking their cell numbers and respective coordinates, we can plot those points on top of the raster image (image that was filtered by applying diff(lag =5). And the results were pretty good. However, it captures other bumps on the images as dust, which could be a very tiny bump that is there because of the non-uniformity of the surface of the image.

    -   Therefore, its required to detect a better threshold/cut off to detect only probable dust spots on the image.

## June 25 - July 1

Look at the detected dust particles at higher lagged differences of the raster image.

-   And it is observed that, when the lag is increased ( used lag = 10, lag =15 , 20 and 25), the above suggested method only captures the larger dust spots. (with out changing the method of taking the cut off)

-   Unfortunately, couldn't find any difference after filtering the images (already filtered taking the lagged differences) using, max and Gaussian filters.

-   Tried the method on several images and it seems the method works pretty well.

Lets call this method as **Method 01**.

## July 2 - July 9

I think we don't need to detect a certain threshold for all the images as for each image, `Q3 + 1.5IQR` of the values can be taken as a good threshold of its own.

And since the values of these images (surface matrices) are very small and the surface is not being as smooth as it seems, its hard to come up with a one universal threshold value that can detect the dust particles (bumps).

Anyways, i will look into other methods available for detecting outliers such as standardization , modified z score (use median and median absolute differences) and etc.

Also, want to apply the method 1, to all the images and see how it works. ***Question is, how can i evaluate the method?***

`Maybe` i will Look forward to fit a 3D plane to the surface and analyze the residuals as an alternative approach.
