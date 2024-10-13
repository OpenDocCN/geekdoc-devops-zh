# 提取变量

> 原文：[`refactoringguru.cn/extract-variable`](https://refactoringguru.cn/extract-variable)

### 问题

你有一个难以理解的表达式。

### 解决方案

将表达式或其部分的结果放在自解释的单独变量中。

之前

```
void renderBanner() {
  if ((platform.toUpperCase().indexOf("MAC") > -1) &&
       (browser.toUpperCase().indexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
```

之后

```
void renderBanner() {
  final boolean isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
  final boolean isIE = browser.toUpperCase().indexOf("IE") > -1;
  final boolean wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) {
    // do something
  }
}
```

之前

```
void RenderBanner() 
{
  if ((platform.ToUpper().IndexOf("MAC") > -1) &&
       (browser.ToUpper().IndexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
```

之后

```
void RenderBanner() 
{
  readonly bool isMacOs = platform.ToUpper().IndexOf("MAC") > -1;
  readonly bool isIE = browser.ToUpper().IndexOf("IE") > -1;
  readonly bool wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) 
  {
    // do something
  }
}
```

之前

```
if (($platform->toUpperCase()->indexOf("MAC") > -1) &&
     ($browser->toUpperCase()->indexOf("IE") > -1) &&
      $this->wasInitialized() && $this->resize > 0)
{
  // do something
}
```

之后

```
$isMacOs = $platform->toUpperCase()->indexOf("MAC") > -1;
$isIE = $browser->toUpperCase()->indexOf("IE")  > -1;
$wasResized = $this->resize > 0;

if ($isMacOs && $isIE && $this->wasInitialized() && $wasResized) {
  // do something
}
```

之前

```
def renderBanner(self):
    if (self.platform.toUpperCase().indexOf("MAC") > -1) and \
       (self.browser.toUpperCase().indexOf("IE") > -1) and \
       self.wasInitialized() and (self.resize > 0):
        # do something
```

之后

```
def renderBanner(self):
    isMacOs = self.platform.toUpperCase().indexOf("MAC") > -1
    isIE = self.browser.toUpperCase().indexOf("IE") > -1
    wasResized = self.resize > 0

    if isMacOs and isIE and self.wasInitialized() and wasResized:
        # do something
```

之前

```
renderBanner(): void {
  if ((platform.toUpperCase().indexOf("MAC") > -1) &&
       (browser.toUpperCase().indexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
```

之后

```
renderBanner(): void {
  const isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
  const isIE = browser.toUpperCase().indexOf("IE") > -1;
  const wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) {
    // do something
  }
}
```

### 为什么重构

提取变量的主要原因是使复杂表达式更易于理解，通过将其划分为中间部分。这些可以是：

+   C 语言中 `if()` 运算符的条件或 `?:` 运算符的一部分

+   一个没有中间结果的长算术表达式

+   长多部分行

如果你看到提取的表达式在代码的其他地方被使用，提取变量可能是执行 提取方法 的第一步。

### 好处

+   更易读的代码！尝试给提取的变量命名，以清晰地表明变量的目的。提高可读性，减少冗长的注释。选择像 `customerTaxValue`、`cityUnemploymentRate`、`clientSalutationString` 等名称。

### 缺点

+   你的代码中存在更多变量。但这被代码的可读性所抵消。

+   在重构条件表达式时，请记住编译器很可能会优化它，以最小化计算结果值所需的计算量。假设你有以下表达式 `if (a() || b()) ...`。如果方法 `a` 返回 `true`，程序将不会调用方法 `b`，因为结果值仍然是 `true`，无论 `b` 返回什么值。

    然而，如果你将该表达式的部分提取到变量中，两个方法将始终被调用，这可能会影响程序的性能，特别是如果这些方法进行了一些重负载的工作。

### 如何重构

1.  在相关表达式之前插入新行并在此声明新变量。将复杂表达式的一部分赋值给该变量。

1.  用新变量替换表达式的那部分。

1.  对表达式中的所有复杂部分重复此过程。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦了阅读？

不奇怪，阅读我们这里的所有文本需要 7 小时。

尝试我们的交互式重构课程。这提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
