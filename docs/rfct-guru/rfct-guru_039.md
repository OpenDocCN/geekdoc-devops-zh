# 提取方法

> 原文：[`refactoringguru.cn/extract-method`](https://refactoringguru.cn/extract-method)

### 问题

你有一个可以组合在一起的代码片段。

### 解决方案

将这段代码移动到一个单独的新方法（或函数）中，并用对该方法的调用替换旧代码。

之前

```
void printOwing() {
  printBanner();

  // Print details.
  System.out.println("name: " + name);
  System.out.println("amount: " + getOutstanding());
}
```

之后

```
void printOwing() {
  printBanner();
  printDetails(getOutstanding());
}

void printDetails(double outstanding) {
  System.out.println("name: " + name);
  System.out.println("amount: " + outstanding);
}
```

之前

```
void PrintOwing() 
{
  this.PrintBanner();

  // Print details.
  Console.WriteLine("name: " + this.name);
  Console.WriteLine("amount: " + this.GetOutstanding());
}
```

之后

```
void PrintOwing()
{
  this.PrintBanner();
  this.PrintDetails();
}

void PrintDetails()
{
  Console.WriteLine("name: " + this.name);
  Console.WriteLine("amount: " + this.GetOutstanding());
}
```

之前

```
function printOwing() {
  $this->printBanner();

  // Print details.
  print("name:  " . $this->name);
  print("amount " . $this->getOutstanding());
}
```

之后

```
function printOwing() {
  $this->printBanner();
  $this->printDetails($this->getOutstanding());
}

function printDetails($outstanding) {
  print("name:  " . $this->name);
  print("amount " . $outstanding);
}
```

之前

```
def printOwing(self):
    self.printBanner()

    # print details
    print("name:", self.name)
    print("amount:", self.getOutstanding())
```

之后

```
def printOwing(self):
    self.printBanner()
    self.printDetails(self.getOutstanding())

def printDetails(self, outstanding):
    print("name:", self.name)
    print("amount:", outstanding)
```

之前

```
printOwing(): void {
  printBanner();

  // Print details.
  console.log("name: " + name);
  console.log("amount: " + getOutstanding());
}
```

之后

```
printOwing(): void {
  printBanner();
  printDetails(getOutstanding());
}

printDetails(outstanding: number): void {
  console.log("name: " + name);
  console.log("amount: " + outstanding);
}
```

### 为什么重构

方法中的行数越多，越难以弄清楚该方法的功能。这是进行这种重构的主要原因。

除了消除代码中的粗糙边缘，提取方法也是许多其他重构方法中的一步。

### 好处

+   更可读的代码！务必给新方法一个描述其目的的名称：`createOrder()`、`renderCustomerInfo()`等。

+   更少的代码重复。通常，方法中的代码可以在程序的其他地方重复使用。因此，你可以用对新方法的调用替换重复代码。

+   隔离代码的独立部分，这意味着出错的可能性较小（例如，如果错误地修改了变量）。

### 如何重构

1.  创建一个新方法，并以使其目的显而易见的方式命名。

1.  将相关的代码片段复制到你的新方法中。从旧位置删除该片段，并在其中放置对新方法的调用。

    找到这个代码片段中使用的所有变量。如果它们在片段内部声明并且不在外部使用，则可以保持不变——它们将成为新方法的局部变量。

1.  如果变量是在你要提取的代码之前声明的，你需要将这些变量传递给新方法的参数，以便使用它们之前包含的值。有时通过使用用查询替换临时变量来去掉这些变量更为简单。

1.  如果你看到在提取的代码中局部变量以某种方式发生了变化，这可能意味着这个变化的值在你的主方法中之后会被需要。请仔细检查！如果确实如此，将这个变量的值返回给主方法以保持一切正常。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 读腻了吗？

难怪，阅读我们这里的所有文本需要 7 小时。

尝试我们的交互式重构课程。这提供了一种更轻松的学习新知识的方法。

*我们来看看…*
