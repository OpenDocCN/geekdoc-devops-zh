# 在 24 小时内用 Clojure 重写你的测试套件

> 原文：<https://circleci.com/blog/rewriting-your-test-suite-in-clojure-in-24-hours/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

这是一个关于我如何构建一个编译器，在大约 24 小时内自动将 [CircleCI 的](https://circleci.com)14000 行测试套件翻译成另一个测试库的故事。

CircleCI 的测试套件可能是当今 Clojure 世界中最大的测试套件之一。我们的服务器端代码是 100% Clojure，包括测试套件，目前是 14000 行，在 140 个文件中，有 5000 个断言。没有[并行化](https://circleci.com/docs/1.0/how-parallelism-works/)，运行需要 40 分钟。

在这次冒险的开始，所有这些测试都是在 [Midje](https://github.com/marick/Midje) 中编写的，这是一个 BDD 测试库，有点类似于 RSpec。我们对 Midje 不是很满意，决定转移到 [`clojure.test`](http://richhickey.github.io/clojure/clojure.test-api.html) ，这可能是最常用的测试库。`clojure.test`更简单，不那么神奇，有更大的工具和插件生态系统。

显然，手工重写 5000 个测试是不实际的。相反，我们决定使用 Clojure 来自动重写它们，使用 Clojure 的内置语言操作特性。

Clojure 是同形的，这意味着所有源文件都可以表示为数据结构。我们的翻译器将每个测试文件解析成一个 Clojure 数据结构。从那里，我们在将代码写回磁盘之前对其进行转换。一旦放到磁盘上，我们就可以加载并运行测试，如果测试通过，甚至可以自动将文件检回版本控制，所有这些都不需要离开 REPL。

## 阅读

这整个操作的关键是`read`。`read-string`是一个内置的 Clojure 函数，它接受包含任何 Clojure 代码的字符串，并将其作为 Clojure 数据结构返回。这与编译器加载源文件时使用的函数相同。一个简单的例子:`(read-string "[1 2 3]")`返回`[1 2 3]`。

我们使用`read`将我们的测试代码变成一个大的嵌套列表，它可以被普通的 Clojure 代码修改。

## 转换

我们的测试是使用`midje`编写的，我们希望将它们转换成使用`clojure.test`。这里有一个使用`midje`的测试示例:

```
(ns circle.foo-test
  (:require [midje.sweet :refer :all]
            [circle.foo :as foo]))
(fact "foo works"
  (foo x) => 42) 
```

以及使用`clojure.test`的转换版本:

```
(ns circle.foo-test
  (:require [clojure.test :refer :all]))

(deftest foo-works
  (is (= 42 (foo x)))) 
```

这种转变包括替换:

*   `midje.sweet`与`clojure.test`呈 ns 形式

*   `(fact "a test name"...)`用`(deftest a-test-name ...)`，因为`clojure.test`名字是变量，不是字符串

*   `(foo x) => 42`同`(is (= 42 (foo x)))`

*   一些小细节，我们现在忽略了

转换是一个简单的深度优先的树遍历:

```
(defn munge-form [form]
  (let [form (-> form
                 (replace-midje-sweet)
                 (replace-foo)
                 ...)]
    (cond
      (or (list? form)
          (vector? form)) (-> form
                              (replace-fact)
                              (replace-arrow)
                              (replace-bar)
                              ...
                              (map munge-form)))
      :else form)) 
```

`->`语法的行为很像 Ruby 或 JQuery 或 Bash 管道中的链接:它将函数调用的结果作为参数传递给下一个函数调用。

第一部分`(let [form ...])`采用 Clojure 形式，并在其上调用每个翻译函数。第二部分接受表单列表——表示其他 Clojure 表达式和函数——并递归地转换它们。

有趣的工作发生在替换函数中。它们通常都是以下形式:

```
(if (this-form-is-relevant? form)
  (some-transformation form)
  form) 
```

也就是说，他们检查传入的表单是否与他们的兴趣相关，如果是，就适当地转换它。所以`replace-midje-sweet`看起来像

```
(defn replace-midje-sweet [form]
  (if (= 'midje.sweet form)
    'clojure.test
    form)) 
```

### 箭头

Midje 的大多数测试行为都围绕着“arrows”展开，这是 Midje 用来实现 BDD 风格的声明性测试用例的一种不通顺的结构。一个简单的例子:

```
(foo 42) => 5 
```

断言`(foo 42)`返回 5。

根据箭头的用途以及箭头两侧的类型，有大量可能的行为。

```
(foo 42) => map? 
```

例如，如果上面的右边是一个函数，当传递给函数`map?`时，它断言结果为真。在标准 clojure 中，这将是:

```
(map? (foo 42)) 
```

以下是一些 midje 箭头的例子:

```
(foo 42) => falsey
(foo 42) => map?
(foo 42) => (throws Exception)
(foo 42) =not=> 3
(foo 42) => #"hello world" ;; regex
(foo 42) =not=> "hello" 
```

#### 更换箭头

实际的翻译由大约 40 个 [core.match](https://github.com/clojure/core.match) 规则处理。他们看起来像

```
(match [actual arrow expected]
  [actual '=> 'truthy] `(is ~actual)
  [actual '=> expected] `(is (= ~expected ~actual)
  [actual '=> (_ :guard regex?)] `(is (re-find ~contents ~actual))
  [actual '=> nil] `(is (nil? ~actual))) 
```

(对于 Clojure 专家，我在上面的宏中省略了很多~ '字符以提高可读性。阅读实际的源代码，看看这到底是什么样子。)

这些翻译大部分都很简单。然而，对于`contains`表单，事情变得复杂得多:

```
(foo 42) => (contains {:a 1})
(foo 42) => (contains [:a :b] :gaps-ok)
(foo 42) => (contains [:a :b] :in-any-order)
(foo 42) => (contains "hello") 
```

最后一个案例特别有意思。在表达式中

```
(foo 42) => (contains "hello") 
```

有两种完全不同的价值观可以让这个测试通过。`(foo 42)`可以是包含项目“hello”的列表，也可以是包含子字符串“hello”的字符串:

```
"hello world" => (contains "hello")
["foo" "hello" "bar"] => (contains "hello") 
```

一般来说，`contains`表格很难自动翻译。有些情况需要运行时信息(比如上一个例子)，因为在标准 Clojure 中没有许多`contains`情况的现有实现，比如`(contains [:a :b] :in-any-order)`，我们决定放弃所有`contains`情况。“失败”规则看起来像是:

```
[actual arrow expected] (is (~arrow ~expected ~actual)) 
```

这将`(foo 42) => (contains bar)`变成了`(is (=> (contains bar) (foo 42)))`。这是故意不编译的，因为 midje 的 arrow 函数定义没有加载，所以我们可以手动修改。

#### 运行时类型信息

自动翻译还有一个额外的复杂性。如果我有两个表达式:

```
(let [bar 3]
  (foo) => bar 
```

和

```
(let [bar clojure.core/map?]
  (foo) => bar 
```

Midje 的 arrow 调度依赖于右边表达式的类型，这只能在运行时确定。如果`bar`解析为数据，比如字符串、数字、列表或映射，midje 测试是否相等。但是如果`bar`解析为一个函数，midje 代替*调用*这个函数，即`(is (= bar (foo)))` vs `(is (bar (foo)))`。我们 90%的解决方案`require`是原始的测试名称空间，而`resolve`是翻译过程中的功能:

```
(defn form-is-fn? [ns f]
  (let [resolved (ns-resolve ns f)]
    (and resolved (or (fn? resolved)
                      (and (var? resolved)
                           (fn? @resolved))))))) 
```

这在大多数情况下非常有效，但是当局部变量隐藏了全局变量名称时就会失败:

```
(let [s [1 2 3]
      count (count s)]
  (foo s) => count) 
```

在这种情况下，我们想要`(is (= count (foo s)))`，但是却得到了`(is (count (foo s)))`，这失败了，因为在局部范围内，count 是一个数字，`(3 [1 2 3])`是一个错误。幸运的是，这种情况并不多，因为要正确解决这个问题，需要编写一个全面的编译器，并理解作用域中的局部变量。

## 运行测试

一旦翻译代码写好了，我们需要一种方法来判断它是否工作。因为我们在运行时在 REPL 中运行代码，所以使用`clojure.test`的内置函数在翻译后立即运行测试是微不足道的。

`clojure.test`的设计决策使得将翻译和评估过程结合在一起变得很容易。所有的测试函数都可以直接从 REPL 调用，而不需要加壳，`(clojure.test/run-all-tests)`甚至返回一个有意义的返回值，一个包含测试、通过和失败次数的映射:

```
{:pass 61, :test 21, :error 0, :fail 0} 
```

能够从 REPL 运行测试使得在一个非常紧密的反馈循环中修改编译器和重新测试变得非常方便。

### 《朗读者》

然而，并不是所有事情都这么简单。

“阅读器”(Clojure 的术语，指实现`read`函数的编译器部分)被设计成将源文件转换成数据结构，主要供编译器使用。阅读器去掉注释，展开宏，要求我们手动检查所有差异，并恢复删除注释或包含宏定义的行。谢天谢地，在测试中只有很少的几个。我们的编码风格倾向于选择文档字符串而不是注释，宏倾向于被隔离到一小组实用程序文件中，所以这对我们没有太大影响。

### 代码缩进

我们没有找到一个非常好的库来缩进我们的新代码。我们使用了`clojure.pprint`的代码模式，尽管它可能是最好的库，但并没有做得那么好。在这个项目中，我们不想写那个库，所以一些文件用非惯用的间距和缩进写回磁盘。现在，当我们用锯齿状的锉刀工作时，我们用手来修补。真正解决这个问题需要一个知道惯用格式的缩进器，并且可能会考虑阅读器数据上的文件和行元数据。

在实际重写测试套件和这篇博文发表之间有很长的延迟。与此同时， [rewrite-clj](https://github.com/xsc/rewrite-clj) 已经发布。我没用过，但它看起来正是我们所缺少的。

## 结果

大约 40%的测试文件在没有人工干预的情况下通过了，考虑到我们将这个解决方案组合在一起的速度，这是相当惊人的。在剩余的文件中，大约 90%的测试断言被翻译并通过。因此，所有文件中 94%的断言都可以自动翻译，这是一个很好的结果。

我们的代码在 GitHub 上[这里](https://github.com/circleci/translate-midje)。如果你最终使用它，请让[美国](https://twitter.com/circleci)知道。虽然我们不推荐它用于无监督的翻译，特别是因为注释和宏问题，但它作为监督过程的一部分，对 [CircleCI](https://circleci.com) 非常有用。

*在[黑客新闻](https://news.ycombinator.com/item?id=6777375)T3 上讨论这个帖子*