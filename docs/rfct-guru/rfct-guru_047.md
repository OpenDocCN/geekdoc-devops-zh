# 替代算法

> 原文：[`refactoringguru.cn/substitute-algorithm`](https://refactoringguru.cn/substitute-algorithm)

### 问题

所以你想用一个新的算法替换现有的算法吗？

### 解决方案

用新算法替换实现算法的方法主体。

之前

```
String foundPerson(String[] people){
  for (int i = 0; i < people.length; i++) {
    if (people[i].equals("Don")){
      return "Don";
    }
    if (people[i].equals("John")){
      return "John";
    }
    if (people[i].equals("Kent")){
      return "Kent";
    }
  }
  return "";
}
```

之后

```
String foundPerson(String[] people){
  List candidates =
    Arrays.asList(new String[] {"Don", "John", "Kent"});
  for (int i=0; i < people.length; i++) {
    if (candidates.contains(people[i])) {
      return people[i];
    }
  }
  return "";
}
```

之前

```
string FoundPerson(string[] people)
{
  for (int i = 0; i < people.Length; i++) 
  {
    if (people[i].Equals("Don"))
    {
      return "Don";
    }
    if (people[i].Equals("John"))
    {
      return "John";
    }
    if (people[i].Equals("Kent"))
    {
      return "Kent";
    }
  }
  return String.Empty;
}
```

之前

```
string FoundPerson(string[] people)
{
  List<string> candidates = new List<string>() {"Don", "John", "Kent"};

  for (int i = 0; i < people.Length; i++) 
  {
    if (candidates.Contains(people[i])) 
    {
      return people[i];
    }
  }

  return String.Empty;
}
```

之前

```
function foundPerson(array $people){
  for ($i = 0; $i < count($people); $i++) {
    if ($people[$i] === "Don") {
      return "Don";
    }
    if ($people[$i] === "John") {
      return "John";
    }
    if ($people[$i] === "Kent") {
      return "Kent";
    }
  }
  return "";
}
```

之后

```
function foundPerson(array $people){
  foreach (["Don", "John", "Kent"] as $needle) {
    $id = array_search($needle, $people, true);
    if ($id !== false) {
      return $people[$id];
    }
  }
  return "";
}
```

之前

```
def foundPerson(people):
    for i in range(len(people)):
        if people[i] == "Don":
            return "Don"
        if people[i] == "John":
            return "John"
        if people[i] == "Kent":
            return "Kent"
    return ""
```

之后

```
def foundPerson(people):
    candidates = ["Don", "John", "Kent"]
    return people if people in candidates else ""

```

之前

```
foundPerson(people: string[]): string{
  for (let person of people) {
    if (person.equals("Don")){
      return "Don";
    }
    if (person.equals("John")){
      return "John";
    }
    if (person.equals("Kent")){
      return "Kent";
    }
  }
  return "";
}
```

之后

```
foundPerson(people: string[]): string{
  let candidates = ["Don", "John", "Kent"];
  for (let person of people) {
    if (candidates.includes(person)) {
      return person;
    }
  }
  return "";
}
```

### 为什么重构

1.  渐进式重构并不是改进程序的唯一方法。有时一个方法存在太多问题，以至于拆除该方法并重新开始更为简单。而且也许你找到了一种更简单、更高效的算法。如果是这样，你应该简单地用新算法替换旧算法。

1.  随着时间的推移，你的算法可能会被纳入一个知名的库或框架中，而你想要摆脱独立实现，以简化维护。

1.  你的程序的需求可能会发生重大变化，以至于你现有的算法无法用于该任务。

### 如何重构

1.  确保你已尽可能简化现有算法。使用提取方法将不重要的代码移动到其他方法中。你算法中的移动部分越少，更容易替换。

1.  在一个新方法中创建你的新算法。用新算法替换旧算法，然后开始测试程序。

1.  如果结果不匹配，请返回旧实现并比较结果。找出差异的原因。虽然原因往往是旧算法中的错误，但更可能是新算法中的某些部分未能正常工作。

1.  当所有测试成功完成后，彻底删除旧算法！

</images/refactoring/banners/tired-of-reading-banner-1x.mp4?id=7fa8f9682afda143c2a491c6ab1c1e56>

</images/refactoring/banners/tired-of-reading-banner.png?id=1721d160ff9c84cbf8912f5d282e2bb4>

你的浏览器不支持 HTML 视频。

### 厌倦阅读了吗？

难怪，阅读我们这里所有的文本需要 7 小时。

尝试我们的交互式重构课程。它提供了一种不那么乏味的学习新知识的方法。

*让我们看看……*
