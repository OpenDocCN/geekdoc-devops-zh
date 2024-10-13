# 不完整的类库

> 原文：[`refactoringguru.cn/smells/incomplete-library-class`](https://refactoringguru.cn/smells/incomplete-library-class)

### 征兆和症状

[图书馆](https://en.wikipedia.org/wiki/Library_(computing))迟早会停止满足用户需求。解决这个问题的唯一方案——更改图书馆——往往是不可能的，因为图书馆是只读的。

![](img/3b8ee9f3114d6e2d9c10786df57b511c.png)

### 问题原因

图书馆的作者没有提供您需要的功能，或者拒绝实施这些功能。

### 处理方法

+   要向库类引入一些方法，请使用引入外部方法。

+   对于类库中的重大更改，请使用引入本地扩展。

### 收益

+   减少代码重复（与其从头创建自己的库，不如依赖现有库）。

![](img/60943c416d0180b62787e86496d576ab.png)

### 何时忽略

+   扩展一个库可能会产生额外的工作，如果对库的更改涉及代码更改。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦阅读？

不奇怪，阅读我们这里所有文本需要 7 小时。

尝试我们的交互式重构课程。这提供了一种更轻松的学习新知识的方法。

*让我们看看…*
