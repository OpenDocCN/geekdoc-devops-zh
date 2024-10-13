# 用查询替换临时变量

> 原文：[`refactoringguru.cn/replace-temp-with-query`](https://refactoringguru.cn/replace-temp-with-query)

### 问题

你将表达式的结果放入一个局部变量，以便在代码中后续使用。

### 解决方案

将整个表达式移动到一个单独的方法中并返回结果。从方法中查询，而不是使用变量。如有必要，将新方法融入其他方法中。

之前

```
double calculateTotal() {
  double basePrice = quantity * itemPrice;
  if (basePrice > 1000) {
    return basePrice * 0.95;
  }
  else {
    return basePrice * 0.98;
  }
}
```

之后

```
double calculateTotal() {
  if (basePrice() > 1000) {
    return basePrice() * 0.95;
  }
  else {
    return basePrice() * 0.98;
  }
}
double basePrice() {
  return quantity * itemPrice;
}
```

之前

```
double CalculateTotal() 
{
  double basePrice = quantity * itemPrice;

  if (basePrice > 1000) 
  {
    return basePrice * 0.95;
  }
  else 
  {
    return basePrice * 0.98;
  }
}
```

之后

```
double CalculateTotal() 
{
  if (BasePrice() > 1000) 
  {
    return BasePrice() * 0.95;
  }
  else 
  {
    return BasePrice() * 0.98;
  }
}
double BasePrice() 
{
  return quantity * itemPrice;
}
```

之前

```
$basePrice = $this->quantity * $this->itemPrice;
if ($basePrice > 1000) {
  return $basePrice * 0.95;
} else {
  return $basePrice * 0.98;
}
```

之后

```
if ($this->basePrice() > 1000) {
  return $this->basePrice() * 0.95;
} else {
  return $this->basePrice() * 0.98;
}

...

function basePrice() {
  return $this->quantity * $this->itemPrice;
}
```

之前

```
def calculateTotal():
    basePrice = quantity * itemPrice
    if basePrice > 1000:
        return basePrice * 0.95
    else:
        return basePrice * 0.98
```

之后

```
def calculateTotal():
    if basePrice() > 1000:
        return basePrice() * 0.95
    else:
        return basePrice() * 0.98

def basePrice():
    return quantity * itemPrice
```

之前

```
 calculateTotal(): number {
  let basePrice = quantity * itemPrice;
  if (basePrice > 1000) {
    return basePrice * 0.95;
  }
  else {
    return basePrice * 0.98;
  }
}
```

之后

```
calculateTotal(): number {
  if (basePrice() > 1000) {
    return basePrice() * 0.95;
  }
  else {
    return basePrice() * 0.98;
  }
}
basePrice(): number {
  return quantity * itemPrice;
}
```

### 为什么重构

这种重构可以为将提取方法应用于一个非常长的方法的一部分奠定基础。

同样的表达式有时也可能在其他方法中出现，这也是考虑创建公共方法的一个原因。

### 好处

+   代码可读性。理解方法`getTax()`的目的比理解`orderPrice() * 0.2`这行代码要容易得多。

+   通过去重实现更精简的代码，尤其是在被替换的行在多个方法中使用时。

### 了解一下

#### 性能

这种重构可能会引发这样一个问题：这种方法是否可能导致性能下降。诚实的回答是：是的，因为结果代码可能因查询新方法而受到影响。但在今天快速的 CPU 和优秀的编译器面前，这种负担几乎总是微不足道的。相比之下，可读代码和在程序代码的其他地方重用该方法的能力——得益于这种重构方法——是非常显著的好处。

然而，如果你的临时变量用于缓存一个真正耗时的表达式的结果，你可能想在将表达式提取到新方法后停止重构。

### 如何重构

1.  确保在方法中变量只被赋值一次。如果不是，请使用拆分临时变量以确保该变量仅用于存储表达式的结果。

1.  使用提取方法将感兴趣的表达式放入一个新方法中。确保该方法只返回一个值，并且不改变对象的状态。如果该方法影响对象的可见状态，请使用将查询与修改分开。

1.  用对新方法的查询替换变量。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

不足为奇，阅读我们这里所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种更不乏味的学习新知识的方法。

*让我们来看看…*
