# Coursera-Deep-Learning Specialization
Deep Learning Specialization taught by Andrew Ng.
## Course 1: Neural Networks & Deep Learning
### Topics Covered:
- Why is Deep Learning Taking Off?
	- In order of importance: Computer Hardware (GPU's), Data, Better Algorithms
- Logistic Regression
- **Shallow vs Deep Neural Networks (few layers)**
 	- Deep Learning just means Neural Networks with many layers.
    - **More layers allow for more complex models. Think of the layers of NN create complex (non-linear) hypothesis while the output layer is simply a logistic regression classifier of the learned paramters by the previous layers.**
    	- Ex Face Detection: First layer finds edges, second layer nose, eyes, lips, last layer finds face
        - Ex Sounds: First layer finds sounds, second layer finds combinations of sounds, last layer finds words
        - Ex Circuit Theory: First layer is nand gates, Second layer is or gates, last layer is xor gates
    - **Without deep networks you require an exponentially large amount of nodes in a single layer to get the same effect of more layers**
       - Ex Circuit Theory: to get xor from one hidden layer, you would have to map exponentially amount of nodes in hidden layer instead of making nand first with one layer, or with second, than finally xor with last layer) therefore more layers the better for complex models.
 	- Input layer does not count as a layer (Ex. a 2 layer NN, consists of input, 1 hidden, output)
- Vectorization
 	- Cleaner code and faster due to optimizations
- **Activation Functions**
	- Non-linear activation functions are required to make complex hypothesis, without it NN would be linear
 	- Sigmoid
  		- Problem: has close to 0 slope and large values of z, this will slow down gradient descent and other optimization methods
 		 - **Used only for output layer since it provides output x{0,1}**
 	- tanh
  		- shifted sigmoid so x{-1, 1}, better for everything than sigmoid except having an output of {0,1} for the output layer
  		- Problems: same as sigmoid
 	- RELU (Rectified Linear Unit)
  		- eliminates problem of sigmoid but has undefined derivative at 0, in practice however, this doesn't matter
  		- **main activation function used today**
        - Has zero slope for z < 0, but this doesn't usually happen
        - Leaky RELU has a non-zero slope when x < 0, slope can be a hyperparameter that can help making the model better
 - Random Initialization
  	- If we initialize to zeros, then the hidden units become the same function which cause the derivatives to be the same as well which will cause the NN to learn only a single unique function. Therefore there is not point of multiple units or layers
    - Initialize to small random values, if weights are too large, sigmoid or tanh functions yield zero derivatives

### Technical Skills Acquired:
- Python 3.0
  - numpy
