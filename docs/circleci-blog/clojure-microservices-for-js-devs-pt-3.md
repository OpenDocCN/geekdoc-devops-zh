# 面向 JavaScript 开发人员的 Clojure 微服务第 3 部分

> 原文：<https://circleci.com/blog/clojure-microservices-for-js-devs-pt-3/>

这个系列由穆萨·巴里格扎伊和泰勒·苏尔贝格共同撰写。

这是 JavaScript 开发人员关于如何设置 Clojure 微服务的系列文章中的第三篇，也是最后一篇。以前的职位是:

那些以前的帖子是有用的背景，但是你可以[克隆回购](https://github.com/tsully/clojure-for-js-devs)并在不阅读它们的情况下进入这篇帖子。

## 使用 Clojure 测试 API

与 JavaScript 相比，Clojure 的一个方便的特性是它自带了一个内置的单元测试库， [clojure.test](https://clojure.github.io/clojure/clojure.test-api.html) 。按照惯例，在 Leinegen 项目中，你有一个 src 目录，我们知道它保存了所有的代码，并且用于测试一个测试目录，将你的代码从你的测试中分离出来。对于您想要测试的在`src`下的任何文件，您可以在后缀为`\_test`的`/test`下创建一个匹配的文件。

假设我们有一个文件`adder.clj`，在 Leiningen 项目的下面的文件结构中。

```
src/
├─ adder/
│  ├─ adder.clj 
```

为了编写`adder.clj`的单元测试，我们将创建以下测试目录:

```
src/
├─ adder/
│  ├─ adder.clj
test/
├─ adder/
│  ├─ adder_test.clj 
```

注意`test`目录是如何镜像我们的`src`目录的，以及我们如何在我们想要测试的文件上添加一个`\_test`后缀。

我们想在`adder.clj`中测试下面的函数。

```
(defn add-numbers [x y]
  (+ x y)) 
```

要在`adder.clj`中为`add-numbers`函数编写测试，首先要引入 Clojure 核心测试框架。

```
(ns adder
  (:require [clojure.test :refer [deftest])) 
```

利用`deftest`宏，编写测试就像:

```
(deftest test-add-numbers
  (is (= 4 (add-numbers 2 2)))) 
```

在我们的[示例项目](https://github.com/tsully/clojure-for-js-devs)中，我们对所有的 HTTP 端点处理程序都进行了单元测试。下面是一个例子(可以在 test/clo jure _ for _ js _ devs/handlers _ test . clj 中找到)。这是为我们的`/counter`路径测试处理程序。`/counter`路径将请求者的 IP 地址作为一个关键字添加到 Redis 中，并将值加 1，记录一个 keeping 的次数`/counter`。

```
(testing "counter-handler"
    (let [response "Counter: 44"
          req (->  (ring-mock/request :get "/counter"))]
      (bond/with-stub! [[redis/getKey (constantly 44)] [redis/incr (constantly nil)]]
        (is (= (handlers/counter-handler req {}) response))))) 
```

这里发生了很多事情，所以让我们来为您分析一下。

首先，我们为想要测试的端点(`/counter`)初始化一个请求对象`req`。我们正在利用 ring-clojure(我们的 HTTP 服务器框架)附带的请求模拟库。这为我们节省了时间；我们不需要写出完整的请求映射。

接下来，我们使用库`bond/with-stub!`。在 JavaScript 中，最流行的测试框架是 [Jest](https://jestjs.io/) 。如果你以前和 Jest 合作过，bond 会给你[类似的特性](https://jestjs.io/docs/mock-function-api)。jest.mock()允许您模仿模块，并声明您希望模仿函数的返回值是什么。这就是`bond/with-stub`在 Clojure 中为我们做的事情。因为这些是单元测试，我们想要模拟对 Redis 的调用，特别是键`getKey`和`incr`。对于`getKey`，我们希望在测试中调用它的任何时候都返回 44，同样，对于`incr`，我们希望返回`nil`。

最后，我们断言我们对`handlers/counter-handler`的调用将匹配我们的响应`Counter: 44`。注意，`handlers/counter-handler`中的最后一个参数是我们的 Redis 组件，但是在这个测试中，我们传入了一个空的 map `{}`。因为我们正在存根化我们的 Redis 调用，所以我们可以为这个参数传递一个空映射，因为在我们的测试中不需要 Redis。

写集成测试怎么样？在 JavaScript 中，Jest 的一个有用特性是测试的设置和拆卸。当编写集成测试时，这个特性很方便。例如，如果我们有一个查询城市数据库的应用程序，您可能会开玩笑地这样做:

```
beforeAll(() => {
  initializeCityDatabase();
});

afterAll(() => {
  clearCityDatabase();
});

test("city database has Vienna", () => {
  expect(isCity("Vienna")).toBeTruthy();
}); 
```

beforeAll/afterAll 允许您在测试之前和之后运行代码。

Clojure 内置了对测试设置和拆卸的支持。它还利用了[夹具](https://clojuredocs.org/clojure.test/use-fixtures)。这里有一个完整的例子:

```
(ns adder_test
  (:require [clojure.test :refer [testing use-fixtures]))

(defn my-fixture [f]
  ;; The function you want to run before all tests
  (initializeCityDatabase)

  (f)  ;;Then call the function we passed.
  ;; The function you want to run after all tests
  (clearCityDatabase)
 )
lang:clojure
(use-fixtures :once my-fixture)

(deftest city-db
  (is (= "'Vienna'" (IsCity)))) 
```

这里我们用`:once`调用 use-fixtures，这意味着在名称空间中的所有测试中只运行一次我的 fixture。您还可以通过`:each`为每个测试重复运行您的夹具。`use-fixtures`这里的工作和以前一模一样/一语双关。我们用我们希望在之前和之后被调用的方法来包装我们的测试(在我的 fixture 中显示为(f))。

你可以在[示例项目](https://github.com/tsully/clojure-for-js-devs)和`test/clojure_for_js_devs/test_core.clj`中看到完整的例子。在[的帖子 2](https://circleci.com/blog/clojure-microservices-for-js-devs-pt-2/) 中，我们讨论了`system-map`的目的。系统图允许我们的应用程序管理它所依赖的每个软件组件的生命周期。在我们的例子中，我们的组件是 Redis 连接和一个 HTTP 服务器:

```
(defn- test-system
  []
  (component/system-map
   :redis (redis/new-redis "redis://localhost:6379")
   :http-server (component/using
                 (http/new-server "localhost" 0)
                 {:redis :redis})))

(defn- setup-system
  []
  (alter-var-root #'system (fn [_] (component/start (test-system)))))

(defn- tear-down-system
  []
  (alter-var-root #'system (fn [s] (when s (component/stop s)))))

(defn init-system
  [test-fn]
  (setup-system)
  (test-fn)
  (tear-down-system)) 
```

我们有一个方法`setup-system`,它使用组件库(来自文章 1)来启动所需的组件(HTTP 服务器和 Redis)。除了`setup-system`，我们还有`tear-down-system`，它运行 component/stop 来在测试完成后关闭所有组件。

现在，如何在 Clojure 中运行测试呢？如果没有测试运行器，您可以在 REPL 中调用(run-all-tests)来运行所有命名空间中的所有测试。或者，如果您正在使用 Leiningen 跟踪，您可以调用`lein test`。

您可能已经使用了 Jest 这样的测试运行器，它具有在代码中检测到变化时自动运行测试的特性。Clojure 社区也有各种具有类似特性的测试运行程序。最受欢迎的是 [Kaocha](https://github.com/lambdaisland/kaocha) 。我们在示例项目中使用 Kaocha。

要设置 Kaocha，首先将它作为一个依赖项添加到 dev dependencies 下的`project.clj`文件中:

```
 :profiles {:uberjar {:aot :all}
                 :dev {:dependencies [[lambdaisland/kaocha "1.0.829"]
                                  [circleci/bond "0.5.0"]
                                  [ring/ring-mock]]}} 
```

在 JavaScript 中，您可以为想要运行的自定义命令创建自定义脚本。比如`npm run prod:ci`。Leinegen 也有这个特性，称为别名。我们可以创建一个别名`test`来加载`lambdaisland/kaocha`依赖项。

```
:aliases {"kaocha" ["run" "-m" "kaocha.runner"]} 
```

最后，添加 Koacha 配置文件。使用以下配置在您的根项目目录中创建一个`tests.edn`文件:

```
#kaocha/v1
{:kaocha/color? true} 
```

现在，如果您调用`lein test`，kaocha 将执行测试并报告结果。

## 在 CircleCI 中运行 Clojure 测试

如果你正在寻找持续集成(CI)重要性的介绍，我强烈推荐你去看看[https://circleci.com/continuous-integration/](https://circleci.com/continuous-integration/)。在这一节中，我们将回顾 CI 工作流以及如何在 CircleCI 中运行 Clojure 测试，

在我们在`.circleci/config.yml`下的示例项目中，我们有一个工作流，用于在每次提交[回购](https://github.com/tsully/clojure-for-js-devs)时测试我们的项目。

```
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/clojure:lein-2.9.5
      - image: redis:4.0.2-alpine
        command: redis-server --port 6379
    working_directory: ~/repo
    environment:
      LEIN_ROOT: "true"
      JVM_OPTS: -Xmx3200m
    steps:
      - checkout
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "project.clj" }}
            - v1-dependencies-
      - run: lein deps
      - save_cache:
          paths:
            - ~/.m2
          key: v1-dependencies-{{ checksum "project.clj" }}
      - run:
          name: install dockerize
          command: wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && sudo tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
          environment:
            DOCKERIZE_VERSION: v0.3.0
      - run:
          name: Wait for redis
          command: dockerize -wait tcp://localhost:6379 -timeout 1m
      - run: lein test

workflows:
  build:
    jobs:
      - build 
```

`jobs`键是我们定义可以在 CI 渠道中使用的工作的地方。我们的管道只有一个任务，“构建”，它将构建我们的应用程序，然后运行我们的测试。“构建”将使用 Docker 映像来设置我们的 CI 环境。我们的 Clojure 应用程序将使用预先构建的 CircleCI Docker 映像:`circleci/clojure:lein-2.9.5`运行。这些映像通常是官方 Docker 映像的扩展，包括对 CI/CD 特别有用的工具。在我们的例子中，`circleci/clojure:lein-2.9.5`安装了 Clojure 和 lein。我们还使用 Redis Docker 映像，因为我们的集成测试需要访问 Redis 服务器。

下面是我们“构建”工作的所有步骤的回顾:

1.  我们首先从我们的回购中检查我们的代码。
2.  然后，我们包装`lein deps`调用，用缓存获取我们所有的依赖项。我们不需要在每次 CI 运行时都重新安装所有的依赖项，因此缓存将加快我们的 CI 构建。
3.  然后我们引入 Dockerize，这是一个工具，它使我们的 CI 能够在运行测试之前等待其他服务可用。在我们的例子中，我们使用 Dockerize 在运行集成测试之前等待 Redis。
4.  最后，我们运行`lein test`来运行我们所有的测试。

我们现在让 CI 管道在每次提交时运行，自动确保没有人对我们的 Clojure 项目进行重大更改。

就是这样！现在，您已经有了一个 Clojure 微服务，它使用持续集成工作流进行基本测试。我们希望您发现这个系列很有价值，并且有信心在您的下一个项目中使用 Clojure。