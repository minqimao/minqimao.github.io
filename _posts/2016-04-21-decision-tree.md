---
layout: post
title: 数据分类：决策树
categories: [机器学习]
tags: 决策树
---

## 背景

$$
\begin{equation}
   P(X = x_i) = p_i, i = 1, 2, ...,n
\end{equation}
$$

则随机变量$X$的熵定义为：

$$
\begin{aligned}
H(X) = - \sum_{i=1}^{n}p_ilogp_i
\end{aligned}
$$

若$p_i=0$，则定义$0log0=0$，上式中的对数底数通常以2为底或以e为底，这是熵的单位分别称为比特(bit)或纳特(nat)。由上式熵的定义式可知，熵只依赖于$X$的分布，而与$X$的取值无关，所以也可将$X$的熵$H(X)$记为$H(p)$。同时，从定义可以验证，$0<=H(p)<=logn$。当随机变量只取两个值，比如0、1时，即$X$服从0-1分布时，熵为：

$$
\begin{aligned}
H(p)=-plog_2p-(1-p)log_2(1-p)
\end{aligned}
$$

我们可以用pyhon画出熵$H(x)$随概率$p$变化的曲线：

```python
import numpy as np

p = np.linspace(0.,1.,11)
print p

Hp = -p*np.log2(p)-(1-p)*np.log2(1-p)
Hp[0] = Hp[-1] = 0
print Hp

%matplotlib inline
import matplotlib
from pylab import *
import matplotlib.pyplot as plt

fig = plt.figure()
ax = fig.gca()
ax.set_xticks(p)
plt.plot(p,Hp)
grid(True)
plt.xlabel('$p$')
plt.ylabel('$H(p)$')

plt.show()
```

运行上面代码，可以得到下图：
![](http://i300.photobucket.com/albums/nn17/willard-yuan/download_zps9lv2ptzd.png)
可以看到，但$p=1-p=0.5$时，即取0或1的概率都是0.5时，熵最大，随机变量不确定性最大。

在了解了信息熵的定义后，我们半手工计算一下前面海洋生物数据集上的信息熵：

```python
marineEnt = -0.4*np.log2(0.4)-0.6*np.log2(0.6)
print "the entropy of the marine organism: %.20f" % marineEnt
# the entropy of the marine organism: 0.97095059445466858072
```
上面显示在海洋生物数据集上的信息熵为0.97095059445466858072。下面对于对计算信息熵进行完整的编码，使得其能够更方便计算某个数据集的信息熵，具体如下：

```python
def creatData():
    dataSet = [[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]
    labels = ['no surfacing', 'flippers']
    return dataSet, labels

myData, labels = creatData()
print myData
# [[1, 1, 'yes'], [1, 1, 'yes'], [1, 0, 'no'], [0, 1, 'no'], [0, 1, 'no']]

from math import log

def calShannonEnt(dataSet):
    dataSet = myData
    numEntries = len(dataSet)
    print "the number of marine organism: %d" % numEntries
    labelCounts = {}
    for featVec in dataSet:
        currentLabel = featVec[-1]  #获取当前样本的类别标签，即"yes"或“no”
        if currentLabel not in labelCounts.keys():
            labelCounts[currentLabel] = 0  #如果当前样本的类别在labelCounts没有记录，则常见该类别的记录
        labelCounts[currentLabel] += 1
    print "label counts:"
    print labelCounts
    shannonEnt = 0.0
    for key in labelCounts:
        prob = float(labelCounts[key])/numEntries
        shannonEnt -= prob*math.log(prob, 2.0)
    return shannonEnt

print "the entropy of the marine organism: %.20f" % calShannonEnt(myData)

#the number of marine organism: 5
#label counts:
#{'yes': 2, 'no': 3}
#the entropy of the marine organism: 0.97095059445466858072
```

### 信息增益

$$
\begin{aligned}
H(Y|X) = \sum_{i=1}^np_iH(Y|X=x_i)
\end{aligned}
$$

$$
\begin{aligned}
g(D,A_2) & = H(D) - \left(\frac{4}{5}H(D_1) + \frac{1}{5}H(D_1)\right) \\\\
& = H(D) - \left(\frac{4}{5}(-\frac{2}{4}log\frac{2}{4}  -\frac{2}{4}log\frac{2}{4})\right)\\\\
& = H(D) + \frac{4}{5}log_2\frac{1}{2}\\\\
& = H(D) - 0.8
\end{aligned}
$$

```python
gA1 = HD+0.4*np.log2(2.0/3.0)+0.2*np.log2(1.0/3.0)
print "g(D, A1): %f" %gA1
gA2 = HD-0.8
print "g(D, A2): %f" %gA2
#g(D, A1): 0.419973
#g(D, A2): 0.170951
```

| 海洋生物样本 | 不浮出水面是否可以生存 | 是否有脚蹼 | 属于鱼类 |
|:-----------:|:------------------:| :--------:|:--------:|
|      1     |        1           |      1    |    yes   |
|      2     |        1           |      1    |    yes   |
|      3     |        1           |      0    |    no    |
|      4     |        0           |      1    |    no    |
|      5     |        0           |      1    |    no    |

下面是按照给定特征划分数据集的代码：

```bash
the number of marine organism: 5
label counts:
{'yes': 2, 'no': 3}
Base entropy: 0.970951

==========feature candidate=========
[1, 1, 1, 0, 0]

sub-dataset:
[[1, 'no'], [1, 'no']]

the number of marine organism: 2
label counts:
{'no': 2}
sub-dataset:
[[1, 'yes'], [1, 'yes'], [0, 'no']]

the number of marine organism: 3
label counts:
{'yes': 2, 'no': 1}
information gain: 0.41997320
==========feature candidate=========
[1, 1, 0, 1, 1]

sub-dataset:
[[1, 'no']]

the number of marine organism: 1
label counts:
{'no': 1}
sub-dataset:
[[1, 'yes'], [1, 'yes'], [0, 'no'], [0, 'no']]

the number of marine organism: 4
label counts:
{'yes': 2, 'no': 2}
information gain: 0.17095120

The selected best feature: 0
```
