# 用测试替换异常

> 原文：[`refactoringguru.cn/replace-exception-with-test`](https://refactoringguru.cn/replace-exception-with-test)

### 问题

你在一个简单测试可以完成工作的地方抛出了异常吗？

### 解决方案

用条件测试替换异常。

之前

```
double getValueForPeriod(int periodNumber) {
  try {
    return values[periodNumber];
  } catch (ArrayIndexOutOfBoundsException e) {
    return 0;
  }
}
```

之后

```
double getValueForPeriod(int periodNumber) {
  if (periodNumber >= values.length) {
    return 0;
  }
  return values[periodNumber];
}
```

之前

```
double GetValueForPeriod(int periodNumber) 
{
  try 
  {
    return values[periodNumber];
  } 
  catch (IndexOutOfRangeException e) 
  {
    return 0;
  }
}
```

之后

```
double GetValueForPeriod(int periodNumber) 
{
  if (periodNumber >= values.Length) 
  {
    return 0;
  }
  return values[periodNumber];
}
```

之前

```
function getValueForPeriod($periodNumber) {
  try {
    return $this->values[$periodNumber];
  } catch (ArrayIndexOutOfBoundsException $e) {
    return 0;
  }
}
```

之后

```
function getValueForPeriod($periodNumber) {
  if ($periodNumber >= count($this->values)) {
    return 0;
  }
  return $this->values[$periodNumber];
}
```

之前

```
def getValueForPeriod(periodNumber):
    try:
        return values[periodNumber]
    except IndexError:
        return 0
```

之后

```
def getValueForPeriod(self, periodNumber):
    if periodNumber >= len(self.values):
        return 0
    return self.values[periodNumber]
```

之前

```
getValueForPeriod(periodNumber: number): number {
  try {
    return values[periodNumber];
  } catch (ArrayIndexOutOfBoundsException e) {
    return 0;
  }
}
```

之后

```
getValueForPeriod(periodNumber: number): number {
  if (periodNumber >= values.length) {
    return 0;
  }
  return values[periodNumber];
}
```

### 为什么重构

异常应该用于处理与意外错误相关的不规则行为。它们不应该替代测试。如果可以通过在运行之前验证条件来避免异常，那么就这么做。异常应该留给真正的错误。

例如，你进入了雷区并触发了一枚地雷，导致了一个异常；这个异常被成功处理，你被抬到安全的地方。但你本可以通过一开始就阅读雷区前的警告标志来避免这一切。

### 好处

+   有时，简单的条件比异常处理代码更明显。

### 如何重构

1.  为边缘案例创建条件，并将其移动到 try/catch 块之前。

1.  将代码从`catch`部分移动到这个条件内。

1.  在`catch`部分，放置抛出普通未命名异常的代码，并运行所有测试。

1.  如果在测试中没有抛出任何异常，去掉`try`/`catch`操作符。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦了阅读？

难怪这里的所有文本要花 7 个小时阅读。

尝试我们的互动重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
