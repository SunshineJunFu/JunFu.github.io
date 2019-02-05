---
title: tensorflow_tutorial_8
date: 2019-02-03 01:00:57
tags:
---

# 如何使用高低阶的API搭建网络

## 全连接

结构：`flatten`$\rightarrow$ `xw+b`$\rightarrow$ `norm layer`(optional)$\rightarrow$`activation function` $\rightarrow$`dropout`(optional)

```python

def add_fc_layer(inputs, in_dim, out_dim, scope_name, activation_function=None):

    with tf.variable_scope(scope_name):

        w = tf.get_variable(shape=[in_dim, out_dim], initializer=tf.contrib.layers.xavier_initializer(), name='weights')

        b = tf.get_variable(shape=[1, out_dim], initializer=tf.contrib.layers.xavier_initializer(), name='biases')

        wx_plus_b = tf.matmul(inputs, w) + b  # broadcast

        if activation_function:

        	wx_plus_b = activation_function(wx_plus_b)

    return wx_plus_b
```

## 卷积

结构： `convolution` $\rightarrow$ `norm layer`(optional) $\rightarrow$`activation function` $\rightarrow$`pooling`(optional)   

```python
def add_conv2d_layer(inputs, in_channel, out_channel, scope_name, activation_function, filter_h, filter_w,stride):

	with tf.variable_scope(scope_name):

		fliter_mask = tf.get_variable(shape=[filter_h, filter_w, in_channel, out_channel], dtype=inputs.dtype, initializer=tf.contrib.layers.xavier_initializer(),name='filter')

		# 'same' 输入输出的宽高一致， 'valid', out_width = xxxx #
		conv_inputs  = tf.nn.conv2d(inputs, fliter_mask, [1,stride,stride,1], 'SAME')

		# batch normalization #

		if activation_function:

			conv_inputs = activation_function(conv_inputs)

		# pooling #

		conv_inputs = tf.nn.max_pool(conv_inputs,[1,2,2,1],[1,2,2,1],'SAME')

	return conv_inputs

```
## 反卷积

结构： `inverse-convolution` $\rightarrow$ `norm layer`(optional) $\rightarrow$`activation function`     





