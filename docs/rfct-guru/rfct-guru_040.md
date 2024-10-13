# 内联方法

> 原文：[`refactoringguru.cn/inline-method`](https://refactoringguru.cn/inline-method)

### 问题

当方法体比方法本身更明显时，可以使用此技术。

### 解决方案

用方法的内容替换对该方法的调用，并删除该方法本身。

之前

```
class PizzaDelivery {
  // ...
  int getRating() {
    return moreThanFiveLateDeliveries() ? 2 : 1;
  }
  boolean moreThanFiveLateDeliveries() {
    return numberOfLateDeliveries > 5;
  }
}
```

之后

```
class PizzaDelivery {
  // ...
  int getRating() {
    return numberOfLateDeliveries > 5 ? 2 : 1;
  }
}
```

之前

```
class PizzaDelivery 
{
  // ...
  int GetRating() 
  {
    return MoreThanFiveLateDeliveries() ? 2 : 1;
  }
  bool MoreThanFiveLateDeliveries() 
  {
    return numberOfLateDeliveries > 5;
  }
}
```

之后

```
class PizzaDelivery 
{
  // ...
  int GetRating() 
  {
    return numberOfLateDeliveries > 5 ? 2 : 1;
  }
}
```

之前

```
function getRating() {
  return ($this->moreThanFiveLateDeliveries()) ? 2 : 1;
}
function moreThanFiveLateDeliveries() {
  return $this->numberOfLateDeliveries > 5;
}
```

之后

```
function getRating() {
  return ($this->numberOfLateDeliveries > 5) ? 2 : 1;
}
```

之前

```
class PizzaDelivery:
    # ...
    def getRating(self):
        return 2 if self.moreThanFiveLateDeliveries() else 1

    def moreThanFiveLateDeliveries(self):
        return self.numberOfLateDeliveries > 5
```

之后

```
class PizzaDelivery:
  # ...
  def getRating(self):
    return 2 if self.numberOfLateDeliveries > 5 else 1
```

之前

```
class PizzaDelivery {
  // ...
  getRating(): number {
    return moreThanFiveLateDeliveries() ? 2 : 1;
  }
  moreThanFiveLateDeliveries(): boolean {
    return numberOfLateDeliveries > 5;
  }
}
```

之后

```
class PizzaDelivery {
  // ...
  getRating(): number {
    return numberOfLateDeliveries > 5 ? 2 : 1;
  }
}
```

### 为什么要重构

一种方法简单地委托给另一种方法。就其本身而言，这种委托没有问题。但当有很多这样的方式时，它们会变成一个令人困惑的纠缠，难以理清。

通常方法最初并不太短，但随着程序的变化而变短。因此，不要害羞，尽量删除那些已经过时的方法。

### 益处

+   通过减少不必要的方法数量，你可以使代码更简洁明了。

### 如何重构

1.  确保该方法在子类中没有被重新定义。如果该方法被重新定义，请避免使用此技术。

1.  找到对该方法的所有调用。用方法的内容替换这些调用。

1.  删除该方法。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦了阅读？

毫无疑问，阅读我们这里所有的文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种更不乏味的学习新知识的方法。

*让我们看看…*
