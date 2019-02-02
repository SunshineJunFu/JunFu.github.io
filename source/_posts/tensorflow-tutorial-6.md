---
title: tensorflow_tutorial_6
date: 2019-02-02 22:43:55
tags: tensorflow
---

# 机器学习中的Loss

## 回归任务

### MAE，L1 Loss

$ L_{MAE} = \frac{1}{N}\sum_i^{N-1}|pred_i - label_i|$

### MSE，L2 Loss

$ L_{MSE} = \frac{1}{N}\sum_i^{N-1}(pred_i - label_i)^{2}$

## 分类任务

### CrossEntropy Loss

$$ loss(x,class)= -log(\frac{exp(x[class])}{\sum_{j}exp(x[j])}) = -x[class]+log(\sum_{j}exp(x[j])) $$

### one-hot 

+ 每个类别等距，更加合理，无需考虑排序问题。

### CrossEntropy

自信息量(Self-information)

定义： 变量x发生概率的对数取反

$$ I(x) = - log(p(x))$$

注：$ p(x) = Pr(X=x) $ represents the probability that message x is choosen from all possible choices in message space X.

作用： 表明该变量x携带的信息量的多少


熵

定义： 表明X的不确定程度

$$H(X) = E[I(X)]$$

对于离散的X信息空间:

$$H(X) = \sum(-p(x)log[p(x)]) $$

对于连续的X信息空间:

$$H(x) = integral(-p(x)log[p(x)]dx)$$ 

注: the bigger entropy means more uncertainty, more self-information. 

KL 散度 又叫相对熵：

作用：D(P||Q)表示当用概率分布Q来拟合真实分布P时，产生的信息损耗，其中P表示真实分布，Q表示P的拟合分布。

注：有人将KL散度称为KL距离，但事实上，KL散度并不满足距离的概念，应为:1）KL散度不是对称的；2）KL散度不满足三角不等式。

$$ Dkl(p||q) =  Ep[log{p(x)/q(x)}]$$

对于离散的X信息空间:

$$DKL(P|Q)=∑iP(i)logP(i)/Q(i)$$

对于连续的X信息空间:

$$DKL(P|Q)=∫∞−∞p(x)logp(x)/q(x)dx$$

Note: the smaller KL means that the fake distribution is more closer to 'true' probability distribution.

交叉熵:

定义：

$$ CEH(p,q) = Ep[-logq] = Ep[-logq/p * p] 
= Ep[-logp-logq/p] 
= Ep[-logp] + Ep[-logq/p]
= Ep[-logp] + Ep[logp/q] 
= H(p) + Dkl(p||q)$$

if distribution is regarded as "true" distribution, H(p) will be a constant. Hence, if we plan to minimize Cross entropy, it can be regarded as minimize KL divergence.

$$CEH(p,q) Loss= Dkl(p||q) $$

$$DKL(P|Q)=\sum_{i}P(i)log(\frac{P(i)}{Q(i)})$$

for binary discrete message space X:

CEH(p,q) Loss= -p(x)log[q(x)] - [1-p(x)]log[1-q(x)]

在one-hot的基础上：

$$DKL(P|Q) =\sum_{i}P(i)log(\frac{P(i)}{Q(i)}) 

		   =  1 * log(\frac{1}{Q(i)})

		   = - log(Q(i))

		   = - log(\frac{exp(x[class])}{\sum_{j}exp(x[j]))

		   = - x[class]+log(\sum_{j}exp(x[j]))
$$

## 推广至多label？
