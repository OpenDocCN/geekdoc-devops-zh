# 用显式方法替换参数

> 原文：[`refactoringguru.cn/replace-parameter-with-explicit-methods`](https://refactoringguru.cn/replace-parameter-with-explicit-methods)

### 问题

方法被拆分为几个部分，每个部分根据参数的值运行。

### 解决方案

将方法的各个部分提取到自己的方法中，并调用它们来替代原始方法。

之前

```
void setValue(String name, int value) {
  if (name.equals("height")) {
    height = value;
    return;
  }
  if (name.equals("width")) {
    width = value;
    return;
  }
  Assert.shouldNeverReachHere();
}
```

之后

```
void setHeight(int arg) {
  height = arg;
}
void setWidth(int arg) {
  width = arg;
}
```

之前

```
void SetValue(string name, int value) 
{
  if (name.Equals("height")) 
  {
    height = value;
    return;
  }
  if (name.Equals("width")) 
  {
    width = value;
    return;
  }
  Assert.Fail();
}
```

之后

```
void SetHeight(int arg) 
{
  height = arg;
}
void SetWidth(int arg) 
{
  width = arg;
}
```

之前

```
function setValue($name, $value) {
  if ($name === "height") {
    $this->height = $value;
    return;
  }
  if ($name === "width") {
    $this->width = $value;
    return;
  }
  assert("Should never reach here");
}
```

之后

```
function setHeight($arg) {
  $this->height = $arg;
}
function setWidth($arg) {
  $this->width = $arg;
}
```

之前

```
def output(self, type):
    if name == "banner"
        # Print the banner.
        # ...
    if name == "info"
        # Print the info.
        # ...
```

之后

```
def outputBanner(self):
    # Print the banner.
    # ...

def outputInfo(self):
    # Print the info.
    # ...
```

之前

```
 setValue(name: string, value: number): void {
  if (name.equals("height")) {
    height = value;
    return;
  }
  if (name.equals("width")) {
    width = value;
    return;
  }

}
```

之后

```
setHeight(arg: number): void {
  height = arg;
}
setWidth(arg: number): number {
  width = arg;
}
```

### 为什么重构

一个包含依赖参数变体的方法变得庞大。每个分支中都运行非平凡的代码，并且新变体很少被添加。

### 好处

+   提高代码可读性。理解`startEngine()`的目的要比理解`setValue("engineEnabled", true)`容易得多。

### 何时不使用

+   如果一个方法很少更改且不会添加新变体，则不要用显式方法替换参数。

### 如何重构

1.  对于每种方法变体，创建一个单独的方法。根据主方法中参数的值运行这些方法。

1.  找到所有调用原始方法的地方。在这些地方，调用其中一个新的依赖参数的变体。

1.  当没有原始方法的调用时，删除它。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 读得累吗？

不奇怪，阅读我们这里所有的文本需要 7 小时。

尝试我们的交互式重构课程。这提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
