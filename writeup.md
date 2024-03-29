# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./output_images/train_sample_data.png "Visualization"
[image2]: ./output_images/train_sample_histogram.png "Histogram"
[image3]: ./output_images/train_sample_grayscale.png "Grayscale"
[image4]: ./output_images/external_images.png "Traffic signs external"
[image5]: ./output_images/predicted_external_images.png "Traffic signs external prediction"
[image6]: ./output_images/conv_layers.png "Neural network states"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data ...

![alt text][image1]

![alt text][image2]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

Looking at the grayscale image it preserves the essential features of traffic signs at a smaller dimension. Also the weights and biases can be found faster by using a single dimension instead of the three components of R, G, B.

Here is an example of a traffic sign image before and after grayscaling.

![alt text][image3]

The normalization is done in order to avoid situations where errors in computations when an operation is performed between a number that is too big and another that is too small. In this case the situation is avoided by scaling the (0-255) range to (0.1-0.9).


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 Grayscale image   					| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Input         		| 28x28x6   									| 
| Convolution 5x5     	| 2x2 stride, valid padding, outputs 14x14x10 	|
| RELU					|												|
| Input         		| 14x14x10    									| 
| Convolution 5x5     	| 2x2 stride, valid padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16 					|
| Flatten	      		| 2x2 stride,  outputs 400 						|
| Fully connected		| input 400, outputs 120        				|
| RELU					|												|
| Fully connected		| input 120, outputs 84        					|
| RELU					|												|
| Fully connected		| input 84, outputs 10        					|
| Softmax				| Used further to calculate the loss			|
|						|												|
 


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

The learning rate was chosen to be 0.001
The number of epochs was chosen to be 50
The batch size was set to 128
To train the model, I used an the cross-entropy to calculate the loss and Adam optimization.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.999
* test set accuracy of 0.919
* validation set accuracy of 0.935

If an iterative approach was chosen:
* As a first step I tried an RGB colorspace with normalization to 0.1-0.9 but the results would not go close to the 9.3 requirement with the validation data.
* To further improve the results 3 consecutive convolution layers were introduced followed by a max_pool layer
* The more epochs were introduced the better the results
* I also experimented with the learning rate but the results were comparable with values close to 0.001
* A huge improvement was noted by converting to grayscale and using normalization
* A dropout layer would help by avoiding overfitting


### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are 11 traffic signs that I found on the web:

![alt text][image4]


#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

![alt text][image5]

| Image				        |     Prediction	        					| 
|:-------------------------:|:---------------------------------------------:| 
| Traffic Signals  			| General caution								| 
| Bumpy road   				| Wild animals crossing							|
| No entry					| No entry										|
| Road narrows on the right	| Right of way at the next intersection			|
| Traffic Signals  			| General caution								|
| Road narrows on the right	| Traffic Signals					 			|
| Speed limit (30km/h)		| Roundabout mandatory					 		|
| Roundabout mandatory		| Go straight or right					 		|
| Go straight or right		| Go straight or right					 		|
| Priority road				| Priority road					 				|
| Traffic Signals  			| Traffic Signals								|


The model was able to correctly guess 4 of the 11 traffic signs, which gives an accuracy of 36.4%. The best results are for traffic signs found in the real world.
The caution signals were confused mostly while other signs with different shapes were successfuly recognized.
After a traffic sign is detected it needs to be scaled to the input shape required by the neural network input in order to classify it.
The further away traffic signs are, and by consequence the smaller the resolution, the less clear the information is. Some traffic signs due to their shape or color can be recognized from images with less resolution: priority road, give way.
Brightness affects the recognition process as it is haed to distinguish the traffic sign shape from the background. Of course such images till need to be used as training data. Another option would be to preprocess the image to increase the brightness and then attempt to recognize it.
Blur also has the same affect to decrease the clarity of the traffic sign. While shapes should still be distinguishable details in traffic signs for "caution" are harder to detect.
Obstruction can also play a role in the recognition process by hiding information that the neural network has learned to put higher emphasis on. In such cases either some form of preprocessing would be useful or feed more information in the nerual network to try to "underfit" ideal cases and be more robust.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 19th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a stop sign (probability of 0.6), and the image does contain a stop sign. The top five soft max probabilities were

| Probability			        |     Prediction	        					| 
|:-----------------------------:|:---------------------------------------------:| 
| 0.98  						| General caution								| 
| 1.00   						| Wild animals crossing							|
| 1.00 							| No entry										|
| 0.97							| Right of way at the next intersection			|
| 0.97							| General caution								|
| 0.78							| Traffic Signals					 			|
| 0.89							| Roundabout mandatory					 		|
| 0.99							| Go straight or right					 		|
| 1.00							| Go straight or right					 		|
| 0.99							| Priority road					 				|
| 0.99							| Traffic Signals								|


### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?

![alt text][image6]

It can be observed that layers closely resemble the cauton signs.