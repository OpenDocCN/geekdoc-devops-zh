# 更改防止器

> 原文：[`refactoringguru.cn/refactoring/smells/change-preventers`](https://refactoringguru.cn/refactoring/smells/change-preventers)

这些坏味道意味着如果你需要在代码中的一个地方进行更改，你也必须在其他地方进行许多更改。因此，程序开发变得更加复杂和昂贵。

发散变化

当你对一个类进行更改时，你会发现自己不得不更改许多不相关的方法。例如，添加新产品类型时，你必须更改查找、显示和订购产品的方法。

霰弹手术

进行任何修改都要求你对许多不同的类进行许多小的更改。

并行继承层次

每当你为一个类创建子类时，你会发现自己需要为另一个类创建子类。
