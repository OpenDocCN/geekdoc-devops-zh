# 拆分临时变量

> 原文：[`refactoringguru.cn/split-temporary-variable`](https://refactoringguru.cn/split-temporary-variable)

### 问题

你有一个局部变量，用于在方法内部存储各种中间值（除了循环变量）。

### 解决方案

对于不同的值，使用不同的变量。每个变量应只负责一件特定的事情。

之前

```
double temp = 2 * (height + width);
System.out.println(temp);
temp = height * width;
System.out.println(temp);
```

之后

```
final double perimeter = 2 * (height + width);
System.out.println(perimeter);
final double area = height * width;
System.out.println(area);
```

之前

```
double temp = 2 * (height + width);
Console.WriteLine(temp);
temp = height * width;
Console.WriteLine(temp);
```

之后

```
readonly double perimeter = 2 * (height + width);
Console.WriteLine(perimeter);
readonly double area = height * width;
Console.WriteLine(area);
```

之前

```
$temp = 2 * ($this->height + $this->width);
echo $temp;
$temp = $this->height * $this->width;
echo $temp;
```

之后

```
$perimeter = 2 * ($this->height + $this->width);
echo $perimeter;
$area = $this->height * $this->width;
echo $area;
```

之前

```
temp = 2 * (height + width)
print(temp)
temp = height * width
print(temp)
```

之后

```
perimeter = 2 * (height + width)
print(perimeter)
area = height * width
print(area)
```

之前

```
let temp = 2 * (height + width);
console.log(temp);
temp = height * width;
console.log(temp);
```

之后

```
const perimeter = 2 * (height + width);
console.log(perimeter);
const area = height * width;
console.log(area);
```

### 为什么要重构

如果你在一个函数内部减少变量的数量，并将它们用于各种无关的目的，当你需要更改包含变量的代码时，你一定会遇到问题。你必须重新检查每个变量使用的案例，以确保使用了正确的值。

### 好处

+   程序代码的每个组件应仅负责一件事。这使得维护代码变得更加容易，因为你可以轻松替换任何特定的部分，而不必担心意外效果。

+   代码变得更加易读。如果一个变量在很久以前匆忙创建，它可能有一个没有任何说明的名称：`k`、`a2`、`value`等。但你可以通过为新变量命名一个易于理解、自解释的名称来解决这个问题。这些名称可能类似于`customerTaxValue`、`cityUnemploymentRate`、`clientSalutationString`等。

+   如果你预计将来会使用提取方法，那么这种重构技术是很有用的。

### 如何重构

1.  找到代码中变量被赋值的第一个地方。在这里，你应该用一个与所赋值对应的名称重命名变量。

1.  在使用该变量值的地方使用新名称替代旧名称。

1.  在变量被赋不同值的地方根据需要重复操作。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读累了吗？

难怪，阅读我们这里的所有文本需要 7 小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
