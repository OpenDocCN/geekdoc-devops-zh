# 用异常替代错误代码

> 原文：[`refactoringguru.cn/replace-error-code-with-exception`](https://refactoringguru.cn/replace-error-code-with-exception)

### 问题

方法返回一个指示错误的特殊值吗？

### 解决方案

抛出一个异常。

之前

```
int withdraw(int amount) {
  if (amount > _balance) {
    return -1;
  }
  else {
    balance -= amount;
    return 0;
  }
}
```

之后

```
void withdraw(int amount) throws BalanceException {
  if (amount > _balance) {
    throw new BalanceException();
  }
  balance -= amount;
}
```

之前

```
int Withdraw(int amount) 
{
  if (amount > _balance) 
  {
    return -1;
  }
  else 
  {
    balance -= amount;
    return 0;
  }
}
```

之后

```
///<exception cref="BalanceException">Thrown when amount > _balance</exception>
void Withdraw(int amount)
{
  if (amount > _balance) 
  {
    throw new BalanceException();
  }
  balance -= amount;
}
```

之前

```
function withdraw($amount) {
  if ($amount > $this->balance) {
    return -1;
  } else {
    $this->balance -= $amount;
    return 0;
  }
}
```

之后

```
function withdraw($amount) {
  if ($amount > $this->balance) {
    throw new BalanceException;
  }
  $this->balance -= $amount;
}

```

之前

```
def withdraw(self, amount):
    if amount > self.balance:
        return -1
    else:
        self.balance -= amount
    return 0
```

之后

```
def withdraw(self, amount):
    if amount > self.balance:
        raise BalanceException()
    self.balance -= amount
```

之前

```
withdraw(amount: number): number {
  if (amount > _balance) {
    return -1;
  }
  else {
    balance -= amount;
    return 0;
  }
}
```

之后

```
withdraw(amount: number): void {
  if (amount > _balance) {
    throw new Error();
  }
  balance -= amount;
}
```

### 为什么重构

返回错误代码是程序设计的过时遗留物。在现代编程中，错误处理由特殊类执行，这些类被称为异常。如果发生问题，你会“抛出”一个错误，然后由其中一个异常处理程序“捕获”它。在正常条件下被忽略的特殊错误处理代码被激活以作出响应。

### 好处

+   使代码摆脱大量检查各种错误代码的条件语句。异常处理程序是一种更加简洁的方式来区分正常执行路径和异常路径。

+   异常类可以实现自己的方法，从而包含部分错误处理功能（例如发送错误消息）。

+   与异常不同，错误代码不能在构造函数中使用，因为构造函数必须仅返回一个新对象。

### 缺点

+   异常处理程序可能变成一种类似于 goto 的拐杖。避免这样做！不要使用异常来管理代码执行。异常应仅用于通知错误或关键情况。

### 如何重构

尝试一次仅为一个错误代码执行这些重构步骤。这将更容易让你将所有重要信息牢记在心，避免错误。

1.  查找所有调用返回错误代码的方法，而不是检查错误代码，将其包装在 `try`/`catch` 块中。

1.  在方法内部，抛出异常，而不是返回错误代码。

1.  更改方法签名，使其包含有关抛出异常的信息（`@throws` 部分）。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读累了吗？

不奇怪，阅读我们这里所有文本需要 7 小时。

尝试我们的互动重构课程。这提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
