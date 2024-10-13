# 合并重复的条件片段

> 原文：[`refactoringguru.cn/consolidate-duplicate-conditional-fragments`](https://refactoringguru.cn/consolidate-duplicate-conditional-fragments)

### 问题

相同的代码可以在条件的所有分支中找到。

### 解决方案

将代码移出条件语句。

之前

```
if (isSpecialDeal()) {
  total = price * 0.95;
  send();
}
else {
  total = price * 0.98;
  send();
}
```

之后

```
if (isSpecialDeal()) {
  total = price * 0.95;
}
else {
  total = price * 0.98;
}
send();
```

之前

```
if (IsSpecialDeal()) 
{
  total = price * 0.95;
  Send();
}
else 
{
  total = price * 0.98;
  Send();
}
```

之后

```
if (IsSpecialDeal())
{
  total = price * 0.95;
}
else
{
  total = price * 0.98;
}
Send();
```

之前

```
if (isSpecialDeal()) {
  $total = $price * 0.95;
  send();
} else {
  $total = $price * 0.98;
  send();
}
```

之后

```
if (isSpecialDeal()) {
  $total = $price * 0.95;
} else {
  $total = $price * 0.98;
}
send();
```

之前

```
if isSpecialDeal():
    total = price * 0.95
    send()
else:
    total = price * 0.98
    send()
```

之后

```
if isSpecialDeal():
    total = price * 0.95
else:
    total = price * 0.98
send()
```

之前

```
if (isSpecialDeal()) {
  total = price * 0.95;
  send();
}
else {
  total = price * 0.98;
  send();
}
```

之后

```
if (isSpecialDeal()) {
  total = price * 0.95;
}
else {
  total = price * 0.98;
}
send();
```

### 为什么要重构

在条件的所有分支中发现重复代码，通常是条件分支内代码演变的结果。团队开发可能是导致这一现象的因素之一。

### 好处

+   代码去重。

### 如何重构

1.  如果重复代码位于条件分支的开头，请将代码移到条件语句之前。

1.  如果代码在分支的末尾执行，请将其放置在条件语句之后。

1.  如果重复代码随机位于分支内部，首先尝试将代码移到分支的开头或结尾，这取决于它是否会改变后续代码的结果。

1.  如果合适，并且重复代码超过一行，尝试使用 提取方法。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 读累了吗？

不奇怪，阅读我们这里所有文本需要 7 小时。

尝试我们的互动重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
