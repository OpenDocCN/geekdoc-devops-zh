# 金丝雀部署的时间序列机器学习模型

> 原文：<https://www.harness.io/blog/time-series-machine-learning-model-for-canary-deployments>

您是否希望了解如何展示用于金丝雀分析的时间序列机器学习模型？点击此处查看该过程的分解。

## 问题陈述

展示用于金丝雀分析的时间序列机器学习模型。

## 模型

给定两个等长的时间序列(例如金丝雀相位)并以相同的频率采样，检测以下内容:

*   如果时间序列模式相似并且值在可接受的偏差范围内，则它们是**相似的**。
*   如果模式不同，或者数值超出可接受的偏差范围，则它们**不相似**。可接受的偏差范围是模型从训练数据中推断出来的。

## 资料组

我们生成一个合成数据集，其灵感来自 UCI 合成控制数据集(见以下截图)，该数据集通常用于学术界的时间序列模型验证。

该数据集中的时间序列遵循正常模式，没有明确的趋势。该模式可以解释为 **y(t) = m + s** 。我们建模 s，它从正态分布**s～N(0，1)** 中捕捉变化或噪声。此外，为了在数据集中引入异常，我们随机改变 m，从 1 到指定的上限。上面的实现由下面的代码片段给出。

**def** normal(n_samples=100，t_samples=30，m _ max = 1):
data =[]
**for**I**in**range(n _ samples):
m = NP。随机的。randint(1，m _ max，1)
样本= m+NP。随机的。正常(0，1，t _ samples)
数据。
追加(样本)**返回**

我们生成多个数据集，每个数据集有 30 个时间序列。通过从 1 到 m_max 改变变量 m 的值的范围来生成每个数据集。范围越大，在数据集中找到不同时间序列的可能性就越大。

## 方法学

对于每个数据集，我们比较所有时间序列对，总计 900 次比较。

我们绘制了数据集检测到的差异百分比与该数据集的变量 m (m_max)的上限。我们期望检测到的相异百分比随着 m_max 值的增加而增加，并在某一点逐渐减少。

下面给出了它的代码片段。突出显示的部分是对我们的 SAX HMM 模型的调用。

n _ samples = 30
n _ comparison = n _ samples * n _ samples
for m _ max in range(2，30，1):
data = NP . array(normal(n _ samples = n _ samples，m_max=m_max))
error = 0。
对于范围内的 I(n _ samples):
test = data[I，:]
对于范围内的 j(n _ samples):
control _ data = data[j，:]
SDF = saxmmdistancefinder(control，test)
result = SDF . compute _ dist()
if result[' risk ']= = 1:
error+= 1
print(m _ max，(error)/n_comparison)

## 结果

我们在 m_max=5 时看到 75%的不相似预测，并且在 m_max=15 时达到峰值。正如所料，标记为不相似的百分比随着 m_max 值的增加而增加，并在大约 93%时逐渐减少。

## 结论

结果展示了用于时间序列金丝雀分析的 SAX HMM 机器学习模型。该数据集是合成生成的，非常像 UCI 合成数据集。如我们所料，差异检测率随着 m_max 的增加而增加。

参考:[Robert Alcock 博士的合成控制图时间序列](https://archive.ics.uci.edu/ml/machine-learning-databases/synthetic_control-mld/synthetic_control.data.html)。

感谢阅读！
Sriram