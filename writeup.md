# **Behavioral Cloning** 


[//]: # (Image References)

[image1]: ./center_1.jpg "Sample Image"

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

I just use the sample training data from workspace path: /opt/carnd_p3/data/IMG, it works well so I didn't collect additional training data myself.

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy to design an appropriate solution including the following steps:
1. Collect the training images
2. Loading the images, then split them to training & validation datasets
3. Design an appropriate network model for training
4. Test the model in the simulator with autonomous mode

Because a sample dataset was provided, so I ignored the first step at the beginning, planned to collect additional data if necessary. Then I write some codes to load the sample images from parsing the driving_log.csv file, codes from line 9 to 38, as the project instructions suggested, I use generators to reduce the memory usage when loading images. 

The most important step should be how to choose an appropriate training model, at the very first, I just implement a simple linear regression model to fit the training dataset using Keras, and then run the model with simulator, in order to verify whether the whole process works or not. After the process was verified correct, I chose a more complex and reasonable network, the LeNet for training, but the performance was not very good, the car often run out of the roads, then I tried to extend the training dataset by flipping the images, and also using the left & right camera images, the performance improved a lot, but still not perfect, the car fell off the track in a few spots. Finally, I tried to use the NVIDIA network, it works very well, the car is able to drive autonomously around the track without leaving the road, even I just use the original sample dataset for training. The detail architecture was introduced in prior section, and the final running result is recored in ```video.mp4```

#### 2. Final Model Architecture

The final model architecture (model.py lines 51-70) consisted of a convolution neural network provided by NVIDIA, the detail model architecture was listed in the prior section: __An appropriate model architecture has been employed__

#### 3. Creation of the Training Set & Training Process

I just use the sample data as training set and validation set, and only the center camera images. Here is an example image of center lane driving:

![alt text][image1]

I finally randomly shuffled the data set and put 20% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 5 as evidenced by the validation loss will increase if epochs greater than 5. I used an adam optimizer so that manually training the learning rate wasn't necessary.
