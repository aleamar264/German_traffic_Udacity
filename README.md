# German_traffic_Udacity
Project of classify german traffic of Udacity

# **Traffic Sign Recognition** 

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

[image1]: ./bar_image.png "Visualization"
[image2]: ./left.png "Left sign"
[image3]: ./80.png "80 Km/h sign"
[image4]: ./denny.png "No entry sign"
[image5]: ./priority.png "Priority road sign"
[image6]: ./work_road.png "Workin on the road sign"


---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/aleamar264/German_traffic_Udacity/blob/main/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* Number of training examples = 34799
* Number of testing examples = 12630
* Image data shape = [32 32]
* Number of unique classes = 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how many data are per ID, we can see a great imbalanced data, with this the net learn more some images, like "Speed limit (20 km/h) or "Keep right". 

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I used 2 approch:
1. I obtain the mean and standard desviation, I substrac the mean of all dataset to use and divide by std (standard desviation),with this I obtain data with 0 mean and 1 std.
2. Convert all the data to gray and after substrac and divide by 128.

With the first approch the model need more epochs to converge. The two metods converge with an accuracy of 93%. The model can obtain more accuracy but  force the stop when the accuracy reach 93%.

#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image  or 32x32x1 GRAY image		| 
| Convolution 5x5     	| 1x1 stride, same padding, outputs 32x32x6 	|
|Batch Normalization    | Using mu and sigma of 0 & 0.1                 |
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 16x16x6 				    |
| Convolution 5x5	    | 1x1 stride, same padding, outputs 16x16x16    |
|Batch Normalization    | Using mu and sigma of 0 & 0.1                 |
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 8x8x16 				    |
| Flatten       		| 400 nodes    									|
| Full conected			| 400 x 120    									|
|Batch Normalization    | Using mu and sigma of 0 & 0.1                 |
| RELU					|												|
| Full conected			| 120 x 84    									|
|Batch Normalization    | Using mu and sigma of 0 & 0.1                 |
| RELU					|												|
|Dropout                | Keep_prob = 0.75                              |
|Full conected          | 84 x 43                                       |



#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used 100 epochs and batch of 256. For the loss  used the loss used on the Lenet plus a l2 regularizer with a beta of 0.00015, also, the learning rate was 0.00035 with Adam optimizer.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* validation set accuracy of 0.94 (> 94%) 
* test set accuracy of 0.924 (92%)

If an iterative approach was chosen:
* The first architecture I tried was Lenet, but after some tries the accuracy was less to 60%. So after that I added more layers (full conected), the model begin to up the accuracy (80%)  but after some epochs the accuracy start to down again.

* I think was a lot of nodes (full conected layers), so I tried to add dropouts on every entry of the next layers, but I got the same problem (I change the keep_prob between 0.5 to 0.75), the accuracy enter a local minimum (~0.85).

* With the same layers added I put l2 regularizer, the validation accuracy go up a little. But after some tries I add a batch normalization to converge more quickly but I got the same result, a local minimum between 0.70 and 0.85. So I choose to delete the extra full conected layers and only keep the last dropout (before last layer "logits"), batch normalization and relu like activation function.

* This "new architecture" converge very quickly, so I reduce the learn rate to 0.00036 from 0.005. This is a custom Lenet.


### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image2] ![alt text][image3] ![alt text][image4] 
![alt text][image5] ![alt text][image6]

The seconf image might be difficult to classify because the dataset had a few examples of this.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Left turn       		| Left turn  									| 
| 80 km/h     			|  No entry 									|
| No entry				|  No entry			    						|
| Priority road	      	| Priority road	 			 				    |
| Road work		        | Road work   					                |


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. This compares favorably to the accuracy on the test set of ...

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

For the first image, the model is relatively sure that this is a stop sign (probability of 0.6), and the image does contain a stop sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .93         			| Turn left ahead								| 
| .02   				| Children crossing								|
| .01					| Right-of-way at the next intersection 		|
| .01	      			| Beware of ice/snow			 				|
| .001				    | Turn right ahead     							|


For the second image the model don't  recognize the sign.
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .20         			| No entry								| 
| .16   				| Keep right							|
| .12					| Yield                             	|
| .08	      			| Stop      			 				|
| .075				    | Road work    							|

For the third image the model can recognize with high accuracy the sign.
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .93         			| No entry							        	| 
| .02   				| Stop						                	|
| .007					| Turn left ahead                             	|
| .006	      			| Right-of-way at the next intersection 		|
| .004				    | Turn right ahead    							|

For the fourth image the model recognize the sign.
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .71         			| Priority road							| 
| .059   				| Keep right							|
| .049					| Speed limit (30km/h)                            	|
| .025	      			| Roundabout mandatory     			 				|
| .023				    | Speed limit (50km/h   							|

For the fifth image the model can recognize with high accuracy the sign.
| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| .86        			| Road work							        	| 
| .01   				| Right-of-way at the next intersection        	|
| .01					| Double curve                             	|
| .01	      			| Pedestrians		|
| .01				    | General caution   							|


