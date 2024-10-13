# 封装字段

> 原文：[`refactoringguru.cn/encapsulate-field`](https://refactoringguru.cn/encapsulate-field)

### 问题

你有一个公共字段。

### 解决方案

将字段设置为私有，并为其创建访问方法。

之前

```
class Person {
  public String name;
}
```

之后

```
class Person {
  private String name;

  public String getName() {
    return name;
  }
  public void setName(String arg) {
    name = arg;
  }
}
```

之前

```
class Person 
{
  public string name;
}
```

之后

```
class Person 
{
  private string name;

  public string Name
  {
    get { return name; }
    set { name = value; }
  }
}
```

之前

```
public $name;
```

之后

```
private $name;

public getName() {
  return $this->name;
}

public setName($arg) {
  $this->name = $arg;
}
```

之前

```
class Person {
  name: string;
}
```

之后

```
class Person {
  private _name: string;

  get name() {
    return this._name;
  }
  setName(arg: string): void {
    this._name = arg;
  }
}
```

### 为什么重构

面向对象编程的支柱之一是 *封装*，即隐蔽对象数据的能力。否则，所有对象都是公共的，其他对象可以在没有任何检查和制衡的情况下获取和修改你的对象的数据！数据与与此数据相关的行为分离，程序部分的模块化受到损害，维护变得复杂。

### 益处

+   如果组件的数据和行为密切相关并且在代码中的同一位置，那么你维护和开发该组件会更容易。

+   你还可以执行与访问对象字段相关的复杂操作。

### 何时不使用

+   在某些情况下，由于性能考虑，封装是不明智的。这些情况很少见，但一旦发生，这种情况非常重要。

    比如说，你有一个图形编辑器，其中包含具有 x 和 y 坐标的对象。这些字段未来不太可能改变。此外，程序涉及大量不同的对象，这些字段都存在。因此，直接访问坐标字段可以节省大量本来会被调用访问方法占用的 CPU 周期。

    作为这种特殊情况的一个例子，Java 中的 [Point](http://docs.oracle.com/javase/7/docs/api/java/awt/Point.html) 类的所有字段都是公共的。

### 如何重构

1.  为字段创建 getter 和 setter。

1.  找到字段的所有调用。用 getter 替换字段值的接收，用 setter 替换新的字段值的设置。

1.  在替换所有字段调用后，将字段设置为私有。

#### 下一步

*封装字段*只是将数据与涉及该数据的行为更紧密结合的第一步。在你为访问字段创建简单方法后，应重新检查这些方法被调用的地方。这些区域的代码很可能在访问方法中看起来更合适。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

难怪，阅读我们这里的所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
