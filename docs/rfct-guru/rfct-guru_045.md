# 移除对参数的赋值

> 原文：[`refactoringguru.cn/remove-assignments-to-parameters`](https://refactoringguru.cn/remove-assignments-to-parameters)

### 问题

一些值在方法体内被赋给参数。

### 解决方案

使用局部变量代替参数。

之前

```
int discount(int inputVal, int quantity) {
  if (quantity > 50) {
    inputVal -= 2;
  }
  // ...
}
```

之后

```
int discount(int inputVal, int quantity) {
  int result = inputVal;
  if (quantity > 50) {
    result -= 2;
  }
  // ...
}
```

之前

```
int Discount(int inputVal, int quantity) 
{
  if (quantity > 50) 
  {
    inputVal -= 2;
  }
  // ...
}
```

之后

```
int Discount(int inputVal, int quantity) 
{
  int result = inputVal;

  if (quantity > 50) 
  {
    result -= 2;
  }
  // ...
}
```

之前

```
function discount($inputVal, $quantity) {
  if ($quantity > 50) {
    $inputVal -= 2;
  }
  ...
```

之后

```
function discount($inputVal, $quantity) {
  $result = $inputVal;
  if ($quantity > 50) {
    $result -= 2;
  }
  ...
```

之前

```
def discount(inputVal, quantity):
    if quantity > 50:
        inputVal -= 2
    # ...
```

之后

```
def discount(inputVal, quantity):
    result = inputVal
    if quantity > 50:
        result -= 2
    # ...
```

之前

```
discount(inputVal: number, quantity: number): number {
  if (quantity > 50) {
    inputVal -= 2;
  }
  // ...
}
```

之后

```
discount(inputVal: number, quantity: number): number {
  let result = inputVal;
  if (quantity > 50) {
    result -= 2;
  }
  // ...
}
```

### 为什么要重构

这个重构的原因与拆分临时变量相同，但在这种情况下我们处理的是参数，而不是局部变量。

首先，如果参数通过引用传递，那么在方法内部更改参数值后，该值会传递给请求调用此方法的参数。这个过程往往是偶然发生的，导致不幸的后果。即使在你的编程语言中通常是通过值（而不是通过引用）传递参数，这种编码怪癖可能会让不习惯的人感到困惑。

其次，将不同值多次赋给单一参数，使你很难知道在任何特定时间点参数中应该包含什么数据。如果参数及其内容有文档记录，但实际值可能与方法内部的预期不同，问题会更加严重。

### 好处

+   程序的每个元素应只负责一件事。这使得今后的代码维护变得更加容易，因为你可以安全地替换代码而不会产生副作用。

+   这个重构有助于将重复代码提取到单独的方法。

### 如何重构

1.  创建一个局部变量并赋予参数的初始值。

1.  在此行之后的所有方法代码中，将参数替换为新的局部变量。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读？

难怪，这里所有文本的阅读需要 7 小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*我们来看……*
