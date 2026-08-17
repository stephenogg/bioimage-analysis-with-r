---
title: "Image Enhancement"
teaching: 45
exercises: 30
---

:::::::::::::::::::::::::::::::::::::: questions

- What is image enhancement?
- How can pixel values be modified?
- What is the difference between display changes and data changes?
- What is image inversion?
- What is thresholding?
- How can histograms help us understand image enhancement?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Apply image enhancement operations using EBImage.
- Distinguish between display operations and image processing operations.
- Modify image intensities using point operations.
- Invert image intensities.
- Create binary images through thresholding.
- Understand how image enhancement affects pixel values.

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

In the previous lesson we explored how image data can be displayed in different
ways.

We learned that changing brightness, contrast, or lookup tables (LUTs) can
dramatically alter the appearance of an image without changing the underlying
pixel values.

In this lesson we will begin modifying the image data itself.

Unlike display operations, image enhancement operations act directly on the
pixel values that make up an image.

::::::::::::::::::::::::::::::::::::: callout

## Display versus Data

Display operations affect how an image appears on screen.

Image enhancement operations modify the pixel values stored in the image.

These are fundamentally different processes.

::::::::::::::::::::::::::::::::::::::::::::::::

## Reading an Image

Load the EBImage package.


``` r
library(EBImage)
```

``` error
Error in `library()`:
! there is no package called 'EBImage'
```

Read an image.


``` r
  img <- readImage(system.file('images', 'cells.tif', package='EBImage')
  )
```

``` error
Error in `readImage()`:
! could not find function "readImage"
```

``` r
# cells.tif contains 4 frames, let's subset the data so that our image object contains on ly the first frame.
  img<- img[,,1]
```

``` error
Error:
! object 'img' not found
```

Display the image.


``` r
display(img)
```

``` error
Error in `display()`:
! could not find function "display"
```

Inspect the range of pixel values.


``` r
range(img)
```

``` error
Error:
! object 'img' not found
```

Create a histogram.


``` r
par(mfrow = c(1, 2))

hist(
  as.vector(img),
  main = "Graphics Histogram",
  xlab = "Pixel Intensity"
)
```

``` error
Error:
! object 'img' not found
```

``` r
hist(
  img,
  main = "EBImage Histogram",
  xlab = "Pixel Intensity"
)
```

``` error
Error:
! object 'img' not found
```

``` r
par(mfrow = c(1,1))
```

The histogram describes the distribution of pixel values that comprise the
image. NB when we call the histogram function on a vector as above, 
R uses the base graphics version of the histogram function. When we call it on an image object, 
the EBImage version of hist() is used. If you prefer one or the other, you can 
specify it in the function call -- `graphics::hist()` or `EBImage::hist()`

## Point Operations

Many image enhancement methods are known as **point operations**.

A point operation calculates each output pixel value using only the corresponding
input pixel value.

Conceptually:

```text
Input Pixel Value -> Operation -> Output Pixel Value
```

Examples include:

- image math
- inversion
- intensity scaling
- thresholding

## Image Inversion

One of the simplest image enhancement operations is image inversion.

Bright pixels become dark and dark pixels become bright.

We can invert the image using:


``` r
img_inv <- 1 - img
```

``` error
Error:
! object 'img' not found
```


Compare the original and inverted images.


``` r
display(EBImage::combine(img, img_inv), all = TRUE)
```

``` error
Error in `display()`:
! could not find function "display"
```

## Histograms and Inversion

Compare histograms of the original & inverted images.


``` r
par(mfrow = c(1,2))

hist(
  as.vector(img),
  main = "Original Image",
  xlab = "Pixel Intensity"
)
```

``` error
Error:
! object 'img' not found
```

``` r
hist(
  as.vector(img_inv),
  main = "Inverted Image",
  xlab = "Pixel Intensity"
)
```

``` error
Error:
! object 'img_inv' not found
```

``` r
par(mfrow = c(1,1))
```

The pixel values have changed.

Consequently, the histogram has also changed.

Unlike some of the display operations in the previous lesson, inversion modifies the
underlying image data.

::::::::::::::::::::::::::::::::::::: challenge

A pixel has an intensity value of:

```text
0.8
```

What value will it have after inversion?

:::::::::::::::::::::::: solution

Inversion calculates:

```text
1 - 0.8
```

giving:

```text
0.2
```

Bright pixels become dark and dark pixels become bright.

NB - we can invert the LUT too. In that case the image would *look* the same as 
if we had performed the inversion operation on every pixel, but would change the 
way we assign brightness to pixel values. Instead of assigning 0 to black and 255 to 
white, we would assign 0 to white and 255 to black.
Be sure to keep track - it can get confusing. 


:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Intensity Scaling

Another common operation is intensity scaling.

We have already encountered an example of intensity scaling in the previous lesson. 
Operations on the histogram, such as normalisation or equalisation change the underlying pixel intensity values. 
We assigned the new data to a new image object to prevent the original data from being changed. 
Some image processing software allows non-destructive brightness and contrast changes, 
usually by operating on a copy of the data.  

Suppose we wish to make the image brighter. We can multiply every pixel value to get
a brighter image.


``` r
img_bright <- img * 2
```

``` error
Error:
! object 'img' not found
```

``` r
display(img_bright)
```

``` error
Error in `display()`:
! could not find function "display"
```

``` r
range(img_bright)
```

``` error
Error:
! object 'img_bright' not found
```

But what happens to pixel intensity values that end up greater than 1 after the multiplication? 
Remember that when the data are read into an image object by EBImage's `readImage()` function, 
the integer values are scaled between 0 and 1. 
While multiplying will make these values bigger (and therefore brigther), 
There is a potential for a pixel to have a value greater than 1. 
In our example above, any pixel with a value of > 0.5 in the original image will 
have a value > 1 in img_bright. Pixel values greater than 1 will be clipped because 
an image display expects an image to have values between 0 and 1. 
The display will silently map any values that are greater than 1 to the brightest 
intensity available --- 1.0


We can visualise where the pixels with values greater than one are by thresholding 
(see below).


``` r
img_mask <- img >= 1.0
```

``` error
Error:
! object 'img' not found
```

``` r
img_bright_mask <- img_bright >= 1.0
```

``` error
Error:
! object 'img_bright' not found
```

Display the original and brightened image and the two masks to see the effect of 
image multiplication. Let's put the mask into the red channel to give the saturated pixels 
a red colour.


``` r
red_img_mask <- rgbImage(
  red = img_mask
)
```

``` error
Error in `rgbImage()`:
! could not find function "rgbImage"
```

``` r
red_img_bright_mask <- rgbImage(
  red = img_bright_mask
)
```

``` error
Error in `rgbImage()`:
! could not find function "rgbImage"
```

``` r
display(EBImage::combine(img, img_bright), all = TRUE)
```

``` error
Error in `display()`:
! could not find function "display"
```

``` r
display(EBImage::combine(red_img_mask, red_img_bright_mask), all = TRUE)
```

``` error
Error in `display()`:
! could not find function "display"
```

Compare the intensity ranges.


``` r
range(img)
```

``` error
Error:
! object 'img' not found
```


``` r
range(img_bright)
```

``` error
Error:
! object 'img_bright' not found
```

## Saturation

Excessive intensity scaling can lead to **saturation**. Saturation can occur by 
manipulating the pixel intensity values, as we are doing here, or it can occur at 
the point of image acquisition, if the detector settings or illumination intensity 
are not set correctly. Inspect the histogram of the original data to see if there 
are a significant number of pixels that have a value of 1.0 before any image 
enhancement has been performed.

Saturated pixels all take the same maximum value:

```text
1.0
```

Once saturation occurs, intensity information is lost.

::::::::::::::::::::::::::::::::::::: challenge

Suppose a pixel has a value of:

```text
0.7
```

The image is multiplied by:

```text
2
```

What value would the pixel have after clipping?

:::::::::::::::::::::::: solution

The new value becomes:

```text
0.7 × 2 = 1.4
```

Because values above 1 are clipped:

```text
1.0
```

The pixel value can no longer be compared to other pixel values in the image.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::



## Thresholding

Thresholding is one of the most widely used operations in image analysis.

A threshold divides pixels into two groups.

For example:

```text
Pixels above threshold (usually those that you care about)
Pixels below threshold (usuaully those that you don't care about)
```

We can create a binary image using:


``` r
mask <- img > 0.2
```

``` error
Error:
! object 'img' not found
```

This operation queries every pixel in the image and asks whether the intensity value is greater than 0.2. 
Then it assigns either `TRUE` or `FALSE` to that location depending on the answer. 
Our `mask` is an array of Boolean values with the same dimensions as the queried image object.

Display the object


``` r
mask
```

``` error
Error:
! object 'mask' not found
```

display the image and the binary mask:


``` r
display(EBImage::combine(img, mask), all = TRUE)
```

``` error
Error in `display()`:
! could not find function "display"
```


The resulting image mask contains only two values: `TRUE` & `FALSE`

or equivalently:  `1` & `0`

Thresholding is often the first step in image segmentation workflows.

## Choosing a Threshold

Different threshold values produce different results.


``` r
mask_1 <- img > 0.1
```

``` error
Error:
! object 'img' not found
```

``` r
mask_2 <- img > 0.2
```

``` error
Error:
! object 'img' not found
```

``` r
mask_3 <- img > 0.3
```

``` error
Error:
! object 'img' not found
```

Display the three thresholded images.


``` r
display(
  EBImage::combine(mask_1, mask_2, mask_3), all = TRUE
)
```

``` error
Error in `display()`:
! could not find function "display"
```

Notice how the detected objects change as the threshold increases.

::::::::::::::::::::::::::::::::::::: challenge

Which threshold would generally identify fewer pixels as objects?

```text
0.1
0.2
0.3
```

:::::::::::::::::::::::: solution

The threshold:

```text
0.3
```

identifies fewer pixels.

As the threshold increases, fewer pixels exceed the threshold value.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Histograms and Threshold Selection

Histograms can help us choose thresholds.

Display the histogram again.


``` r
hist(
  breaks = 60, 
  as.vector(img),
  main = "Pixel Intensity Histogram",
  xlab = "Pixel Intensity"
)
```

``` error
Error:
! object 'img' not found
```

``` r
abline(
  v = 0.2,
  col = "red",
  lwd = 2
)
```

``` error
Error in `int_abline()`:
! plot.new has not been called yet
```

The red line indicates the threshold value.

Pixels to the left side of the threshold become `0` (background).

Pixels to the right side become `1` (foreground objects).

::::::::::::::::::::::::::::::::::::: callout

## Histograms Support Decision Making

Image histograms are your friends!
Histograms provide a useful summary of image intensities and can help guide the
selection of thresholds for segmentation and measurement workflows.

::::::::::::::::::::::::::::::::::::::::::::::::

## Comparing Display and Enhancement Operations

Consider the following operations.

### Display Operations

- changing brightness and contrast
- changing LUTs
- altering display settings

These operations affect visualisation only.

### Enhancement Operations

- inversion
- intensity scaling
- thresholding

These operations modify image data.

The distinction is important because image-analysis results depend on pixel
values rather than image appearance.

::::::::::::::::::::::::::::::::::::: challenge

Which of the following modify image data?

1. Changing a lookup table
2. Adjusting display contrast
3. Inverting an image
4. Thresholding an image
5. img <- normalize(img)

:::::::::::::::::::::::: solution

The correct answers are:

```text
3. Inverting an image
4. Thresholding an image (although we've used it non-destructively)
5. Assigning the normalized values to the original object.
```

Lookup tables and display contrast affect visualisation only.

Inversion and thresholding modify the underlying pixel values. Best practice 
guidelines assign these modified pixel arrays to new objects so that original data 
does not get lost.

:::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Why Perform Image Enhancement?

Image enhancement is often performed to:

- improve image visibility
- simplify image interpretation
- support segmentation
- support quantitative measurements

However, enhancement operations should always be applied thoughtfully because
they modify the image data itself. **Always work on a copy of the original data**

::::::::::::::::::::::::::::::::::::: callout

## Document Your Processing Steps

Image enhancement changes data.

To support reproducibility, image-processing workflows should document the
operations that have been applied to an image.

::::::::::::::::::::::::::::::::::::::::::::::::

## Looking Ahead

Thresholding converts grayscale images into binary images.

In the next lesson we will use these binary images to identify, label, and
measure biological objects.

This process is known as image segmentation.

## Key Points

::::::::::::::::::::::::::::::::::::: keypoints

- Image enhancement modifies image pixel values.
- Point operations apply a mathematical operation to each pixel independently.
- Image inversion converts bright pixels to dark pixels and vice versa.
- Intensity scaling can improve visibility but may lead to saturation.
- Thresholding converts grayscale images into binary images.
- Histograms provide useful information about image intensity distributions.
- Display operations and image enhancement operations are fundamentally different.
- Image enhancement should be documented to support reproducibility.

::::::::::::::::::::::::::::::::::::::::::::::::
