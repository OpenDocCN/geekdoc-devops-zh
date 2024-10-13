# 用对象替换数组

> 原文：[`refactoringguru.cn/replace-array-with-object`](https://refactoringguru.cn/replace-array-with-object)
> 
> 这种重构技术是用对象替换数据值的特殊情况。

### 问题

你有一个包含各种数据类型的数组。

### 解决方案

用一个将为每个元素拥有单独字段的对象替换数组。

之前

```
String[] row = new String[2];
row[0] = "Liverpool";
row[1] = "15";
```

之后

```
Performance row = new Performance();
row.setName("Liverpool");
row.setWins("15");
```

之前

```
string[] row = new string[2];
row[0] = "Liverpool";
row[1] = "15";
```

之后

```
Performance row = new Performance();
row.SetName("Liverpool");
row.SetWins("15");
```

之前

```
$row = [];
$row[0] = "Liverpool";
$row[1] = 15;
```

之后

```
$row = new Performance;
$row->setName("Liverpool");
$row->setWins(15);
```

之前

```
row = [None * 2]
row[0] = "Liverpool"
row[1] = "15"
```

之后

```
row = Performance()
row.setName("Liverpool")
row.setWins("15")
```

之前

```
let row = new Array(2);
row[0] = "Liverpool";
row[1] = "15";
```

之后

```
let row = new Performance();
row.setName("Liverpool");
row.setWins("15");
```

### 为什么重构

数组是存储数据和单一类型集合的绝佳工具。但如果你像使用邮政箱一样使用数组，把用户名存储在箱子 1 中，把用户地址存储在箱子 14 中，总有一天你会对此感到非常不满。这种方法会导致灾难性的失败，当有人把东西放入错误的“箱子”时，还需要花时间弄清楚哪个数据存储在哪里。

### 好处

+   在结果类中，你可以放置之前存储在主类或其他地方的所有相关行为。

+   一个类的字段比数组的元素更容易文档化。

### 如何重构

1.  创建一个新类来包含数组中的数据。将数组本身作为公共字段放入类中。

1.  在原始类中创建一个字段来存储该类的对象。不要忘记在你初始化数据数组的地方也创建该对象。

1.  在新类中，为每个数组元素逐一创建访问方法。给它们起自解释的名称，表明它们的功能。同时，将主代码中对数组元素的每次使用替换为相应的访问方法。

1.  当所有元素的访问方法都创建完成后，使数组变为私有。

1.  对于数组的每个元素，在类中创建一个私有字段，然后更改访问方法以便使用这个字段而不是数组。

1.  当所有数据被移动后，删除数组。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦了阅读？

难怪阅读这里所有文本需要 7 小时。

尝试我们的交互式重构课程。它提供了一种更轻松的学习新知识的方法。

*让我们看看…*
