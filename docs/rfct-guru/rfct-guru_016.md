# 临时字段

> 原文：[`refactoringguru.cn/smells/temporary-field`](https://refactoringguru.cn/smells/temporary-field)

### 征兆和症状

临时字段的值（因此被对象所需）仅在特定情况下存在。在这些情况下之外，它们是空的。

![](img/ebbc72fcfdd7d52352de6f66adc8a062.png)

### 问题的原因

通常，临时字段是为了在需要大量输入的算法中使用而创建的。因此，程序员决定在类中为这些数据创建字段，而不是在方法中创建大量参数。这些字段仅在算法中使用，其余时间未被使用。

这种代码很难理解。您期望在对象字段中看到数据，但由于某种原因，它们几乎总是空的。

![](img/70fb89ddf3c711e6c8f4d74d0c29c084.png)

### 处理

+   临时字段及其操作的所有代码可以通过提取类放入一个单独的类中。换句话说，您正在创建一个方法对象，达到与执行用方法对象替换方法相同的效果。

+   引入空对象并将其整合进替代用于检查临时字段值是否存在的条件代码中。

![](img/010f4121883240788cc9bc21cbb3d7ee.png)

### 收益

+   更好的代码清晰度和组织性。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

难怪，阅读我们这里的所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
