# 内联临时

> 原文：[`refactoringguru.cn/inline-temp`](https://refactoringguru.cn/inline-temp)

### 问题

你有一个临时变量，它的值是简单表达式的结果，仅此而已。

### 解决方案

用表达式本身替换对变量的引用。

之前

```
boolean hasDiscount(Order order) {
  double basePrice = order.basePrice();
  return basePrice > 1000;
}
```

之后

```
boolean hasDiscount(Order order) {
  return order.basePrice() > 1000;
}
```

之前

```
bool HasDiscount(Order order)
{
  double basePrice = order.BasePrice();
  return basePrice > 1000;
}
```

之后

```
bool HasDiscount(Order order)
{
  return order.BasePrice() > 1000;
}
```

之前

```
$basePrice = $anOrder->basePrice();
return $basePrice > 1000;
```

之后

```
return $anOrder->basePrice() > 1000;
```

之前

```
def hasDiscount(order):
    basePrice = order.basePrice()
    return basePrice > 1000
```

之后

```
def hasDiscount(order):
    return order.basePrice() > 1000
```

之前

```
hasDiscount(order: Order): boolean {
  let basePrice: number = order.basePrice();
  return basePrice > 1000;
}
```

之后

```
hasDiscount(order: Order): boolean {
  return order.basePrice() > 1000;
}
```

### 为什么重构

内联局部变量几乎总是用作用查询替换临时变量的一部分，或者为提取方法铺平道路。

### 优点

+   这种重构技术本身几乎没有好处。然而，如果变量被赋值为方法的结果，通过去掉不必要的变量，可以稍微提高程序的可读性。

### 缺点

+   有时，看似无用的临时变量用于缓存重复使用的昂贵操作的结果。因此，在使用这种重构技术之前，请确保简单性不会以牺牲性能为代价。

### 如何重构

1.  找到所有使用该变量的地方。用赋值给它的表达式替代变量。

1.  删除变量的声明和赋值行。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读得累了吗？

难怪，阅读我们这里所有文本需要 7 小时。

尝试我们的互动重构课程，它提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
