# 类固醇上的 Java 个超级有用的 JIT 优化技术

> 原文：<https://www.harness.io/blog/5-jit-optimization-techniques>

JVMs JIT 编译器是 Java 平台上令人着迷的机制之一。它优化了您的代码的性能，而没有放弃它的可读性。

有哪些最有用的 JVM JIT 优化，以及如何使用它们？

即使您没有积极地计划，JVM 也有不少锦囊妙计来帮助您的代码执行得更好。有些人不需要你做任何事情就可以工作，而其他人可能需要一点帮助。

## 一次编写，随处运行，及时优化

大多数人都知道，他们的 Java 源代码由 javac 编译器转换成字节码，然后由 JVM 运行，JVM 再将它编译成汇编，并提供给 CPU。
‍
知道兔子洞更深的人更少。JVM 有两种不同的操作模式。生成的字节码是原始 Java 源代码的准确表示，没有任何优化。当 JVM 将它翻译成汇编时，事情变得更加棘手，魔力开始显现:

1.  **解释模式**——JVM 读取并运行字节码本身。
2.  **编译模式(字节码到汇编)**——在这种模式下，JVM 放松了控制，不涉及字节码。

两者之间的纽带是 JIT 编译器。你可能已经猜到了，解释模式比没有中间人直接在 CPU 上运行要慢得多。解释模式为 Java 平台提供了一个宝贵的机会来收集关于代码及其实际行为的信息——让它有机会了解如何优化最终的汇编代码。
‍

运行足够长的时间后，JIT 编译器能够在汇编级执行优化。主要是根据代码的实际行为，最大限度地减少不必要的跳转，这些跳转会增加应用程序的大量开销。此外，这个过程不会在应用程序的“预热阶段”后停止，而是会发生多次，以进一步优化组装。
‍

在，我们正在构建一个 Java 代理来监控生产中的服务器，我们非常重视开销。每一点代码都得到了优化，在这个过程中，我们有机会使用和了解一些很酷的 JIT 特性。这里有 5 个更有用的例子。

## 1.零检查消除

在许多情况下，在开发过程中向代码中添加某些条件检查是非常有意义的。就像臭名昭著的 null 检查一样。nulls 得到特殊处理并不奇怪，因为它是生产中发生的顶级异常的原因。尽管如此，在大多数情况下，显式空检查可以从优化的汇编代码中消除，这适用于任何很少发生的情况。在极少数情况下，条件确实发生了，JIT 使用一种称为“不常见陷阱”的机制，让它知道它需要返回并通过拉回发生时需要执行的指令集来修复优化——而不会出现实际的 NullPointerException。
‍

这些检查是优化代码值得关注的原因，因为它们可能会将跳转引入汇编代码，从而降低其执行速度。
我们来看一个简单的例子:

###### 私有静态 void runsome algorithm(Graph Graph){
if(Graph = = null){
return；
}
//用图做点什么
}

如果 JIT 发现 graph 从不使用 null 调用，那么编译后的版本看起来就像是反映了没有使用 null 检查编写的代码:

###### private static void runsome algorithm(Graph Graph){
//用 graph
做点什么

‍ **底线:**我们不需要做任何特别的事情来享受这种优化，当一种罕见的情况发生时，JVM 会知道“去优化”并得到想要的结果。

## 2.分支预测

与空检查消除类似，还有另一种 JIT 优化技术，可以帮助它决定某些代码行是否比其他代码行更“热”和更频繁地发生。我们已经知道，如果某个条件很少或从不成立，很可能“非常规陷阱”机制会介入并将其从编译后的程序集中消除。如果有不同的平衡，If 条件的两个分支都成立，但一个比另一个发生得多，JIT 编译器可以根据最常见的一个重新排序它们，并显著减少汇编跳转的次数。

下面是它在实践中的工作方式:

###### private static int isOpt(int x，int y){
int veryHardCalculation = 0；

if(x>= y){
veryHardCalculation = x * 1000+y；
} else {
veryHardCalculation = y * 1000+x；
}
返回 veryHardCalculation
}

现在，假设大部分时间 x < y，条件将翻转，以统计方式减少装配跳跃次数:

###### private static int isOpt(int x，int y){
int veryHardCalculation = 0；

if(x<y){
//这样就不需要跳转了
veryHardCalculation = y * 1000+x；
返回 veryHardCalculation
} else {
veryHardCalculation = x * 1000+y；
返回 veryHardCalculation
}
}

如果你不相信，我们实际上已经运行了一个小测试，并提取了优化的汇编代码:

###### 0x 00007 FD 715062 d0s 0 c:CMP % EDX，% ESI
0x 00007 FD 715062 d0s 0 e:jge 0x 00007 FD 715062 d24；* if _ icmplt
：-opt::isot @ 4(线 117)

如您所见，跳转指令是 jge(如果大于或等于则跳转)，对应于条件的 else 分支。**底线:**另一个我们喜欢的开箱即用的 JIT 优化。我们还在最近的一篇关于一些最有趣的 Stackoverflow Java 答案的帖子中讨论了分支预测。在所示的示例中，我们看到了如何使用分支预测来解释为什么在有助于分支预测的特定条件下，在排序数组上运行操作要快得多。

## 3.循环展开

您可能已经注意到，JIT 编译器不断地试图消除编译代码中的汇编跳转。这就是为什么循环闻起来像麻烦。每一次迭代实际上是一次回到指令集开始的汇编跳转。通过循环展开，JIT 编译器打开循环，并一个接一个地重复相应的汇编指令。

理论上，javac 可以在将 Java 翻译成字节码时做类似的事情，但是 JIT 编译器对代码将在其上运行的实际 CPU 有更好的了解，并且知道如何以更有效的方式微调代码。

比如我们来看一个矩阵乘以向量的方法:

###### private static double[]loop unrolling(double[][]matrix 1，double[]vector 1){
double[]result = new double[vector 1 . length]；

for(int I = 0；I<matrix 1 . length；i++){
for(int j = 0；j <向量 1 .长度；j++){ result[I]+= matrix 1[I][j]* vector 1[j]；
}
}
返回结果；
}

展开的版本如下所示:

###### private static double[]loop unrolling 2(double[][]matrix 1，double[]vector 1){
double[]result = new double[vector 1 . length]；

for(int I = 0；I<matrix 1 . length；i++){
result[I]+= matrix 1[I][0]* vector 1[0]；
结果[I]+= matrix 1[I][1]* vector 1[1]；
结果[I]+= matrix 1[I][2]* vector 1[2]；
//也许它会进一步扩展——例如 4 次迭代，因此
//添加代码来修复索引
//我们会浪费更多的时间来正确有效地完成
}

返回结果；
}

一遍又一遍地重复同样的动作，没有头顶上的跳跃:

###### ...我...
0x 00007 FD 715060743:vmovsd 0x 10(% r8，%rcx，8)，% xmm 0；*达累斯萨拉姆
；-opt::loopunolling @ 26(line 179)
0x 00007 FD 71506074 a:vmovsd 0x 10(% RBP)，% xmm 1；*达累斯萨拉姆
；-opt::loopunolling @ 36(line 179)
0x 00007 FD 71506074 f:vmulsd 0x 10(% R12，% r9.8)，%xmm1，%xmm1
0x 00007 FD 715060756:vaddsd % xmm 0，% xmm 1，% xmm 0；* dadd
；-opt::loopunolling @ 38(line 179)
0x 00007 FD 71506075 a:vmovsd % xmm 0.0 X10(% r8，% rcx.8)；*起重机〔t8〕-opt::loophole @ 39(线 179)...我...

‍ **底线:**JIT 编译器在展开循环方面做得很好，只要你保持它们的内容简单，没有任何不必要的复杂。也不建议试图优化这个过程并自己展开它们，结果可能会导致比它解决的问题更多的问题。

## 4.内联方法

接下来，另一个跳跃杀手优化。汇编跳转的一个巨大来源实际上是方法调用。如果可能的话，JIT 编译器会尝试内联它们，消除来回跳转、发送参数和返回值的需要——将其全部内容传递给调用方法。还可以用两个 JVM 参数微调 JIT 编译器内联方法的方式:

1.  **-XX:MaxInlineSize**–可以内联的方法的字节码的最大大小(对任何方法都这样做，即使不经常执行)。默认值约为 35 字节。
2.  **-XX:FreqInlineSize**–被认为是热点(频繁执行)的方法的字节码的最大大小，应该被内联。默认值因平台而异。

例如，让我们来看看计算一条简单直线的坐标的方法:

###### private static void calcLine(int a，int b，int from，int to){
Line l = new Line(a，b)；
for(int x = from；x<= to；x++){
int y = l . gety(x)；
系统。呃。println("+x+"，"+y+")；

###### }
}
静态类线{
公共 final int a；
公开最终 int b；
public Line(int a，int b){
this . a = a；
this . b = b；
}

//内联
public int getY(int x){
return(a * x+b)；
}
}

优化的内联版本将消除跳转、参数 l 和 x 的发送以及 y 的返回:

###### private static void calcLine(int a，int b，int from，int to){
Line l = new Line(a，b)；
for(int x = from；x<= to；x++){
int y =(l . a * x+l . b)；
系统。呃。println("+x+"，"+y+")；
}
}

‍ **底线:**内联是一个非常有用的优化，但是只有当你用最少的行数保持你的方法尽可能简单时，它才会发挥作用。复杂的方法不太可能被内联，所以这是您可以帮助 JIT 编译器完成其工作的一点。

## 5.线程字段和线程本地存储(TLS)

事实证明，线程字段比常规变量快得多。线程对象存储在实际的 CPU 寄存器中，使其字段成为非常有效的存储空间。使用线程本地存储，您可以创建存储在线程对象上的变量。

下面是访问线程本地存储的方法，有一个请求计数器的简单例子:

###### 私有静态最终 thread local<integer>counter = new thread local<integer>()；

私有静态 void handle request(){
if(counter . get()= = null){
counter . set(0)；
}

counter . set(counter . get()+1)；</integer></integer>

在相应的汇编代码中，我们可以看到数据直接放在寄存器中，与静态类变量相比，数据访问速度更快:

###### 0x 00007 FD 71508 B1 EC:mov 0x1b 0(% r15)，% r10* invoke static current thread
；- java.lang.ThreadLocal::get@0(第 143 行)
；- Opt::handleRequest@3(第 70 行)
0x 00007 FD 71508 B1 f 3:mov 0x 50(% r10)，% r11d* getfield thread locals
；-Java . lang . thread local::get map @ 1(第 213 行)
；- java.lang.ThreadLocal::get@6(第 144 行)
；- Opt::handleRequest@3(第 70 行)

‍ **底线:**某些敏感数据类型最好存储在线程本地存储器上，以便更快地访问和检索。

## 最后的想法

JVMs JIT 编译器是 Java 平台上令人着迷的机制之一。它优化了您的代码的性能，而没有放弃它的可读性。不仅如此，除了内联的“静态”优化方法之外，它还根据代码实际执行的方式做出决策。我们希望您喜欢学习一些 JVM 最有用的优化技术。