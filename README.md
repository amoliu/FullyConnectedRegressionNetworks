# Cell_Counting_using_FCRNs
This project deals with the task of automated cell counting in microscopy using Fully Constitutional Regression Networks

# TEAM MEMBERS:
1. KALYAN GHOSH
2. BHARGAV RAM
3. VAGEESH

## CONTENTS:
1. ABSTRACT
2. LITERATURE SURVEY
3. INTRODUCTION
4. METHODOLOGY
5. MATHEMATICAL FORMULATIONS
6. MESA DISTANCE
7. ARCHITECTURE
8. DATASETS
9. RESULTS
10. CONCLUSION
11. REFERENCES

## ABSTRACT:
This project deals with the task of automated cell counting in microscopy. The approach we take is to use Convolutional Neural Networks (CNNs) to regress a cell spatial density across the image. This is applicable to situations where traditional single-cell segmentation-based methods do not work well due to cell clumping or overlap. We make the following contributions: (i) We implement two Fully Convolutional Regression Networks (FCRNs) for this task in Keras with Tensorflow backend, we follow the architecture given in [1]; (ii) We fine-tune our model to get more accuracy than what is obtained in [1]; (iii) We compare the results of our architecture which uses density based method to results obtained by an U-Net which uses a segmentation-based approach (iv) We show that FCRNs trained entirely on synthetic data are able to give excellent predictions on real microscopy images. We set a new state-of-the-art performance for cell counting on standard synthetic image benchmarks and, as a side benefit, show the potential of the FCRNs for providing cell detections for overlapping cells.

## LITERATURE SURVEY:
In this project, we implement the architecture proposed in the paper, "Microscopy Cell Counting with Fully
Convolutional Regression Networks" by Zisserman & Weidi at University of Oxford https://www.robots.ox.ac.uk/~vgg/publications/2015/Xie15/weidi15.pdf for cell counting and also compare its performance to the traditional segmentation based approach to cell counting using an an U-net convolutional network. 

## INTRODUCTION:
Automatic cell counting can be approached from two directions, one is detection- based counting which requires prior detection or segmentation; the other is based on density estimation without the need for prior object detection or segmentation. Both the methods produce similar error performance.  However, in recent years it has been found that the density-based estimation is a faster method for automatic cell counting than the segmentation method.
In this project, we focus on cell counting in microscopy, but the developed methodology could also be used in other counting applications.

## METHODOLOGY:
In our project, we develop and compare the architecture of two Fully Convolutional Regression Networks (FCRNs) A and B along with UNet which is a Convolutional Network used extensively for Biomedical Image Segmentation. The networks being fully convolutional, they can predict the density map of an input image of arbitrary size. We use this property of fully convolutional networks to improve the efficiency by training end-to-end on image patches. In this project we use synthetic data to train our model. 

![pic1](https://github.com/kalyanghosh/Cell_Counting_using_FCRNs/blob/master/pic1.JPG)

![pic2](https://github.com/kalyanghosh/Cell_Counting_using_FCRNs/blob/master/pic2.JPG)

## MATHEMATICAL FORMULATIONS:
We assume that a set of N training images (pixel grids) I_1,I_2, …., I_N is given. It is also assumed that each pixel p in each image I_i is associated with a real-valued feature vector x_p^iER^K. It is finally assumed that each training image I_i is annotated with a set of 2D points P_i = {P_1, …., P_(C(i))}, where C(i) is the total number of objects annotated by the user.
The density functions in our approaches are real-valued functions over pixel grids, whose integrals over image regions should match the object counts. For a training image I_i , we define the ground truth density function to be a kernel density estimate based on the provided points:

                            ∀p ∈ I_i   F_i^0 (p)= ∑_(P∈P_i)N(p;P,σ^2 I_2x2)                  (1)
In the above equation p denotes a pixel and N(p;P,σ^2 I_2x2) denotes  a normalized 2D Gaussian kernel evaluated at p, with the mean at the user-placed dot P , and an isotropic covariance matrix with σ being a small value (typically, a few pixels). With this definition, the sum of the ground truth density over the entire image will not match the dot count Ci exactly, as dots that lie very close to the image boundary result in their Gaussian probability mass being partly outside the image. This is a natural and desirable behavior for most applications, as in many cases an object that lies partly outside the image boundary should not be counted as a full object, but rather as a fraction of an object.
Given a set of training images together with their ground truth densities, we aim to learn the linear transformation of the feature representation that approximates the density function at each pixel:
                            ∀p ∈ I_i     F_i (p│c)=c^T x_p^i                               (2)

Where cER^K    is the parameter vector of the linear transform that we aim to learn from the training data, and F_i (.│c)  is the estimate of the density function for a particular value of c. The regularized risk framework then suggests choosing c so that it minimizes the sum of the mismatches between the ground truth and the estimated density functions (the loss function) under regularization:

                     c=argmin(c^T c+λ*∑_(i=1)to N D(F_i^0 (.),F_i (.│c)))            (3)




