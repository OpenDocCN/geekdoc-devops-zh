# 引入断言

> 原文：[`refactoringguru.cn/introduce-assertion`](https://refactoringguru.cn/introduce-assertion)

### 问题

为了让一段代码正确工作，某些条件或值必须为真。

### 解决方案

用具体的断言检查替换这些假设。

之前

```
double getExpenseLimit() {
  // Should have either expense limit or
  // a primary project.
  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit :
    primaryProject.getMemberExpenseLimit();
}
```

之后

```
double getExpenseLimit() {
  Assert.isTrue(expenseLimit != NULL_EXPENSE || primaryProject != null);

  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit:
    primaryProject.getMemberExpenseLimit();
}
```

之前

```
double GetExpenseLimit() 
{
  // Should have either expense limit or
  // a primary project.
  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit :
    primaryProject.GetMemberExpenseLimit();
}
```

之后

```
double GetExpenseLimit() 
{
  Assert.IsTrue(expenseLimit != NULL_EXPENSE || primaryProject != null);

  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit:
    primaryProject.GetMemberExpenseLimit();
}
```

之前

```
function getExpenseLimit() {
  // Should have either expense limit or
  // a primary project.
  return ($this->expenseLimit !== NULL_EXPENSE) ?
    $this->expenseLimit:
    $this->primaryProject->getMemberExpenseLimit();
}
```

之后

```
function getExpenseLimit() {
  assert($this->expenseLimit !== NULL_EXPENSE || isset($this->primaryProject));

  return ($this->expenseLimit !== NULL_EXPENSE) ?
    $this->expenseLimit:
    $this->primaryProject->getMemberExpenseLimit();
}
```

之前

```
def getExpenseLimit(self):
    # Should have either expense limit or
    # a primary project.
    return self.expenseLimit if self.expenseLimit != NULL_EXPENSE else \
        self.primaryProject.getMemberExpenseLimit()
```

之后

```
def getExpenseLimit(self):
    assert (self.expenseLimit != NULL_EXPENSE) or (self.primaryProject != None)

    return self.expenseLimit if (self.expenseLimit != NULL_EXPENSE) else \
        self.primaryProject.getMemberExpenseLimit()
```

之前

```
getExpenseLimit(): number {
  // Should have either expense limit or
  // a primary project.
  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit:
    primaryProject.getMemberExpenseLimit();
}
```

之后

```
getExpenseLimit(): number {
  // TypeScript and JS doesn't have built-in assertions, so we'll use
  // good-old console.error(). You can always extract this into a
  // designated assertion function.
  if (!(expenseLimit != NULL_EXPENSE ||
       (typeof primaryProject !== 'undefined' && primaryProject))) {
      console.error("Assertion failed: getExpenseLimit()");
  }

  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit:
    primaryProject.getMemberExpenseLimit();
}
```

### 为什么重构

假设一段代码假设了某个对象的当前状态或参数或局部变量的值。通常，这种假设在出现错误时才会失效。

通过添加相应的断言使这些假设变得明显。与方法参数中的类型提示一样，这些断言可以作为代码的实时文档。

作为检查代码需要断言的指导方针，请查看描述特定方法工作条件的注释。

### 好处

+   如果一个假设不成立，导致代码给出错误结果，那么最好在此之前停止执行，以免造成致命后果和数据损坏。这也意味着在设计测试程序时，你忽略了写必要的测试。

### 缺点

+   有时候，抛出异常比简单的断言更合适。你可以选择必要的异常类，并让其余代码正确处理它。

+   什么时候异常比简单断言更好？如果异常可以由用户或系统的操作引起，并且你能够处理该异常。另一方面，普通的未命名和未处理的异常基本上等同于简单的断言——你不处理它们，它们是程序错误的结果，这种错误本不该发生。

### 如何重构

当你看到某个条件被假设时，添加对此条件的断言以确保其正确性。

添加断言不应改变程序的行为。

不要在代码的**所有**地方过度使用断言。只检查对代码正确运行所必需的条件。如果你的代码即使在某个特定断言为假时仍能正常工作，你可以安全地移除该断言。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读累了吗？

难怪，阅读我们这里所有文本需要 7 个小时。

尝试我们的互动重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看…*
