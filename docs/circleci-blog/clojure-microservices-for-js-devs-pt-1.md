# 面向 JavaScript 开发者的 Clojure 微服务

> 原文：<https://circleci.com/blog/clojure-microservices-for-js-devs-pt-1/>

这个系列由泰勒·苏伯格和穆萨·巴里格扎伊共同撰写。

CircleCI 在成长，这太棒了。然而，我们面临的一个增长挑战是，我们的后端主要是用 Clojure 编写的，很少有开发人员知道 Clojure。包括我自己在内的许多 CircleCI 工程师都在工作中学习了 Clojure。

在加入 CircleCI 之前，我是一名 JavaScript 开发人员。作为软件工程师的通用语言，JavaScript 是一种相对简单易学的语言。当然，它有(许多)怪癖，但这些都被出色的文档、无数的教程帖子和高度审查的堆栈溢出答案所掩盖。

相比之下，Clojure 的环境就不那么友好了。一旦学会了如何在括号的密集森林中导航，就很容易学会编写 Clojure 函数的基础知识。然而，构建可用的微服务有一个陡峭的学习曲线。在 Clojure 之旅的这个阶段，开发人员几乎没有什么资源。

这是展示如何设置 Clojure 微服务的系列文章的第一部分:

在这些帖子中，我们将使用 JavaScript 作为一个比较点来理解 Clojure 的新特性和有趣之处。我们不会讨论 ClojureScript ，一个面向 JavaScript 的 Clojure 编译器(应该有自己的博文)。我的假设是您已经知道编写 Clojure 函数的基础，所以我们可以直接开始设置一个简单的 Clojure 微服务。

到本系列结束时，我们希望您会发现编写 Clojure 微服务可以变得简单而愉快。

## Clojure vs JavaScript

在我们开始创建您的第一个 Clojure 微服务之前，我们应该探索一下 Clojure 和 JavaScript 之间的主要区别和相似之处。这将帮助我们在心理上准备好用 Clojure 而不是 JavaScript 思考意味着什么。

### 函数式编程

Clojure 设计的基本前提是，开发人员在面向对象程序中创造复杂性的能力远远超过他们理解这种复杂性的能力。Clojure 的要点是尽可能简单地获取、转换和移动数据。

这在 Clojure 中的主要表现方式是通过函数式编程风格。幸运的是，如果您来自 JavaScript，这种函数式风格会有些熟悉。毕竟函数在 JavaScript 中是一流的，现代 web 开发(如 [React](https://reactjs.org/) )往往是用函数风格编写的。事实上，Clojure 据称是 [React 的设计](https://youtu.be/VSdnJDO-xdg?t=571)的灵感来源。

有*种在 JavaScript 中使用的*种常见模式，但您不会在 Clojure 中看到:

*   创建大量对象来管理复杂的过程，如维护数据库连接
*   在对象中定义函数作为命名函数的一种方式
*   维护对象中的状态

这实际上意味着，至少根据 Peter Norvig 的估计，经典的[“四人帮”的 23 种设计模式中的 16 种在功能范例中已经过时了。在本系列的](https://www.amazon.com/Design-Patterns-Object-Oriented-Addison-Wesley-Professional-ebook/dp/B000SEIBB8)[下一篇文章](https://circleci.com/blog/clojure-microservices-for-js-devs-pt-2/)中，我们将探索我们的微服务如何在没有传统对象的情况下管理复杂的进程，如数据库或 RabbitMQ 连接。

#### 不变

在面向对象的语言中，对象是通过引用传递的。对象的可变性会使您的 JavaScript 应用程序更难预测，更难调试。

```
const cheese = { foodGroup: "dairy" };
const limburger = cheese;
limburger.smellsBad = true;
console.log(cheese);
// {foodGroup: "dairy", smellsBad: true} 
```

在 JavaScript 中，可以用不变性库来解决这个问题；比如 [Immutable.js](https://immutable-js.github.io/immutable-js/) 、 [Lodash](https://lodash.com/) ，或者 [Ramda](https://ramdajs.com/) 。在 Clojure 中，默认情况下数据是不可变的。

```
(def cheese {:food-group "dairy"})
(def limburger (assoc cheese :smells-bad true))
(println cheese)
; {food-group "dairy"} 
```

不可变的数据结构使我们更容易编写纯粹的、易于测试的函数。这一事实将决定我们如何在微服务中处理组合功能。与 JavaScript 相比，Clojure 倾向于用更小的纯函数编写，这些函数被重用并重新组合成更大的函数。

#### 动态类型

当然了。JavaScript 也是动态类型化的，所以我们可能期望 Clojure 有一个等价的 TypeScript。事实并非如此。

有一个名为 [Clojure.spec](https://clojure.org/guides/spec) 的 Clojure 核心库，乍一看像是 Clojure 的 TypeScript。但是，Clojure.spec 不是类型系统，与 TypeScript 有重要的区别:

*   Clojure.spec 是为灵活性和动态组合数据而设计的。它不像 TypeScript 那样严格，但是让您能够以几乎任何您可以想象的方式定义数据应该是什么样子。例如，假设一个函数的返回类型应该是一个质数。
*   Clojure.spec 在运行时而不是编译时运行。这意味着您不会在 IDE 中得到参数与所需类型不匹配的提示，但这确实意味着您可以使用 Clojure.spec 来检查生产中的类型(例如，实时验证来自 API 的数据)
*   Clojure.spec 允许您自动生成测试，而不是编写单独的单元测试。

简而言之，Clojure 中的动态类型是该语言的一个显式设计决定(尽管是一个[有争议的决定](https://news.ycombinator.com/item?id=15567164)), clo jure . spec 围绕动态编程进行了优化。Clojure 没有类似的 TypeScript，这是故意的。

#### 一切都是数据

来自 JavaScript 世界，Clojure(和 Lisps)的另一个奇怪的属性是代码是数据(自命不凡的人用[同形异义](https://en.wikipedia.org/wiki/Homoiconicity))。

您可以用`’`告诉 Clojure 将函数调用视为列表数据结构，而不是对其求值。在这个例子中，我们创建了一个定义，它等于一个未赋值的函数调用。这个未赋值的函数调用只是一个列表。然后我们取列表中的第一个成员，加法运算符。

```
clj꞉dev꞉>  (def code-is-data '(+ 1 3))
clj꞉dev꞉> (first code-is-data)
; + 
```

理解 Clojure 的这一方面对于理解[宏](https://clojure.org/reference/macros)很重要。宏看起来与函数非常相似；他们接受论点并有一个身体。然而，它们在一个关键方面不同于函数。函数评估它们的参数并返回数据，而宏返回一个数据结构，然后被评估。为了演示，我们可以创建一个使用中缀而不是[前缀](https://en.wikipedia.org/wiki/Polish_notation)符号的宏。

```
clj꞉dev꞉>  (defmacro infix
            [[num-1 operator num-2]]
            (list operator num-1 num-2))
clj꞉dev꞉>  (infix (2 * 3))
; 6 
```

当您构建 Clojure 微服务时，您会经常遇到外部库中的宏。这些宏最初可能会令人困惑，因为它们看起来好像违反了 Clojure 语法规则。实际上，它们是 _ 扩展 _Clojure 的语法。

#### 虚拟机（Java Virtual Machine 的缩写）

前面我说过，在 Clojure 中，您编写功能性代码，而不是创建有状态对象。这并不完全正确。

Clojure 运行在 JVM 平台上(虽然 Clojure 也可以[编译成 JavaScript](https://clojurescript.org/) )。这意味着在 Clojure 中，您可以访问 Java 世界提供的一切。你可以[导入 Java 库并创建类](https://clojure.org/reference/java_interop#new)。

这是一种“鱼与熊掌不可兼得”的情况，将功能性动态语言的表达能力与 JVM 的能力和信任结合在一起。

例如，我们可以使用 Clojure 的“ [new](https://clojuredocs.org/clojure.core/new) ”表单来创建 Java 的 [Date 类](https://docs.oracle.com/javase/8/docs/api/java/sql/Date.html)的实例。

```
(defn now [] (new java.util.Date))
(now)
; #inst "2021-04-22T15:06:19.329-00:00" 
```

#### 取代

Clojure 和 JavaScript 都允许您在 REPL(读取-评估-打印循环)中编写代码。然而，在 JavaScript 中，我们倾向于依赖测试驱动的开发，并在本地运行我们的 web 应用来驱动我们的开发工作流。

在 Clojure(和许多其他 Lisps)中，REPL 是开发工作流的核心。事实上，Clojure 是专门为 REPL 驱动的工作流设计的。Clojure 的 REPL 允许您进行交互式的灵活开发体验，最终收紧您的迭代/反馈循环。

如果您喜欢冒险，您甚至可以使用 Clojure 的 REPL 来修改生产中的代码，而无需提交。众所周知，[深空 1 号探测器在 1 亿英里之外使用普通的 LISP REPL](http://www.flownet.com/gat/jpl-lisp.html) 修复了一个错误。

## 包扎

作为一名 JavaScript 开发人员，使用 Clojure 进行思考可能会很自然。您可能已经使用函数式风格编写代码，使用动态类型，并使用 REPL 编写代码。

尽管如此，Clojure 在几个关键方面与 JavaScript 有本质的不同:不可变数据、多态而不是在对象内定义方法，以及同象性(代码就是数据)。这些方面中的任何一个都可能是 JavaScript 开发人员的挑战。

理解 Clojure 和 JavaScript 之间的异同将在我们的下一篇文章中对你大有裨益，在下一篇文章中，我们将[构建你的第一个 Clojure 微服务](https://circleci.com/blog/clojure-microservices-for-js-devs-pt-2/)。