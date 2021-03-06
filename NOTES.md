# Coursera-Deep-Learning Specialization
Deep Learning Specialization taught by Andrew Ng.
Coded using numpy and TensorFlow through Jupyter Notebook.
## Course 1: Neural Networks & Deep Learning
### Topics Covered:
- Why is Deep Learning Taking Off?
	- In order of importance: Computer Hardware (GPU's), Data, Better Algorithms
- Logistic Regression
	- Linear model, linearly separable data
	- A neural net is one way to model non-linearly separable data
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
  	- If we initialize to zeros, then the hidden units become the same function which cause the derivatives to be the same as well which will cause the NN to learn only a single unique function. Therefore there is not point of multiple units or layers. It has the same performance as a linear classifier such as logistic regression
    	- Initialize to small random values, if weights are too large, sigmoid or tanh functions yield zero derivatives
	- well chosen init can speeds up convergence of gradient descent, increase the odds of gradient descent converging to a lower error
	- He's Initializations works well for RELU
## Course 2: Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization
### Topics Covered:
- Train/Dev/Test Sets
	- With large data these days usually a 99/0.5/0.5 split or lower. (Ex. m = 1 000 000, then m_dev = 10 000 which is sufficient)
- Basic Recipe for Machine Learning
	- These days the bias/variance tradeoff does not need to be considered that much if you: 1) Reduce J as much as possible, will introduce overfitting. 2) Reduce overfitting by introducing regularization.  If J still has high bias/variance you can tweak certain parameters and reduce one without affecting the other. (Ex more example will reduce variance and not affect bias)
- Reguluarization
	- You can vary regularization layer by layer as well to address overfitting in certain layers, ex the more units, the higher the weights, therfore have a higher regularization parameter for larger layers
	- L2 Regularizaiton
		- Add sqaure of each theta
	- Dropout Regularization
		- Randomly dropout units in network. This will reduce overfitting by 1) Creating a smaller network, 2) A unit can not rely on just one feature, therefore the weights will be spread out across all features
		- Used predominately in computer vision since there are so many features, not used that much in other fields
		- The problem is that J is no longer defined and therefore it is harder to debug your algorithm
		- Apply both in foward and back prop
		- Scale each layer by dropout prop to keep the same expected value for the activations for both foward and back prop
	- Data Augmentation
		- Creating new data from existing data (Ex. distorting, cropping or transforming an image)
		- Not as good as getting new unique data but still help reduce variance
	- Early Stopping
		- w starts as small random values and grows as you train, by stoping early you can keep w small
		- A good time to stop early is when J_dev is minimized
		- The problem with this approach is that if you want to further optimize your NN, you will have to focus on a variety of different things at the same time, ie reducing bias, then reducing variance, then reducing bias again. In contrast to orthgonolization where you focus on only one thing at at time and only once (Ex. 1. Min J_train -> overfitting 2. Reduce overfitting)
		- Not reccomended
- Vanishing/Exploding Gradients
	- The more layers you have, derivatives can become exponentially big or small. If w > 1 then explode, w < 1 then vanish
	- Solution: set variance of random initialized weights to be a function of n, will keep w close to 1
- Gradient Checking
	- Use two sided numerical approximation of gradient, it is more accurate
	- Difference between numerical approxmation should be less then 10^-7
- Mini-batch gradient descent
	- Speeds up computation, better than stochastic (mini-batch size of 1) since you can take advantage of vectorization if mini-batch size is greater than 1
	- Problems is that it will never converge to a global minimum but always jumps around eventually only jumping around close to the global minimum. This is because we are taking steps for a subset of m
		- With batch gradient descent J should decrease every iteration
		- With mini-batch J will oscillate but will have an overall downward trend
- Exponentially Weighted Averages
	- Average is computed with an exponential weight
	- Better than a simple average because you do not need to store all examples in memory to compute, just the previously computed exponential weighted average
	- Bias correction gets rid of the bias during the initial stage of weights average if you are concerned about it
- Gradient Descent with Momentum
	- When we have oscilaation in a dimension we want to reduce it in that dimension and follow a straighter path to the global min. To do so we want to eliminate oscillating gradients in certain dimension and retain non-oscillating gradients in other dimensions. To do so we can take the exponentially weighted moving average of the gradients. Oscillating gradients will create average close to 0 since it is going positve and negative while non-osccilating gradients will retain a high value since they do not change signs.
	- This elimanted osccilating gradients and thus speeds up learning
	- Intuitively you can think of using exponentially weights moving averages as applying an acceleration to the velocity of descent. Thus if osscilating, velocity will not increase since acceleration is negative and positive, while non oscillating will increase in velocity since acceleration never change in sign
- RMSProp (root mean square)
	- Divide gradients by exponentially weighted moving average, thereby making large gradients which usually osccilate update less and small gradients which usually don't osccilate update more. 
- Adam Optimization (adapative moment estimation)
	- Basically RMSProp and Momentum combined
	- Most popular optimization algo
	- Divid momentum gradients by exponentially weight moving average
- Learning Rate Decay
	- For mini-batch gradient descent there is always osccialtion even when near the global minium, therfore reduce learning rate when close to global miniumum/after iterations so there is less osccilation when you are near the global min
- Local Optima
	- Is not problem in high dimensional spaces because you have saddle points rather than optima. J is usually has such large dimension that it is very unlikely to get stuck
	- The real problem is plateau, areas of small gradients since learning will progress very slowly when you are in these regions. Optimization algos help you get past these plateaus.
- Hyperparamter tuning
	- Order of importance: 1. learning rate 2. batch norm, #hidden units, # mini-batch size 3. #layers, learning rate decay 4. adam (usually just leave default though)
	- Random values, not increamental grid
	- Coarse to fine
- Approriate Scale of HyperParameters
	- use randomly sampled log scale, not randomly sampled linear 
- Pandas vs Caviar
	- Either have one model and take a lot of care of it, or have multiple models and survival of the fittest
- Batch Normalization
	- Allows each layer to train faster since its inputs are normalized (fixed mean and variance)
	- Makes the distribution more constant which reduces covariate shifts (shifts in the distrbution of inputs), thus later layers have less coupling with previous layers, and later layers can more easily learn
	- Also has a slight regularization effect, the scaling effect adds some noise since mean and variance is calculated from mini-batches and not population (not intended to be used for regularization however, use traditional regularization methods)
- Batch Norm @ Test Time
	- Batch norm is calculated on mini-batches, at test time you might not be able to test on minibatches (you don't have enough exmaples in the test set). Therefore you can compute mean and variance through an exponentially weighted average across mini-batches and use it on test set.
- Multi-Class Classification
	- Use softmax activation function for multi-classes, takes a C dimensionalvector and maps it to C probabilities that sum to 1.
	- Ex z = [5 2 -1 3], t = [e^5 e^2 e^-1 e^3], x = sum(t) ( -> a = t / x
	- Exponential Normalization
	- Contrast to hardmax function: z = [5 2 -1 3], a = [1 0 0 0]
	- softmax generalizes logistic regression to C classes (if c = 2 then you just get logistic regression)
## Course 3: Structuring Machine Learning Projects
### Topics Covered:
- Orthogonalization
	- Focus on one thing at a time. One thing should control one thing. Which allows incremental improvement in performance, step by step. Ex. Early stopping does not follow orthogonalization since it affects both training set and dev set cost function.
	- Chain of assumption in ML
		1. Fit training set well on cost function (reduce bias to approx human-level performance )
			- bigger network, Adam
		2. Fit dev set well on cost function (reduce variance)
			- regularization, bigger training set
		3. Fit test set well on cost function
			- bigger dev set
		4. Performas well in real world
			- change dev set or cost function
- Single Number Evaluation Metric
	- Precision, Recall, FScore
- Satisficing and Optimising Metric (satisficing = it must meet a threshhold aftwer we don' really care, optimising = reduce as much as possible
- Train/Dev/Test Distribution
	- Ensure that train, dev, test sets have the same distribution
	- Choose a dev set and test set to reflect data you expect to get in the future and consider important to do well on
-Dev and Test Set Size
	- Dev set should be big enough to evalute different models/parameters
	- Test set should be big enough to see overall performance of classifier
	- With big data era usually 99/0.5/0.5 split, much smaller than 70/30 back when there was not that much data
- When to change dev/test set and metrics
	- If evalutation metric does not give you the desired classification efect, replace it
- Human Level Performance
	- Gives us a threshold/idea for Bayes Optimal Error (minimum theoritcal level of error)
	- Shows us avoidable bias. From this we can see whether we have a bias and variance ossue
	- If our algo is worse than humans we can :
		1. Get labeled data from humans
		2. Gain insight from manual error anlysis
		3. Better anlysis of bias/variance
	- Human-level error is used as a proxy for Bayes error 	
	- The closer you are to human level performance the harder it is to improve your algo (you do not have a proxy for Bayes error anymore, thus you don't know what is avoidable bias)
- Improving your Model Performance
	- Fundamental Assumptions of Supervised Learning
		1. You can fit the training set pretty well (low avoidable bias)
			- Compare Bayes Error (ex Human-level Performance) to training error to obtain avoidable bias
				- Train bigger model
				- Train longer/better optimization algo (momentum, RMSProp, Adam)
				- NN architecture/hyperparameters search
		2. Training set performance gernerales pretty well to the dev/test set (low variance)
			- Compare training error to dev error to obtain variance
				- More Data
				- Regularization (L2, dropout, data augmentation)
				- NN architecture/hyperparameters search
- Error Analysis
	- Manually go through errors and determine what is the most worthwhile to pursue
- Cleaning up incorrectly Labelled Errors
	- Training set: DL algo are robust to random errors in training set but not systematic (consistent errors)
	- Test/Dev set: Relabel if worthwhile to pursue over other errors
	- If fixing labels, do same process to both test and dev set so they have same distribution
	- Look at both wrong and right errors
- Build system quickly and then iterate
	- Set up dev/test set and an evaluation metric
	- Build initial system quickly
	- Use Bias/Variance analysis & Error analysis to prioritize next steps
- Different Test/Dev Set Distributions
	- If distrbutions are different, the distrbution of test/dev set should be the target distrbution
	- Thus you can use other distrbutions for training set and have more examples than you normally would have with just target set distrbution
	- You now introduce another source of error : data mismatch
- Data Mismatch, Bias and Variance with Mismatched Data Distributions 
	- Training-Dev Set: Same distribution as training set but not used for training
	- Bayes Error (Human Peformance) and Training Error = Avoidable Bias
	- Training Error and Training-Dev Error = Variance
	- Trainin-Dev and Dev Error = Data Mismatch
	- Dev and Test Error = degree of Overfitting to Dev set
	- Thus  you can see if different distrbution has a big impact or not
- Addressing Data Mismatch
	- Not systemic way to fix, bottom line: you need to make the distrbutions similar
	- Artificial Data Synthesis: Ex. add background noise to simulate a cafe
- Transfer Learning
	- Use previously trained NN for similar task, and transfer to new task
	- Provides a good starting point, ie low level feature (old task of speech recognition for a new task of wakeword detection)
	- Good when new task doesn't have much data and old task has been trained on a lot of data
- Multi-task Learning
	- Makes sense when
		- Trainig on tasks that would benefit from shared low level features (ex autonomous driving, different tasks include stop sign detection, road line detection, etc)
		- the number of examples are similar between different tasks
		- Can train one neural network rather than many different one
	- Usually used in computer vision, transfer learning is used more these days
- End-To-End Learning
	- No steps in between that have been hand designed by us
	- Required a lot of data, hand designed components inject knowledge from us, thus we require less data if hand designed
	- Ideal, but not realistic right now
### Technical Skills Acquired:
- Python 3.0
  - numpy
  - Tensorflow
