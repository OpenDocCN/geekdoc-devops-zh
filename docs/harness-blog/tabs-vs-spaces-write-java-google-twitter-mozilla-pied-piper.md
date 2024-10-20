# Java | Harness 中的制表符与空格

> 原文：<https://www.harness.io/blog/tabs-vs-spaces-write-java-google-twitter-mozilla-pied-piper>

在本帖中，我们将重点介绍 Google、Twitter、Mozilla、Java standard 等公司的格式指南和不同的 Java 编码风格，以及我们自己在 Harness 的团队。

当谈到编码风格时，大多数选择都是相当随意的，取决于个人偏好。是的，即使制表符宽度在编辑器之间变化，空格也趋向于更加精确。如果有开发团队人类学这样的东西，风格指南可能是它的主要部分。

在本帖中，我们将重点介绍 Google、Twitter、Mozilla、Java standard 等公司的格式指南和不同的 Java 编码风格，以及我们自己在 Harness 的团队。

(无论你使用制表符还是空格，你在它们后面写的内容才是最重要的。代码质量至关重要，这就是为什么 Harness 会在运行时分析您的代码，从而为您提供关于所有错误和速度变慢的可操作的洞察力——甚至是那些没有在日志中结束的错误和速度变慢。)

## 为什么首先要使用指南？

可读性是这里的主要考虑因素。几乎可以肯定的是，你不会是唯一一个阅读你写的代码的人。对于下一个阅读你代码的人来说，你能做的最好的事情就是遵守约定。

一致的写作风格不仅有助于创建好看的代码，而且使代码更容易理解。twitter 指南规定了一个例外，我们倾向于同意，“如果更‘可读’的变体带有危险或陷阱，可读性可能会被牺牲”。

1.  [谷歌 Java 风格指南](https://google.github.io/styleguide/javaguide.html)(还有[另一款](http://source.android.com/source/code-style.html)安卓版)
2.  [推特风格指南](https://github.com/twitter/commons/blob/master/src/java/com/twitter/common/styleguide.md)
3.  [官方 Java 代码惯例](http://www.oracle.com/technetwork/java/codeconvtoc-136057.html)(新的 OpenJDK 指南可从[这里获得](http://cr.openjdk.java.net/~alundblad/styleguide/index-v6.html))
4.  [Mozilla 指南](https://developer.mozilla.org/en-US/docs/Mozilla/Developer_guide/Coding_Style#Java_practices)
5.  我们自己的指导方针

让我们看看他们有什么。

## 1.缩进:制表符与空格

首先，在继续之前，我们需要把这件事说出来。在样式指南中，空格明显优于制表符。我们在这里不讨论利弊，只分享研究结果:

*   Google: 2 个空格(android 是 4 个空格，8 个用于换行)
*   **Twitter:** 2 或 4 个空格(用于换行)
*   **Mozilla:** 4 个空格
*   **Java:** 4 个空格，制表符必须设置在 8 个空格处。两者都可以接受。

来自 Github 的数据表明，大约 10-33%的 Java 存储库喜欢制表符，大多数使用不同格式的空格，喜欢 4 个空格而不是 2 个。实际上有一个非常好的模块来运行这种分析(比较不同的语言)。顺便说一下，看看其他 JVM 语言，比如 Scala 和 Clojure，我们看到几乎 100%的 2 空格。

考虑到一个包含单个提交的更大的数据集，我们得到了不同的结果(约定分析项目，Github 数据挑战赛的获胜者之一)，但我们可以估计它在两者之间，可能接近 10%。

## 2.线条长度、换行和换行

有时 Java 代码行会变得很长，样式指南会设置何时适合中断或换行的约定。一般惯例是大约 80-100 最大长度

*   谷歌: 100 列
*   **Twitter:** 偏好 100 列
*   **Mozilla:** 恰当的判断
*   **爪哇:** 80 列

当然，除了分号后的自然换行，换行符不仅用于过长的行，也用于逻辑分隔。一般惯例是在逗号之后、运算符之前断开，并使用常识提示。

下面是 twitter 风格指南中的一个例子，很好地利用了这个概念:

###### //不好。
// -换行符是任意的。
// -扫码很难把信息拼凑起来。
抛出 new IllegalStateException("未能处理请求"+request . getid()
+" for user "+user . getid()+" query:' "+query . gettext()
+" ' ")；
//好。
// -消息的每个组成部分都是独立和自包含的。
// -添加或删除消息的一个组成部分需要最少的重新格式化。
throw new IllegalStateException("未能处理"
+" request "+request . getid()
+" for user "+user . getid()
+" query:" "+query . gettext()+" ' ")；

这有助于分离语句并创建一个逻辑，其中每行代码代表一个包含的/“原子”操作。风格指南倾向于同意这一点。

空行在混合中也有重要的作用，分隔逻辑块。Java 标准样式指南也提到了双换行符，将接口和实现分开。

## 3.变量命名

广泛认同所有风格指南。first letter 大写表示类名，camelCase 表示方法和变量名，所有小写包名和 ALL_CAPS 表示最终静态常量。

一个常见的例外是记录器，我们通常将其定义为:

私有静态最终记录器记录器=记录器工厂。获取记录器(类。类)；

Twitter 的指南增加了另一种有趣的方式，在变量名中包含单元:

###### //不好。
// -字段名很少透露字段的用途。
类用户{
private final int a；
私终串 m；
...
}
//好。
类用户{
private final int agein years；
private final String maidenName；
...
}

## 4.异常捕获条款

例外是一个棘手的问题。最近我们报道了一项研究，调查了 Github 和 Sourceforge 上的 600，000 多个项目，揭示了一些关于异常的非标准使用的严峻事实。Google 和 Twitter 的风格指南引用了臭名昭著的空 catch 块:

*   Google: 没有空的 catch 块
*   **推特:**换句话说，不要吞异常
*   **Mozilla:** 无参考
*   **Java:** 无引用

此外，我们建议至少要遵守的另一个准则是确保您的异常是可操作的，并避免控制流异常。生产环境中产生的所谓“正常”异常的数量是可怕的。

异常和错误处理的当前状态主要依赖于日志文件来找到它们在生产中的根源，这是我们构建 Harness 背后的主要动机。如果您还没有，请查看一下！我们很想听听你的想法。

## 5.为清晰起见，括号和花括号

即使括号不是必需的，它也有助于提高可读性。对于复合谓词，为了清晰起见，通常使用括号，即使执行顺序很明显。例如:

###### if(x = = y & & a > 10)//bad
if((x = = y)&&(a>10))//good

那么样式指南是怎么说圆括号分组的呢？

*   **谷歌:**“推荐”
*   **推特:**“直截了当”(即使很明显)
*   **Java:** “一般的好主意”
*   遵循 Java 标准

当处理花括号时，所有的样式指南都检查了在开始花括号后的支撑符断开。

## 6.构造函数中没有大脑

有一条我们遵守但没有在任何风格指南中找到的指导原则是不要在构造函数中保留任何“大脑”(这样它们就不会被僵尸攻击——但显然不会被低级笑话攻击)。

如果我们需要创建一个对象并做一些繁重的操作来构建它，我们使用 creator 方法来代替。例如:

###### //简单构造函数
//
private final int x；
private final int y；
私终串 z；
public my class(int x，int y，String z){
this . x = x；
this . y = y；
this . z = z；
}
//综合楼
//
public static my class create(List<Stuff>List，Set<Stuff>Set){
//int x = some brains...
// int y =更多的大脑...
//其他大脑...
// String z =这里更复杂的东西
//
返回新的 MyClass(x，y，z)；
}
私有 final int x；
private final int y；
私终串 z；
private MyClass(int x，int y，String z){
this . x = x；
this . y = y；
this . z = z；
}

## 最后的想法

还有很多风格指南，我们没有在这篇文章中涉及，以避免这是一个详尽的列表，它们都可以在文章开头链接的原始文档中找到。可读性是保持你的代码没有错误的一个主要因素，而且…保持这些强迫症的感觉没有刺痛是正确的。

你遵循的一些独特的指导方针/怪癖是什么？你的公司/团队使用你自己的风格指南吗？