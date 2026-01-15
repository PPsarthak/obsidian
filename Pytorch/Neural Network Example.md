#neural-network #pytorch 

> Ref : https://youtu.be/Jy4wM2X21u0?si=4kWreI8gA6JqDhfk

```python
import torch

import torch.nn as nn 
# All neural network modules -> nn.Linear, nn.Conv2d, BatchNorm, Loss functions

import torch.optim as optim 
# For all Optimization algorithms -> SGD, Adam, etc.

import torch.nn.functional as F 
# All functions that don't have any parameters -> activation func like ReLU

from torch.utils.data import DataLoader 
# Gives easier dataset managment and creates batches

import torchvision.datasets as datasets 
# Has standard datasets we can import

import torchvision.transforms as transforms 
# Transformations we can perform on our dataset
```

## Building the models
The general way of building a model is to create a class where the neural network's layers are defined in the `__init__()` function
We define each layer of the neural network here
The class extends `nn.Module`



