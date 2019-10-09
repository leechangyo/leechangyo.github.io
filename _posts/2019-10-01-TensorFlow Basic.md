---
layout: post
title: 1. Introduction to Neural Networks
category: Deep Learning
tag: Deep Learning
---

# Deep learning
- This will cover key theory aspects
  - Neurons and Activation Functions
  - Cost Functions
  - Gradient Descent
  - Backpropagation

## 1. Introduction to the Perceptron
- Before we launch straight into neural networks, we need to understand the individual components first, such as a single “neuron”.
- Artificial Neural Networks (ANN) actually have a basis in biology!
- Let’s see how we can attempt to mimic biological neurons with an artificial neuron, known as a perceptron!
- The biological neuron:
<a href="https://postimg.cc/Ty5GdJpL"><img src="https://i.postimg.cc/tg2gzrb5/4235.png" width="700px" title="source: imgur.com" /><a>
- Dendrites树状突（가지돌기)
<a href="https://imgur.com/bn25BoB"><img src="https://i.imgur.com/bn25BoB.png" width="700px" title="source: imgur.com" /><a>
- The artificial neuron also has inputs and outputs!
  - This simple model is known as a perceptron.
  - We have two inputs and an output
  - Inputs will be values of features
  - Inputs are multiplied by a weight
  - Weights initially start off as random
  - Inputs are now multiplied by weights
  - Then these results are passed to an activation function
    - Many activation functions to choose from, we’ll cover this in more detail later!
  - If sum of inputs is positive return 1, if sum is negative output 0.
  - In this case 6-4=2 so the activation function returns 1.
  - There is a possible issue. What if the original inputs started off as zero?
    - Then any weight multiplied by the input would still result in zero!
  - We fix this by adding in a bias term, in this case we choose 1.
- So what does this look like mathematically?
- Let’s quickly think about how we can represent this perceptron model mathematically:
<a href="https://imgur.com/m2NtqQN"><img src="https://i.imgur.com/m2NtqQN.png" width="700px" title="source: imgur.com" /><a>
- Once we have many perceptrons in a network we’ll see how we can easily extend this to a matrix form
- Review
  - Biological Neuron
  - Perceptron Model
  - Mathematical Representation

## 2. Introduction to Neural Networks
- We’ve seen how a single perceptron behaves, now let’s expand this conceptto the idea of a neural network
- Let’s see how to connect many perceptrons together and then how to represent this mathematically!
<a href="https://imgur.com/3YbwQSd"><img src="https://i.imgur.com/3YbwQSd.png" width="700px" title="source: imgur.com" /><a>
- Multiple Perceptrons Network
- Input Layer. 2 hidden layers. Output Layer
- Input Layer
  - Real values from the data
- Hidden Layers
  - Layers in between input and output
  - 3 or more layers is “deep network"
- Output Layer
  - Final estimate of the output
- As you go forwards through more layers, the level of abstraction increases
- Let’s now discuss the activation function in a little more detail!
<a href="https://imgur.com/xnHdG7h"><img src="https://i.imgur.com/xnHdG7h.png" width="700px" title="source: imgur.com" /><a>
- Previously our activation function was just a simple function that output 0 or 1.
- This is a pretty dramatic function, since small changes aren’t reflected
- It would be nice if we could have a more dynamic function, for example the red line!
<a href="https://imgur.com/Ct78LIn"><img src="https://i.imgur.com/Ct78LIn.png" width="700px" title="source: imgur.com" /><a>
- Lucky for us, this is the sigmoid function
- Changing the activation function used can be beneficial depending on the task!
- Let’s discuss a few more activation functions that we’ll encounter!
<a href="https://imgur.com/r1n1ikc"><img src="https://i.imgur.com/r1n1ikc.png" width="700px" title="source: imgur.com" /><a>
- Hyperbolic Tangent: tanh(z)
<a href="https://imgur.com/s13iA3w"><img src="https://i.imgur.com/s13iA3w.png" width="700px" title="source: imgur.com" /><a>
- Rectified Linear Unit (ReLU): This is actually a relatively simple function: max(0,z)
- ReLu and tanh tend to have the best performance, so we will focus on these two.
- Deep Learning libraries have these built in for us, so we don’t need to worry about having to implement them manually!
- As we continue on, we’ll also talk about some more state of the art activation functions.
- Up next, we’ll discuss cost functions, which will allow us to measure how well these neurons are performing!

## 3. Cost Functions
- Let’s now explore how we can evaluate performance of a neuron!
- We can use a cost function to measure how far off we are from the expected value.
- We’ll use the following variables:
  - y to represent the true value
  - a to represent neuron’s prediction
- In terms of weights and bias:
  - w*x + b = z
  - Pass z into activation function σ(z) = a
- Quadratic Cost
  - C = Σ(y-a)2 / n
- We can see that larger errors are more prominent(重要的) due to the squaring（原型）.
- Unfortunately this calculation can cause a slowdown in our learning speed.
- Cross Entropy（比Quadratic Cost速度更快)
  - C = (-1/n) Σ (y⋅ln(a) + (1-y)⋅ln(1-a)
  - This cost function allows for faster learning.
- The larger the difference, the faster the neuron can learn.
- We now have 2 key aspects of learning with neural networks
  - the neurons with their activation function and the cost function.
- We’re still missing a key step, actually “learning”!
- We need to figure out how we can use our neurons and the measurement of error (our cost function) and then attempt to correct our prediction, in other words, **“learn”!**

## 4. Gradient Descent and Backpropagation
- Gradient descent is an optimization algorithm for finding the minimum of a function.
- To find a local minimum, we take steps proportional to the negative of the gradient.
- Gradient Descent (in 1 dimension)
<a href="https://imgur.com/2iwhJBu"><img src="https://i.imgur.com/2iwhJBu.png" width="700px" title="source: imgur.com" /><a>
<a href="https://imgur.com/prL0yrU"><img src="https://i.imgur.com/prL0yrU.png" width="700px" title="source: imgur.com" /><a>
<a href="https://imgur.com/I9GChcv"><img src="https://i.imgur.com/I9GChcv.png" width="700px" title="source: imgur.com" /><a>
- Visually we can see what parameter value to choose to minimize our Cost
- Finding this minimum is simple for 1 dimension, but our cases will have many more parameters, meaning we’ll need to use the built-in linear algebra that our Deep Learning library will provide!
- Using gradient descent we can figure out the best parameters for minimizing our cost
  - for example, finding the best values for the weights of the neuron inputs.
- We now just have one issue to solve, **how can we quickly adjust the optimal parameters or weights across our entire network?**
- This is where backpropagation comes in!
- Backpropagation is used to calculate the error contribution of each neuron after a batch of data is processed.
- It relies heavily on the chain rule to go back through the network and calculate these errors.
- Backpropagation works by calculating the error at the output and then distributes back through the network layers.
- It requires a known desired output for each input value (supervised learning)
- The implementation of backpropagation will be further clarified when we dive into the math example!

## 5. Manual Neural Network Operation
- Operation Class
  - Input Nodes
  - Output Nodes
  - Global Default Graph Variable
  - Compute
    - Overwritten by extended classes
- Graph - A global variable
<a href="https://imgur.com/5jKyhak"><img src="https://i.imgur.com/5jKyhak.png" width="700px" title="source: imgur.com" /><a>
<a href="https://imgur.com/sG3DSK5"><img src="https://i.imgur.com/sG3DSK5.png" width="700px" title="source: imgur.com" /><a>
<a href="https://imgur.com/F0LmSUe"><img src="https://i.imgur.com/F0LmSUe.png" width="700px" title="source: imgur.com" /><a>

## 6. Manual Neural Network Variables, Placeholders, and Graphs
- Placeholder - An “empty” node that needs a value to be provided to compute output(빈껍데기 이며 아웃풋 계산을 위해 값이 들어가야된다.)
- Variables - Changeable parameter of Graph
- Graph - Global Variable connecting variables and placeholders to operations (오퍼레이션 만나게 해주는 브로커 역할)

## 7. Manual Neural Network Session
- Now that the Graph has all the nodes, we need to execute all the operations within a Session.
- We’ll use a PostOrder Tree Traversal to make sure we execute the nodes in the correct order.
  - y = mx + b
  - y = -1x + 5
  - Remember that both y and x are features!
  - Feat2 = -1*Feat1 + 5
  - Feat2 + Feat1 - 5 = 0
  - FeatMatrix[ 1, 1] - 5 = 0
- manually build out a neural network that mimics the TensorFlow API. This will greatly help your understanding when working with the real TensorFlow!

### Quick note on Super() and OOP

```python
class SimpleClass():

    def __init__(self,str_input):
        print("SIMPLE"+str_input)
```

```python
class ExtendedClass(SimpleClass):

    def __init__(self):
        print('EXTENDED')
```

```python
s = ExtendedClass() # EXTENDED
# self = s
```

```python
class ExtendedClass(SimpleClass):

    def __init__(self):

        super().__init__(" My String")
        #super : grab whatever class im inheriting from  (simpleclass
        #종속 클래스에 넘어간다.
        print('EXTENDED')
```

```python
s = ExtendedClass()
#SIMPLE My String
#EXTENDED

# self = s
```

### Operation

```python
class Operation():
    """
    An Operation is a node in a "Graph". TensorFlow will also use this concept of a Graph(Global Variable.

    This Operation class will be inherited by other classes that actually compute the specific
    operation, such as adding or matrix multiplication.
    """

    def __init__(self, input_nodes = []):
        """
        Intialize an Operation
        """
        self.input_nodes = input_nodes # The list of input nodes
        self.output_nodes = [] # List of nodes consuming this node's output
        #placeholde
        # For every node in the input, we append this operation (self) to the list of
        # the consumers of the input nodes
        for node in input_nodes:
            node.output_nodes.append(self)
        # self which is essentially just a reference to this current operation
        # graph
        # There will be a global default graph (TensorFlow works this way)
        # We will then append this particular operation
        # Append this operation to the list of operations in the currently active default graph
        _default_graph.operations.append(self)

    def compute(self):
        """
        This is a placeholder function. It will be overwritten by the actual specific operation
        that inherits from this class.

        """

        pass
```

### Example Operations(addition)

```python
class add(Operation):

    def __init__(self, x, y):
        #x,y is the our input node to operation

        super().__init__([x, y])
        #super chain 같은것이다

    def compute(self, x_var, y_var):

        self.inputs = [x_var, y_var]
        return x_var + y_var
```

### Example Operations(Multiplication)

```python
class add(Operation):

    def __init__(self, x, y):
        #x,y is the our input node to operation

        super().__init__([x, y])
        #super chain 같은것이다

    def compute(self, x_var, y_var):

        self.inputs = [x_var, y_var]
        return x_var * y_var
```

### Example Operations(Matrix Multiplication)

```python
class matmul(Operation):

    def __init__(self, a, b):

        super().__init__([a, b])

    def compute(self, a_mat, b_mat):

        self.inputs = [a_mat, b_mat]
        return a_mat.dot(b_mat)
```

### PlaceHolder (변수를 받는 곳)

```python
class Placeholder():
    """
    A placeholder is a node that needs to be provided a value for computing the output in the Graph.
    """

    def __init__(self):

        self.output_nodes = []

        _default_graph.placeholders.append(self)
        #going to grab that global variable from the default graph
```

### Variables

```python
class Variable():
    """
    This variable is a changeable parameter of the Graph.
    """

    def __init__(self, initial_value = None):

        self.value = initial_value
        self.output_nodes = []


        _default_graph.variables.append(self)
```

### Graph

```python
class Graph():


    def __init__(self):

        self.operations = []
        self.placeholders = []
        self.variables = []

    def set_as_default(self):
        """
        Sets this Graph instance as the Global Default Graph
        """
        global _default_graph
        _default_graph = self
        # that's going to allow us to access it inside this place holder insde this calss
```

### Basic Graph
- z = Ax + b
- with A=10 and b=1
- z = 10x + 1
- Just need a placeholder for x and then once x is filled in we can solved it!

```python
g = Graph()
g.set_as_default()
#none .
#_default_grapht is the grobal graph
A = Variable(10)
b = Variable(1)
# Will be filled out later
x = Placeholder()
y = multiply(A,x)
z = add(y,b)
```

### Session(help to execute all the operations)

```python
import numpy as np
def traverse_postorder(operation):
    """
    PostOrder Traversal of Nodes. Basically makes sure computations are done in
    the correct order (Ax first , then Ax + b). Feel free to copy and paste this code.
    It is not super important for understanding the basic fundamentals of deep learning.
    """

    nodes_postorder = []
    def recurse(node):
        #递归
        if isinstance(node, Operation):
            for input_node in node.input_nodes:
                recurse(input_node)
        nodes_postorder.append(node)

    recurse(operation)
    return nodes_postorder
```

```python
class Session:

    def run(self, operation, feed_dict = {}):
        """
          operation: The operation to compute
          feed_dict: Dictionary mapping placeholders to input values (the data)# mapping data in placeholder  
        """

        # Puts nodes in correct order
        nodes_postorder = traverse_postorder(operation)

        for node in nodes_postorder:

            if type(node) == Placeholder:

                node.output = feed_dict[node]

            elif type(node) == Variable:

                node.output = node.value

            else: # Operation

                node.inputs = [input_node.output for input_node in node.input_nodes]


                node.output = node.compute(*node.inputs)
                #* : just wait for us to provide these inputs without knowing how many inputs we may have throughout the operation
                #so technically we don't really know the size of this list so we pass it into computer

            # Convert lists to numpy arrays
            if type(node.output) == list:
                node.output = np.array(node.output)

        # Return the requested node value
        return operation.output

```

```python
sess = Session()
result = sess.run(operation=z,feed_dict={x:10})
result # 101
# 10*10+1
```

```python
g = Graph()

g.set_as_default()

A = Variable([[10,20],[30,40]])
b = Variable([1,1])

x = Placeholder()

y = matmul(A,x)

z = add(y,b)
sess = Session()
result = sess.run(operation=z,feed_dict={x:10})
result
# array([[101, 201],
       # [301, 401]])
```

### Activation Function

```python
import matplotlib.pyplot as plt
%matplotlib inline

def sigmoid(z):
    return 1/(1+np.exp(-z))

```

```python
sample_z = np.linspace(-10,10,100)
sample_a = sigmoid(sample_z)

plt.plot(sample_z,sample_a)

```

<a href="https://imgur.com/EG54mSZ"><img src="https://i.imgur.com/EG54mSZ.png" width="700px" title="source: imgur.com" /><a>

### Sigmoid as an Operation

```python
class Sigmoid(Operation):


    def __init__(self, z):

        # a is the input node
        super().__init__([z])

    def compute(self, z_val):

        return 1/(1+np.exp(-z_val))
```

### Classification Example

```python
from sklearn.datasets import make_blobs
#make_blob(binary Large object): 이미지 사운드 비디오 같은 멀티미디어 데이터를 다룰떄 사용 된다.
data = make_blobs(n_samples = 50,n_features=2,centers=2,random_state=75)
#center is how many blob we going to make
# value in array is the n_feature
```

```
data

(array([[  7.3402781 ,   9.36149154],
        [  9.13332743,   8.74906102],
        [  1.99243535,  -8.85885722],
        [  7.38443759,   7.72520389],
        [  7.97613887,   8.80878209],
        [  7.76974352,   9.50899462],
        [  8.3186688 ,  10.1026025 ],
        ....
array([1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, 1, 1, 1,
        0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0,
        0, 0, 0, 1]))
```

```python
features = data[0]
plt.scatter(features[:,0],features[:,1])
```

<a href="https://imgur.com/GgcXnA1"><img src="https://i.imgur.com/GgcXnA1.png" width="700px" title="source: imgur.com" /><a>

```python
labels = data[1]
plt.scatter(features[:,0],features[:,1],c=labels,cmap='coolwarm')
```

<a href="https://imgur.com/btgIews"><img src="https://i.imgur.com/btgIews.png" width="700px" title="source: imgur.com" /><a>

```python
# DRAW A LINE THAT SEPERATES CLASSES
x = np.linspace(0,11,10)
y = -x + 5
plt.scatter(features[:,0],features[:,1],c=labels,cmap='coolwarm')
# to make color , c=labels, cmp='coolwarm'
plt.plot(x,y)
```

<a href="https://imgur.com/9zyegQI"><img src="https://i.imgur.com/9zyegQI.png" width="700px" title="source: imgur.com" /><a>

### Defining the Perception
- 𝑦=𝑚𝑥+𝑏
- 𝑦=−𝑥+5
- 𝑓1=𝑚𝑓2+𝑏,𝑚=1
- 𝑓1=−𝑓2+5
- 𝑓1+𝑓2−5=0

### Convert to a Matrix Representation of Features
- $𝑤^𝑇$𝑥+𝑏=0
- (1,1)𝑓−5=0

### Example Point
- Let's say we have the point f1=2 , f2=2 otherwise stated as (8,10). Then we have:
  - (1,1)$(8 10)^T$+5=

```python
np.array([1, 1]).dot(np.array([[8],[10]])) - 5 #array([13])
np.array([1,1]).dot(np.array([[4],[-10]])) - 5 #array([-11])
```

### Using an Example Session Graph
```python
g = Graph()
g.set_as_default()
x = Placeholder()
w = Variable([1,1])
b = Variable(-5)
z = add(matmul(w,x),b)
a = Sigmoid(z)
sess = Session()
sess.run(operation=a,feed_dict={x:[8,10]})
# graph output
# 0.99999773967570205

sess.run(operation=a,feed_dict={x:[0,-10]})
# 3.0590222692562472e-07
```

# Reference

[Pieriandata](https://www.pieriandata.com/)
