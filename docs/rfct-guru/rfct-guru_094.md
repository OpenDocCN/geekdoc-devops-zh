# 用工厂方法替换构造函数

> 原文：[`refactoringguru.cn/replace-constructor-with-factory-method`](https://refactoringguru.cn/replace-constructor-with-factory-method)

### 问题

您有一个复杂的构造函数，除了设置对象字段中的参数值外还做其他事情。

### 解决方案

创建一个工厂方法，并用它替换构造函数调用。

之前

```
class Employee {
  Employee(int type) {
    this.type = type;
  }
  // ...
}
```

之后

```
class Employee {
  static Employee create(int type) {
    employee = new Employee(type);
    // do some heavy lifting.
    return employee;
  }
  // ...
}
```

之前

```
public class Employee 
{
  public Employee(int type) 
  {
    this.type = type;
  }
  // ...
}
```

之后

```
public class Employee
{
  public static Employee Create(int type)
  {
    employee = new Employee(type);
    // Do some heavy lifting.
    return employee;
  }
  // ...
}
```

之前

```
class Employee {
  // ...
  public function __construct($type) {
   $this->type = $type;
  }
  // ...
}
```

之后

```
class Employee {
  // ...
  static public function create($type) {
    $employee = new Employee($type);
    // do some heavy lifting.
    return $employee;
  }
  // ...
}
```

之前

```
class Employee {
  constructor(type: number) {
    this.type = type;
  }
  // ...
}
```

之后

```
class Employee {
  static create(type: number): Employee {
    let employee = new Employee(type);
    // Do some heavy lifting.
    return employee;
  }
  // ...
}
```

### 为什么重构

使用此重构技术的最明显原因与用子类替换类型代码有关。

您的代码中之前创建了一个对象，并将编码类型的值传递给它。使用重构方法后，出现了多个子类，您需要根据编码类型的值创建对象。改变原始构造函数以返回子类对象是不可能的，因此我们创建一个静态工厂方法，它将返回所需类的对象，之后用它替换所有原始构造函数的调用。

工厂方法也可以在其他情况下使用，当构造函数无法胜任时。它们在尝试将值更改为引用时可能很重要。它们还可以用于设置超出参数数量和类型的各种创建模式。

### 优势

+   工厂方法不一定返回调用它的类的对象。通常，这些可以是其子类，基于传递给该方法的参数进行选择。

+   工厂方法可以有一个更好的名称，描述它返回什么以及如何返回，例如`Troops::GetCrew(myTank)`。

+   工厂方法可以返回已经创建的对象，而构造函数总是创建一个新的实例。

### 如何重构

1.  创建一个工厂方法。在其中调用当前构造函数。

1.  用对工厂方法的调用替换所有构造函数调用。

1.  将构造函数声明为私有。

1.  调查构造函数代码，尝试隔离与当前类对象构造无关的代码，将这些代码移动到工厂方法中。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 读累了吗？

不奇怪，阅读我们这里的所有文本需要 7 小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
