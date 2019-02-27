---
title: tensorflow_tutorial_7
date: 2019-02-03 00:59:00
tags: tensorflow
---

# 分类任务里的衡量指标

## 二分类

### 混淆矩阵

| |predicted 1| predicted 0|
|:---|:---:|:---:|
|label 1|TP|FN|
|label 0|FP|TN|

+ TP：label为正例，预测为正例
+ FP：label为负例，预测为正例
+ TN：label为正例，预测为正例
+ FN：label为正例，预测为负例


精度(Accuracy):
\begin{align}
Accuracy &=\frac{预测正确样本个数}{总样本数} \\\\
&=\frac{TP+TN}{TP+FP+TN+FN} 
\end{align}

查全率(Recall): 预测正确的正例个数与总的正例的比值。

$$Recall=\frac{TP}{TP+FN}$$

查准率(Precision)：预测正确的正例个数与预测为正例个数的比值。

$$Precision=\frac{TP}{TP+FP}$$

性质: Recall 和 Precision 是一对矛盾的指标

证明： 

假定`Recall` $\uparrow$, 由于$TP+FP=数据集内的正样本数=常数$ $\Rightarrow$ `TP`$\uparrow$, 由于`TP`$\uparrow$,为了提高`TP`，会同时增加`FP`且`FP+TP`的增量大于`TP`的增量(直觉上，为了提高`TP`，分类器会偏向于将分类为正类，这样也增加了`FP`)，所以`Precision`$\downarrow$。

假定`Recall` $\downarrow$, 由于$TP+FP=数据集内的正样本数=常数$ $\Rightarrow$ `TP`$\downarrow$, 由于`TP`$\downarrow$,为了减小`TP`，会同时减小`FP`且`FP+TP`的减小量大于`TP`的减小量(直觉上，为了减小`TP`，分类器会偏向于将分类为负类，这样也减小了`FP`)，所以`Precision`$\uparrow$。

所以：

+ `Recall` $\uparrow$ $\Rightarrow$ `TP`$\uparrow$，`FP+TP`$\uparrow$ $\Rightarrow$ `Precision` $\downarrow$
+ `Recall` $\downarrow$ $\Rightarrow$ `TP`$\downarrow$，`FP+TP`$\downarrow$ $\Rightarrow$ `Precision` $\uparrow$

有的模型是将预测结果与一个阈值进行比较，如果大于该阈值则预测为正例，否则为反例。所以这个阈值非常重要，直接决定了模型的泛化能力。同时根据这个阈值我们可以将样本进行排序，“最可能”是正例的排在最前面，“最不可能”是正例的排在最后面。因此，如果我们想让precision更高，可以将阈值设置在排序靠前的位置，相反如果想让recall更高，可以将阈值设置在排序靠后的位置。

F1 Measure/Score

为了更准确反映模型的性能，引入了F1 score, 该指标同时考虑了`Recall`和`Precision`， 能让我们表达出对查准率/查全率的不同偏好。

$$F_{\alpha}=(1+\alpha^{2})\frac{Precision\*Recall}{\alpha^{2}\*Precision+Recall}$$

当\alpha > 1 时查全率有更大影响，当\alpha < 1 时查准率有更大影响。

当$\alpha=1$, 即为F1-score，`Recall`和`Precision`的调和平均数。
$$F_{1}=(2)\frac{Precision\*Recall}{Precision+Recall}$$

## 多label二分类

### Macro-f1-score
(宏观角度)先在每个confusion matrix上计算precision和recall，最后计算平均值。

$$ Precision_{macro} = \frac{1}{N}\sum_{i}Precision_{i} $$
$$ Recall_{macro} = \frac{1}{N}\sum_{i}Recall_{i} $$
$$ F1_{macro}=(2)\frac{Precision_{macro}\*Recall_{macro}}{Precision_{macro}+Recall_{macro}}$$

### Micro-f1-score
(微观角度)先将每个confusion matrix中各个对应位置上的元素平均，得到TP、FP、TN、FN的平均值，然后计算precision、recall和F1
$$Recall_{micro}=\frac{\overline{TP}}{\overline{TP}+\overline{FN}}$$
$$Precision_{micro}=\frac{\overline{TP}}{\overline{TP}+\overline{FP}}$$
$$F1_{micro}=(2)\frac{Precision_{micro}\*Recall_{micro}}{Precision_{micro}+Recall_{micro}}$$

### ROC和AUC(Area under curve)

每次预测一个样本后计算当下的两个值，最后用这两个值作为坐标画曲线。这两个值分别是真正例率（True Positive Rate）和假正例率（False Positive Rate），定义如下：
$$ TPR = \frac{TP}{TP+FN}$$
$$ FPR = \frac{FP}{TN+FP}$$

AUC越大越好，考虑了所有阈值。

ROC曲线的横坐标为false positive rate（FPR）即负类样本中被判定为正类的比例，也就是传说中的误纳率

   

纵坐标为true positive rate（TPR）即正类样本中被判定为正类的样本，1-TPR也就是传说中的误拒率

   

接下来我们考虑ROC曲线图中的四个点和一条线。

   

第一个点，(0,1)，即左上角的点，在这个点意味着FPR=0，TPR=1，稍微翻译一下就是误纳率为0，误拒率为0，再翻译成人话就是负类样本中被判断为正类的比例为0，说明负类样本都被判断为负类，判断正确，正类样本中被判断为正类的比例为1，说明正类样本都被判断正确，所以这是一个完美的分类器，它将所有的样本都正确分类。

   

第二个点，(1,0)，即右下角的点，在这个点意味着FPR=1，TPR=0，类似地分析可以发现这是一个最糟糕的分类器，因为它成功避开了所有的正确分类。把该判断为正类的判断为负类，把该判断为负类的判断为正类

   

第三个点，(0,0)，即左下角的点，在这个点意味着FPR=TPR=0，可以发现该分类器预测所有的样本都为负样本（negative），在后面我们可以看到这种情况说明阈值选得过高。

   

第四个点（1,1），即右下角的点，分类器实际上预测所有的样本都为正样本，在后面我们可以看到这种情况说明阈值选得过低。


对角线： 

如果是随机分类，则有$Tp=Fp$，$Tn=Fn$，$\Rightarrow$ $FPR=TPR$

好的分类器：$Tp>Fp$，$Tn>Fn$,，$\Rightarrow$ $TPR=\frac{TP\uparrow}{TP\uparrow+FN\downarrow}>TPR=\frac{FP\downarrow}{FP\downarrow+TN\uparrow}$，位于对角线右侧

差的分类器：$Tp<Fp$，$Tn<Fn$,，$\Rightarrow$ $TPR=\frac{TP\downarrow}{TP\downarrow+FN\uparrow}>TPR=\frac{FP\uparrow}{FP\uparrow+TN\downarrow}$，位于对角线右侧


# AUC = 0 

说明样本中没有正例



# 如何画ROC曲线

   

由于每次从分类模型中只能得到一个用于判定分类结果的分数，要将这个分数与一个阈值进行比较，判定这个样本属于哪个类，所以我们可以更改阈值，得到不同的分类结果，也就是不同的TPR和FPR

   

之前说到当我们将threshold设置为1和0时，分别可以得到ROC曲线上的(0,0)和(1,1)两个点

   

将这些(FPR,TPR)对连接起来，就得到了ROC曲线。当threshold取值越多，ROC曲线越平滑。

   

既然有了ACC为什么要有ROC呢(既生瑜何生亮呢)

   

我们知道，我们常用ACC准确率来判断分类器分类结果的好坏，既然有了ACC为什么还需要ROC呢，很重要的一个因素是实际的样本数据集中经常会出现数据偏斜的情况，要么负类样本数大于正类样本数，要么正类样本数大于负类样本数。

   

比如说我负类样本数有9,000个，正类样本数有100个，如果阈值选得过高，正类样本都判断为负类，同样负类样本都判定为负类，那么准确率90%，看起来还不错，但是如果考虑ROC中的TPR和FPR的话就会知道，此时的TPR=0，FPR=0，也就是误纳率是0，但是误拒率是100%，是左下角的点，并不是很好的一个点，而原来的ACC就不具有代表性

   

既然有了ROC为什么要有AUC呢(既生瑜何生亮呢)

   

使用AUC值作为评价标准是因为很多时候ROC曲线并不能清晰的说明哪个分类器的效果更好，而相对于AUC是个数值而言，对应AUC更大的分类器效果更好，数值更好判断一些。


# References

https://zhuanlan.zhihu.com/p/39957290

http://www.cnblogs.com/zle1992/p/6689136.html

https://www.zhihu.com/question/39840928

http://www.cnblogs.com/keedor/p/4463988.html

https://www.cnblogs.com/keedor/category/669162.html