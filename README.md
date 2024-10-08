# Face recognition using PCA, eigenvectors, eigenvalues and eigenfaces

The following code has been programmed in R in order to perform face image analysis using Pricipal Components Analysis and matrix and vector arithmetic with Eigen.

## 1. Reading and visualizing the images:

Step 1: Create the initial parameters, some may be used later-on. Create the image visualization function, it will be used to visualize any image of our image set.

Step 2: Install the required packages, in this case, OpenImageR and ramify.

Step 3: Import those libraries to use them.

Step 4: Create the image_directory variable, it will store the path to our image set (the image set has not been uploaded to Github due to image-weight issues).

![R Project setup](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/1-set-up.jpg)

Step 5: Read the images from our directory, save them in a list and combine them into a 3D array.

Step 6: Reshape the data. The vector will be used for calculations and the matrix to paint the images.

Step 7: Visualize the assigned image (I).

![R Project image visualization](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/2-show-assigned-face.jpg)

The face will show under "Plots" in RStudio:
![R Project assigned face](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/3-assigned-face.png)

Step 8: Calculate the mean and the standard-deviation faces. The calculation of the mean and standard deviation of the faces allows us to normalize each element so that we obtain the centered matrix. In this case we are not going to perform the calculation manually since we have the scale function that does it automatically.

![R Project mean standard deviation face calculation](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/4-mean-face-std-face.jpg)

The mean face:
![R Project mean face](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/5-mean-face.png)

The standard deviation face:
![R Project standard deviation face](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/6-std-face.png)

The following steps, from 9 to 11, are mandatory in order to be able to perform the Principal Component Analysis (PCA) afterwards.

Step 9: Normalize data (Xs variable). Calculate and write down the mean of the normalized image.
As mentioned above, we normalize the data using the scale function since this function has the parameters center and scale, which contain the arithmetic mean and standard deviation respectively. If we wanted to do it by hand, we would have to perform the following operation:

> The value of the element minus the mean, divided by the standard deviation.

Step 10: Calculate the mean of the assigned person, normalized.

![R Project data normalization](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/7-data-normalization.jpg)

Step 11: Calculate the covariance matrix of the normalized data. The covariance matrix is symmetric and measures the degree of linear relationship of the data set between each of the pairs of variables. In this case we use the cov() function for the calculation of this matrix, as it is much simpler.

The main diagonal terms correspond to the variance of each of the variables, and, as we are passing normalized data, the diagonal value is 1. The rest of the data, which is not in the main diagonal, corresponds to the covariances between pairs of data.

![R Project covariance matrix](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/8-covariance-matrix.jpg)

Step 12: Let's begin with the Principal Component Analysis: we are going to use the covariance matrix and perform the calculations with the eigen function. This algorithm provides us with the eigenvalues and eigenvectors. We know from theory that these data constitute the principal components and, due to the characteristics of the covariance matrix (symmetric and positive), all eigenvalues are positive.

Step 13: We calculate and write the eigenvalue associated with the principal component P. The principal components are shown in descending order, from highest to lowest, representing the weight of each of them in the total composition (100%) (i.e., the eigenvalues always add-up to 100).

![R Project eigenvalues](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/9-eigenvalues.jpg)

Step 14: We draw and attach the distribution of the cumulative variance (Y-axis) for each principal component (X-axis) with respect to the total variance of the data. We store eigenvectors in their own variable.

Step 15: We perform a loop to calculate the cumulative variance. In the case of the eigenvalues, they represent the variance, so we add up the eigenvalues at each iteration to get the cumulative variance. We have to let it run through the 2576 eigenvalues, which takes a while. 

![R Project eigenvectors](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/10-eigenvectors.jpg)

Step 16: Once the accumulated variance has been stored in the Y_P-axis variable, we proceed to make a graph. We see in it that the first 35 principal components absorb the most variance, while the rest are almost irrelevant.

![R Project cumulative variance](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/12-cumulative-variance.jpg)
![R Project cumulative variance plot](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/13-cumulative-variance-plot.png)

Step 17: We display and attach the eigenface P (the eigenvalue associated with the principal component P). The objective of the PCA is to find a linear transformation (a linear application) such that the original data (X_vector) is transformed or projected into a new space by means of the product T = XP such that the covariance matrix Cx of the new data is diagonal. The first thing we do is to associate to the variable P the matrix of eigenvectors, because this matrix constitutes our P or linear application. We finally calculate the coordinates of the original data in the vector space generated.

![14 R Project eigenface associated to P](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/14-eigenface-associated-to-P.jpg)

Step 18: We visualize the eigenface P by means of the eigenvectors associated with the eigenvalue of P:

![15 R Project eigenface associated to P](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/15-eigenface-associated-to-P.png)

Step 19: We calculate and write the sum of all the coefficients of the eigenface P.

Step 20: How many principal components or “eigenfaces” (L) are needed, at least, to explain E% of the total variance of the data? 

What we are being asked here is to start reducing the dimensionality, since that is the main objective of the PCA. Therefore, we are going to consider an L, smaller than the m eigenvalues, that can give us a good enough approximation to the initial image quality with a much smaller weight. That is, we only select the largest L vectors. We are left with E% of the total variance of the data (60%) and we look for the first value that immediately exceeds this 60% (L = eigenfaces). In this case it looks like we need to consider the first 7 Principal Components.

![15 R Project eigenfaces needed to explain variance](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/16-eigenfaces-needed-to-explain-variance.jpg)

Step 21: We calculate the reconstruction error for each person X_err = Xs-Xs_rec. Once found, we find the R2 coefficient that determines the quality of the reconstruction.

We take the L that we got in the previous section and we select the eigenvectors indicated by that value (the first 7). Remember that the eigenvectors come from the covariance matrix, and that the eigenmatrix works as a linear application. Next, we calculate the reduced matrix T_reduc.

Having reduced the dimensionality, the P_reduc matrix is no longer invertible. That is to say that the original data contained in X cannot be completely recovered by the T_reduc matrix, and if we still perform the inversion, as is the case in this exercise, we must know that we will have a loss of information. This loss of information is called residual error.

![15 R Project reconstruction error residual error](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/17-reconstruction-error.jpg)

Step 22: We now find which person obtains a higher and lower R2. We identify each person with the corresponding face number between [1, 40].  We finally visualize and attach the face of the people identified in the previous point.

![15 R Project worst best reconstruction](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/18-best-worst-reconstruction.jpg)

Best face reconstruction:

![15 R Project best reconstruction](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/19-best-reconstruction.png)

Worst face reconstruction:

![15 R Project worst reconstruction](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/20-worst-reconstruction.png)

End result:

![15 R Project worst reconstruction](https://github.com/MaiteLizarraga/r_eigenfaces_pca/blob/main/capturas/21-end-result.jpg)
