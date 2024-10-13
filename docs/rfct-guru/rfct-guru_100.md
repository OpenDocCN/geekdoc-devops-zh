# 提升构造函数主体

> 原文：[`refactoringguru.cn/pull-up-constructor-body`](https://refactoringguru.cn/pull-up-constructor-body)

### 问题

你的子类具有大部分相同的构造函数代码。

### 解决方案

创建一个超类构造函数，并将子类中相同的代码移到其中。在子类构造函数中调用超类构造函数。

之前

```
class Manager extends Employee {
  public Manager(String name, String id, int grade) {
    this.name = name;
    this.id = id;
    this.grade = grade;
  }
  // ...
}
```

之后

```
class Manager extends Employee {
  public Manager(String name, String id, int grade) {
    super(name, id);
    this.grade = grade;
  }
  // ...
}
```

之前

```
public class Manager: Employee 
{
  public Manager(string name, string id, int grade) 
  {
    this.name = name;
    this.id = id;
    this.grade = grade;
  }
  // ...
}
```

之后

```
public class Manager: Employee 
{
  public Manager(string name, string id, int grade): base(name, id)
  {
    this.grade = grade;
  }
  // ...
}
```

之前

```
class Manager extends Employee {
  public function __construct($name, $id, $grade) {
    $this->name = $name;
    $this->id = $id;
    $this->grade = $grade;
  }
  // ...
}
```

之后

```
class Manager extends Employee {
  public function __construct($name, $id, $grade) {
    parent::__construct($name, $id);
    $this->grade = $grade;
  }
  // ...
}
```

之前

```
class Manager(Employee):
    def __init__(self, name, id, grade):
        self.name = name
        self.id = id
        self.grade = grade
    # ...
```

之后

```
class Manager(Employee):
    def __init__(self, name, id, grade):
        Employee.__init__(name, id)
        self.grade = grade
    # ...
```

之前

```
class Manager extends Employee {
  constructor(name: string, id: string, grade: number) {
    this.name = name;
    this.id = id;
    this.grade = grade;
  }
  // ...
}
```

之后

```
class Manager extends Employee {
  constructor(name: string, id: string, grade: number) {
    super(name, id);
    this.grade = grade;
  }
  // ...
}
```

### 为什么要重构

这个重构技术与提升方法有什么不同？

1.  在 Java 中，子类不能继承构造函数，因此你不能简单地将提升方法应用于子类构造函数，并在将所有构造函数代码移到超类后删除它。除了在超类中创建构造函数外，子类中还需要有构造函数，以便简单地委托给超类构造函数。

1.  在 C++和 Java 中（如果你没有显式调用超类构造函数），超类构造函数会在子类构造函数之前自动调用，这使得只需要从子类构造函数的开头移动公共代码（因为你不能在子类构造函数的任意位置调用超类构造函数）。

1.  在大多数编程语言中，子类构造函数可以有与超类不同的参数列表。因此，你应该仅创建一个真正需要的超类构造函数。

### 如何重构

1.  在超类中创建一个构造函数。

1.  将每个子类构造函数开头的公共代码提取到超类构造函数中。在这样做之前，尽量将尽可能多的公共代码移动到构造函数的开头。

1.  在子类构造函数的第一行放置对超类构造函数的调用。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦阅读？

不奇怪，阅读我们这里所有文本需要 7 小时。

尝试我们的互动重构课程。它提供了一种更轻松的学习新知识的方法。

*让我们看看…*
