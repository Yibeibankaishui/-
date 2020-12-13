#Pytorch 学习笔记

---

[toc]

---

##数据操作
####tensor创建
#####创建满足特定条件的tensor：

torch.empty()
torch.rand()
torch.randn()
torch.normal()
torch.uniform()
torch.randperm()
torch.zeros()
torch.ones()
torch.eye()
torch.arange()
torch.linspace()

#####从已有数据创建tensor：

torch.tensor()

#####从现有tensor创建：

torch.new_ones()
torch.randn_like()

*示例：*
```python
x = x.new_ones(5,3,dtype = torch.float64)
print(x)

x = torch.randn_like(x,dtype = torch.float)
print(x)
```
out:
```
tensor([[1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.],
        [1., 1., 1.]], dtype=torch.float64)
tensor([[-0.1602, -2.5196, -2.0136],
        [-0.7362, -1.4640, -0.7246],
        [ 1.4043,  0.9578, -1.0649],
        [ 0.1659, -0.4313, -1.2750],
        [-1.2840,  0.5682, -0.5221]])
```
####tensor算术运算
同一种运算有不同形式，以加法为例：
#####形式一：
```python
y = torch.rand(5,3)
print(x + y)
```
#####形式二：
```python
print(torch.add(x,y))

result = torch.empty(5,3)
torch.add(x,y,out = result)
print(result)
```
#####形式三：
inplace版本加法，**会改变操作对象的值**
```python
y.add_(x)
print(y)
```

####tensor索引
索引操作类似Numpy，**索引出来的结果与原数据共享内存，即，修改一个，另外一个也会被改变**

*示例：*

```python
y = x[0,:]
y += 1
print(y)
print(x[0,:])
```
out:

```
tensor([ 0.8398, -1.5196, -1.0136])
tensor([ 0.8398, -1.5196, -1.0136])
```

#####其他高级选择函数：
index_select(input,dim,index)
masked_select(input,mask)
non_zero(input)
gather(input,dim,index)
####tensor改变形状
可以用**view()**来改变tensor的形状，**view()返回的新tensor与源tensor共享内存，改变其中一个，另一个也被修改（即，view仅仅是改变了对这个张量的观察角度）**
如果不共享内存，可以使用**reshape()**，或者先用**clone()**创建一个副本再使用**view()**
*示例：*
```python
x_cp = x.clone().view(15)
x -= 1
print(x)
print(x_cp)
```
out:
```
tensor([[ 0.8398, -1.5196, -1.0136],
        [-0.7362, -1.4640, -0.7246],
        [ 1.4043,  0.9578, -1.0649],
        [ 0.1659, -0.4313, -1.2750],
        [-1.2840,  0.5682, -0.5221]])
tensor([ 1.8398, -0.5196, -0.0136,  0.2638, -0.4640,  0.2754,  2.4043,  1.9578,
        -0.0649,  1.1659,  0.5687, -0.2750, -0.2840,  1.5682,  0.4779])
```
####tensor线性代数运算

trace()
diag()
triu()/tril()
mm()/bmm()
addmm()
t
dot()/cross()
inverse()
svd()

####广播机制
>当两个形状不同的tensor按元素进行运算时，可能会触发**广播broadcasting**机制：**先适当复制元素使这两个tensor形状相同后再按元素运算**

```python
x = torch.arange(1,3).view(1,2)
print(x)
y = torch.arange(1,4).view(3,1)
print(y)
print(x + y)
```
output:
```
tensor([[1, 2]])
tensor([[1],
        [2],
        [3]])
tensor([[2, 3],
        [3, 4],
        [4, 5]])
```
####运算的内存开销
>

###tensor和numpy相互转换
numpy()
from_numpy()
torch.tensor()

###tensor on GPUs

使用 **to()** 方法让tensor在CPU和GPU之间相互移动

##自动求梯度


>.
.
.
.
.
.

##杂项
####pytorch中的dim

>在这些pytorch函数中，很多都有dim这个控制参数，但是我们很难明白这个含义是什么。
本文试着总结一下：
1）**dim的不同值表示不同维度**。特别的在dim=0表示二维中的行，dim=1在二维矩阵中表示列。
2）知道dim的值是什么意思还不行，还要**知道函数中这个dim给出来会发生什么**。


>**只有dim指定的维度是可变的，其他都是固定不变的。**
dim = 0，指定的是行，那就是列不变，理解成：同一列中每一行之间的比较或者操作，是每一行的比较，因为行是可变的。

**不同函数中dim意义不同**

[Pytorch笔记：维度dim的定义及其理解使用](https://blog.csdn.net/qq_41375609/article/details/106078474)

#####sum()
```python
X = torch.tensor([[1, 2, 3], [4, 5, 6]])
print(X.sum(dim=0, keepdim=True))
print(X.sum(dim=1, keepdim=True))
```
output:
```
tensor([[5, 7, 9]])
torch.Size([2, 1])
```
*sum()中，dim=0表示按列对每行求和，行维度消失；行dim=1表示按行求和列维度消失*
#####argmax()
```python
import torch
import os
import numpy as np
# os.environ['CUDA_VISIBLE_DEVICES'] = '1'

a = torch.rand((3,4))

print(a.size())
print(a)

b = torch.argmax(a, dim=1)
print(b)
print(b.shape)
```
output:
```
torch.Size([3, 4])
tensor([[0.7773, 0.6262, 0.6627, 0.0417],
        [0.6541, 0.7286, 0.8185, 0.8624],
        [0.2223, 0.3188, 0.3368, 0.5666]])
tensor([0, 3, 3])
torch.Size([3])
```
*argmax()中，dim表示该维度会消失*
####pytorch gather