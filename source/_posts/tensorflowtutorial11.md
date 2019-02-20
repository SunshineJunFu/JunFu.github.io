---
title: tensorflowtutorial11
date: 2019-02-20 21:01:22
tags: tensorflow
---

# tf.estimator.RunConfig()

函数：

replace(**kwargs)

返回新的RunConfig

以下可以被修改：

+ model_dir,
+ tf_random_seed,
+ save_summary_steps,
+ save_checkpoints_steps,
+ save_checkpoints_secs,
+ session_config,
+ keep_checkpoint_max,
+ keep_checkpoint_every_n_hours,
+ log_step_count_steps,
+ train_distribute,
+ device_fn,
+ protocol.
+ eval_distribute,
+ experimental_distribute, 

```python

config = tf.estimator.RunConfig().replace(
        session_config=tf.ConfigProto(device_count={'CPU':self._num_threads}),
        log_step_count_steps=20)

```

tf.ConfigProto()

配置seseion参数

...

# tf.estimator.Estimator

创建Estimator

```python
__init__(
    model_fn,
    model_dir=None,
    config=None,
    params=None,
    warm_start_from=None
)
```
Args:

+ model_fn: 模型函数
	
	(features, labels, mode, params, config) or (features, labels, mode, params)

	+ features: This is the first item returned from the input_fn passed to train, evaluate, and predict. This should be a single tf.Tensor or dict of same.
	+ labels: This is the second item returned from the input_fn passed to train, evaluate, and predict. 
	+ mode: Optional. Specifies if this training, evaluation or prediction. See tf.estimator.ModeKeys
	+ params: Optional dict of hyperparameters. Will receive what is passed to Estimator in params parameter. This allows to configure Estimators from hyper parameter tuning
	+ config: Optional estimator.RunConfig object. Will receive what is passed to Estimator as its config parameter, or a default value. Allows setting up things in your model_fn based on configuration such as num_ps_replicas, or model_dir

	return: tf.estimator.EstimatorSpec()

+ model_dir: 保存模型的路径

+ config： estimator.RunConfig configuration object.

+ params： dict of hyper parameters that will be passed into model_fn. Keys are names of parameters, values are basic python types.

Returns:

an Estimator instance

# tf.estimator.EstimatorSpec()
__new__(
    cls,
    mode,
    predictions=None,
    loss=None,
    train_op=None,
    eval_metric_ops=None,
    export_outputs=None,
    training_chief_hooks=None,
    training_hooks=None,
    scaffold=None,
    evaluation_hooks=None,
    prediction_hooks=None
)


Depending on the value of mode, different arguments are required. Namely

+ For mode == ModeKeys.TRAIN: required fields are loss and train_op.
+ For mode == ModeKeys.EVAL: required field is loss.
+ For mode == ModeKeys.PREDICT: required fields are predictions.

# tf.estimator.TrainSpec

创建训练的TrainSpec实例

```python
@staticmethod
__new__(
    cls,
    input_fn,
    max_steps=None,
    hooks=None
)
```
Args:
+ input_fn: 提供minibatches的输入数据的函数。函数的返回值结构如下：
	+ a tf.data.Dataset 对象
	+ (features, labels): features 是tensor或者dict{name： tensor}
+ max_steps: int, 训练的最大步数，未设置，则永远训练
+ hooks： 钩子函数

Returns：

TrainSpec实例

# tf.estimator.EvalSpec

创建验证的EvalSpec实例

```python
@staticmethod
__new__(
    cls,
    input_fn,
    steps=100,
    name=None,
    hooks=None,
    exporters=None,
    start_delay_secs=120,
    throttle_secs=600
)
```

Args:
+ input_fn: 提供minibatches的输入数据的函数。函数的返回值结构如下：
	+ a tf.data.Dataset 对象
	+ (features, labels): features 是tensor或者dict{name： tensor}
+ steps: None,则在整个验证集验证

Returns：

EvalSpec实例

# tf.estimator.train_and_evaluate

函数： Train and evaluate the estimator 

```python

tf.estimator.train_and_evaluate(
    estimator,
    train_spec,
    eval_spec
)

```
Args:

+ estimator:  estimator实例

+ train_spec：  train_spec实例

+ eval_spec： eval_spec实例

#  tf.nn.embedding_lookup