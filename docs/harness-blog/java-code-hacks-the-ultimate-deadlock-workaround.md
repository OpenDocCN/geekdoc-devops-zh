# Java 代码黑客——终极死锁解决方法| Harness

> 原文：<https://www.harness.io/blog/java-code-hacks-the-ultimate-deadlock-workaround>

Jstack 死锁的创造性解决方法。

**快速和肮脏的死锁解决方案，以摆脱那些线程**

到底什么是代码黑客？我们问过开发人员:你使用的最有用的调试技巧是什么，你做了哪些大多数开发人员都不知道的事情，以及你是如何解决困扰你太久的问题的。基本上，任何非直截了当的解决方法都是用一块口香糖和一个回形针。

这篇文章是这个系列的第一个故事，从 Akamai 的首席系统软件工程师 Uri Shamay 开始，他分享了一个关于死锁疯狂的故事。

## 该设置

### 10 年遗留下来的未记录在案的 Dode 带有一丝僵局

在我之前工作的一家公司，我们正在开发一个系统，该系统极大地减少了银行某些业务逻辑的交付时间。过去需要 30 天才能完成的操作现在只需一天。这个流程从一个验证过程开始，在这个过程中，系统接收一个客户，并根据外部政府 API 和内部 API 运行各种检查。为了获得这些数据，旧模型的实现通常为每个 API 获取调用一个本地线程。

一位技术负责人认为这种 API 调用模型无法扩展，所以他转向了一种更新的实现，使用 Java NIO 的非阻塞 I/O，这使他可以只创建几个基于 CPU 架构的线程。新的复杂流还包含 JNI 和 dll，用于在属于消息队列系统的本地路径上发生更改时发出文件系统通知。

## 问题是

### Jstack 死锁的奇怪情况

新的部署出现了意外的行为，但是以前负责的技术主管辞职了，所以没有人知道源代码在哪里以及它实际上做了什么。银行的授权政策要求我们使用现有的 API 库从政府系统中获取数据。每隔几个小时，它就会停止运行，jstack 告诉我们这里有一个死锁。实际上，当我们使用这些库时，有很多死锁，但是我们从来没有完全理解为什么，因为我们在 jstack 输出中没有看到任何模式。最重要的是，有些代码只是 jar、JNI 和 DLL，没有源文件，也没有文档，所以我使用 DJ–Java 反编译器来处理 jar，使用 WinDbg 来拦截 DLL。

死锁调查很难，而您不熟悉的代码中的死锁更难。但是其他开发人员不理解并发模式时发生的死锁才是最难的！当我开始调查生产中的死锁时，我确信这只是一个小问题，我可以很快修复它。这种乐观很快成为 R&D 团队的一个笑话，许多开发人员在没有人真正理解死锁的模式和根本原因的情况下查看 jstack 输出。

我们寻找开源工具来获取更多对我们有帮助的信息，并尝试了一些跟踪工具，但都没有用。我们试图获得系统的状态，并在 QA 环境中再现它，但是我们模拟的数据不够多样化，所以死锁没有再次出现。由于监管政策导致无法提供真实的生产数据，我们走进了死胡同。
[adrotate group="11 "

### “我们不想让 DevOps 杀了我们”

经过几个月的问题和半夜手动重启，jokes 24x7 的笑话，预算问题和项目新的严格期限，我们决定重写有问题的库。它们包含了太多的问题，以至于我们需要退回到旧版本，对每个调用使用本地线程。经过对代码的深入分析，我们明白这不是我们需要的行为。不要逃避重写庞大的代码库。现在我们正在从头开始重写一切。

## 变通办法

所以，我们决定重写一切，但同时，我们不想 devops 杀了我们，所以我们写了一些 JMX 代码，轮询“findDeadlockedThreads ”,并在它发生时自动重启。这就是我们在遇到死锁后自动重启应用程序的方式。DeadLockDetector 线程轮询 findDeadlockedThreads 的 JMX，并在检测到死锁时由 System.exit($CODE)粗暴地退出:

###### [java]
公共静态类 DeadLockDetector 扩展 Thread {
@Override
公共 void run(){
while(true){
ThreadMXBean ThreadMXBean = management factory . getthreadmxbean()；
long[]ids = threadmxbean . findeadlockedthreads()；
如果(ids！= null){
system . exit(127)；
}
试试{
thread . sleep(1000)；
} catch(interrupted exception e){ }
}
}
}
[/Java]

然后，错误代码被返回到主循环，如果是死锁代码，应用程序将从头开始再次执行。注意，如果您没有源代码或者您想在一个活动系统上进行远程管理，您可以使用程序外部的 JMX:

###### [Java]
public static void main(String[]args)抛出异常{
Process Process = null；
int exit code = 0；
try {
while(true){
process = runtime . get runtime()。exec("java -cp。DeadlockedApp ")；
exit code = process . wait for()；
System.out.println("退出代码:"+exit code)；
if(exit code = = 0)
break；
}
} catch(异常 e) {
if(进程！= null){
process . destroy()；
}
}
}
[/java]

‍
**带有模拟死锁的完整代码示例可在 [GitHub](https://github.com/takipi/java-deadlock-workaround) 上获得。