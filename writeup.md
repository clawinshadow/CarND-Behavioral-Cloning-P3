# **Behavioral Cloning** 


[//]: # (Image References)

[image1]: ./examples/placeholder.png "Model Visualization"
[image2]: ./examples/placeholder.png "Grayscaling"
[image3]: ./examples/placeholder_small.png "Recovery Image"
[image4]: ./examples/placeholder_small.png "Recovery Image"
[image5]: ./examples/placeholder_small.png "Recovery Image"
[image6]: ./examples/placeholder_small.png "Normal Image"
[image7]: ./examples/placeholder_small.png "Flipped Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* __model.py__: containing the script to create and train the model
* __drive.py__: for driving the car in autonomous mode
* __model.h5__: containing a trained convolution neural network 
* __writeup.md__: summarizing the results
* __video.mp4__: the video recording the autonomous mode running result in the simulator 

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

My model comes from the NVIDIA auto-driving network introduced in the course. The architecture is listed as below:
1. **Normalization layer**: it's a lambda layer, convert the pixel value range from [0, 255] to [-0.5, 0.5]
2. **Crop layer**: the original image shape is (160, 320, 3), crop the top 70 pixels and the bottom 25 pixels, output image shape of (66, 320, 3)
3. **Convoluation layer 1st**: with the kernel size (5, 5), filter depth of 24, and a subsampling stride of (2, 2), and activation function of RELU to introduce nonlinearity
4. **Convoluation layer 2nd**: with the kernel size (5, 5), filter depth of 36, and a stride of (2, 2), RELU activation function also
5. **Convoluation layer 3rd**: with the kernel size (5, 5), filter depth of 48, and a stride of (2, 2), RELU activation function also
6. **Convoluation layer 4th**: with the kernel size (3, 3), filter depth of 64, RELU activation function also
7. **Convoluation layer 5th**: with the kernel size (3, 3), filter depth of 64, RELU activation function also
8. **Full connected layer 1st**: input should be (1, 33x64), output (1, 100)
9. **Full connected layer 2nd**: input (1, 100), output (1, 50)
10. **Full connected layer 3rd**: input (1, 50), output (1, 10)
11. **Final Full connected layer**: output the final steering measurement


#### 2. Attempts to reduce overfitting in the model
I split the original dataset into training & validation dataset (0.2 of the total dataset), shuffle them and training 5 epochs, to reduce overfitting. Code line 16, 66

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 25).

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road ... 

For details about how I created the training data, see the next section. 

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy for deriving a model architecture was to ...

My first step was to use a convolution neural network model similar to the ... I thought this model might be appropriate because ...

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. 

To combat the overfitting, I modified the model so that ...

Then I ... 

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track... to improve the driving behavior in these cases, I ....

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

The final model architecture (model.py lines 18-24) consisted of a convolution neural network with the following layers and layer sizes ...

Here is a visualization of the architecture (note: visualizing the architecture is optional according to the project rubric)

![alt text][image1]

#### 3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![alt text][image2]

I then recorded the vehicle recovering from the left side and right sides of the road back to center so that the vehicle would learn to .... These images show what a recovery looks like starting from ... :

![alt text][image3]
![alt text][image4]
![alt text][image5]

Then I repeated this process on track two in order to get more data points.

To augment the data sat, I also flipped images and angles thinking that this would ... For example, here is an image that has then been flipped:

![alt text][image6]
![alt text][image7]

Etc ....

After the collection process, I had X number of data points. I then preprocessed this data by ...


I finally randomly shuffled the data set and put Y% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was Z as evidenced by ... I used an adam optimizer so that manually training the learning rate wasn't necessary.
