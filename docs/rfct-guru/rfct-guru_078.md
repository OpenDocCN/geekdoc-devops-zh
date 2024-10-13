# 用保护子句替换嵌套条件

> 原文：[`refactoringguru.cn/replace-nested-conditional-with-guard-clauses`](https://refactoringguru.cn/replace-nested-conditional-with-guard-clauses)

### 问题

您有一组嵌套条件，难以确定代码执行的正常流程。

### 解决方案

将所有特殊检查和边界情况隔离到单独的子句中，并将它们放在主要检查之前。理想情况下，您应该有一个“扁平”的条件列表，一个接一个。

之前

```
public double getPayAmount() {
  double result;
  if (isDead){
    result = deadAmount();
  }
  else {
    if (isSeparated){
      result = separatedAmount();
    }
    else {
      if (isRetired){
        result = retiredAmount();
      }
      else{
        result = normalPayAmount();
      }
    }
  }
  return result;
}
```

之后

```
public double getPayAmount() {
  if (isDead){
    return deadAmount();
  }
  if (isSeparated){
    return separatedAmount();
  }
  if (isRetired){
    return retiredAmount();
  }
  return normalPayAmount();
}
```

之前

```
public double GetPayAmount()
{
  double result;

  if (isDead)
  {
    result = DeadAmount();
  }
  else 
  {
    if (isSeparated)
    {
      result = SeparatedAmount();
    }
    else 
    {
      if (isRetired)
      {
        result = RetiredAmount();
      }
      else
      {
        result = NormalPayAmount();
      }
    }
  }

  return result;
}
```

之后

```
public double GetPayAmount() 
{
  if (isDead)
  {
    return DeadAmount();
  }
  if (isSeparated)
  {
    return SeparatedAmount();
  }
  if (isRetired)
  {
    return RetiredAmount();
  }
  return NormalPayAmount();
}
```

之前

```
function getPayAmount() {
  if ($this->isDead) {
    $result = $this->deadAmount();
  } else {
    if ($this->isSeparated) {
      $result = $this->separatedAmount();
    } else {
      if ($this->isRetired) {
        $result = $this->retiredAmount();
      } else {
        $result = $this->normalPayAmount();
      }
    }
  }
  return $result;
}
```

之后

```
function getPayAmount() {
  if ($this->isDead) {
    return $this->deadAmount();
  }
  if ($this->isSeparated) {
    return $this->separatedAmount();
  }
  if ($this->isRetired) {
    return $this->retiredAmount();
  }
  return $this->normalPayAmount();
}
```

之前

```
def getPayAmount(self):
    if self.isDead:
        result = deadAmount()
    else:
        if self.isSeparated:
            result = separatedAmount()
        else:
            if self.isRetired:
                result = retiredAmount()
            else:
                result = normalPayAmount()
    return result
```

之后

```
def getPayAmount(self):
    if self.isDead:
        return deadAmount()
    if self.isSeparated:
        return separatedAmount()
    if self.isRetired:
        return retiredAmount()
    return normalPayAmount()
```

之前

```
getPayAmount(): number {
  let result: number;
  if (isDead){
    result = deadAmount();
  }
  else {
    if (isSeparated){
      result = separatedAmount();
    }
    else {
      if (isRetired){
        result = retiredAmount();
      }
      else{
        result = normalPayAmount();
      }
    }
  }
  return result;
}
```

之后

```
getPayAmount(): number {
  if (isDead){
    return deadAmount();
  }
  if (isSeparated){
    return separatedAmount();
  }
  if (isRetired){
    return retiredAmount();
  }
  return normalPayAmount();
}
```

### 为什么要重构

识别“地狱条件”相对简单。每个嵌套层级的缩进形成一支箭头，指向痛苦与困惑的方向：

```
if () {
    if () {
        do {
            if () {
                if () {
                    if () {
                        ...
                    }
                }
                ...
            }
            ...
        }
        while ();
        ...
    }
    else {
        ...
    }
}

```

很难弄清楚每个条件的作用和如何运作，因为代码执行的“正常”流程并不明显。这些条件表明了混乱的演变，每个条件都是作为权宜之计添加的，而没有考虑到优化整体结构。

为简化情况，将特殊情况隔离到单独的条件中，如果保护子句为真，则立即结束执行并返回一个空值。实际上，您在这里的任务是使结构变得扁平。

### 如何重构

尝试消除代码中的副作用——将查询与修改分离可能对这个目的有帮助。这个解决方案对于下面描述的重组是必要的。

1.  将所有导致调用异常或立即返回值的保护子句隔离出来。将这些条件放在方法的开头。

1.  在重排完成并且所有测试成功后，查看是否可以使用合并条件表达式来处理导致相同异常或返回值的保护子句。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 读累了吗？

难怪，阅读这里所有文本需要 7 个小时。

尝试我们的交互式重构课程。它提供了一种不那么繁琐的学习新知识的方法。

*让我们看看…*
