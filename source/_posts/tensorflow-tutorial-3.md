---
title: tensorflow_tutorial_3
date: 2019-01-31 16:25:42
tags: tensorflow
---

# Optimizer

## Preface 

[Optimizer](https://www.tensorflow.org/api_docs/python/tf/train)的作用：
+ 根据loss定义，由链式法则从后往前推算出网络各层的梯度
+ 利用梯度值进行更新网络参数

## classification

<img src="optimizer.png" width="500" hegiht="313" align=center />

Optimizer 属于tf.train空间，其常见的子类有：

+ [AdagradOptimizer](https://www.tensorflow.org/api_docs/python/tf/train/AdagradOptimizer)
+ [AdamOptimizer](https://www.tensorflow.org/api_docs/python/tf/train/AdamOptimizer)
+ [MomentumOptimizer](https://www.tensorflow.org/api_docs/python/tf/train/MomentumOptimizer)
+ [RMSPropOptimizer](https://www.tensorflow.org/api_docs/python/tf/train/RMSPropOptimizer)
+ [GradientDescentOptimizer](https://www.tensorflow.org/api_docs/python/tf/train/GradientDescentOptimizer)

注：在定义Optimizer时，至少指定下learning_rate

## Function

Optimizer常用的3个函数：

+ compute_gradients
+ apply_gradients
+ minimize

### compute_gradients

```python
compute_gradients(
    loss,
    var_list=None,
    gate_gradients=GATE_OP,
    aggregation_method=None,
    colocate_gradients_with_ops=False,
    grad_loss=None
)
```
作用： 计算梯度

Args:

+ loss: 定义的损失函数
+ var_list: 指定计算梯度的变量，默认是键值为GraphKeys.TRAINABLE_VARIABLES的colle内的变量。

Return:

A list of (gradient, variable) pairs.

### apply_gradients

```python

apply_gradients(
    grads_and_vars,
    global_step=None,
    name=None
)
```
作用： 将梯度应用至变量。

Args： 

+ grads_and_vars：A list of (gradient, variable) pairs

### minimize

```python
minimize(
    loss,
    global_step=None,
    var_list=None,
    gate_gradients=GATE_OP,
    aggregation_method=None,
    colocate_gradients_with_ops=False,
    name=None,
    grad_loss=None
)
```

作用： 计算并将梯度应用至变量。

Args：

+ loss: 定义的损失函数。

For example

```python

import tensorflow as tf

inputs = tf.placeholder(dtype=tf.float32, shape=[], name="inputs")

w = tf.Variable(3.0, name='w')

y = inputs * tf.multiply(w, w) 

opt = tf.train.GradientDescentOptimizer(0.1)

grad_var = opt.compute_gradients(y)

train_op = opt.apply_gradients(grad_var)

init_opt = tf.global_variables_initializer()

with tf.Session() as sess:

	sess.run(init_opt)

	# y = w2 y|w = 2w

	print(sess.run(grad_var, feed_dict={inputs: 1.0})) ==> [(6.0, 3.0)] 变化最快的量是6

	print("梯度下降法更新前", w.eval()) ==> 3.0

	sess.run(train_op,  feed_dict={inputs: 1.0})

	print("梯度下降法更新后",w.eval()) ==> 2.4 : 梯度下降： w <- w + 0.1 * (-0.6)

```

注： [梯度](https://www.zhihu.com/question/36301367)

## To do

- [ ] 分析各种Optimizer[工作原理](https://arxiv.org/pdf/1609.04747.pdf)
- [ ] 给出如何选择Optimizer的指导性方法