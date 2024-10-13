# 删除设置方法

> 原文：[`refactoringguru.cn/remove-setting-method`](https://refactoringguru.cn/remove-setting-method)

### 问题

字段的值应在创建时设置，之后不应更改。

### 解决方案

因此，删除设置字段值的方法。

之前！删除设置方法 - 之前之后！删除设置方法 - 之后

### 为什么重构

您希望防止字段值的任何更改。

### 如何重构

1.  字段的值只能在构造函数中更改。如果构造函数中没有设置值的参数，请添加一个。

1.  查找所有 setter 调用。

    +   如果 setter 调用位于当前类构造函数调用后面，请将其参数移动到构造函数调用中并删除 setter。

    +   在构造函数中用直接访问字段替换 setter 调用。

1.  删除 setter。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦了阅读？

不奇怪，阅读我们这里所有的文本需要 7 个小时。

尝试我们的交互式重构课程。这是一种更轻松的学习新知识的方法。

*让我们看看…*
