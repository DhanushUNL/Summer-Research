# Detecting Dust Particles  

This is a small study mainly circles around the main objective, **"Identify the dust particles on the scans obtained to evaluate the striation patterns obtained by running (either pulling or pushing) screw drivers (flat headed) with various angles (60, 70, 80) using both sides on lead plates"**

Here we are ignoring the experiment totally that collected the data and solely focus on detecting the coordinates of dust particles. And, these images are high resolution images and scales are also very small (basically in nano meters). We can use the graphs (scatter-plots) drawn for `nearby` **(lags)** cross sections (both vertical and horizontal) to identify the existing dust particles using the property of the uniformity of the nearby vertical and horizontal cross-sections . but that is tedious.

So we will be using an appropriate focal matrix `(Spacial Data analysis)` in order to find these peaks in the scanned images, rather than the previous approach.

## Objectives:

-   Identify the location of the peak of the dust particles.

-   identify the locations around the peaks with in a certain radius.

## To Do 

1.  install git-lfs

2.  get familiar with working with x3p formatted data. (Just for one gel type)

    i.  Horizontal surfaces

    ii. vertical surfaces

3.  Later extend this to other gels.

4.  find a suitable a focal matrix to work with these image data.
