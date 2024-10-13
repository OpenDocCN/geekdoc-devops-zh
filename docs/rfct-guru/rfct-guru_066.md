# 用符号常量替换魔法数字

> 原文：[`refactoringguru.cn/replace-magic-number-with-symbolic-constant`](https://refactoringguru.cn/replace-magic-number-with-symbolic-constant)

### 问题

你的代码使用了一个具有特定含义的数字。

### 解决方案

用一个具有易读名称的常量替换这个数字，以解释该数字的含义。

之前

```
double potentialEnergy(double mass, double height) {
  return mass * height * 9.81;
}
```

之后

```
static final double GRAVITATIONAL_CONSTANT = 9.81;

double potentialEnergy(double mass, double height) {
  return mass * height * GRAVITATIONAL_CONSTANT;
}
```

之前

```
double PotentialEnergy(double mass, double height) 
{
  return mass * height * 9.81;
}
```

之后

```
const double GRAVITATIONAL_CONSTANT = 9.81;

double PotentialEnergy(double mass, double height) 
{
  return mass * height * GRAVITATIONAL_CONSTANT;
}
```

之前

```
function potentialEnergy($mass, $height) {
  return $mass * $height * 9.81;
}
```

之后

```
define("GRAVITATIONAL_CONSTANT", 9.81);

function potentialEnergy($mass, $height) {
  return $mass * $height * GRAVITATIONAL_CONSTANT;
}
```

之前

```
def potentialEnergy(mass, height):
    return mass * height * 9.81
```

之后

```
GRAVITATIONAL_CONSTANT = 9.81

def potentialEnergy(mass, height):
    return mass * height * GRAVITATIONAL_CONSTANT
```

之前

```
potentialEnergy(mass: number, height: number): number {
  return mass * height * 9.81;
}
```

之后

```
static const GRAVITATIONAL_CONSTANT = 9.81;

potentialEnergy(mass: number, height: number): number {
  return mass * height * GRAVITATIONAL_CONSTANT;
}
```

### 为什么要重构

魔法数字是源代码中遇到的数值，但没有明显的含义。这个“反模式”使得理解程序和重构代码变得更加困难。

当你需要更改这个魔法数字时，会出现更多困难。查找和替换无法解决这个问题：相同的数字可能在不同地方用于不同目的，这意味着你需要验证每一行使用这个数字的代码。

### 好处

+   符号常量可以作为其值含义的实时文档。

+   更改常量的值比在整个代码库中搜索这个数字要容易得多，且不会意外改变用于其他目的的相同数字。

+   减少代码中对数字或字符串的重复使用。这在值复杂且较长时尤其重要（例如`3.14159`或`0xCAFEBABE`）。

### 重要信息

#### 不是所有的数字都是神奇的。

如果数字的目的很明显，就不需要替换。一个经典例子是：

```
for (i = 0; i < сount; i++) { ... }

```

#### 替代方案

1.  有时可以用方法调用替换魔法数字。例如，如果你有一个表示集合中元素数量的魔法数字，你不需要在检查集合的最后一个元素时使用它。相反，使用标准方法获取集合长度。

1.  魔法数字有时用作类型代码。假设你有两种用户类型，并在一个类中使用数字字段来指定哪个是哪个：管理员为`1`，普通用户为`2`。

    在这种情况下，你应该使用一种重构方法来避免类型代码：

    +   用类替换类型代码

    +   用子类替换类型代码

    +   用状态/策略替换类型代码

### 如何重构

1.  声明一个常量并将魔法数字的值赋给它。

1.  找到所有魔法数字的提及。

1.  对于你找到的每个数字，请仔细检查在这种特定情况下的魔法数字是否与常量的目的相对应。如果是，请用你的常量替换这个数字。这是一个重要步骤，因为相同的数字可能意味着完全不同的事情（并可能用不同的常量替换）。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

不奇怪，阅读我们这里所有的文本需要 7 小时。

尝试我们的交互式重构课程，它提供了更轻松的学习新知识的方法。

*让我们看看……*
