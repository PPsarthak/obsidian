#pytorch #machine-learning 

Ref - https://youtu.be/3Kb0QS6z7WA?si=Asy8jMPm7vbCmu2q

**Gradient**
Imagine you're riding a bike up a hill, and you're trying to figure out how steep the hill is at a specific point. The steeper the hill, the harder it is to pedal. This steepness is called the *gradient*—it tells you how much the hill is going up or down at that point.
> PyTorch uses autograd (automatic differentiation) to compute these gradients.

When training a machine learning model, the goal (when training) is to minimize the loss function. 
The gradient helps your model learn! It tells the model how to tweak its parameters so it can make better predictions, like adjusting the pedals on your bike to climb the hill more efficiently.

**Backpropagation**
Backpropagation is the method used to train neural networks. It’s the process of calculating the gradients (using the chain rule from calculus) and updating the network's parameters (like weights and biases) to minimize the error in predictions.

*General Process in Backpropagation*
- Forward Pass:
	- Each layer perform operations like - matrix multiplication, activation functions, etc.
- Compute the Gradient:
    - For each parameter w, compute `∂Loss\∂w`(partial derivative of the loss function with respect to www).
- Update the Parameters:
    - Adjust the parameters in the opposite direction of the gradient: `w'=w−η⋅∂Loss\∂w`​ where:
        - η the *learning rate*, a small number controlling how big the steps are.
- Iterate:
    - Repeat this process for multiple iterations until the loss stops decreasing.

*Learning Rate*: hyperparameter in machine learning that controls the size of the steps taken during the optimization process to adjust the model’s parameters. Or simply put, it determines how much (what percentage) it should learn during optimization