# 分解条件

> 原文：[`refactoringguru.cn/decompose-conditional`](https://refactoringguru.cn/decompose-conditional)

### 问题

你有一个复杂的条件（`if-then`/`else`或`switch`）。

### 解决方案

将条件的复杂部分分解为单独的方法：条件、`then`和`else`。

之前

```
if (date.before(SUMMER_START) || date.after(SUMMER_END)) {
  charge = quantity * winterRate + winterServiceCharge;
}
else {
  charge = quantity * summerRate;
}
```

之后

```
if (isSummer(date)) {
  charge = summerCharge(quantity);
}
else {
  charge = winterCharge(quantity);
}
```

之前

```
if (date < SUMMER_START || date > SUMMER_END) 
{
  charge = quantity * winterRate + winterServiceCharge;
}
else 
{
  charge = quantity * summerRate;
}
```

之后

```
if (isSummer(date))
{
  charge = SummerCharge(quantity);
}
else 
{
  charge = WinterCharge(quantity);
}
```

之前

```
if ($date->before(SUMMER_START) || $date->after(SUMMER_END)) {
  $charge = $quantity * $winterRate + $winterServiceCharge;
} else {
  $charge = $quantity * $summerRate;
}

```

之后

```
if (isSummer($date)) {
  $charge = summerCharge($quantity);
} else {
  $charge = winterCharge($quantity);
}

```

之前

```
if date.before(SUMMER_START) or date.after(SUMMER_END):
    charge = quantity * winterRate + winterServiceCharge
else:
    charge = quantity * summerRate
```

之后

```
if isSummer(date):
    charge = summerCharge(quantity)
else:
    charge = winterCharge(quantity)
```

之前

```
if (date.before(SUMMER_START) || date.after(SUMMER_END)) {
  charge = quantity * winterRate + winterServiceCharge;
}
else {
  charge = quantity * summerRate;
}
```

之后

```
if (isSummer(date)) {
  charge = summerCharge(quantity);
}
else {
  charge = winterCharge(quantity);
}
```

### 为什么重构

代码越长，理解起来就越困难。当代码充满条件时，事情变得更加难以理解：

+   当你忙于弄清楚`then`块中的代码时，你会忘记相关条件是什么。

+   当你忙于解析`else`时，你会忘记`then`中的代码做了什么。

### 好处

+   通过将条件代码提取到明确命名的方法中，你为将来维护代码的人（比如两个月后的你）简化了工作。

+   这个重构技术也适用于条件中的短表达式。字符串`isSalaryDay()`比用于比较日期的代码要美观且更具描述性。

### 如何重构

1.  通过提取方法将条件提取到单独的方法中。

1.  对`then`和`else`块重复此过程。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读腻了吗？

不奇怪，阅读我们这里的所有文本需要 7 个小时。

尝试我们的互动重构课程。这提供了一种不那么乏味的学习新知识的方法。

*我们来看看…*
