---
title: tensorboard_install
date: 2019-02-27 11:19:31
tags: tensorflow
---

# 安装Tensorflow-gpu

+ 查看cuda版本: `cat /usr/local/cuda/version.txt`

+ 查看cudnn版本: `cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2 `

+ 在[该网站](https://www.tensorflow.org/install/source#linux)上，根据cuda和cudnn版本选择合适的tensorflow-gpu版本

+ 在[该网站](https://pypi.tuna.tsinghua.edu.cn/simple/tensorflow-gpu/)上下载对应版本的whl文件

+ 执行`pip install xxx.whl`, 即可

+ 验证安装成功：
    
    + import tensorflow a tf

    + tf.Session() 

    + 上述语句无报错，则表示安装无误