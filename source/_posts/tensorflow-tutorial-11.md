---
title: tensorflow-tutorial-11
date: 2019-02-11 10:53:52
tags: tensorflow
---

# 导入数据

## tf.data.Dataset

### 属性

|属性|描述|
|---|---|
|output_classes|返回数据集中每个元素的每个组成部分的类名|
|output_shapes|返回数据集中每个元素的每个组成部分的形状|
|output_types|返回数据集中每个元素的每个组成部分的数据类型|

### 方法

#### 创建数据集

|方法|描述|
|---|---|
|from_tensor_slices(tensors)|建立给定tensors的数据集，多个元素，该数据集嵌入在图中，适用于小数据集（小于1G),更大的数据集[操作](https://www.tensorflow.org/guide/datasets#consuming_numpy_arrays)|
|from_tensors(tensors)|同上，但针对单个元素|

#### 基于数据集上的变换

|方法|描述|
|---|---|
|map(map_func,num_parallel_calls=None)|对数据集中的每个元素执行map_func操作|
|shuffle(buffer_size,seed=None,reshuffle_each_iteration=None)|随机抽取元素|
|batch(batch_size,drop_remainder=False)|设置batch的大小|
|repeat(count=None)|将数据集重复几次，默认为无限循环|
|zip(datasets)|datasets 元组，将多个数据集合并|

#### 创建迭代器

|方法|描述|
|---|---|
|make_initializable_iterator(shared_name=None)|创建一个未初始化的iterator|
|make_one_shot_iterator()|创建一个已经自动初始化的iterator， 每次只返回单个元素|


## tf.data.Iterator

### 属性

|属性|描述|
|---|---|
|initializer|返回初始化迭代器的操作|
|output_classes|返回数据集中每个元素的每个组成部分的类名|
|output_shapes|返回数据集中每个元素的每个组成部分的形状|
|output_types|返回数据集中每个元素的每个组成部分的数据类型|

### 方法

#### 创建迭代器

|方法|描述|
|---|---|
|from_string_handle(string_handle,output_types)|基于给定的句柄创建一个未初始化的迭代器|
|from_structure(output_types)|基于给定的结构创建一个未初始化的迭代器|

注： from_structure可以不同的数据集进行复用（reusable，Iterator.make_initializer(dataset)重新初始化，from_string_handle "feedable" iterator 。

区别：


#### 常用方法

|方法|描述|
|---|---|
|get_next(name=None)|返回下一个数据集中的元素|
|make_initializer(dataset,name=None)|返回通过指定的dataset初始化iterator的操作|
|string_handle(name=None)|返回代表迭代器的string|

注：make_initializer 与 from_structure 连用， string_handle与from_string_handle连用。get_next抵达数据集最后一个元素会有`tf.errors.OutOfRangeError错误`。


# References

1. [tf.data.Dataset](https://tensorflow.google.cn/api_docs/python/tf/data/Dataset#shuffle)
2. [tf.data.Iterator](https://www.tensorflow.org/api_docs/python/tf/data/Iterator)




