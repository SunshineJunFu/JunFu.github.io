---
title: tensorflow_tutorial_5
date: 2019-01-31 16:25:52
tags: tensorflow
---

# [Summary](https://www.tensorflow.org/api_docs/python/tf/summary?hl=en)

## class tf.Summary.FileWriter

### __init__

创建一个writer

```python
__init__(
    logdir,
    graph=None,
    max_queue=10,
    flush_secs=120,
    graph_def=None,
    filename_suffix=None,
    session=None
)
```
### add_summary

将summary string添加至事件文件中

```python
add_summary(
    summary,
    global_step=None
)
```

### flush

将事件文件写到磁盘
```python
flush()
```
### close

将事件文件写到磁盘并关闭FileWriter。

```python
close()
```

## visulization Function

### scalar

```python
tf.summary.scalar(
    name,
    tensor,
    collections=None,
    family=None
)
```
Args:

+ name: 显示的名字

+ family： 同一族

+ Return： 一个`operation`

### image

```python
tf.summary.image(
    name,
    tensor,
    max_outputs=3,
    collections=None,
    family=None
)
```

Args:

+ name: 显示的名字

+ tensor： shape [batch_size, height, width, channels]， channels可以是1,3,4，分别对应于Grayscale，RGB，RGBA。

### histogram

```python

tf.summary.histogram(
    name,
    values,
    collections=None,
    family=None
)

```
+ name: 显示的名称

+ values： 处理的数据，可以是任意形状

### text

```python

tf.summary.text(
    name,
    tensor,
    collections=None
)
```
+ tensor: a string-type Tensor to summarize.

### [audio](https://www.tensorflow.org/api_docs/python/tf/summary/audio?hl=en)


## 合并可视化操作

目的: 使得1次forward propagation，获取多个summary

### tf.summary.merge

```python

tf.summary.merge(
    inputs,
    collections=None,
    name=None
)

```

+ inputs:  A list of string Tensor objects containing serialized Summary protocol buffers.

### tf.summary.merge_all

```python
tf.summary.merge_all(
    key=tf.GraphKeys.SUMMARIES,
    scope=None,
    name=None
)
```

Merges all summaries collected in the default graph.

Args:
+ key: GraphKey used to collect the summaries. Defaults to GraphKeys.SUMMARIES.
+ scope: Optional scope used to filter the summary ops, using re.match

Return: 返回一个ops

## Conclusion

可视化主要流程：

+ 预定义Summary objects

+ 合并Summary objects

+ 启动Session

+ 定义tf.summary.FileWriter

+ 获取summary string ： sess.run(summary_ops， feed_dict={})

+ 添加至事件文件

+ 写入磁盘

For example

```python

import tensorflow as tf

var = tf.Variable(tf.zeros([], name='var'))

scalar_op = tf.Summary.scalar('var', var)

merge_op = tf.Summary.merge_all()

init_op = tf.global_variables_initializer()

with tf.Session() as sess:

	writer = tf.Summary.FileWriter('./logs')

	sess.run(init_op)

	summary_string = sess.run(merge_op)

	writer.add_summary(summary_string, global_step=0)

	writer.flush()

```

