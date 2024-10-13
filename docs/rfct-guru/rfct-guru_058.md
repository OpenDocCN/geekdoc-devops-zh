# 自我封装字段

> 原文：[`refactoringguru.cn/self-encapsulate-field`](https://refactoringguru.cn/self-encapsulate-field)
> 
> 自我封装与普通的封装字段不同：这里给出的重构技术是在私有字段上执行的。

### 问题

你在类内部直接访问私有字段。

### 解决方案

创建一个字段的 getter 和 setter，并仅使用它们来访问该字段。

之前

```
class Range {
  private int low, high;
  boolean includes(int arg) {
    return arg >= low && arg <= high;
  }
}
```

之后

```
class Range {
  private int low, high;
  boolean includes(int arg) {
    return arg >= getLow() && arg <= getHigh();
  }
  int getLow() {
    return low;
  }
  int getHigh() {
    return high;
  }
}
```

之前

```
class Range 
{
  private int low, high;

  bool Includes(int arg) 
  {
    return arg >= low && arg <= high;
  }
}
```

之后

```
class Range 
{
  private int low, high;

  int Low {
    get { return low; }
  }
  int High {
    get { return high; }
  }

  bool Includes(int arg) 
  {
    return arg >= Low && arg <= High;
  }
}
```

之前

```
private $low;
private $high;

function includes($arg) {
  return $arg >= $this->low && $arg <= $this->high;
}
```

之后

```
private $low;
private $high;

function includes($arg) {
  return $arg >= $this->getLow() && $arg <= $this->getHigh();
}
function getLow() {
  return $this->low;
}
function getHigh() {
  return $this->high;
}
```

之前

```
class Range {
  private low: number
  private high: number;
  includes(arg: number): boolean {
    return arg >= low && arg <= high;
  }
}
```

之后

```
class Range {
  private low: number
  private high: number;
  includes(arg: number): boolean {
    return arg >= getLow() && arg <= getHigh();
  }
  getLow(): number {
    return low;
  }
  getHigh(): number {
    return high;
  }
}
```

### 为什么重构

有时直接在类内部访问私有字段根本不够灵活。你希望能够在首次查询时初始化字段值，或者在字段的新值分配时对其执行某些操作，或者在子类中以各种方式做到这一点。

### 优势

+   *间接访问字段*是通过访问方法（getter 和 setter）对字段进行操作。这种方法比*直接访问字段*灵活得多。

    +   首先，当字段中的数据被设置或接收时，你可以执行复杂的操作。*惰性初始化*和*字段值验证*可以很容易地在字段的 getter 和 setter 中实现。

    +   第二，更重要的是，你可以在子类中重新定义 getter 和 setter。

+   你可以选择不为字段实现 setter。字段值将仅在构造函数中指定，从而使字段在整个对象生命周期内不可更改。

### 缺点

+   当使用*直接访问字段*时，代码看起来更简单且更具表现力，尽管灵活性降低。

### 如何重构

1.  为该字段创建一个 getter（和可选的 setter）。它们应为`protected`或`public`。

1.  查找所有对字段的直接调用，并将它们替换为 getter 和 setter 调用。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读腻了吗？

难怪，阅读我们这里的所有文本需要 7 小时。

尝试我们的互动重构课程。它提供了一种更轻松的学习新知识的方法。

*让我们看看…*
