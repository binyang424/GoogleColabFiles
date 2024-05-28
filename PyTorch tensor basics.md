---
title: PyTorch Tensor的基本操作
post_date: 2024-04-27 23:46:00
---


```python
import torch
import numpy as np

torch.__version__
```


    '2.2.1+cu121'



## Create a torch tensor from Numpy


```python
arr = np.array([1, 2, 3, 4, 5])
print(arr)
```

    [1 2 3 4 5]

```python
arr.dtype
```


    dtype('int64')


```python
type(arr)
```


    numpy.ndarray




```python
x = torch.from_numpy(arr)
print(x)
```

    tensor([1, 2, 3, 4, 5])



```python
x.dtype
```


    torch.int64




```python
type(x)
```


    torch.Tensor




```python
torch.as_tensor(arr)
```


    tensor([1, 2, 3, 4, 5])



**Note** here any changes to `arr` will change the value of `x`


```python
arr[0] = 99
x
```


    tensor([99,  2,  3,  4,  5])

## 2D array


```python
arr2d = np.arange(0.0, 12.0)
arr2d = arr2d.reshape(4, 3)
arr2d
```


    array([[ 0.,  1.,  2.],
           [ 3.,  4.,  5.],
           [ 6.,  7.,  8.],
           [ 9., 10., 11.]])




```python
x2 = torch.from_numpy(arr2d)
x2
```


    tensor([[ 0.,  1.,  2.],
            [ 3.,  4.,  5.],
            [ 6.,  7.,  8.],
            [ 9., 10., 11.]], dtype=torch.float64)

## `torch.tensor()`, `torch.Tensor()` and `torch.FloatTensor()`


```python
my_arr = np.arange(10)
my_tensor = torch.tensor(my_arr)
my_tensor
```


    tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])




```python
my_other_tensor = torch.from_numpy(my_arr)
my_other_tensor
```


    tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])




```python
my_arr[0] = 999
print(my_tensor)
print(my_other_tensor)
```

    tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
    tensor([999,   1,   2,   3,   4,   5,   6,   7,   8,   9])



```python
my_tensor[0] = 998
my_tensor
```


    tensor([998,   1,   2,   3,   4,   5,   6,   7,   8,   9])




```python
new_arr = np.array([1, 2, 3], dtype=np.int32)
print("Data type of new_arr: ", new_arr.dtype)

my_tensor = torch.tensor(new_arr)
print(f"Data type of my_tensor: {my_tensor.dtype}")

my_Tensor = torch.Tensor(new_arr)
print(f"Data type of my_Tensor: {my_Tensor.dtype}")

my_FloatTensor = torch.FloatTensor(new_arr)
print(f"Data type of my_FloatTensor: {my_FloatTensor.dtype}")
```

    Data type of new_arr:  int32
    Data type of my_tensor: torch.int32
    Data type of my_Tensor: torch.float32
    Data type of my_FloatTensor: torch.float32
