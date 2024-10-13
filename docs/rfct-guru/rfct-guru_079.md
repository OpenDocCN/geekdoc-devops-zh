# 用多态性替换条件

> 原文：[`refactoringguru.cn/replace-conditional-with-polymorphism`](https://refactoringguru.cn/replace-conditional-with-polymorphism)

### 问题

您有一个根据对象类型或属性执行各种操作的条件。

### 解决方案

创建与条件分支匹配的子类。在这些子类中，创建一个共享方法，并将相应条件分支的代码移到其中。然后用相关方法调用替换条件。最终通过多态性将获得正确的实现，具体取决于对象类。

之前

```
class Bird {
  // ...
  double getSpeed() {
    switch (type) {
      case EUROPEAN:
        return getBaseSpeed();
      case AFRICAN:
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    throw new RuntimeException("Should be unreachable");
  }
}
```

之后

```
abstract class Bird {
  // ...
  abstract double getSpeed();
}

class European extends Bird {
  double getSpeed() {
    return getBaseSpeed();
  }
}
class African extends Bird {
  double getSpeed() {
    return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  double getSpeed() {
    return (isNailed) ? 0 : getBaseSpeed(voltage);
  }
}

// Somewhere in client code
speed = bird.getSpeed();
```

之前

```
public class Bird 
{
  // ...
  public double GetSpeed() 
  {
    switch (type) 
    {
      case EUROPEAN:
        return GetBaseSpeed();
      case AFRICAN:
        return GetBaseSpeed() - GetLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return isNailed ? 0 : GetBaseSpeed(voltage);
      default:
        throw new Exception("Should be unreachable");
    }
  }
}
```

之后

```
public abstract class Bird 
{
  // ...
  public abstract double GetSpeed();
}

class European: Bird 
{
  public override double GetSpeed() 
  {
    return GetBaseSpeed();
  }
}
class African: Bird 
{
  public override double GetSpeed() 
  {
    return GetBaseSpeed() - GetLoadFactor() * numberOfCoconuts;
  }
}
class NorwegianBlue: Bird
{
  public override double GetSpeed() 
  {
    return isNailed ? 0 : GetBaseSpeed(voltage);
  }
}

// Somewhere in client code
speed = bird.GetSpeed();
```

之前

```
class Bird {
  // ...
  public function getSpeed() {
    switch ($this->type) {
      case EUROPEAN:
        return $this->getBaseSpeed();
      case AFRICAN:
        return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
    }
    throw new Exception("Should be unreachable");
  }
  // ...
}
```

之后

```
abstract class Bird {
  // ...
  abstract function getSpeed();
  // ...
}

class European extends Bird {
  public function getSpeed() {
    return $this->getBaseSpeed();
  }
}
class African extends Bird {
  public function getSpeed() {
    return $this->getBaseSpeed() - $this->getLoadFactor() * $this->numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  public function getSpeed() {
    return ($this->isNailed) ? 0 : $this->getBaseSpeed($this->voltage);
  }
}

// Somewhere in Client code.
$speed = $bird->getSpeed();
```

之前

```
class Bird:
    # ...
    def getSpeed(self):
        if self.type == EUROPEAN:
            return self.getBaseSpeed()
        elif self.type == AFRICAN:
            return self.getBaseSpeed() - self.getLoadFactor() * self.numberOfCoconuts
        elif self.type == NORWEGIAN_BLUE:
            return 0 if self.isNailed else self.getBaseSpeed(self.voltage)
        else:
            raise Exception("Should be unreachable")
```

之后

```
class Bird:
    # ...
    def getSpeed(self):
        pass

class European(Bird):
    def getSpeed(self):
        return self.getBaseSpeed()

class African(Bird):
    def getSpeed(self):
        return self.getBaseSpeed() - self.getLoadFactor() * self.numberOfCoconuts

class NorwegianBlue(Bird):
    def getSpeed(self):
        return 0 if self.isNailed else self.getBaseSpeed(self.voltage)

# Somewhere in client code
speed = bird.getSpeed()
```

之前

```
class Bird {
  // ...
  getSpeed(): number {
    switch (type) {
      case EUROPEAN:
        return getBaseSpeed();
      case AFRICAN:
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    throw new Error("Should be unreachable");
  }
}
```

之后

```
abstract class Bird {
  // ...
  abstract getSpeed(): number;
}

class European extends Bird {
  getSpeed(): number {
    return getBaseSpeed();
  }
}
class African extends Bird {
  getSpeed(): number {
    return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  getSpeed(): number {
    return (isNailed) ? 0 : getBaseSpeed(voltage);
  }
}

// Somewhere in client code
let speed = bird.getSpeed();
```

### 为什么重构

如果您的代码包含根据以下内容执行各种任务的操作符，这种重构技术将有所帮助：

+   对象的类或它实现的接口

+   对象字段的值

+   调用对象方法之一的结果

如果出现新的对象属性或类型，您需要在所有类似条件中搜索并添加代码。因此，如果对象的所有方法中散布着多个条件，这种技术的好处将成倍增加。

### 好处

+   这种技术遵循*告知-不询问*原则：与其询问对象的状态并根据此执行操作，不如简单地告诉对象它需要做什么，让它自己决定如何执行。

+   消除重复代码。您摆脱了许多几乎相同的条件。

+   如果需要添加新的执行变体，只需添加一个新子类，而无需修改现有代码（*开放/封闭原则*）。

### 如何重构

#### 准备重构

对于这种重构技术，您应该有一个准备好的类层级，包含替代行为。如果没有这样的层级，请创建一个。其他技术将帮助实现这一目标：

+   用子类替换类型代码。将为特定对象属性的所有值创建子类。这种方法简单但灵活性较差，因为您无法为对象的其他属性创建子类。

+   用状态/策略替换类型代码。将为特定对象属性专门创建一个类，并为该属性的每个值从中创建子类。当前类将包含对这种类型对象的引用，并将执行委托给它们。

以下步骤假设您已经创建了层级结构。

#### 重构步骤

1.  如果条件在执行其他操作的方法中，请执行提取方法。

1.  对于每个层级子类，重定义包含条件的方法，并将相应条件分支的代码复制到该位置。

1.  从条件中删除此分支。

1.  重复替换直到条件为空。然后删除条件并将方法声明为抽象。

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

您的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

毫无疑问，阅读我们这里的所有文本需要 7 小时。

尝试我们的交互式重构课程。这提供了一种不那么乏味的学习新知识的方法。

*我们来看…*
