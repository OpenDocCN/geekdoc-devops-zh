# 面向对象的滥用者

> 原文：[`refactoringguru.cn/refactoring/smells/oo-abusers`](https://refactoringguru.cn/refactoring/smells/oo-abusers)

所有这些异味都是对面向对象编程原则的不完整或不正确的应用。

开关语句

你有一个复杂的 `switch` 操作符或一系列 `if` 语句。

临时字段

临时字段仅在特定情况下获得其值（因此对象需要它们）。在这些情况下之外，它们是空的。

拒绝遗赠

如果一个子类只使用从父类继承的一部分方法和属性，则层次结构就不正确。未使用的方法可能会被忽略或重新定义，从而引发异常。

具有不同接口的替代类

两个类执行相同的功能，但方法名称不同。
