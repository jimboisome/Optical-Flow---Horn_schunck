# Code breakdown

## Load image
All images used are from proc directory (real-time flat-field corrected images using PCA, c/o Sarlota)

## High-Pass adaptive filter
Filtering of the image is done using

    a. Gaussian convolution (5x5 kernel, std. dev = 5) of the proc image
    b. Subtracting 'a' from original proc image to produce difference_image
    c. difference_image is used for ROI and Optical Flow

## ROI identification
    a. Identify coordinates

## Horn-Schunck Method Optical Flow algorithm 
Please see the powerpoint for reference. 

    a. Precompute the image and temporal gradient
        Calculations of image gradients are done using Sobel operator with kernel size = 3. 
        This is implemented using OpenCV. 
    b. Initializing flow field and Iteration
        Smoothing operator alpha is defined; number of iteration is defined
        Minimum alpha makes the algorithm more sensitive to noise and subtle changes. Increasing alpha smoothens out the flow velocity.
        I used alpha = 2, iteration = 100
    c. Averaging kernel for iteration
        3x3 kernel based on discrete approx. of Laplacian smoothing filter
    d. Conversion of velocity
        Conversion is based on the detector parameters (pixel size, frame rate) using Shimadzu HPV-X2 (1.125 MHz)
  
## Visualization
    a. Quiver plot 
    b. Step size
        Minimum step size of 1 makes the quiver plotting very dense. Increasing the step size makes the quiver plot more sparse. 
        I used step size = 3
    c. Scale factor is used to increase visibility of quiver plot
    d. Masking out
        I used masked out to display only quiver plots related to the moving objects and only show the results based on the threshold.
        For threshold, I use either the mean velocity magnitude or fixed value (e.g. >= 0.2)

        
