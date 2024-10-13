# 引入空对象

> 原文：[`refactoringguru.cn/introduce-null-object`](https://refactoringguru.cn/introduce-null-object)

### 问题

由于一些方法返回`null`而不是实际对象，您的代码中有许多对`null`的检查。

### 解决方案

返回空对象而不是`null`，使其表现出默认行为。

之前

```
if (customer == null) {
  plan = BillingPlan.basic();
}
else {
  plan = customer.getPlan();
}
```

之后

```
class NullCustomer extends Customer {
  boolean isNull() {
    return true;
  }
  Plan getPlan() {
    return new NullPlan();
  }
  // Some other NULL functionality.
}

// Replace null values with Null-object.
customer = (order.customer != null) ?
  order.customer : new NullCustomer();

// Use Null-object as if it's normal subclass.
plan = customer.getPlan();
```

之前

```
if (customer == null) 
{
  plan = BillingPlan.Basic();
}
else 
{
  plan = customer.GetPlan();
}
```

之后

```
public sealed class NullCustomer: Customer 
{
  public override bool IsNull 
  {
    get { return true; }
  }

  public override Plan GetPlan() 
  {
    return new NullPlan();
  }
  // Some other NULL functionality.
}

// Replace null values with Null-object.
customer = order.customer ?? new NullCustomer();

// Use Null-object as if it's normal subclass.
plan = customer.GetPlan();
```

之前

```
if ($customer === null) {
  $plan = BillingPlan::basic();
} else {
  $plan = $customer->getPlan();
}
```

之后

```
class NullCustomer extends Customer {
  public function isNull() {
    return true;
  }
  public function getPlan() {
    return new NullPlan();
  }
  // Some other NULL functionality.
}

// Replace null values with Null-object.
$customer = ($order->customer !== null) ?
  $order->customer :
  new NullCustomer;

// Use Null-object as if it's normal subclass.
$plan = $customer->getPlan();
```

之前

```
if customer is None:
    plan = BillingPlan.basic()
else:
    plan = customer.getPlan()
```

之后

```
class NullCustomer(Customer):

    def isNull(self):
        return True

    def getPlan(self):
        return self.NullPlan()

    # Some other NULL functionality.

# Replace null values with Null-object.
customer = order.customer or NullCustomer()

# Use Null-object as if it's normal subclass.
plan = customer.getPlan()
```

之前

```
if (customer == null) {
  plan = BillingPlan.basic();
}
else {
  plan = customer.getPlan();
}
```

之后

```
class NullCustomer extends Customer {
  isNull(): boolean {
    return true;
  }
  getPlan(): Plan {
    return new NullPlan();
  }
  // Some other NULL functionality.
}

// Replace null values with Null-object.
let customer = (order.customer != null) ?
  order.customer : new NullCustomer();

// Use Null-object as if it's normal subclass.
plan = customer.getPlan();
```

### 为什么要重构

对`null`的多次检查使您的代码变得更长且更丑。

### 缺点

+   摆脱条件语句的代价是创建另一个新类。

### 如何重构

1.  从相关类创建一个子类，作为空对象的角色。

1.  在两个类中创建方法`isNull()`，该方法对空对象返回`true`，对实际类返回`false`。

1.  找到所有可能返回`null`而不是实际对象的地方。更改代码以返回一个空对象。

1.  找到所有将实际类的变量与`null`进行比较的地方。用对`isNull()`的调用替换这些检查。

1.  +   如果原始类的方法在值不等于`null`的条件下运行，请在空类中重新定义这些方法，并将`else`部分的代码插入其中。然后可以删除整个条件，差异化的行为将通过多态性实现。

    +   如果事情并不简单且方法无法重新定义，请看看是否可以将原本应该在`null`值情况下执行的操作提取到空对象的新方法中。将这些方法替换为`else`中的旧代码作为默认操作。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 读腻了吗？

难怪，阅读我们这里的所有文本需要 7 个小时。

尝试我们的互动重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
