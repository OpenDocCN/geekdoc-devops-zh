# 移除控制标志

> 原文：[`refactoringguru.cn/remove-control-flag`](https://refactoringguru.cn/remove-control-flag)

### 问题

你有一个布尔变量作为多个布尔表达式的控制标志。

### 解决方案

不要使用变量，使用`break`、`continue`和`return`。

### 为什么要重构

控制标志可以追溯到古老的编程时代，那时“合格”的程序员总是为他们的函数设置一个入口点（函数声明行）和一个出口点（在函数的最后）。

在现代编程语言中，这种风格的编程已过时，因为我们有特殊的操作符来修改循环和其他复杂结构中的控制流：

+   `break`：停止循环。

+   `continue`：停止当前循环分支的执行，并在下一个迭代中检查循环条件。

+   `return`：停止整个函数的执行并返回其结果（如果在操作符中给出）。

### 好处

+   控制标志代码通常比使用控制流操作符编写的代码要繁琐得多。

### 如何重构

1.  找到导致退出循环或当前迭代的控制标志的值赋值。

1.  如果这是退出循环，则用`break`替换；如果这是退出迭代，则用`continue`替换；如果需要从函数返回此值，则用`return`替换。

1.  删除与控制标志相关的剩余代码和检查。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读累了吗？

难怪，阅读我们这里的所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种不那么繁琐的学习新知识的方法。

*让我们看看……*
