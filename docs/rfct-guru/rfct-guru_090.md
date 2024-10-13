# 用方法调用替换参数

> 原文：[`refactoringguru.cn/replace-parameter-with-method-call`](https://refactoringguru.cn/replace-parameter-with-method-call)

### 问题

调用查询方法并将其结果作为另一个方法的参数，而那个方法本可以直接调用查询。

### 解决方案

不要通过参数传递值，尝试在方法体内放置查询调用。

之前

```
int basePrice = quantity * itemPrice;
double seasonDiscount = this.getSeasonalDiscount();
double fees = this.getFees();
double finalPrice = discountedPrice(basePrice, seasonDiscount, fees);
```

之后

```
int basePrice = quantity * itemPrice;
double finalPrice = discountedPrice(basePrice);
```

之前

```
int basePrice = quantity * itemPrice;
double seasonDiscount = this.GetSeasonalDiscount();
double fees = this.GetFees();
double finalPrice = DiscountedPrice(basePrice, seasonDiscount, fees);
```

之后

```
int basePrice = quantity * itemPrice;
double finalPrice = DiscountedPrice(basePrice);
```

之前

```
$basePrice = $this->quantity * $this->itemPrice;
$seasonDiscount = $this->getSeasonalDiscount();
$fees = $this->getFees();
$finalPrice = $this->discountedPrice($basePrice, $seasonDiscount, $fees);
```

之后

```
$basePrice = $this->quantity * $this->itemPrice;
$finalPrice = $this->discountedPrice($basePrice);
```

之前

```
basePrice = quantity * itemPrice
seasonalDiscount = self.getSeasonalDiscount()
fees = self.getFees()
finalPrice = discountedPrice(basePrice, seasonalDiscount, fees)

```

之后

```
basePrice = quantity * itemPrice
finalPrice = discountedPrice(basePrice)
```

之前

```
let basePrice = quantity * itemPrice;
const seasonDiscount = this.getSeasonalDiscount();
const fees = this.getFees();
const finalPrice = discountedPrice(basePrice, seasonDiscount, fees);
```

之后

```
let basePrice = quantity * itemPrice;
let finalPrice = discountedPrice(basePrice);
```

### 为什么要重构

一长串参数很难理解。此外，对这样的方式的调用往往类似于一系列的级联，复杂且令人兴奋的值计算难以导航，但必须传递给方法。因此，如果参数值可以借助某个方法计算，请在方法内部执行此操作，去掉参数。

### 好处

+   我们去掉不必要的参数，简化方法调用。这些参数通常不是为了当前的项目而创建，而是为了未来可能永远不会到来的需求。

### 缺点

+   你可能明天需要这个参数以满足其他需求……这会让你重新编写方法。

### 如何重构

1.  确保获取值的代码不使用当前方法的参数，因为它们在另一个方法内部不可用。如果是这样，移动代码就不可能。

1.  如果相关代码比单个方法或函数调用更复杂，请使用 提取方法 将这些代码隔离到一个新方法中，使调用更简单。

1.  在主方法的代码中，将所有对被替换参数的引用替换为获取值的方法调用。

1.  使用 移除参数 来消除现在未使用的参数。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

难怪，阅读我们这里所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
