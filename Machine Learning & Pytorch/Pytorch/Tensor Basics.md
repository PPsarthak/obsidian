#pytorch  #machine-learning #tensor
#### Torch Basics

Creating simple tensors 
```python
import torch

# Create a tensor of zeros
zeros = torch.zeros(3, 3)
print("Zeros Tensor:\n", zeros)

# Create a tensor of ones
ones = torch.ones(2, 2)
print("Ones Tensor:\n", ones)

# Create a random tensor
random_tensor = torch.rand(3, 4)
print("Random Tensor:\n", random_tensor)

# Create a tensor from a list
list_tensor = torch.tensor([[1, 2], [3, 4]])
print("List Tensor:\n", list_tensor)

# Create an identity matrix
identity = torch.eye(3)
print("Identity Matrix:\n", identity)
```

Math operations
```python
x = torch.tensor([1, 2, 3])
y = torch.tensor([9, 8, 7])

# Addition
z = torch.add(x, y)
torch.add(x, y, out=z)  
z = x + y                 # This is my preferred way, simple and clean.

t = torch.zeros(3)
t.add_(x)                 # in place addition, t+=x

# Subtraction
z = torch.sub(x,y)
torch.sub(x,y, out=z)
z = x - y 

# Multiplication
z = torch.mul(x,y)
torch.mul(x,y, out=z)
z = x*y

# Division
z = torch.divide(x, y)
z = x/y                 # Will do element wise division if of equal shape

# Matrix Multiplication
z = torch.mm(x, y)

# Dot product 
z = torch.dot(x, y)

```


