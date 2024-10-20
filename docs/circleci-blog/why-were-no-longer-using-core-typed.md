# 为什么我们不再使用 Core.typed - CircleCI

> 原文：<https://circleci.com/blog/why-were-no-longer-using-core-typed/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

2013 年 9 月，我们在博客中讨论了为什么我们支持类型化 Clojure，您也应该这样做！现在，两年后，我们的工程团队已经集体决定停止使用类型化 Clojure(特别是 core.typed 库)。作为这个决定的一部分，我们想写一篇关于我们使用 core.typed 的经验的博文。

我们决定停止使用 core.typed 的原因是因为我们发现使用它的成本大于我们获得的收益。当然，这是一个主观的观点，所以我们将在下面详述我们的推理。

[core.typed](https://github.com/clojure/core.typed) 库是[类型的 Clojure 项目](http://typedclojure.org/)的一部分。它是一个为 Clojure 代码添加可选类型的库。Core.typed 允许开发人员向 Clojure 表单添加类型注释，然后可以运行类型检查过程来验证程序的类型信息。

首先，介绍一下我们项目的背景。我们的主应用程序的 repo 包含 93，983 行 Clojure 代码。我们的主应用程序的`src`目录包含 369 个名称空间，其中 84 个用 core.typed 进行了注释，285 个没有。

因此，我们的代码库大致分为三组:带类型注释的代码，带类型注释并带有 [`^:no-check`修饰符](https://github.com/clojure/core.typed/wiki/User-Guide#unchecked-vars)的代码，以及不带类型注释的代码。

我们在使用类型化 Clojure 时遇到的问题是编程时迭代时间较慢，core.typed 不支持整个 Clojure 编程语言，以及使用没有用 core.typed 注释进行注释的第三方库。

## 更慢的迭代时间

当在 Clojure 中开发时，我们的团队在 REPL 交互地编写代码。我们希望能够编写代码，然后立即运行测试。在我们的代码库上使用 core.typed 时，运行类型检查器需要很长时间。在我的机器上，(一台 3GHz i7 MacBook Pro)对单个名称空间进行类型检查需要 2 分钟。这导致了代码领先于类型注释的情况。我通常同时编写代码和相应的测试，快速迭代。当使用 core.typed 时，我会在我写的代码中添加类型注释，然后运行类型检查器，因为两分钟的运行时间[太慢，不能交互运行](https://www.youtube.com/watch?v=RAxiiRPHS9k)。

类型检查需要 2 分钟的原因是因为它重新扫描我们项目中的所有文件来收集类型注释。我们确实在一段时间内更改了 core.typed 设置，以便在运行类型检查器时只重新扫描当前文件。这改进了类型检查器的运行时间，使其接近瞬时，但是这阻止了我们处理两个相互依赖的文件。我们发现修改一个文件中的类型注释并在另一个文件上运行类型检查器在这个设置中是不可能的——对第一个文件的更改被忽略了。

## Core.typed 没有实现整个 Clojure 语言

我个人在使用 core.typed 时的委屈是，它没有实现整个 Clojure 语言。[有些函数不能进行类型检查，例如`get-in`、`comp`、`or`、](http://dev.clojure.org/jira/browse/CTYP-167)的一些用法。这意味着在编写了一些代码之后，我们经常不得不回去重新编写一些表单来满足类型检查器的要求。

一个特别的例子是，我需要取出一个表单，该表单接受一个函数`f`并返回:

```
(comp keyword f name) 
```

(在调用`name`和`keyword`之间编写我的函数`f`)。这段代码[不能被 core.typed](http://dev.clojure.org/jira/browse/CTYP-167) 检查，所以我不得不将表单提取到一个顶级函数中，并添加一个类型注释(并添加一个注释来解释我为什么这样做):

```
(t/ann keyword-transformer
  [(t/IFn [String -> String]) -> (t/IFn [t/Keyword -> t/Keyword])])

(defn- keyword-transformer [f]
  (fn [x] (-> x name f keyword))) 
```

我的沮丧不是因为有 core.typed 无法检查的代码，这是可以理解的。问题是我们不能轻易地区分代码中的类型错误和 core.typed 不能检查代码的情况。这两种情况下产生的错误消息是相同的。

以下是上面的`comp`示例产生的错误示例:

```
Polymorphic function comp could not be applied to arguments:
Polymorphic Variables: x y b
Domains: [x -> y] [b ... b -> x]
Arguments:
  (t/IFn [(t/U t/Symbol java.lang.String t/Keyword) -> t/Keyword]
         [java.lang.String java.lang.String -> t/Keyword])
         [java.lang.String -> java.lang.String]
         [(t/U clojure.lang.Named java.lang.String) -> java.lang.String]
Ranges: [b ... b -> y]
with expected type: [t/Keyword -> t/Keyword] 
```

当面临这样的问题时，用户不清楚被检查的代码是否包含导致类型检查失败的错误，或者代码是否暴露了 core.typed 可以检查的内容的限制。我解决这类问题的第一个尝试是以几种不同的形式重写代码，看看是否能通过类型检查。这就是 2 分钟迭代时间变得令人沮丧的地方——我会做一点小小的改变，提取出一个表单并添加类型注释，然后运行类型检查器。如果类型检查器再次失败，我将不得不重复提取表单和添加类型注释的过程。每一次迭代将强加另外 2 分钟的等待时间。

在几次失败的迭代之后，我会假设问题是 core.typed 本身的限制，并在我的代码中添加一个`^:no-check`注释，然后继续。我发现我经常选择添加一个`^:no-check`注释，而不是花时间去诊断潜在的问题。每一个带有`^:no-check`注释的表单都会削弱类型系统的保证，并允许 bug 绕过类型检查器。

我们在 CircleCI 遇到的另一个问题是，函数的类型签名不能用 core.typed 进行类型检查，所以我们必须将返回类型注释为 [`Any`](http://clojure.github.io/core.typed/#clojure.core.typed/Any) 。我们的`apply-map`函数就是一个很好的例子——该函数类似于 [`apply`](http://clojure.github.io/clojure/clojure.core-api.html#clojure.core/apply) ,但是它可以用于需要一组关键字参数作为 rest 参数的函数:

```
(t/ann ^:no-check apply-map
  [(t/IFn [t/Any -> t/Any]) t/Any * -> t/Any])

(defn apply-map
  "Takes a fn and any number of arguments. Applies the arguments like
  apply, except that the last argument is converted into keyword
  pairs, for functions that keyword arguments.

  (apply foo :a :b {:c 1 :d 2}) => (foo :a :b :c 1 :d 2)"
  [f & args*]
  (let [normal-args (butlast args*)
        m (last args*)]
    (when m
      (assert (map? m) "last argument must be a map"))
      (apply f (concat normal-args (apply concat (seq m)))))) 
```

我们无法让这个函数使用 core.typed 进行正确的类型检查，所以我们不得不将它注释为返回`Any`([`apply`的类型签名相当复杂](https://github.com/clojure/core.typed/blob/0947387913babb0e8db52b560a3c0e42b45cb40b/module-check/src/main/clojure/clojure/core/typed/base_env.clj#L491-L508))。像这样使用`Any`类型具有传染性——当调用`apply-map`时，所有类型信息都会丢失——所以任何调用`apply-map`的函数都不能正确地进行类型检查。我们总是必须用`^:no-check`标志来注释这些函数，这意味着任何调用`apply-map`的函数都不会进行类型检查。在 Circle 中，我们大量使用这种风格的关键字参数:在我们的代码库中有 115 个对`apply-map`的调用，这将是 115 个我们不能进行类型检查的函数，这再次削弱了整个类型系统。

为某些函数编写类型注释可能非常困难——在搜索我们的代码时，我发现了一个特别的注释，我觉得它代表了我们在必须添加`^:no-check`注释时的感受:

```
(t/ann ^:no-check all-granted-by
  [t/Hierarchy t/Keyword -> (t/Coll t/Any)])
;; sigh. i still can't figure out how to write moderately 
;; complicated core.typed checks for stuff like this. 
```

## 第三方代码

我们使用的第三方库都没有使用 core.typed 进行注释。这意味着我们必须自己为这些第三方库创建和维护一个类型注释列表。我们维护一个文件，其中包含我们所依赖的第三方代码的类型注释。每当我们添加对以前没有调用过的库函数的调用时，我们都需要为该函数编写一个注释。我们编写的类型注释不会被 core.typed 检查(它们被标记为`^:no-check`)。所以我们需要非常确定这些注释是正确的。如果这些注释被添加到库本身，那么它们将被检查类型。我们[已经提出将我们的注释添加回一些项目](https://github.com/clj-time/clj-time/issues/168)，但是对于那些自己不使用 core.typed 的项目维护者来说，接受一个添加类型注释的补丁并没有什么好处。额外的维护开销是不值得的，core.typed 会给库增加额外的依赖。

## 我们的未来- schema.core

现在，我们已经禁用了我们的单元测试，该单元测试将对我们用 core.typed 注释的名称空间运行类型检查器。

我们发现类型检查中最有价值的代码是映射中的字段访问，因为映射是“[字符串类型的](http://c2.com/cgi/wiki?StringlyTyped)”(或者对 Clojure 更准确地说，“关键字类型的”)。在这些领域，我们已经采用了[棱镜模式](https://github.com/Prismatic/schema)，其方式由[杰西卡·克尔](http://jessitron.com/)在最近一集的认知广播中描述。我们将`schema.core`定义添加到我们想要进行类型检查的函数中。这些大多是将地图作为参数之一的函数。当运行我们的测试时，我们使用`schema.test/validate-schemas`测试夹具来启用运行时模式检查。对于一些函数，我们还启用了`^:always-validate`标志，以确保在生产中也检查模式。

使用 schema.core 允许我们逐渐地向我们的代码添加模式检查，而没有我们在使用 core.typed 时遇到的一些问题。为了这种灵活性，我们必须放弃编译时类型检查，而是依赖于运行时类型检查，这延迟了我们在代码中发现类型错误的时间点。模式定义确实给了我们 core.typed 给我们的相同的结构化文档，这是我们真正喜欢的东西。