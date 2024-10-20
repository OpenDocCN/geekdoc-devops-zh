# Clojure web 框架管道-构建一个 Clojure web 应用程序| CircleCI

> 原文：<https://circleci.com/blog/build-a-clojure-web-app-using-duct/>

在这篇博文中，我们将使用 [Clojure](https://clojure.org) 和一个名为 [Duct](https://github.com/duct-framework/duct) 的框架来构建一个服务器端 web 应用程序。为什么是管道？大多数 Clojure web 应用程序都是以定制的方式构建的，使用的是由开发人员挑选并组合在一起的库集合。Duct 提供了一个模块化框架，减少了搜索这些库的工作量，使您能够更快地启动和运行一个基本的服务器端 web 应用程序。还有其他 Clojure web 框架，但是 Duct 有很好的默认组合，不需要太多的学习曲线。

我假设你有一定的 Clojure 基础知识。如果你是 Clojure 的新手，看看[clo jure for the Brave and True](https://www.braveclojure.com/)、[clo jure from the ground](https://aphyr.com/posts/301-clojure-from-the-ground-up-welcome)，或者[伦敦 ClojureBridge 网站](https://clojurebridgelondon.github.io/)上的大量有用资源。

如果你想看完整的代码，我已经把它提交到 GitHub [这里](https://github.com/chrishowejones/blog-film-ratings)。

## 先决条件

为了构建这个 web 应用程序，您需要安装以下软件:

1.  [Java JDK 8 或更高版本](https://openjdk.java.net/install/) - Clojure 运行在 Java 虚拟机上，事实上，它只是一个 Java 库(JAR)。我用版本 8 构建了这个，但是一个更好的版本应该也可以。
2.  [Leiningen](https://leiningen.org/) - Leiningen，通常简称为 lein(读作‘line’)，是最常用的 Clojure 构建工具。
3.  Git -无处不在的分布式版本控制工具。

## 获得基本的 web 应用程序

关于 Duct 的一个好处是，你可以使用它的 lein 模板给你一个现成的 web 应用程序框架。我们将使用它为您的 web 应用程序提供一个起点，允许您输入并列出每部电影的描述和评级。对于这个入门应用程序，我们将使用 SQLite 作为开发的数据库引擎。在以后的博客文章中，我们将对此进行重构以使用 PostgreSQL。以下命令将为您提供一个具有我们所需的基本结构的初始项目:

```
$ lein new duct film-ratings +site +ataraxy +sqlite +example 
```

这将创建一个名为“电影分级”的新目录，其结构如下:

```
.
├── db
├── dev
│   ├── resources
│   │   └── dev.edn
│   └── src
│       ├── dev.clj
│       └── user.clj
├── project.clj
├── README.md
├── resources
│   └── film_ratings
│       ├── config.edn
│       ├── handler
│       │   └── example
│       │       └── example.html
│       └── public
├── src
│   └── film_ratings
│       ├── boundary
│       ├── handler
│       │   └── example.clj
│       └── main.clj
└── test
    └── film_ratings
        ├── boundary
        └── handler
            └── example_test.clj 
```

我们稍后将研究这些文件，但是现在，您需要为 Duct 生成本地配置。这个配置只是为您的机器准备的，并且会使用在`.gitignore`文件中为您生成的条目自动从 Git 存储库中排除。要生成此本地配置，请输入:

```
$ lein duct setup 
```

## 让我们检查一下它是否运行

此时，我们应该运行应用程序来检查我们的设置是否正常。为此，请输入以下命令:

```
$ lein repl
nREPL server started on port 37347 on host 127.0.0.1 - nrepl://127.0.0.1:37347
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.9.0
...
user=> (dev)
:loaded
dev=> (go)
:duct.server.http.jetty/starting-server {:port 3000}
:initiated
dev=> 
```

这将启动应用程序监听端口`3000`。打开浏览器，输入网址`http://localhost:3000/`，你会看到:

![](img/bb9a9f906d5cfcb4db4fcda9dd0d30fb.png)

这可能看起来很奇怪，但是服务器正在工作。只是你没有路径`/`的路线。让我们看一下应用程序的配置。在您最喜欢的文本编辑器或 IDE 中打开文件`film-ratings/resources/film_ratings/config.edn`,您会看到:

```
{:duct.core/project-ns  film-ratings
 :duct.core/environment :production

 :duct.module/logging {}
 :duct.module.web/site {}
 :duct.module/sql {}

 :duct.module/ataraxy
 {[:get "/example"] [:example]}

 :film-ratings.handler/example
 {:db #ig/ref :duct.database/sql}} 
```

这是管道的基本配置文件。它指定了项目名称空间、目标环境(在本例中是生产环境，尽管您将在后面查看开发配置)，以及日志、站点和 SQL 配置的一些占位符。

该文件中最有趣的行是:

```
 :duct.module/ataraxy
 {[:get "/example"] [:example]}

 :film-ratings.handler/example
 {:db #ig/ref :duct.database/sql}} 
```

它们指定如何将 URL 映射到处理 URL 的 HTTP 请求的处理函数。在本例中，我们使用一个名为`ataraxy`的库来指定从 URL 到函数的路径。您可以在`:duct.module/ataraxy`映射条目中看到，模板生成了一个路由，将对 URL `/example`的 HTTP GET 请求映射到关键字`:example`。

由于已经为`/example`定义了一条路线，您可以在浏览器中键入`http://localhost:3000/example`,您将会看到:

![](img/43928505688996c5221b23ec150d127e.png)

## 处理请求

`{[:get "/example"] [:example]}`路由如何服务于这个示例处理程序页面？

默认情况下，Duct 假设路由值(`:example`)中的关键字将以`{{project-ns}}.handler`为前缀，它将查找该关键字来确定处理程序配置。在这个例子中，`film-ratings.handler/example`在它的选项值中定义了一个对`:duct.database/sql`键的集成引用。 [Integrant](https://github.com/weavejester/integrant) 是一个微框架，它构建一个配置，然后通过以正确的顺序启动配置中定义的组件来构建一个运行系统。稍后我们将再次讨论数据库选项。现在，只需注意，Duct 寻找一个整合键来确定哪个函数充当处理程序。

您将在文件`film-ratings/src/film_ratings/handler/example.clj`中找到处理函数:

```
(defmethod ig/init-key :film-ratings.handler/example [_ options]
  (fn [{[_] :ataraxy/result}]
    [::response/ok (io/resource "film_ratings/handler/example/example.html")])) 
```

处理程序是一个函数，它返回另一个接受 HTTP 请求并返回响应的函数。这里的内部函数只是返回一个向量，告诉 ataraxy 返回一个状态 200 (OK)和作为响应主体的`example.html`文件。

Integrant 根据配置为处理程序提供初始化选项。在这种情况下，不使用这些选项，但这是一种处理数据库等资源的方式。

## 设置持续集成

在我们继续之前，让我们提交到目前为止我们所拥有的 Git 并建立我们的持续集成构建。

首先，让我们在本地创建 Git 存储库。打开一个新的终端会话(保持运行的终端会话`lein`打开),并从`film-handler`根目录输入以下命令:

```
$ git init
$ git add .
$ git commit -m "Duct app generated w/ +site +ataraxy +sqlite +example" 
```

此时，您将需要一个 GitHub 帐户。如果你没有，在 [GitHub](https://github.com) 注册。登录您的帐户，添加一个名为`film-handler`的新存储库。复制存储库的 URL，并在以下命令中使用它:

```
$ git remote add origin <github repo url>
$ git push --set-upstream origin master 
```

如果您的 URL 是`https`版本，您将被提示输入您的 GitHub 用户名和密码(如果您启用了多重身份验证，您将需要在 GitHub 中生成一个[个人访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)以用作密码)。

您现在在 GitHub 中有了自己的代码。我们将使用 CircleCI 来运行持续集成构建。去[https://circleci.com/](https://circleci.com/)创建一个账户，用你的 GitHub 证书注册。

在告诉 CircleCI 如何运行一个构建之前，有必要手动运行一个:

```
$ lein do test, uberjar

lein test film-ratings.handler.example-test

Ran 1 tests containing 1 assertions.
0 failures, 0 errors.
Compiling film-ratings.main
Compiling film-ratings.handler.example
Created .../film-ratings/target/film-ratings-0.1.0-SNAPSHOT.jar
Created .../film-ratings/target/film-ratings-0.1.0-SNAPSHOT-standalone.jar 
```

您可以看到，这运行了一个测试，然后将应用程序打包为一个独立的 Uber jar(Uber jar 是一个包含所有所需库的单一归档文件)。让我们通过编辑`project.clj`文件并添加一个 uberjar-name 键值来简化 uberjar 的名称:

```
...
  :main ^:skip-aot film-ratings.main
  :uberjar-name "film-ratings.jar"
  :resource-paths ["resources" "target/resources"]
... 
```

如果您重新运行`lein do test, uberjar`，您将看到 jar 名称被更改为`film-ratings.jar`。现在是时候给根`film-handler`目录添加一个`.circleci`目录和一个`config.yml`目录了。

```
$ mkdir .circleci
$ cd .circleci
$ touch config.yml
$ cd .. 
```

现在编辑空的`config.yml`得到下面几行 YAML 代码。

```
version: 2
jobs:
  build:
    working_directory: ~/cci-film-ratings # directory where steps will run
    docker:
      - image: circleci/clojure:lein-2.8.1
    environment:
      LEIN_ROOT: nbd
      JVM_OPTS: -Xmx3200m # limit the maximum heap size to prevent out of memory errors
    steps:
      - checkout
      - restore_cache:
          key: film-ratings-{{ checksum "project.clj" }}
      - run: lein deps
      - save_cache:
          paths:
            - ~/.m2
          key: film-ratings-{{ checksum "project.clj" }}
      - run: lein do test, uberjar 
```

然后我们需要将编辑好的`project.clj`文件和 CircleCI 配置添加到 GitHub 中。

```
$ git add .
$ git commit -m "Added circleci"
$ git push origin 
```

接下来，进入你的 CircleCI 账户，选择**添加项目**。从列表中选择您的回购，然后点击**设置项目**。默认情况下，操作系统和语言应该预先选择为`Linux`和`Clojure`，所以只需点击**开始构建**。如果您随后单击 **Building** 并进入构建作业，您将看到构建正在运行，最终您将看到一个成功的构建。

![](img/e7cdf313e334e9e7093eba38c97765cd.png)

## 添加索引页

您并不真的需要这个示例页面，所以让我们做一些更改来删除它，并为`/`路线添加一个索引页面。为此，您需要编辑`config.edn`来删除示例路由并添加新的索引路由。配置应该如下所示:

```
{:duct.core/project-ns  film-ratings
 :duct.core/environment :production

 :duct.module/logging {}
 :duct.module.web/site {}
 :duct.module/sql {}

 :duct.module/ataraxy
 {[:get "/"] [:index]}

 :film-ratings.handler/index {}
} 
```

您现在需要为索引页面创建一个处理程序，因此在`film-ratings/src/film_ratings/handler`目录中创建一个名为`index.clj`的文件。将以下内容添加到索引文件中:

```
(ns film-ratings.handler.index
  (:require [ataraxy.core :as ataraxy]
            [ataraxy.response :as response]
            [film-ratings.views.index :as views.index]
            [integrant.core :as ig]))

(defmethod ig/init-key :film-ratings.handler/index [_ options]
  (fn [{[_] :ataraxy/result}]
    [::response/ok (views.index/list-options)])) 
```

这个`defmethod`用一个处理函数初始化`:film-ratings.handler/index`键，这个处理函数接受一个请求并解结构 ataraxy 路由的结果。在这种情况下，我们没有命名它，因为它没有被使用。您还可以看到`defmethod`的两个参数，第一个将是关键字 key，`:film-ratings.handler/index`，在本例中，第二个是在 config 中定义的任何初始化的选项，在本例中也没有使用。

这个处理程序现在引用了一个不存在的`film-ratings.views.index`名称空间中的`list-options`函数，所以让我们创建这个文件。添加一个名为`film-ratings/src/film_ratings/views`的新目录，并创建一个新的`index.clj`文件，其中包含:

```
(ns film-ratings.views.index
  (:require [film-ratings.views.template :refer [page]]))

(defn list-options []
  (page
    [:div.container.jumbotron.bg-white.text-center
     [:row
      [:p
       [:a.btn.btn-primary {:href "/add-film"} "Add a Film"]]]
     [:row
      [:p
       [:a.btn.btn-primary {:href "/list-films"} "List Films"]]]])) 
```

您可以看到,`list-options`函数返回了一个类似 HTML 的数据结构，该结构封装在对`page`函数的函数调用中；再说一遍，那还不存在。使用一个名为 [hiccup](https://github.com/weavejester/hiccup) 的库将这种类似 HTML 的数据结构转换成真正的 HTML。为了使用 hiccup，您需要将它作为一个依赖项添加到`project.clj`文件中:

```
 :dependencies [[org.clojure/clojure "1.9.0"]
                 [duct/core "0.6.2"]
                 [duct/module.logging "0.3.1"]
                 [duct/module.web "0.6.4"]
                 [duct/module.ataraxy "0.2.0"]
                 [duct/module.sql "0.4.2"]
                 [org.xerial/sqlite-jdbc "3.21.0.1"]
                 [hiccup "1.0.5"]] 
```

现在让我们添加索引视图名称空间中引用的`page`函数。创建一个`film-ratings/src/film_ratings/views/template.clj`文件:

```
(ns film-ratings.views.template
  (:require [hiccup.page :refer [html5 include-css include-js]]
            [hiccup.element :refer [link-to]]
            [hiccup.form :as form]))

(defn page
  [content]
  (html5
    [:head
     [:meta {:name "viewport" :content "width=device-width, initial-scale=1, shrink-to-fit=no"}]
     [:title "Film Ratings"]
     (include-css "https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css")
     (include-js
       "https://code.jquery.com/jquery-3.3.1.slim.min.js"
       "https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
       "https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js")
     [:body
      [:div.container-fluid
       [:div.navbar.navbar-dark.bg-dark.shadow-sm
        [:div.container.d-flex.justify-content-between
         [:h1.navbar-brand.align-items-center.text-light "Film Ratings"]
         (link-to {:class "py-2 text-light"} "/" "Home")]]
       [:section
        content]]]]))

(defn labeled-radio [group]
  (fn [checked? label]
    [:div.form-check.col
     (form/radio-button {:class "form-check-input"} group checked? label)
     (form/label {:class "form-check-label"} (str "label-" label) (str label))])) 
```

您还可以看到另一个函数`labeled-radio`，它返回单选按钮的 hiccup。你以后会需要这个的。您现在可以删除`film-ratings/src/film_ratings/handler/example.clj`和`film-ratings/test/film_ratings/handler/example_test.clj`文件以及`film-ratings/resources/handler/example`目录和内容。

## 运行新的索引页面

让我们检查一下索引页面是否正确呈现。

回到您正在运行的`lein repl`终端。通常，您可以通过在 repl 中运行`(reset)`来刷新应用程序的状态，但是在这种情况下，您添加了一个新的依赖项(hiccup)。这需要通过重新启动 repl 来重新加载，如下所示:

```
user=> (quit)
Bye for now!
$ lein repl
...
user=> (dev)
:loaded
dev=> (go)
:duct.server.http.jetty/starting-server {:port 3000}
:initiated
dev=> 
```

接下来，打开浏览器进入`http://localhost:3000/` URL，您应该会看到:

![](img/871fabf192991a749354c2eef90a2f64.png)

在提交这些更改之前，我们应该为索引页面添加一个测试，以防我们在某个时候意外破坏它。创建一个包含以下内容的文件`film-ratings/test/film_ratings/handler/index_test.clj`:

```
(ns film-ratings.handler.index-test
  (:require [film-ratings.handler.index]
            [clojure.test :refer [deftest testing is]]
            [ring.mock.request :as mock]
            [integrant.core :as ig]))

(deftest check-index-handler
  (testing "Ensure that the index handler returns two links for add and list films"
    (let [handler (ig/init-key :film-ratings.handler/index {})
          response (handler (mock/request :get "/"))]
      (is (= :ataraxy.response/ok (first response)))
      (is (= "href=\"/add-film\""
            (re-find #"href=\"/add-film\"" (second response))))
      (is (= "href=\"/list-films\""
            (re-find #"href=\"/list-films\"" (second response))))))) 
```

然后运行测试:

```
$ lein test

lein test film-ratings.handler.index-test

Ran 1 tests containing 3 assertions.
0 failures, 0 errors. 
```

现在提交这些更改并将其推送到 GitHub。

```
$ git add .
$ git commit -m "Added index page"
$ git push 
```

这将启动 CircleCI 上的一个构建，你可以在你的 CircleCI 控制台上看到。

## 添加电影

到目前为止，我们有一个应用程序，它只显示了一个索引页面，这个页面上有一些按钮，但没有链接到任何东西。接下来，我们需要添加一个处理程序来添加电影和一个处理程序来列出电影。我们还需要把这个输入数据库。

让我们从加入我们的数据库开始。如果你查看`film-ratings/dev/resources`，你会看到一个`dev.edn`文件。在开发模式下启动时(这是您目前运行应用程序的方式)，在 Integrant 启动应用程序之前，Duct 会将该开发配置文件与生产配置文件合并。

```
{:duct.core/environment :development
 :duct.core/include ["film_ratings/config"]

 :duct.module/sql
 {:database-url "jdbc:sqlite:db/dev.sqlite"}} 
```

如您所见，这将把`:duct.module/sql`设置为一个 SQLite 数据库的引用，Integrant 在您启动应用程序时启动该数据库。在启动时，这将是对包含正在运行的数据库连接的映射的引用。

目前，这引用了一个空数据库。但是，您可以使用 Duct 的 Ragtime 模块来使用数据库迁移库 [Ragtime](https://github.com/weavejester/ragtime) ，用电影表填充数据库。将以下几行添加到`film-ratings/resources/film_ratings/config.edn`:

```
 :film-ratings.handler/index {}

 :duct.migrator/ragtime
 {:migrations [#ig/ref :film-ratings.migrations/create-film]}

 [:duct.migrator.ragtime/sql :film-ratings.migrations/create-film]
 {:up ["CREATE TABLE film (id INTEGER PRIMARY KEY, name TEXT UNIQUE, description TEXT, rating INTEGER)"]
  :down ["DROP TABLE film"]}

 } 
```

让我们为 add films 表单视图和 post 请求添加处理程序，以便将电影添加到数据库中。首先，将路线添加到`film-ratings/resources/film_ratings/config.edn`文件中:

```
 :duct.module/ataraxy
 {[:get "/"] [:index]
  "/add-film"
  {:get [:film/show-create]
   [:post {film-form :form-params}] [:film/create film-form]}} 
```

新的`add-film` URL 现在被映射到两个处理程序，一个用于 GET 方法，一个用于 POST 方法。注意，post route 使用 Clojure 析构从请求中提取表单参数，并将它们作为参数传递给`:film/create`处理程序。

此时，我们还没有定义`:film/create`或:`film/show-create` Integrant 键及其选项。为此，在`config.edn`中添加这些行:

```
 :film-ratings.handler/index {}
 :film-ratings.handler.film/show-create {}
 :film-ratings.handler.film/create {:db #ig/ref :duct.database/sql} 
```

`create`键有一个对数据库的集成引用，它将被传递给选项映射中的处理程序。接下来，我们需要为这两个键创建处理程序。这些键的命名空间为`film-ratings.handler.film`。这是命名空间管道，Integrant 将寻找它来找到初始化处理程序键的函数。我们需要为这个名称空间创建一个新文件，`film-ratings/src/film_ratings/handler/film.clj`:

```
(ns film-ratings.handler.film
  (:require [ataraxy.core :as ataraxy]
            [ataraxy.response :as response]
            [film-ratings.boundary.film :as boundary.film]
            [film-ratings.views.film :as views.film]
            [integrant.core :as ig]))

(defmethod ig/init-key :film-ratings.handler.film/show-create [_ _]
  (fn [_]
    [::response/ok (views.film/create-film-view)]))

(defmethod ig/init-key :film-ratings.handler.film/create [_ {:keys [db]}]
  (fn [{[_ film-form] :ataraxy/result :as request}]
    (let [film (reduce-kv (fn [m k v] (assoc m (keyword k) v))
                          {}
                          (dissoc film-form "__anti-forgery-token"))
          result (boundary.film/create-film db film)
          alerts (if (:id result)
                   {:messages ["Film added"]}
                   result)]
      [::response/ok (views.film/film-view film alerts)]))) 
```

`:film-ratings.handler.film/show-create` defmethod 返回一个处理程序，该处理程序简单地返回调用`create-film-view`的结果，该结果被包装在一个带有`::response/ok`关键字的向量中，由`ataraxy`呈现为一个状态为 200 的 HTTP 响应。

`:film-ratings.handler.film/create` defmethod 将数据库作为一个选项，其包含的处理函数从`ataraxy`结果中分解出胶片格式。电影表单将是 HTML 表单的映射(我们还没有创建)。该映射的键是字符串形式的表单字段的名称，此外它还有一个防伪标记。为了方便起见，`let`的第一行删除了防伪标记，将字符串密钥改为关键字。然后调用一个`create-film`函数，将数据库和电影形式作为参数。这将返回一个应该有一个`:id`或`:messages`键值对的结果映射。然后，处理函数返回对`film-view`调用的响应。

此时，views 函数和`create-film`函数都不存在。我们需要创建一个新的视图文件，`film-rating/src/film_ratings/views/film.clj`:

```
(ns film-ratings.views.film
  (:require [film-ratings.views.template :refer [page labeled-radio]]
            [hiccup.form :refer [form-to label text-field text-area submit-button]]
            [ring.util.anti-forgery :refer [anti-forgery-field]]))

(defn create-film-view
  []
  (page
   [:div.container.jumbotron.bg-light
    [:div.row
     [:h2 "Add a film"]]
    [:div
     (form-to [:post "/add-film"]
              (anti-forgery-field)
              [:div.form-group.col-12
               (label :name "Name:")
               (text-field {:class "mb-3 form-control" :placeholder "Enter film name"} :name)]
              [:div.form-group.col-12
              (label :description "Description:")
               (text-area {:class "mb-3 form-control" :placeholder "Enter film description"} :description)]
              [:div.form-group.col-12
               (label :ratings "Rating (1-5):")]
              [:div.form-group.btn-group.col-12
               (map (labeled-radio "rating") (repeat 5 false) (range 1 6))]
              [:div.form-group.col-12.text-center
               (submit-button {:class "btn btn-primary text-center"} "Add")])]]))

(defn- film-attributes-view
  [name description rating]
  [:div
   [:div.row
    [:div.col-2 "Name:"]
    [:div.col-10 name]]
   (when description
     [:div.row
      [:div.col-2 "Description:"]
       [:div.col-10 description]])
   (when rating
     [:div.row
      [:div.col-2 "Rating:"]
      [:div.col-10 rating]])])

(defn film-view
  [{:keys [name description rating]} {:keys [errors messages]}]
  (page
   [:div.container.jumbotron.bg-light
    [:div.row
     [:h2 "Film"]]
    (film-attributes-view name description rating)
    (when errors
      (for [error (doall errors)]
       [:div.row.alert.alert-danger
        [:div.col error]]))
    (when messages
      (for [message (doall messages)]
       [:div.row.alert.alert-success
        [:div.col message]]))])) 
```

`create-film-view`返回 hiccup，显示一个表单来输入电影名称、描述和 1 到 5 之间的评分。注意来自`film-ratings.view.template`名称空间的`page`函数被用来将来自`create-film-view`的 hiccup 封装到提供导航条等的 hiccup 中。`film-view`函数获取一个表示电影的地图和一个包含错误或消息的地图，并渲染 hiccup 以显示电影以及错误和消息警报。渲染电影属性的打嗝声已经被提取到它自己的函数`film-attributes-view`中，因为您将在电影列表中重用它。

## 添加数据库函数作为边界

到目前为止，我们一直在实现主要处理 HTTP 请求和响应的函数和配置。我们已经添加了迁移以在开发 SQLite 数据库中创建一个`Film`表和对该数据库的 Integrant 引用，但是我们实际上并没有对该数据库做任何事情。

是时候添加一个边界名称空间来处理数据库交互了。边界是一个管道概念，用于将外部依赖项与代码的其余部分隔离开来。边界是 Clojure 协议和相关的实现。Clojure 协议是与其他语言中的接口类似的概念。

当前配置将传递给`:film-ratings.handler.film/create`处理程序的选项映射到`:duct.database/sql`键，Integrant 将通过引用一个`duct.database.sql.Boundary`记录来初始化这个键。在处理程序中，我们有一个对现在需要实现的函数的引用。

这个函数是在`film-ratings.boundary.film`名称空间中的`create-film`，并且这个函数需要在一个协议中定义，该协议使用我们的`create-film`函数的实现来扩展管道框架中的`duct.database.sql.Boundary`记录。

让我们继续创建那个`film-ratings/src/film_ratings/boundary/film.clj`文件:

```
(ns film-ratings.boundary.film
  (:require [clojure.java.jdbc :as jdbc]
            duct.database.sql)
  (:import java.sql.SQLException))

(defprotocol FilmDatabase
  (list-films [db])
  (create-film [db film]))

(extend-protocol FilmDatabase
  duct.database.sql.Boundary
  (list-films [{db :spec}]
    (jdbc/query db ["SELECT * FROM film"]))
  (create-film [{db :spec} film]
    (try
     (let [result (jdbc/insert! db :film film)]
       (if-let [id (val (ffirst result))]
         {:id id}
         {:errors ["Failed to add film."]}))
     (catch SQLException ex
       {:errors [(format "Film not added due to %s" (.getMessage ex))]})))) 
```

让我们专注于`create-film`函数。该功能在您的`FilmDatabase`协议中定义。然后，这个协议被实现为用您的`create-film`的实现来扩展`duct.database.sql.Boundary`记录。

不出所料，`create-film`函数引用了`FilmDatabase`(在启动时由 Integrant 通过实时数据库连接为您初始化)和一个来自处理程序解析的表单参数的电影地图引用。该函数使用`clojure.java.jdbc/insert!`函数将电影地图插入电影表。该函数返回一个带有新插入记录的 id 的映射，或者如果发生错误，返回一个错误的映射。

## 添加电影

我们现在有足够的实现来将电影添加到数据库中。如果您的 repl 仍然在终端会话中运行，那么切换回终端会话。如果没有，启动一个新的 repl。然后重置应用程序以重新加载新代码:

```
dev=> (reset)
:reloading (film-ratings.boundary.film film-ratings.handler.film film-ratings.main film-ratings.views.film film-ratings.handler.example dev user)
:duct.migrator.ragtime/applying :film-ratings.migrations/create-film#5fc9a814
:resumed
dev=> 
```

如果您现在转到`http://localhost:3000/`并点击`Add Film`按钮，您会看到:

![](img/d52d8c999f9d0fa7a756e7324787f3ff.png)

继续填写表格并点击**添加**。

![](img/d70f03633491201410710e1191337b9f.png)

我们添加了大量代码。让我们提交它并推送到 GitHub。

```
$ git add .
$ git commit -m "Add films functionality."
$ git push 
```

我们可以通过转到[仪表板](https://app.circleci.com/dashboard)来检查 CircleCI，看看我们的构建是否已经正确运行。

## 列出电影

最后，我们只需要实现列出电影的配置、处理程序和视图。

更改 ataraxy 路由，并在 config.edn 中添加一个新的列表处理程序键:

```
 :duct.module/ataraxy
 {[:get "/"] [:index]
  "/add-film"
  {:get [:film/show-create]
   [:post {film-form :form-params}] [:film/create film-form]}
  [:get "/list-films"] [:film/list]}

 :film-ratings.handler/index {}
 :film-ratings.handler.film/show-create {}
 :film-ratings.handler.film/create {:db #ig/ref :duct.database/sql}
 :film-ratings.handler.film/list {:db #ig/ref :duct.database/sql} 
```

将以下处理函数添加到`film-ratings/src/film_ratings/handler/film.clj`文件:

```
(defmethod ig/init-key :film-ratings.handler.film/list [_ {:keys [db]}]
  (fn [_]
    (let [films-list (boundary.film/list-films db)]
      (if (seq films-list)
       [::response/ok (views.film/list-films-view films-list {})]
       [::response/ok (views.film/list-films-view [] {:messages ["No films found."]})])))) 
```

我们已经定义了在处理程序中调用的`list-films`函数，所以不需要添加它。

将新的`list-film-view`函数添加到`film-ratings/src/film_ratings/views/film.clj`文件中:

```
(defn list-films-view
  [films {:keys [messages]}]
  (page
   [:div.container.jumbotron.bg-light
    [:div.row [:h2 "Films"]]
    (for [{:keys [name description rating]} (doall films)]
      [:div
       (film-attributes-view name description rating)
       [:hr]])
    (when messages
      (for [message (doall messages)]
       [:div.row.alert.alert-success
        [:div.col message]]))])) 
```

因为这将为每部电影调用先前定义的`film-attributes-view`函数，所以我们需要确保文件中的`list-films-view`在`film-attributes-view`之后。

通过再次重置 repl 来测试这是否有效:

```
dev=> (reset)
:reloading (film-ratings.views.film film-ratings.handler.film)
:resumed
dev=> 
```

转到索引页面`http://localhost:3000/`并选择`List Films`按钮，您应该会看到您添加的所有电影的列表。

![](img/1202ccd4c780d40132fe7b04560ac5ec.png)

最后，向 Git 添加并提交您的更改。

## 摘要

恭喜您，您已经创建了一个功能性的管道网络应用程序！🎉目前，这个应用程序只有一个为开发配置文件定义的数据库，所以我们在构建中创建的 uberjar 实际上不会工作，因为生产 SQL 模块是一个空映射`:duct.module/sql {}`。

我将把这作为一个练习留给读者，但如果你感觉不够自信，或者如果你在这方面有所挣扎，我将在随后的博客中详细介绍如何将生产数据库添加到应用程序中，如何使用 Docker 打包，以及如何让 circle ci[将其部署到 AWS](https://circleci.com/blog/deploy-a-clojure-web-application-to-aws-using-terraform/) 。

阅读更多信息:

* * *

Chris Howe-Jones 是顾问 CTO、软件架构师、精益/敏捷蔻驰、开发人员和 DevCycle 的技术导航员。他主要从事 Clojure/ClojureScript、Java 和 Scala 方面的工作，客户从跨国组织到小型创业公司。

[阅读更多克里斯·豪-琼斯的文章](/blog/author/chris-howe-jones/)