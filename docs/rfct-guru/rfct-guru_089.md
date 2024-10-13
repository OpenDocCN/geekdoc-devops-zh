# 保持整个对象

> 原文：[`refactoringguru.cn/preserve-whole-object`](https://refactoringguru.cn/preserve-whole-object)

### 问题

你从一个对象获取多个值，然后将它们作为参数传递给一个方法。

### 解决方案

相反，试着传递整个对象。

之前

```
int low = daysTempRange.getLow();
int high = daysTempRange.getHigh();
boolean withinPlan = plan.withinRange(low, high);
```

之后

```
boolean withinPlan = plan.withinRange(daysTempRange);
```

之前

```
int low = daysTempRange.GetLow();
int high = daysTempRange.GetHigh();
bool withinPlan = plan.WithinRange(low, high);
```

之后

```
bool withinPlan = plan.WithinRange(daysTempRange);
```

之前

```
$low = $daysTempRange->getLow();
$high = $daysTempRange->getHigh();
$withinPlan = $plan->withinRange($low, $high);
```

之后

```
$withinPlan = $plan->withinRange($daysTempRange);
```

之前

```
low = daysTempRange.getLow()
high = daysTempRange.getHigh()
withinPlan = plan.withinRange(low, high)
```

之后

```
withinPlan = plan.withinRange(daysTempRange)
```

之前

```
let low = daysTempRange.getLow();
let high = daysTempRange.getHigh();
let withinPlan = plan.withinRange(low, high);
```

之后

```
let withinPlan = plan.withinRange(daysTempRange);
```

### 为什么重构

问题是每次在调用方法之前，未来参数对象的方法都必须被调用。如果这些方法或获取的数据量发生变化，你需要仔细找到程序中十几个这样的地方，并在每个地方实施这些更改。

在应用此重构技术后，获取所有必要数据的代码将存储在一个地方——方法本身。

### 好处

+   你看到的不是一堆杂乱无章的参数，而是一个具有可理解名称的单一对象。

+   如果方法需要从一个对象中获取更多数据，你不需要重写所有使用该方法的地方——只需在方法内部进行修改。

### 缺点

+   有时这种转变会导致方法变得不那么灵活：之前方法可以从许多不同来源获取数据，但现在由于重构，我们将其使用限制在只有特定接口的对象。

### 如何重构

1.  在方法中创建一个参数，以获取所需值的对象。

1.  现在开始逐一删除方法中的旧参数，用参数对象相关方法的调用替换它们。每替换一个参数后测试程序。

1.  从参数对象中删除在方法调用之前的获取器代码。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

难怪，阅读我们这里所有的文本需要 7 小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
