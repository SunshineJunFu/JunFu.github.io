---
title: tensorflow_tutorial_4
date: 2019-01-31 16:25:48
tags: tensorflow
---

# class [saver](https://www.tensorflow.org/api_docs/python/tf/train/Saver)

作用：用于保存和恢复网络的参数

## Function

### Init

```python

__init__(
    var_list=None,
    reshape=False,
    sharded=False,
    max_to_keep=5,
    keep_checkpoint_every_n_hours=10000.0,
    name=None,
    restore_sequentially=False,
    saver_def=None,
    builder=None,
    defer_build=False,
    allow_empty=False,
    write_version=tf.train.SaverDef.V2,
    pad_step_number=False,
    save_relative_paths=False,
    filename=None
)

```
Args:

var_list: 指定保存的变量。可以使list或者dict

max_to_keep: 最近可以保存ckeckpoints的最大数目

keep_checkpoint_every_n_hours： 多久保存一次ckeckpoints
```python


v1 = tf.Variable(..., name='v1')
v2 = tf.Variable(..., name='v2')

# Pass the variables as a dict:
saver = tf.train.Saver({'v1': v1, 'v2': v2})

# Or pass them as a list.
saver = tf.train.Saver([v1, v2])
# Passing a list is equivalent to passing a dict with the variable op names
# as keys:
saver = tf.train.Saver({v.op.name: v for v in [v1, v2]})
```
## save

```python
save(
    sess,
    save_path,
    global_step=None,
    latest_filename=None,
    meta_graph_suffix='meta',
    write_meta_graph=True,
    write_state=True,
    strip_default_attrs=False
)
```
作用：保存网络参数

Args:
sess: 会话句柄
save_path：保存路径
global_step：步数

## restore

作用：恢复网络参数

```python
restore(
    sess,
    save_path
)
```
Args:
sess: 会话句柄
save_path：保存路径

## Example

```python
# Create a saver.
saver = tf.train.Saver(...variables...)
# Launch the graph and train, saving the model every 1,000 steps.
sess = tf.Session()
for step in xrange(1000000):
    sess.run(..training_op..)
    if step % 1000 == 0:
        # Append the step number to the checkpoint name:
        saver.save(sess, 'my-model', global_step=step)
```

## [查看保存文件里的tensor](https://www.jianshu.com/p/27d7955da2f6)

```python
from tensorflow.python import pywrap_tensorflow

# 从检查点文件中读取数据
reader = pywrap_tensorflow.NewCheckpointReader(checkpoint_path)
var_to_shape_map = reader.get_variable_to_shape_map()
# 显示变量名及其值
for key in var_to_shape_map:
    print("tensor_name: ", key)
    print(reader.get_tensor(key))
```
保存文件：

+ .meta文件保存了当前图结构
+ .index文件保存了当前参数名
+ .data文件保存了当前参数值

注：每调用一次save方法会产生新的文件

## To do

- [ ] transfer learning 里如何恢复部分模型参数




