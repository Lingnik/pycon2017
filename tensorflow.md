# A gentle introduction to deep learning with TensorFlow
presenter: Michelle Fullwood (@michelleful)
michelleful.github.io/PyCon2017

Supervised machine learning
Linear and regression

Logistic Regression == 2 steps away from deep learning
Traditional ML = hammer
Deep learning = fancier hammer

"What do the extra knobs and whistles buy us"

# Tensorflow
(missed)

# Linear Regression from Scratch
* Using numpy

# Typical Linear Regression Problem
* Predict prices of houses
* Three features: floor area, dist from public transport, # of rooms
* Matrix w/ rows houses, columns for features
* Predict vector Y, housing prices

# Inputs (numpy)

# Model
* Family of functions to consider in mapping X to Y
* Multiply each feature by a weight and add them up
* Add an additional intercept to get estimate
* Draw a line of best fit

# Model - Parameters
* 3 weights, one for each feature, and intercept
* Matrix multiplication of X by weight, (missed) to get prediction

# Model - Cost Function (loss function)
* Measure how good fit with parameters is
* Take difference between prediction and value and square it

# Cost Function (numpy)

# Optimization
* Find parameters that have the best fit
* Each set of parameters will yield a cost
* Goal in optimization is to find parameters that correspond to lowest point
* Use trial and error - not random set of parameters
* (gradient descent)

# Optimization - Gradient Calculation
* Calculate cost of error w/r/t
* Apply chain rule to break them into two pieces

# Optimization - Parameter Update
* Move the weights in the direction of the gradient
* Subtract the gradient from the weight

# Possible Problems
- Overshoot: get worse every time
- Undershoot: descend very slowly

# Learning Rate
(missed)

# Training

# Testing

# Relationship to Neural Networks/Deep Learning
* Surprise! Linear Regression is one of the simplest, first neural networks.
* Input layer of 3 x_i neurons and an output neuron, with weights between.
* Linear Regression is neural network in disguise.

# TensorFlow
* Ingredient 1- Inputs
    * Placeholders- for x is matrix of floats, None columns (for flexibility so we don't tie ourselves to a certain # of houses)
* Ingredient 2- Parameters --> Variables
    * Weights- random distribution
    * Intercept- set to 0
* 3- (missed)
* 4- Cost Function (missed)
* 5- Optimization- minimize the cost that we just defined

## Training
* Nothing happens outside of a session
* Only within a session can start writing to CPU or GPU
* Can't even add two numbers without going into a session

* Initialize the variables
* X_train, Y_train actual numpy arrays

* Two parts: outside the session (blueprint), within the session (the instance/thing we're building--the computation graph)

## Backpropagation

## Variable Update
* Update everything that isn't a variable

## Testing
* Within the same session, run a separate set of data to compute the error.

# Logistic Regression (Classification)
* Considers binary classification case first, then 10-way case.
* Use logistic regression for binary case.
* Uses an activation function (red semicircle)
* Softmax transforms into a probability distribution


* .relu is popular nonlinear activation filter
* Not "deep" learning until you have 2 hidden layers

# Why go deep?
1. Deeper networks are more powerful
  * There exist functions that can be approximated in a deep network with k layers; but if we take away any of those laysers, we need an exponential number of layers in our hidden layers to approximate the same function
2. Narrower networks are less prone to overfitting
  * Doesn't sacrifice generalized solution
  * Need to communicate most useful features as best you can
3. Deeper networks learn hierarchical feature representations
  * e.g. image classification features
  * each higher level based on the last
  * deep learning system learns all of the features for you

* Combine features in very simple ways
* Often problem is "do I have enough data to train a neural network?"

# Regularization
* Put the brakes on the training data by enforcing constraints on weights.
* Previously: If you have an outlier, contort the boundary to contain that outlier.
* Regularization forces constraints on the parameters that we learn.

* L1 & L2 Regularization used in linear regression.
* L2 regularization: weights should be small

# Dropout
* At each TRAINING step, knock out half of our training neurons
  * (Multiply each logit by 2...)
* "Building a completely new prediction model" --> "averaging" over several models
* Forces useful features to repeat themselves --> redundancy of useful features
* No conspiracies! Hidden neurons must be individually useful.
* At TEST time, we keep all the hidden neurons.
  * (Each logit is weighted 1, balancing the weights back from the dropped-out neurons.)


