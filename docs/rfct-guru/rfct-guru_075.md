# 整合条件表达式

> 原文：[`refactoringguru.cn/consolidate-conditional-expression`](https://refactoringguru.cn/consolidate-conditional-expression)

### 问题

你有多个条件导致相同的结果或动作。

### 解决方案

将所有这些条件整合到一个表达式中。

之前

```
double disabilityAmount() {
  if (seniority < 2) {
    return 0;
  }
  if (monthsDisabled > 12) {
    return 0;
  }
  if (isPartTime) {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

之后

```
double disabilityAmount() {
  if (isNotEligibleForDisability()) {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

之前

```
double DisabilityAmount() 
{
  if (seniority < 2) 
  {
    return 0;
  }
  if (monthsDisabled > 12) 
  {
    return 0;
  }
  if (isPartTime) 
  {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

之后

```
double DisabilityAmount()
{
  if (IsNotEligibleForDisability())
  {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

之前

```
function disabilityAmount() {
  if ($this->seniority < 2) {
    return 0;
  }
  if ($this->monthsDisabled > 12) {
    return 0;
  }
  if ($this->isPartTime) {
    return 0;
  }
  // compute the disability amount
  ...
```

之后

```
function disabilityAmount() {
  if ($this->isNotEligibleForDisability()) {
    return 0;
  }
  // compute the disability amount
  ...
```

之前

```
def disabilityAmount():
    if seniority < 2:
        return 0
    if monthsDisabled > 12:
        return 0
    if isPartTime:
        return 0
    # Compute the disability amount.
    # ...
```

之后

```
def disabilityAmount():
    if isNotEligibleForDisability():
        return 0
    # Compute the disability amount.
    # ...
```

之前

```
disabilityAmount(): number {
  if (seniority < 2) {
    return 0;
  }
  if (monthsDisabled > 12) {
    return 0;
  }
  if (isPartTime) {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

之后

```
disabilityAmount(): number {
  if (isNotEligibleForDisability()) {
    return 0;
  }
  // Compute the disability amount.
  // ...
}
```

### 为什么重构

你的代码包含许多交替的操作符，执行相同的操作。操作符分开的原因并不明确。

整合的主要目的是将条件提取到一个单独的方法中，以获得更大的清晰度。

### 好处

+   消除了重复的控制流代码。结合多个具有相同“目的地”的条件，有助于表明你只在进行一个复杂的检查，导致一个动作。

+   通过整合所有操作符，你现在可以用一种新的方法将这个复杂表达式隔离开来，其名称解释了条件的目的。

### 如何重构

在重构之前，确保条件没有任何“副作用”或以其他方式修改某些内容，而只是返回值。副作用可能隐藏在操作符本身内部执行的代码中，例如，当根据条件的结果向变量添加内容时。

1.  通过使用`and`和`or`将条件整合到一个表达式中。整合时的一般规则是：

    +   嵌套条件使用`and`连接。

    +   连续条件使用`or`连接。

1.  对操作符条件执行提取方法，并给方法命名以反映表达式的目的。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读？

难怪，阅读我们这里的所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种更轻松的学习新知识的方法。

*我们来看看…*
