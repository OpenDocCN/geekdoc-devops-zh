# 基准测试:误用流如何让你的代码慢 5 倍

> 原文：<https://www.harness.io/blog/misusing-streams-make-code-slower>

流是 Java 8 中一个令人兴奋的特性，但是在使用时需要小心。

与长期实现相比，Java 8 lambdas 和 streams 的性能如何？

Lambda 表达式和流在 Java 8 中受到了热烈欢迎。这些是很长一段时间以来 Java 中最令人兴奋的特性。新的语言特性允许我们在代码中采用功能性更强的风格，我们从中获得了很多乐趣。太有趣了，这应该是非法的。后来我们起了疑心，决定对他们进行测试。

我们做了一个简单的任务，在数组列表中寻找最大值，并测试了长期实现和 Java 8 中的新方法。老实说，结果相当令人惊讶。 *‍*

## Java 8 中命令式与函数式编程的比较

我们喜欢开门见山，所以让我们看看结果。对于这个基准测试，我们创建了一个 ArrayList，用 100，000 个随机整数填充它，并实现了 7 种不同的方法来遍历所有的值以找到最大值。实现分为两组:函数式风格和命令式风格，前者包含 Java 8 中引入的新语言特性，后者包含由来已久的 Java 方法。

## 外卖食品

1.  哎呦！用 Java 8 提供的任何新方法实现一个解决方案都会导致大约 5 倍的性能损失。有时使用一个简单的循环和迭代器比混合使用 lambdas 和 streams 要好。即使这意味着多写几行代码，跳过那些甜蜜的语法。
2.  使用迭代器或 for-each 循环是遍历数组列表最有效的方法。比索引为 int 的传统 for 循环好两倍。
3.  在 Java 8 方法中，使用并行流被证明是更有效的。但是要小心，在某些情况下，它可能会降低你的速度。
4.  Lambas 在流和 parallelStream 实现之间占据了它们的位置。这有点令人惊讶，因为它们的实现是基于流 API 的。
5.  **【EDIT】事情并不总是像看起来那样:**虽然我们想展示在 lambdas 和 streams 中引入错误是多么容易，但我们收到了许多社区反馈，要求对基准代码进行更多优化，并删除整数的装箱/取消装箱。包括优化在内的第二组结果可以在这篇文章的底部找到。

## 等等，我们到底测试了什么？

让我们从最快到最慢的顺序快速浏览一下每种方法:

### 命令式风格

**iteratorMaxInteger()**–用迭代器遍历列表:

**【forEachLoopMaxInteger()**–丢失迭代器，用 [For-Each 循环遍历列表](http://docs.oracle.com/javase/6/docs/technotes/guides/language/foreach.html)(不要和 Java 8 forEach 混淆):

**forMaxInteger()**–用简单的 For 循环和 int 索引遍历列表:

### 功能风格

**parallelStreamMaxInteger()**–在并行模式下使用 Java 8 流检查列表:

**【lambdaMaxInteger()**–使用流的 lambda 表达式。甜蜜的一行程序:

**forEachLambdaMaxInteger()**——这个对于我们的用例来说有点乱。新的 Java 8 forEach 特性最令人讨厌的地方可能是它只能使用 final 变量，所以我们创建了一个 final 包装类来访问我们正在更新的最大值:顺便说一下，如果我们已经在谈论 forEach，请查看我们遇到的这个 StackOverflow 答案，它提供了一些关于它的一些缺点的有趣见解。
**streamMaxInteger()**–使用 Java 8 流查看列表:

## 优化基准

根据这篇文章的反馈，我们创建了另一个版本的基准。所有与原始代码的不同之处都可以在这里看到[。](https://github.com/takipi/loops-jmh-playground/commit/d91c55aee2d480c31fa7fad72f6f0b94fb59612e)

### TL；DR:变更摘要

1.  这份名单不再是不稳定的。
2.  新方法 forMax2 移除了字段访问。
3.  forEachLambda 中冗余的 helper 函数是固定的。现在 lambda 也赋值了。可读性较差，但速度更快。
4.  自动装箱被取消。如果在 Eclipse 中打开项目的自动装箱警告，旧代码有 15 个警告。
5.  通过在 reduce 之前使用 mapToInt 修复了流代码。

感谢[帕特里克·莱因哈特](https://github.com/reinhapa)、[理查德·沃伯顿](https://github.com/RichardWarburton)、[燕·邦纳](https://github.com/ybonnel)、[谢尔盖·库先科](https://twitter.com/kuksenk0)、[杰夫·马克斯韦尔](https://github.com/jmax01)、[亨里克·古斯塔夫松](https://github.com/gsson)以及所有在推特上发表评论和做出贡献的人！

## 基础工作

为了运行这个基准测试，我们使用了 Java 微基准测试工具 JMH。

基准配置包括 JVM 的 2 个分支、5 次预热迭代和 5 次测量迭代。测试在一个 c3.xlarge Amazon EC2 实例(4 个 vCPUs，7.5 个 Mem (GiB)，2 个 40 GB SSD 存储)上运行，使用 Java 8u66 和 JMH 1.11.2。GitHub 上有完整的源代码[，你可以在这里](https://github.com/takipi/loops-jmh-playground/blob/master/src/main/java/com/takipi/oss/benchmarks/jmh/loops/LoopBenchmarkMain.java)查看原始结果输出[。](http://pastebin.com/fB1jirqU)

尽管如此，一个小小的免责声明:基准测试往往是相当危险的，而且要做到正确是非常困难的。虽然我们试图以最准确的方式运行它，但我们总是建议对结果有所保留。

## 最后的想法

当你使用 Java 8 时，首先要做的是尝试 lambda 表达式和流。但是要小心:这感觉真的很好很甜，所以你可能会上瘾！我们已经看到，使用迭代器和 for-each 循环坚持更传统的 Java 编程风格明显优于 Java 8 提供的新实现。当然，情况并不总是如此，但在这个相当常见的例子中，它显示情况可能会糟糕 5 倍左右。如果它影响了系统的核心部分或者产生了新的瓶颈，这可能会变得非常可怕。