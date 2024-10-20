# 谷歌番石榴:5 件你从不知道它会做的事|驾驭

> 原文：<https://www.harness.io/blog/google-guava-5-things>

谷歌番石榴有一些鲜为人知的功能，你可能会觉得有用。

每个开发者都可以使用的谷歌番石榴有哪些鲜为人知的功能？

这是最受欢迎的图书馆之一，它是开源的，你可能已经知道了，它来自一个人们把魁地奇作为一项真正的运动来玩的地方(至少在[实习](http://40.media.tumblr.com/d6a571ac9af15b7db1427b2c509909b9/tumblr_mv1fgztEv81s72vvio1_1280.jpg)的时候)。它不是《哈利·波特》中的霍格沃茨图书馆，但它确实有很多魔法:谷歌番石榴包含一系列核心 Java 库，这些库是在谷歌内部诞生的，在生产中经过了战斗测试，并公开发布。而且在 Java 8 上出现之前，它还有可选的[。](http://www.quickmeme.com/img/7a/7ab6a83c9d14593059c23dde8eb3fcb0e6e3b0364efec0798d06bcc581063d85.jpg)

‍
[番石榴](https://github.com/google/guava)的主要焦点是用实用程序改进围绕普通任务的工作流程，帮助编写更好、更干净的代码，并且更有生产力。最著名的是它的集合和缓存功能，它包含许多更有用但鲜为人知的特性。对于集合和缓存，它引入了对 JDKs 集合 API 的改进，并填补了去年最终[发布的缺失的 JCache 的空白。在这篇文章中，我想和你分享一些我们喜欢的谷歌番石榴的功能和一些我们刚刚做出的更有趣的发现。
*‍*](https://blogs.oracle.com/theaquarium/entry/jcache_is_final_i_repeat)

*注:番石榴支持 Java 6 及以上版本。*

## 1.无符号原语:它们存在！

Java 8 的一个鲜为人知的特性是针对 Integer 类中的无符号原语的一种新的变通方法。Guava 的一个更鲜为人知的特性是，所有这些在 Java 8 发布前几年就已经存在了，现在可以用于 Java 6 和更高版本。让我们来看看在番石榴中是如何处理的。摆在我们面前的有两个选择，我们必须保持一致:

‍
直接将原始类型作为 int 处理，记住在我们的逻辑中它是无符号的:

###### ‍
【Java】
int notreallyint = unsigned ints . parseunsigned int(4294967295)；//Max unsigned int
String Max unsigned = unsigned ints . tostring(notReallyInt)；//我们是合法的！
[/java]

‍
unsigned int 和 unsignedlongs 也支持比较、除法、最小、最大等方法。
避免直接处理原语并导致错误的包装器:

###### 
【Java】
无签名者 newType =无签名者. value of(max 无签名)；
new type = new type . plus(unsigned nteer . value of(" 1 "))；//increment
[/Java]

‍
UnsignedInteger 和 UnsignedLong 也支持像减、乘、除和模这样的方法。[在番石榴的维基上阅读更多信息](https://code.google.com/p/guava-libraries/wiki/PrimitivesExplained#Unsigned_support)

## 2.哈希:128 位 MurmurHash，用于获胜

当我们研究标准 Java 库的非加密哈希功能时，我们真正错过的一件事是 [MurmurHash](http://en.wikipedia.org/wiki/MurmurHash) 。它简单、快速、分布均匀，并且在许多语言中都有很好的支持。不要取代 Java 的 hashCode()，但是如果你需要生成很多散列，当 32 位不够用，你需要在不损害你的性能的情况下超快地完成的时候，它是很棒的。以下是番石榴的情况:

###### ‍
【Java】
hash function HF = hashing . mur mur 3 _ 128()；//也有 32 位版本
HashCode HC = HF . new hasher()
。
普特龙(id)。putString(名称，字符集。
_ 8)。putObject(person，personFunnel)
。hash()；
[/java]

分解对象是通过一个漏斗完成的，这个漏斗包括如何读取对象的指令，所以如果我们有一个人的 id、姓名和出生年份:

###### ‍
【Java】
漏斗<人>人漏斗=新漏斗<人> () {
@Override
公共无效漏斗(Person person，PrimitiveSink into) {
into
。putInt(person.id)
。putString(person.firstName，Charsets。
【UTF _ 8】。putString(person.lastName，Charsets。UTF_8)
。putInt(出生年份)；
}
}；
[/java]

‍
[在番石榴的维基上阅读更多内容](https://code.google.com/p/guava-libraries/wiki/HashingExplained)

## 3.InternetDomainName:将替换您的域名验证器

Guava 的另一个很酷的小工具是 InternetDomainName，毫无疑问，它可以帮助解析和操作域名。如果您曾经自己编写过类似的实用程序，您将会体会到它是如何以一种优雅的方式快速解决这个问题的。并且根据更新的 RFC 规范是有效的，使用来自 Mozilla 基金会发起的[公共后缀列表](https://publicsuffix.org/)的域列表。总的来说，它还有比 apache-commons 验证器更具体的方法。让我们看一个简单的例子:

###### ‍
【Java】
互联网域名所有者=
互联网域名. from("blog.overops.com ")。topPrivateDomain()；//返回 takipi.com
internet domain name . is valid(" taki pi . monsters ")；//返回 false
[/java]

‍
关于域名，有几个概念可能会让人混淆:

‍
public suffix()–根据公共后缀列表，顶级域名是一个独立的实体。所以我们会得到像 co.uk 这样的结果。com，。cool(没错，是实打实的后缀和[javais . cool](http://www.javais.cool/)&[Scala is . cool](http://www.scalais.cool/))。

‍
topprivatedomain()–根据[公共后缀列表(PSL)](https://publicsuffix.org/list/effective_tld_names.dat) 作为独立实体的顶级域名。在 blog.overops.com 上应用它会返回 takipi.com，但如果你在 Github pages 网站上尝试，username.github.io 会返回 username.github.io，因为它是出现在 PSL 上的一个独立实体。

‍
当你需要验证域时，这个工具就派上了用场，比如我们最近添加到 Takipi 的 JIRA 集成，在连接到 Takipi 的生产错误分析工具之前，我们首先检查你的 JIRA 主机。

‍
[在番石榴的维基上阅读更多内容](https://code.google.com/p/guava-libraries/wiki/InternetDomainNameExplained)

## 4.类路径反射:墙上的镜像

当检查 Java 的反射能力，即检查我们自己的代码的能力时，您会发现没有简单的方法来获得您的包或项目中所有类的列表。这是我们非常喜欢的 Guava 特性之一，因为它有助于获得关于您正在运行的环境的更多信息。工作原理就是这么简单:

###### ‍
【Java】
class path class path = class path . from(class loader)；
为(ClassPath。class info class info:class path . gettop level classes(" com . my comp . my package "){
system . out . println(class info . getname())；
}
[/java]

‍
这个片段将遍历并打印出我们指定的包中的所有类名。这里值得一提的是，扫描只包括物理上位于我们提到的包下的类。它不包括从其他地方加载的类，所以要小心使用它，因为它有时会给你一个不完整的图片。

‍
[在番石榴的维基上阅读更多内容](https://code.google.com/p/guava-libraries/wiki/ReflectionExplained#ClassPath)

## 5.CharMatcher:简化的正则表达式？

让我们以另一个我相信你会认识到的问题来结束这篇专题综述。你有一个字符串或一系列字符串，你想用某种格式，删除空白或其他字符，替换一个特定的字符，点画数字或什么的。一般来说，抓取匹配某种模式的字符，然后用它做一些事情。在这里，番石榴提供了 CharMatcher 方法优雅地处理这类问题。

‍
对于这个任务，我们有一些预定义的模式，比如 JAVA_UPPER_CASE(大写字符)、JAVA_DIGIT(数字)、INVISIBLE(不可见的 unicode 字符)等等。除了预定义的模式之外，我们可以自己尝试并创建我们自己的模式。让我们通过一个简单的代码示例来看看这是如何工作的:

‍
string spaced = charmatcher .空白。trimandcallapsefom(string，' ')；

‍
这将从字符串末尾开始修剪所有的空白，并将所有后续的空白合并成一个。

###### ‍
【Java】
字符串保持 Alex = charmatcher。任何一个(“亚历克斯”).retain from(someOtherString)；
[/java]

‍
这一行将获取一个字符串，并去掉所有没有出现在我名字中的字符。如果我将来会成为一名说唱歌手，我所有的歌都是这样开始的🙂
[阅读更多关于番石榴的维基](https://code.google.com/p/guava-libraries/wiki/StringsExplained#CharMatcher)

## 结论

我们已经看到了谷歌番石榴的一些最有趣的功能，不包括流行的收藏和缓存库。其中一些在 [OverOps](https://www.overops.com/) 中被大量使用，其他的是我们认为许多项目可以从中受益的有用的东西。谷歌番石榴有助于开发人员提高工作效率，这正是我们 OverOps 希望通过我们正在开发的工具实现的目标(顺便说一句，这些工具超级酷，但嘿，我可能有偏见)。
我们很想知道，你还使用了哪些大多数开发者没有使用的番石榴特性？(收藏和缓存不算！).