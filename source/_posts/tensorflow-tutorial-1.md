---
title: tensorflow_tutorial_1
date: 2019-01-30 15:50:15
tags: tensorflow
---

# Basic concepts

## Preface

机器学习中的大部分任务是去学习一个将输入$X$映射到标签$Y$的映射函数$f(X)$: $X \rightarrow Y$，由于在实际中，我们很难通过解析式求得$f(X)$的具体表达形式。因而，我们希望通过数值迭代的方式去求得一个近似函数$g(X;\theta)$并且使得$g(X)$尽可能地逼近$f(X)$: $g(X;\theta) \rightarrow f(X)$。

## Graph
![图1 graph的框架](tensors_flowing.gif)
`Tensorflow`将这一过程表征为`graph`的形式。graph的组成可以分成以下3部分：
+ 参数化层： `Input` $\rightarrow$ `Reshape` $\rightarrow$ `ReLu Layer` $\rightarrow$`ReLu Layer` $\rightarrow$ `softmax`
+ 逼近量化层： `Input` $\rightarrow$ `Class Labels`$\rightarrow$ `Cross Entropy`
+ 参数更新层： `Gradients` $\rightarrow$ `Logit Layer` $\rightarrow$ `ReLu Layer`

graph由`节点`和`边`组成，节点和边分别对应`操作(operations)`和`张量(Tensors)`。 操作(operations)包含:
+ 定义Tensor
+ 基于Tensor的运算($e.g.$ +-\*/)。

从动态图可知，graph中的Tensors flow(流动)分为两步:

+ forward propagation: `Input` $\rightarrow$ `Reshape` $\rightarrow$ `ReLu Layer` $\rightarrow$ `Logit Layer` $\rightarrow$ `softmax`

+ backpropagation: `Gradients` $\rightarrow$ `Logit Layer` $\rightarrow$ `ReLu Layer`

`forward propagation` 用于预测输入数据对应的label; `backpropagation` 用于更新网络参数$\theta$，使得$g(X)$尽可能地逼近$f(X)$。此外，从图中可以看出，为了方便区分operations，定义了作用域(scope), $e.g.$ `ReLu Layer`, `Logit Layer`。

```python
# 显示定义图：（默认是定义了一张图）
g = tf.Graph()
# 往图里添加operations
with g.as_default():
	c = tf.constant(30.0)
	assert c.graph == g
# 返回当前图
g = tf.get_default_graph()
# 查看当前图的operations
operations = g.get_operations() #list
```
## Tensors

张量是对矢量和矩阵向潜在的更高维度的泛化，TensorFlow 在内部将张量表示为基本数据类型的 n 维数。

Tensor的属性：

+ 数据类型(dtype): $e.g.$ `tf.float32`, `tf.int32`, `tf.string`
+ 形状(shape)
+ 名称(name)

Tensor的阶的定义

| rank| Description|
|:---:|:----------:|
|0	  | 标量		   |
|1	  | 一维数组	   |
|2	  | 二维数组	   |
|3	  | 三维数组	   |
|n	  | n维数组	   |
注:`rank = len(tensor.shape)=tf.rank(tensor)`

Tensor的分类

| kind| Description|
|:---:|:---:|
|tf.Variable | 用于存储graph的状态|
|tf.placeholder| 用于graph的输入 |
|tf.constant| 用于存常量 |
|tf.SparseTensor| 用于稀疏张量|

Tensor Auxiliary:
+ 改变形状，使用`tf.reshape(tensor, new_shape)`
+ 改变数据类型，使用`tf.cast(tensor，new_dtype)`
+ 获取Tensor的值，在某个Session下使用`tensor.eval()`

## Session

会话(Session)和图(graph)相互绑定的，graph只是定义了符号运算，但并未获得实际的物理运行环境，会话(Session)则为graph提供了可实际运行的上下文。因而，在一个会话下，才能进行数值运算。

```python
# create Session

tf.Session(target='',graph=None,config=None) #graph默认指定为当前默认图

# usage1：

define some operations

sess = tf.Session()

sess.run(ops, feed_dict={a: xxx})

sess.close()

# usage2:

define some operations

with tf.Session() as sess:

	sess.run(ops, feed_dict={a: xxx}) #feed_dict{xxx} 对应于该ops所需要的输入值

```


