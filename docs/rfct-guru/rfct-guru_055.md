# 引入外部方法

> 原文：[`refactoringguru.cn/introduce-foreign-method`](https://refactoringguru.cn/introduce-foreign-method)

### 问题

实用类不包含你需要的方法，你不能将方法添加到该类中。

### 解决方案

将方法添加到客户端类中，并将实用类的对象作为参数传递给它。

之前

```
class Report {
  // ...
  void sendReport() {
    Date nextDay = new Date(previousEnd.getYear(),
      previousEnd.getMonth(), previousEnd.getDate() + 1);
    // ...
  }
}
```

之后

```
class Report {
  // ...
  void sendReport() {
    Date newStart = nextDay(previousEnd);
    // ...
  }
  private static Date nextDay(Date arg) {
    return new Date(arg.getYear(), arg.getMonth(), arg.getDate() + 1);
  }
}
```

之前

```
class Report 
{
  // ...
  void SendReport() 
  {
    DateTime nextDay = previousEnd.AddDays(1);
    // ...
  }
}
```

之后

```
class Report 
{
  // ...
  void SendReport() 
  {
    DateTime nextDay = NextDay(previousEnd);
    // ...
  }
  private static DateTime NextDay(DateTime date) 
  {
    return date.AddDays(1);
  }
}
```

之前

```
class Report {
  // ...
  public function sendReport() {
    $previousDate = clone $this->previousDate;
    $paymentDate = $previousDate->modify("+7 days");
    // ...
  }
}
```

之后

```
class Report {
  // ...
  public function sendReport() {
    $paymentDate = self::nextWeek($this->previousDate);
    // ...
  }
  /**
   * Foreign method. Should be in Date.
   */
  private static function nextWeek(DateTime $arg) {
    $previousDate = clone $arg;
    return $previousDate->modify("+7 days");
  }
}
```

之前

```
class Report:
    # ...
    def sendReport(self):
        nextDay = Date(self.previousEnd.getYear(),
            self.previousEnd.getMonth(), self.previousEnd.getDate() + 1)
        # ...
```

之后

```
class Report:
    # ...
    def sendReport(self):
        newStart = self._nextDay(self.previousEnd)
        # ...

    def _nextDay(self, arg):
        return Date(arg.getYear(), arg.getMonth(), arg.getDate() + 1)
```

之前

```
class Report {
  // ...
  sendReport(): void {
    let nextDay: Date = new Date(previousEnd.getYear(),
      previousEnd.getMonth(), previousEnd.getDate() + 1);
    // ...
  }
}
```

之后

```
class Report {
  // ...
  sendReport() {
    let newStart: Date = nextDay(previousEnd);
    // ...
  }
  private static nextDay(arg: Date): Date {
    return new Date(arg.getFullYear(), arg.getMonth(), arg.getDate() + 1);
  }
}
```

### 为什么要重构

你有代码使用某个类的数据和方法。你意识到代码在该类的新方法中看起来和工作得会更好。但你无法将方法添加到类中，因为，例如，该类位于第三方库中。

当你想将代码移到方法中时，如果代码在程序的不同地方重复多次，这种重构会带来很大的回报。

由于你将实用类的对象传递给新方法的参数，你可以访问其所有字段。在方法内部，你可以做几乎所有你想做的事情，就像该方法是实用类的一部分一样。

### 好处

+   消除代码重复。如果你的代码在多个地方重复，可以用方法调用替换这些代码片段。即使考虑到外部方法位于次优位置，这种做法也优于重复。

### 缺点

+   在客户端类中有实用类的方法并不总是对维护代码的人来说是清晰的。如果该方法可以在其他类中使用，你可以通过为实用类创建一个包装器并将方法放在那里来获益。当有多个这样的实用方法时，这也会很有帮助。引入本地扩展可以帮助解决这个问题。

### 如何重构

1.  在客户端类中创建一个新方法。

1.  在此方法中创建一个参数，以传递实用类的对象。如果可以从客户端类中获得该对象，则不必创建这样的参数。

1.  将相关代码片段提取到此方法中，并用方法调用替换它们。

1.  一定要在方法的注释中保留*外部方法*标签，并建议如果将来可能的话，将该方法放入实用类中。这将使未来维护软件的人更容易理解该方法为何位于此特定类中。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

不奇怪，阅读我们这里所有的文本需要 7 小时。

尝试我们的互动重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
