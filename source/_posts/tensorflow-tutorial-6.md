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


# 推导CrossEntropy Loss

## one-hot 

定义：用N个二进制位来表示N个状态，对于任意一状态，其N个二进制位，只有1位非零。例如， 0，1,2 分别会表示为001,010,100。

为什么用one-hot？

![Iris 数据集](1.jpeg)

以Iris数据集为例，数据集中共有3种花：Versicolor,Setosa,Virginica。如果直接按顺序给这3种花编号，并将编号作为每种花的label，会存在以下问题：
+ 欧式距离问题：如果将Versicolor,Setosa,Virginica的label采用编号的方式则是：1,2,3。则$L_{Ver,Setosa}=1$,$L_{Ver,Vir}=2$,$L_{vir,Setosa}=1$,可以发现这三者两两的距离不一致。如果将label编号为001,010,100，则两两之间的距离为$L_{Ver,Setosa}=2$。因而，这样做显得更加合理。

## 交叉熵(CrossEntropy)

自信息量(Self-information)

定义： 变量x发生概率的对数取反，表明该变量x携带的信息量的多少。

$$ I(x) = - log(p(x))$$

其中，$ p(x) = Pr(X=x) $ 代表了在信息空间X中x出现的的概率。
 

熵

定义： 表明X的不确定程度，熵越大，不确定程度越大，越多的信息量。

$$H(X) = E[I(X)]$$

对于离散的X信息空间:

$$H(X) = \sum_{i} (-p(x_i)logp(x_i)) $$

对于连续的X信息空间:

\begin{align} 
H(x) &= \int_{-\infty}^{+\infty} (p(x)I(x)dx) \\\\
 &= \int_{-\infty}^{+\infty}(-p(x)logp(x)dx)
\end{align}  

KL 散度 又叫相对熵：

定义：D(P||Q)表示当用概率分布Q来拟合真实分布P时，产生的信息损耗，其中P表示真实分布，Q表示P的拟合分布。散度越小，意味着Q越接近于P。           

Noted that：有人将KL散度称为KL距离，但事实上，KL散度并不满足距离的概念
+ KL散度不是对称的
+ KL散度不满足三角不等式。

$$ D(p||q) =  E_{~p}[log{p(x)/q(x)}]$$

对于离散的X信息空间:

$$D(P||Q)=\sum_{i} p(i)logp(i)/q(i)$$

对于连续的X信息空间:

$$D(P||Q)=\int_{-\infty}^{+\infty} p(x)logp(x)/q(x)dx$$

交叉熵: 

定义：$ CEH(p,q) = Ep[-logq]$

性质： 交叉熵 = 熵 + KL散度

证明： 

\begin{align} 
CEH(p,q) &=  E_{~p}[-logq] \\\\
&= E_{~p}[-logq/p * p] \\\\
&= E_{~p}[-logp-logq/p] \\\\
&= E_{~p}[-logp] + E_{~p}[-logq/p] \\\\
&= E_{~p}[-logp] + E_{~p}[logp/q] \\\\
&= H(p) + D(p||q)
\end{align}

由此可以发现，当p为真实分布时，其熵$H(p)$是固定不变的。因此，最小化交叉熵，可以被认为是最小化KL散度。            


## 交叉熵的损失公式推导

\begin{align} 
L_{CEH(p,q)} &= Dkl(p||q) \\\\
&= \sum_{i}p(i)log(\frac{p(i)}{q(i)}) 
\end{align}

对于二分类问题：

\begin{align} 
L_{CEH(p,q)} &= Dkl(p||q) \\\\
&= -p(0)logq(0) - p(1)log(q(1)) \\\\
&= -p(0)logq(0) - [1-p(0)]log[1-q(0)]
\end{align}      

对于n分类：

神经网络的输出层是n维的向量，x[i]为类别i的取值，那么类别为i的概率为：

$$q(i)= \frac{exp(x[i])}{\sum_{j}exp(x[j])} $$

这一过程叫做归一化，学名Softmax。

根据前面所述的one-hot可知，对于单个样本，真实分布p是只在类别正确处有值且为1，所以



\begin{align} 
L_{CEH(p,q)} &= Dkl(p||q) \\\\
&= \sum_{i}p(i)log(\frac{p(i)}{q(i)}) \\\\
&= 0 +  1 * log(\frac{1}{q(i)}) \\\\
&= - log(q(i)) \\\\
&= - log(\frac{exp(x[class])}{\sum_{j}exp(x[j]))} \\\\
&= - x[class]+log(\sum_{j}exp(x[j])) 
\end{align} 
 
## 推广至多label
 