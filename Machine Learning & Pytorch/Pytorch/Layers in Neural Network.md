#pytorch  #machine-learning 
#### Convolutional Layer (nn.Conv2d):
- This layer applies convolutional filters/kernels to the input data.
- Convolutional filters slide over the input feature maps, extracting spatial patterns. 
- Parameters include the number of input and output channels, kernel size, stride, padding, etc.

![[2D Convolution.gif|650]]

#### Pooling Layer (nn.MaxPool2d, nn.AvgPool2d):   
- Pooling layers downsample the feature maps obtained from convolutional layers.
- Max pooling takes the maximum value from a window of pixels, while average pooling computes the average.
- Reduces spatial dimensions, helping to decrease computational complexity and control overfitting.

#### Batch Normalization (nn.BatchNorm2d):
- Batch normalization normalizes the activations of the previous layer by adjusting and scaling them.
- Helps in faster convergence, regularization, and reducing the impact of the internal covariate shift problem.

#### Activation Functions (nn.ReLU, nn.LeakyReLU, etc.):
- Activation functions introduce non-linearity into the network.
- ReLU, LeakyReLU, sigmoid, or tanh are commonly used activation functions after convolutional or fully connected layers.

#### Fully Connected Layer (nn.Linear):
- Fully connected layers connect every neuron in one layer to every neuron in the next layer.
- Typically used at the end of the network to map extracted features to the output classes.
- Parameters include the input and output size.

#### Dropout (nn.Dropout):
- Dropout layers randomly set a fraction of input units to zero during training to prevent overfitting.
- Helps in improving the generalization capability of the model by reducing co-adaptation of neurons.
    
#### Flatten (torch.flatten):
- Converts multi-dimensional feature maps into a one-dimensional vector.
- Flatten layers are used to reshape the multi-dimensional feature maps into a one-dimensional vector before passing them to fully connected layers.
- Fully connected layers expect one-dimensional input, while convolutional layers output multi-dimensional feature maps. 
- Flatten layers bridge this gap by converting the feature maps into a format suitable for fully connected layers.

![[Flattening in Pytorch.png|500]]