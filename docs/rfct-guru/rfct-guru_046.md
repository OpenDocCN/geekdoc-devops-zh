# 用方法对象替换方法

> 原文：[`refactoringguru.cn/replace-method-with-method-object`](https://refactoringguru.cn/replace-method-with-method-object)

### 问题

你有一个很长的方法，其中的局部变量交织在一起，以至于无法应用*提取方法*。

### 解决方案

将方法转变为一个单独的类，以便局部变量成为类的字段。然后你可以在同一个类中将方法拆分为几个方法。

之前

```
class Order {
  // ...
  public double price() {
    double primaryBasePrice;
    double secondaryBasePrice;
    double tertiaryBasePrice;
    // Perform long computation.
  }
}
```

之后

```
class Order {
  // ...
  public double price() {
    return new PriceCalculator(this).compute();
  }
}

class PriceCalculator {
  private double primaryBasePrice;
  private double secondaryBasePrice;
  private double tertiaryBasePrice;

  public PriceCalculator(Order order) {
    // Copy relevant information from the
    // order object.
  }

  public double compute() {
    // Perform long computation.
  }
}
```

之前

```
public class Order 
{
  // ...
  public double Price() 
  {
    double primaryBasePrice;
    double secondaryBasePrice;
    double tertiaryBasePrice;
    // Perform long computation.
  }
}
```

之后

```
public class Order 
{
  // ...
  public double Price() 
  {
    return new PriceCalculator(this).Compute();
  }
}

public class PriceCalculator 
{
  private double primaryBasePrice;
  private double secondaryBasePrice;
  private double tertiaryBasePrice;

  public PriceCalculator(Order order) 
  {
    // Copy relevant information from the
    // order object.
  }

  public double Compute() 
  {
    // Perform long computation.
  }
}
```

之前

```
class Order {
  // ...
  public function price() {
    $primaryBasePrice = 10;
    $secondaryBasePrice = 20;
    $tertiaryBasePrice = 30;
    // Perform long computation.
  }
}
```

之后

```
class Order {
  // ...
  public function price() {
    return (new PriceCalculator($this))->compute();
  }
}

class PriceCalculator {
  private $primaryBasePrice;
  private $secondaryBasePrice;
  private $tertiaryBasePrice;

  public function __construct(Order $order) {
      // Copy relevant information from the
      // order object.
  }

  public function compute() {
    // Perform long computation.
  }
}
```

之前

```
class Order:
    # ...
    def price(self):
        primaryBasePrice = 0
        secondaryBasePrice = 0
        tertiaryBasePrice = 0
        # Perform long computation.
```

之后

```
class Order:
    # ...
    def price(self):
        return PriceCalculator(self).compute()

class PriceCalculator:
    def __init__(self, order):
        self._primaryBasePrice = 0
        self._secondaryBasePrice = 0
        self._tertiaryBasePrice = 0
        # Copy relevant information from the
        # order object.

    def compute(self):
        # Perform long computation.
```

之前

```
class Order {
  // ...
  price(): number {
    let primaryBasePrice;
    let secondaryBasePrice;
    let tertiaryBasePrice;
    // Perform long computation.
  }
}
```

之后

```
class Order {
  // ...
  price(): number {
    return new PriceCalculator(this).compute();
  }
}

class PriceCalculator {
  private _primaryBasePrice: number;
  private _secondaryBasePrice: number;
  private _tertiaryBasePrice: number;

  constructor(order: Order) {
    // Copy relevant information from the
    // order object.
  }

  compute(): number {
    // Perform long computation.
  }
}
```

### 为什么重构

一个方法太长，无法分离，因为局部变量交织在一起，很难彼此隔离。

第一步是将整个方法隔离到一个单独的类中，并将其局部变量转换为类的字段。

首先，这可以在类级别上隔离问题。其次，它为将一个庞大且笨重的方法拆分成几个小方法铺平了道路，而这些小方法与原始类的目的并不相符。

### 好处

+   将一个长方法隔离到自己的类中，可以防止方法膨胀。同时，这也允许在类内部将其拆分为子方法，而不会用工具方法污染原始类。

### 缺点

+   新增一个类，增加了程序的整体复杂性。

### 如何重构

1.  创建一个新类。根据你要重构的方法的目的命名它。

1.  在新类中创建一个私有字段，用于存储对之前方法所在类的实例的引用。如果需要，可以用来从原始类中获取所需数据。

1.  为方法中的每个局部变量创建一个单独的私有字段。

1.  创建一个构造函数，接受方法中所有局部变量的值作为参数，并初始化相应的私有字段。

1.  声明主方法并将原始方法的代码复制到其中，用私有字段替换局部变量。

1.  通过创建一个方法对象并调用其主方法，替换原始类中原始方法的主体。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读累了吗？

难怪，阅读我们这里所有的文本需要 7 小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*我们来看看…*
