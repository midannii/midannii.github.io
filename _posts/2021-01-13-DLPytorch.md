---
layout: post
title:  "[모두를 위한 딥러닝2 Pytorch] Tensor Manipulation "
date:   2021-01-13
desc: " "
keywords: "DL, ML"
categories: [Deeplearning]
tags: [DL, ML, pytorch]
icon: icon-html
---

19년 겨울 모두를 위한 딥러닝1을 공부했었는데, 이번에 `pytorch` 버전으로 2가 나왔길래 복습겸 공부 ! 👩‍💻👩‍💻



<br>

## Vector, Matrix, Tensor


- 차원 x ) scalar

- 1차원 ) Vector

- 2차원 ) Matrix

- 3차원 이상 ) tensor


### Tensor shape

```
|t| = (batch size, dim) # 2D
|t| = (batch size, width, height) # 3D images
|t| = (batch size, length, dim) # 3D - Typical Natural language processing
```

- 이미지 뿐 아니라 NLP에서도 3차원 tensor가 쓰임

  - 이 경우, 각 문장(dim x length)이 batch size 만큼 쌓여있음



# Pytorch

PyTorch is like NumPy (but better)

```python
import numpy as np
import torch


# 1D array
n1 = np.array([0., 1., 2., 3., 4., 5., 6.])
t1 = torch.FloatTensor([0., 1., 2., 3., 4., 5., 6.])

# 2D array
n2 = np.array([[1., 2., 3.], [4., 5., 6.], [7., 8., 9.], [10., 11., 12.]])
t2 = torch.FloatTensor([[1., 2., 3.],
                       [4., 5., 6.],
                       [7., 8., 9.],
                       [10., 11., 12.]
                      ])

```


이와 같이 매우 비슷하다


```python
print(t2.dim())  # rank
print(t2.size()) # shape
print(t2[:, 1])
print(t2[:, 1].size())
print(t2[:, :-1])
```

```
2
torch.Size([4, 3])
tensor([ 2.,  5.,  8., 11.])
torch.Size([4])
tensor([[ 1.,  2.],
        [ 4.,  5.],
        [ 7.,  8.],
        [10., 11.]])
```


## BroadCasting


두 텐서의 크기가 같을때, 곱, 합 등을 쉽게 계산 가능

크기가 다른 경우, 자동적으로 크기 같게 맞춰주므로 사용에 유의해야 함

(원래 크기가 다르면 에러와 종료가 되어야 하는데, 자동으로 broadcasting 하게 되면 나중에 에러 찾기 힘들어질 수도 있음 )

```python
# Same shape
m1 = torch.FloatTensor([[3, 3]])
m2 = torch.FloatTensor([[2, 2]])
print(m1 + m2) # tensor([[5., 5.]])

# Vector + scalar
m1 = torch.FloatTensor([[1, 2]])
m2 = torch.FloatTensor([3]) # 3 -> [[3, 3]]
print(m1 + m2) # tensor([[4., 5.]])

# 2 x 1 Vector + 1 x 2 Vector
m1 = torch.FloatTensor([[1, 2]])
m2 = torch.FloatTensor([[3], [4]])
print(m1 + m2)
# tensor([[4., 5.],
# [5., 6.]])

```

## 다양한 연산


### mean

```python
t = torch.FloatTensor([1, 2])
print(t.mean())
```


### sum

```python
t = torch.FloatTensor([[1, 2], [3, 4]])
print(t.sum()) # tensor(10.)
print(t.sum(dim=0)) # tensor([4., 6.])
print(t.sum(dim=1)) # tensor([3., 7.])
print(t.sum(dim=-1)) # tensor([3., 7.])
```


### view

```python
t = np.array([[[0, 1, 2],
               [3, 4, 5]],

              [[6, 7, 8],
               [9, 10, 11]]])
ft = torch.FloatTensor(t)
print(ft.shape) # torch.Size([2, 2, 3])

print(ft.view([-1, 3]))
# tensor([[ 0.,  1.,  2.],
#        [ 3.,  4.,  5.],
#        [ 6.,  7.,  8.],
#        [ 9., 10., 11.]])
print(ft.view([-1, 3]).shape) # print(ft.view([-1, 3]).shape)
```


### Squeeze

```python
ft = torch.FloatTensor([[0], [1], [2]])
print(ft)
#tensor([[0.],
#        [1.],
#        [2.]])
print(ft.shape)
# torch.Size([3, 1])

print(ft.squeeze()) # tensor([0., 1., 2.])
print(ft.squeeze().shape) # torch.Size([3])
```

### unsqueeze

```python
ft = torch.Tensor([0, 1, 2])
print(ft.shape) #torch.Size([3])

print(ft.unsqueeze(0)) # tensor([[0., 1., 2.]])
print(ft.unsqueeze(0).shape) # torch.Size([1, 3])

print(ft.unsqueeze(1))
# tensor([[0.],
#        [1.],
#        [2.]])
print(ft.unsqueeze(1).shape) # torch.Size([3, 1])
```

### Scatter (for one-hot encoding)

```python
lt = torch.LongTensor([[0], [1], [2], [0]])
print(lt)
#tensor([[0],
#        [1],
#        [2],
#        [0]])

one_hot = torch.zeros(4, 3) # batch_size = 4, classes = 3
one_hot.scatter_(1, lt, 1)
print(one_hot)
#tensor([[1., 0., 0.],
#        [0., 1., 0.],
#        [0., 0., 1.],
#        [1., 0., 0.]])
```


### Concatenation
```python
x = torch.FloatTensor([[1, 2], [3, 4]])
y = torch.FloatTensor([[5, 6], [7, 8]])

print(torch.cat([x, y], dim=0))
#tensor([[1., 2.],
#        [3., 4.],
#        [5., 6.],
#        [7., 8.]])

print(torch.cat([x, y], dim=1))
# tensor([[1., 2., 5., 6.],
#        [3., 4., 7., 8.]])
```

### Stacking

```python
x = torch.FloatTensor([1, 4])
y = torch.FloatTensor([2, 5])
z = torch.FloatTensor([3, 6])

print(torch.stack([x, y, z]))
#tensor([[1., 4.],
#        [2., 5.],
#        [3., 6.]])

print(torch.stack([x, y, z], dim=1))
tensor([[1., 2., 3.],
        [4., 5., 6.]])
```

### In-place Operation

```python
x = torch.FloatTensor([[1, 2], [3, 4]])

print(x.mul(2.))
# tensor([[2., 4.],
#        [6., 8.]])
print(x)
# tensor([[1., 2.],
#        [3., 4.]])
```

### Zip
```python
for x, y in zip([1, 2, 3], [4, 5, 6]):
    print(x, y)
#1 4
#2 5
#3 6

for x, y, z in zip([1, 2, 3], [4, 5, 6], [7, 8, 9]):
    print(x, y, z)
#1 4 7
#2 5 8
#3 6 9
```
