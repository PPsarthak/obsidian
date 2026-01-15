#pytorch #machine-learning 

> [!tip] 
> **Installation**
`pip install torch torchvision torchaudio`
>To cross check if installation is successful, open terminal and enter python
>paste this command - `import torch;` `print(torch.__version__)`

*Cuda*
PyTorch leverages CUDA to perform tensor computations on NVIDIA GPUs
To check if cuda is available or not
`print(torch.cuda.is_available())`

You can assign a <span style="color:rgb(102, 207, 255)">tensor</span> either to a CPU or a GPU
It is a general practice to use the below line in Pytorch to assign CPU/GPU to a tensor
```python
device = "cuda" if torch.cuda.is_available() else "CPU"`
```

You can move a tensor from CPU to GPU:
```python
tensor = torch.tensor([1, 2, 3])
tensor_gpu = tensor.to(device)
```

