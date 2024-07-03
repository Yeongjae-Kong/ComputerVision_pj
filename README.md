# Computer Vision Project

### - Project 1 : Recovering noisy images

#### * Ideas - histogram equalization, spatial filtering, frequency filtering

In the case of the first Cars1_noise, we first tried to use a Gaussian filter to remove Gaussian noise and soften the image, and then preserve the edge while removing salt-and-pepper noise through the Midian filter. In addition, histogram equalization improved image contrast and increased visibility.
For edge enhanced image, Sobel and Laplacian filter were applied respectively, and edge_enhanced_img was generated as a max value, and this was binarized.

In the case of the second Cars2_noise, the number of the license plate was not clear because the above method was used, so we tried to save the edges and details of the image by using a high pass filter for the output that passed through the midian and Gaussian filters. Since then, the visibility has been increased by using histogram equalization. In the case of kernel size, the clarity of smaller letters was lowered, so it was set to 3, and in the case of cutoff frequency, a heuristic appropriate value (130 in this code) was found. In the case of edge_enhanced, it was clearer to add equalized_img to Laplacian, so two outputs were added and binarized.

In the case of the last clock_noise, similar to Cars1, we first tried to remove Gaussian noise and soften the image, and we tried to remove residual salt-and-pepper noise through the Median filter. And we tried to improve the overall contrast of the image and increase the visibility of the image through histogram equalization.
In addition, the Laplacian filter was applied to the output using the Sobel filter for edge enhancement, and the Laplacian was repeatedly used once again for clearer results. In addition, for clarity correction, the max value was selected among the results of Sobel and Laplacian, and binarization was performed.

### - Project 2 : Hand-written numbers classification

#### * Ideas - Bayes Classifier, CNN

First, the Gaussian Bayes Classifier was used as the first method for hand-written numbers classification. Several data preprocessing was performed as specified in the assignment specification.
• Binaryization: Binaryization was applied to clearly distinguish between numbers and backgrounds. The optimal threshold of the image was calculated using the threshold_otsu function, and the image was binarized based on this.
• Number Area Detection: In the case of cursive numbers, the numbers are not completely located in the center of the image, and there are little errors. In consideration of this, a process of detecting and cutting the number area was performed. The outline of numbers was detected in the binarized image using the find_contours function. Find the boundary of consecutive pixels in the image and return the outline. The largest outline among the detected contours was considered the number area.
• Size Normalization: The numerical area was adjusted to 32 x 32 size for consistent size input.
• Morphology filtering: Morphology operations were applied to improve the line thickness and breakage of cursive numbers. The thin function was used to thin the lines of numbers. Through this, the skeleton of numbers was extracted. In addition, the skeleton of numbers was extracted using the skeletonize function.
• Extraction of characteristics: In order to extract characteristics from the preprocessed image, the image was converted into a one-dimensional array, and the pixel values of the images were used as characteristics.
• Data Augmentation: Data augmentation techniques were applied to consider various cases. -5 degrees and 5 degrees rotation transformation were applied to cope with the rotational deformation of numbers. In addition, movement transformation was applied by 2 pixels each up, down, left, and right to make the number position change robust. In addition, random distortion was added to augment the data.

Using the above methods, the Bayes classifier was able to have 85% of the accumulation. In particular, the accumulation was remarkably high in data augmentation including rotational transformation. Adding random distortions and including them in the learning dataset could also increase the accumulation. In the case of various parameters, various numbers were substituted and empirically tuned to output optimal results.

In the second method, CNN was applied based on the sample code. The epoch was set to 30, the batch size was set to 8, and the learning rate was set to 0.001. For loss, a commonly used Adam optimizer was used, similar to the commonly used crossover loss in multi-class classification problems. Hyperparameters set as suggested in the existing sample code of pdf, and among them, by raising the epoch to 30, the acuity could be increased to 100%.

### - Project 3 : Camera Calibration

#### * Ideas - Camera Calibration 

The internal camera parameters and distortion were obtained by creating a chessboard with w=9 and h=5 and taking 15 pictures with different viewpoints to perform calibration.

Afterwards, through the values derived above, marker with w=3, h=3.
Tracking was performed with a marker on the board. A 20 cm-10 cm square serving as a ruler was left at the bottom, and the distance was measured while moving the marker.
At this time, using the tvec extracted above, the actual horizontal and vertical sizes were calculated by calculating the difference between the tvec. Four pictures of the marker at each vertex are set,
The lengths of the left and right/upper sides were calculated respectively. This was done three times at different viewpoints, and measurements were performed 3*2=6 times for width and height with a total of 12 photos.

The total accuracy is about 95%, and this error must have occurred naturally in the process of setting and photographing manually. Since the marker printed it on a4 paper, there is a possibility of folding or bending rather than a plane during photographing, and even if it was placed on a 20cm square for distance measurement, the location may not have been accurate, and it is believed that such an error occurred during calibration due to a printing error, such as the possibility that each pattern size is not exactly 2.5cm. Reducing this human error will increase the accumulation further.
