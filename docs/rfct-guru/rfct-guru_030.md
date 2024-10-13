# 耦合器

> 原文：[`refactoringguru.cn/refactoring/smells/couplers`](https://refactoringguru.cn/refactoring/smells/couplers)

这个组中的所有气味都导致类之间的过度耦合，或者展示了如果耦合被过度委托所替代会发生什么。

[特性嫉妒](https://refactoringguru.cn/refactoring/smells/feature-envy)

一个方法访问另一个对象的数据多于它自己数据的访问。

[不当亲密](https://refactoringguru.cn/refactoring/smells/inappropriate-intimacy)

一个类使用另一个类的内部字段和方法。

[消息链](https://refactoringguru.cn/refactoring/smells/message-chains)

在代码中，你会看到一系列类似于`$a->b()->c()->d()`的调用。

[中介者](https://refactoringguru.cn/refactoring/smells/middle-man)

如果一个类只执行一个操作，将工作委托给另一个类，那么它存在的意义是什么？
