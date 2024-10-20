# Clojure Web 框架使用 Docker - Clojure web 开发| CircleCI

> 原文：<https://circleci.com/blog/package-a-clojure-web-application-using-docker/>

这是关于构建、测试和部署 Clojure web 应用程序的系列文章的第二篇。你可以在这里找到第一帖[，在这里](https://circleci.com/blog/build-a-clojure-web-app-using-duct/)找到第三帖[。](https://circleci.com/blog/deploy-a-clojure-web-application-to-aws-using-terraform/)

在这篇文章中，我们将关注如何向应用程序添加生产数据库(在本例中是 PostgreSQL ),如何将应用程序打包为 Docker 实例，以及如何在 Docker 中运行应用程序和数据库。为了跟进，我建议浏览第一篇文章，并按照步骤创建应用程序。否则，您可以通过分叉[这个](https://github.com/chrishowejones/blog-film-handler)存储库并检查主分支来获得源代码。如果你选择这种方法，你还需要按照第一篇文章中的描述设置你的 CircleCI 账户。

尽管我们正在构建一个 Clojure 应用程序，但是本系列的这一部分并不需要太多 Clojure 知识。

## 先决条件

为了构建这个 web 应用程序，您需要安装以下软件:

1.  [Java JDK 8 或更高版本](https://openjdk.java.net/install/)——clo jure 运行在 Java 虚拟机上，实际上只是一个 Java 库(JAR)。我用版本 8 构建了这个，但是一个更好的版本应该也可以。
2.  [Leiningen](https://leiningen.org/) - Leiningen，通常被称为 lein(读作‘line’)是最常用的 Clojure 构建工具。
3.  Git -无处不在的分布式版本控制工具。
4.  Docker——一个工具，旨在通过使用容器来简化应用程序的创建、部署和运行。
5.  Docker Compose -一个定义和运行多容器 Docker 应用程序的工具。

您还需要注册:

1.  [CircleCI 账号](https://circleci.com/) - CircleCI 是一个持续集成和交付平台。
2.  GitHub 账户 - GitHub 是一个基于网络的托管服务，使用 Git 进行版本控制。
3.  Docker Hub 帐户 - Docker Hub 是一个基于云的存储库，Docker 用户和合作伙伴可以在其中创建、测试、存储和分发容器映像。

## 运行 PostgreSQL 数据库

在本节中，我们将介绍如何运行 PostgreSQL 数据库，我们将从本博客系列的第一部分中构建的 web 应用程序连接到该数据库。

我们将使用 Docker 来“打包”我们的应用程序以进行部署。已经有很多关于为什么以及如何使用 Docker 的文章，比我计划在这里讨论的还要详细。我之所以决定使用它，是为了提供与我们要部署到的物理机器的一定程度的隔离，我认为，更重要的是，为了确保应用程序在本地或远程环境之间运行时的运行时行为的一致性。

出于这个原因，最终的游戏将是运行我们在上一篇博客中构建的 web 应用程序和 Docker 中的 PostgreSQL 数据库，并让两者进行通信。

该应用程序当前在开发模式下运行时使用 SQLite。在博客系列的第一部分中，我们只在开发模式下运行，要么使用`lein repl`从 REPL 运行服务器，要么使用`lein test`运行单元测试。如果我们通过从我们的项目目录中发出`lein run`命令，尝试在生产模式下运行应用程序，我们将得到一个错误，因为没有指定生产数据库连接。

```
$ lein run
Exception in thread "main" clojure.lang.ExceptionInfo: Error on key :duct.database.sql/hikaricp when building system {:reason :integrant.core/build-threw-exception ... 
```

我们将使用官方的 [postgres Docker 映像](https://circleci.com/docs/postgres-config/) (alpine 版本)在 Docker 容器中运行数据库。为此，我们可以发出以下命令:

```
$ docker run -p 5432:5432 -e POSTGRES_USER=filmuser -e POSTGRES_DB=filmdb -e POSTGRES_PASSWORD=password postgres:alpine
...
2019-01-20 10:08:32.064 UTC [1] LOG:  database system is ready to accept connections 
```

这个命令运行 postgres Docker 镜像(如果需要，从 Docker Hub 中下载),数据库监听 TCP 端口 5432，设置一个名为`filmuser`的默认用户，将该用户的密码设置为`password`,并创建一个名为`filmdb`的空数据库。如果您已经将 PostgreSQL 作为一项服务安装在您的计算机上，您可能会收到一条关于端口 5432 正在使用的消息。如果发生这种情况，要么停止本地 PostgreSQL 服务，要么更改`-p 5432:5432`条目以公开不同的端口，例如端口 5500 `-p 5500:5432`。

为了检查您是否可以连接到数据库，请在不同的终端窗口中发出以下命令:

```
psql -h localhost -p 5432 -U filmuser filmdb
Password for user filmuser:
psql (11.1 (Ubuntu 11.1-1.pgdg16.04+1))
Type "help" for help.

filmdb=# 
```

虽然您现在已经连接到了数据库，但是此时您还不能做很多事情，因为我们还没有创建任何表、视图等(关系)。

```
filmdb=# \d
Did not find any relations. 
```

所以让我们关闭 psql 实用程序。

```
filmdb=# exit 
```

接下来，让 postgres 的 Docker 容器保持运行，并更改我们的应用程序，使其具有可以连接到数据库的生产配置。

打开电影分级项目目录中的`resources/film_ratings/config.edn`文件。然后找到`:duct.module/sql`条目，并在它下面添加以下内容:

```
:duct.database.sql/hikaricp {:adapter "postgresql"
                              :port-number #duct/env [ "DB_PORT" :or "5432" ]
                              :server-name #duct/env [ "DB_HOST" ]
                              :database-name "filmdb"
                              :username "filmuser"
                              :password #duct/env [ "DB_PASSWORD" ]} 
```

此条目使用 PostgreSQL 定义了光连接池的配置。注意，我们从[环境变量](https://circleci.com/docs/env-vars/) `DB_HOST`和`DB_PASSWORD`中获取服务器名称和密码。我们还考虑了一个可选的环境变量`DB_PORT`，但是如果需要的话，可以用来设置应用程序连接到不同的端口而不是`5432`。

您还需要在`project.clj`文件中添加 PostgreSQL 数据库驱动程序和 hikaricp 库的依赖项，因此依赖项部分如下所示:

```
:dependencies [[org.clojure/clojure "1.9.0"]
                 [duct/core "0.6.2"]
                 [duct/module.logging "0.3.1"]
                 [duct/module.web "0.6.4"]
                 [duct/module.ataraxy "0.2.0"]
                 [duct/module.sql "0.4.2"]
                 [org.xerial/sqlite-jdbc "3.21.0.1"]
                 [org.postgresql/postgresql "42.1.4"]
                 [duct/database.sql.hikaricp "0.3.3"]
                 [hiccup "1.0.5"]] 
```

此外，我们希望在插入一部新电影时，`id`列被自动分配一个唯一的编号，因此我们需要稍微改变一下迁移，以便`id`列的类型不再是整数(适用于 SQLite ),而是 PostgreSQL 中的类型`serial`。这意味着您需要将`resources/film_ratings/config.edn`中的 migrator ragtime 条目更改为:

```
[:duct.migrator.ragtime/sql :film-ratings.migrations/create-film]
 {:up ["CREATE TABLE film (id SERIAL PRIMARY KEY, name TEXT UNIQUE, description TEXT, rating INTEGER)"]
  :down ["DROP TABLE film"]} 
```

为了测试这个配置，首先需要设置环境变量。因此，从运行 Docker postgres 实例的终端窗口中打开一个单独的终端窗口，并像这样设置两个环境变量:

```
$ export DB_HOST=localhost
$ export DB_PASSWORD=password 
```

**注意** : *如果您必须更改 postgres Docker 实例正在使用的端口号，您还需要将`DB_PORT`环境变量设置为相同的端口号。*

一旦您设置了这些环境变量，您就可以在生产配置文件中运行应用程序，如下所示(首先将目录更改为您的项目根目录):

```
$ lein run
lein run
19-01-21 07:19:51 chris-XPS-13-9370 REPORT [duct.server.http.jetty:13] - :duct.server.http.jetty/starting-server {:port 3000} 
```

正如您从输出中看到的，我们的迁移(在博客的第一部分中定义)并不是为了插入`film`表而运行的。默认情况下，迁移不是通过生产配置文件中的管道运行的，但是我们将在以后解决这个问题。为了创建`film`表，我们可以通过打开另一个终端会话并执行以下命令来手动运行我们的迁移(在设置环境变量并将目录更改为项目根目录之后):

```
$ lein run :duct/migrator
19-01-21 07:48:59 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["CREATE TABLE ragtime_migrations (id varchar(255), created_at varchar(32))"], :elapsed 4}
19-01-21 07:48:59 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["SELECT id FROM ragtime_migrations ORDER BY created_at"], :elapsed 6}
19-01-21 07:48:59 chris-XPS-13-9370 REPORT [duct.migrator.ragtime:14] - :duct.migrator.ragtime/applying :film-ratings.migrations/create-film#11693a5d
19-01-21 07:48:59 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["CREATE TABLE film (id SERIAL PRIMARY KEY, name TEXT UNIQUE, description TEXT, rating INTEGER)"], :elapsed 10}
19-01-21 07:48:59 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["INSERT INTO ragtime_migrations ( id, created_at ) VALUES ( ?, ? )" ":film-ratings.migrations/create-film#11693a5d" "2019-01-21T07:48:59.960"], :elapsed 4}
19-01-21 07:48:59 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:31] - :duct.database.sql/batch-query {:queries [], :elapsed 0} 
```

您现在可以通过打开浏览器并指向`http://localhost:3000`来测试正在运行的应用程序。

如果您尝试添加电影，您将看到一条错误消息，指出列`rating`是整数类型，但提供的值是字符类型。

![](img/6a89a9ff9267728f2599885440079fb5.png)

这是因为 add film 表单中的所有值都是字符串，但是`film`表中的`ratings`列需要一个`INTEGER`。在我们的开发模式下不会发生这种情况，因为 SQLite 数据库驱动程序将评级的字符串值强制转换为整数，但 PostgreSQL 驱动程序不会。这说明使用不同的数据库进行生产和开发可能会有问题！

现在，让我们修复处理程序中的这个错误。编辑`src/film_ratings/handler/film.clj`文件，将处理电影形式的代码重构为一个单独的函数，该函数还处理将`ratings`强制为整数的操作:

```
(defn- film-form->film
  [film-form]
  (as-> film-form film
    (dissoc film "__anti-forgery-token")
    (reduce-kv (fn [m k v] (assoc m (keyword k) v))
               {}
               film)
    (update film :rating #(Integer/parseInt %))))

(defmethod ig/init-key :film-ratings.handler.film/create [_ {:keys [db]}]
  (fn [{[_ film-form] :ataraxy/result :as request}]
    (let [film (film-form->film film-form)
          result (boundary.film/create-film db film)
          alerts (if (:id result)
                   {:messages ["Film added"]}
                   result)]
      [::response/ok (views.film/film-view film alerts)]))) 
```

为了测试这一点，停止运行应用程序的服务器并重新启动它(CTRL-C 停止，`lein run`重新启动)。然后将您的浏览器指向`http://localhost:3000`并试用该应用程序。

一旦您对一切正常感到满意，现在您可以使用 CTRL-C 关闭服务器，并通过在运行 Docker 实例的终端会话中发出 CTRL-C 来关闭 Docker postgres 实例。

为了确保我们没有破坏任何东西，我们可以尝试在开发模式下运行服务器，这应该使用 SQLite 数据库实例。

```
$ lein repl
 lein repl
nREPL server started on port 38553 on host 127.0.0.1 - nrepl://127.0.0.1:38553
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.9.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_191-b12
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (dev)
:loaded
dev=> (go)
Jan 20, 2019 12:16:20 PM org.postgresql.Driver connect
SEVERE: Connection error:
org.postgresql.util.PSQLException: Connection to localhost:5432 refused. Check that the hostname and port are correct and that the postmaster is accepting TCP/IP connections. 
```

我们的配置应用方式有问题。这里不使用`dev/resources/dev.edn`中的配置:

```
:duct.module/sql
{:database-url "jdbc:sqlite:db/dev.sqlite"} 
```

我们似乎在`resources/config.edn`中至少获得了一些 postgres 配置。实际发生的是开发配置和生产配置映射被合并。因此，postgres 适配器配置和其余的键-值对被添加到`:database-url`中。我们需要解决这个问题，所以让我们在`dev/src/dev.clj`文件中添加一些代码来删除这些生产数据库属性。让我们添加一个函数来完成这项工作，并从对`set-prep!`的调用中调用这个新函数:

```
(defn remove-prod-database-attributes
  "The prepared config is a merge of dev and prod config and the prod attributes for 
  everything except :jdbc-url need to be dropped or the sqlite db is 
  configured with postgres attributes"
  [config]
  (update config :duct.database.sql/hikaricp 
    (fn [db-config] (->> (find db-config :jdbc-url) (apply hash-map)))))

(integrant.repl/set-prep! 
  (comp remove-prod-database-attributes duct/prep read-config)) 
```

现在，如果我们像这样在开发模式下运行服务器(使用`quit`退出之前运行的 repl):

```
$ lein repl
nREPL server started on port 44721 on host 127.0.0.1 - nrepl://127.0.0.1:44721
REPL-y 0.3.7, nREPL 0.2.12
Clojure 1.9.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_191-b12
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (dev)
:loaded
dev=> (go)
:duct.server.http.jetty/starting-server {:port 3000}
:initiated
dev=> 
```

**注意:** *如果你克隆了我的回购协议，而不是按照之前的博客，你可能会看到这样的错误:`SQLException path to 'db/dev.sqlite': '.../blog-film-ratings/db' does not` `exist org.sqlite.core.CoreConnection.open (CoreConnection.java:192)`。在这种情况下，使用`mkdir db`在项目根目录下手动创建一个空的‘db’目录，然后重试 repl 命令。*

我们可以看到服务器现在成功启动，并连接到 SQLite 数据库。现在，让我们提交我们的更改。

```
$ git add --all .
$ git commit -m "Add production database config, dependencies & fix dev db config"
$ git push 
```

## 在 Docker 中运行我们的应用程序

学习了如何在 Docker 中运行 PostgreSQL 数据库，以及如何从我们的应用程序连接到它，下一步是在 Docker 中运行我们的应用程序。

我们将添加一个 Docker 文件以在 Docker 中构建我们的应用程序，并且我们将使用 [Docker Compose](https://circleci.com/docs/docker-compose/) 在它们自己的网络中同时运行我们的应用程序 Docker 实例和 postgres Docker 实例。

在我们创建 docker 文件之前，让我们检查一下我们的 web 应用程序将如何在 docker 文件中实际运行。到目前为止，我们一直使用 repl 在开发模式下运行我们的应用程序，或者使用 lein 在生产模式下运行我们的应用程序。

通常，Clojure 应用程序在生产中的运行方式与 Java 应用程序相同。最常见的是将应用程序编译成。类文件，然后将它们打包在一个 Uberjar 中，这是一个包含所有。我们的应用程序所需的类文件和所有依赖库(jar 文件)以及任何资源文件(例如，我们的配置文件)。然后，这个 Uberjar 将使用 JVM (Java 虚拟机)运行。

在我们必须在 Docker 内部运行应用程序之前，让我们尝试在本地以这种方式运行我们的应用程序。首先，我们可以使用以下命令在 Uberjar 中编译和打包我们的应用程序:

```
$ lein uberjar
Compiling film-ratings.main
Compiling film-ratings.views.template
Compiling film-ratings.views.film
Compiling film-ratings.views.index
Compiling film-ratings.boundary.film
Compiling film-ratings.handler.film
Compiling film-ratings.handler.index
Created /home/chris/circleciblogs/repos/film-ratings/target/film-ratings-0.1.0-SNAPSHOT.jar
Created /home/chris/circleciblogs/repos/film-ratings/target/film-ratings.jar 
```

这将创建两个 JAR 文件。标记为快照的那个只包含没有库依赖关系的应用程序，而名为`film-ratings.jar`的那个是我们的 Uberjar，包含所有的依赖关系。我们现在可以从这个 Uberjar 运行我们的应用程序，但是首先要确保 postgres 的 Docker 实例正在运行，并且在发出这个命令之前，已经在终端会话中设置了`DB_HOST`和`DB_PASSWORD`环境变量:

```
$ java -jar target/film-ratings.jar
19-01-22 07:26:20 chris-XPS-13-9370 REPORT [duct.server.http.jetty:13] - :duct.server.http.jetty/starting-server {:port 3000} 
```

我们现在需要做的是编写一个 docker 文件来执行这个命令。这意味着我们需要 Docker 实例拥有一个 Java 运行时环境(在这种情况下，我们实际上使用的是 Java 开发工具包环境)。在项目基本目录中创建 Dockerfile 文件，并添加以下内容:

```
FROM openjdk:8u181-alpine3.8

WORKDIR /

COPY target/film-ratings.jar film-ratings.jar
EXPOSE 3000

CMD java -jar film-ratings.jar 
```

您现在可以像这样构建 Docker 实例:

```
$ docker build . -t film-ratings-app
Sending build context to Docker daemon  23.71MB
Step 1/5 : FROM openjdk:8u181-alpine3.8
 ---> 04060a9dfc39
Step 2/5 : WORKDIR /
 ---> Using cache
 ---> 2752489e606e
Step 3/5 : COPY target/film-ratings.jar film-ratings.jar
 ---> Using cache
 ---> b282e93eff39
Step 4/5 : EXPOSE 3000
 ---> Using cache
 ---> 15d2e1b9197e
Step 5/5 : CMD java -jar film-ratings.jar
 ---> Using cache
 ---> 2fe0b1e058e5
Successfully built 2fe0b1e058e5
Successfully tagged film-ratings-app:latest 
```

这创建了一个新的 [Docker 图像](https://circleci.com/docs/circleci-images/)，并将其标记为`film-ratings-app:latest`。我们现在可以像这样运行我们的 dockerized 应用程序:

```
$ docker run --network host -e DB_HOST=localhost -e DB_PASSWORD=password film-ratings-app
19-01-22 09:12:20 chris-XPS-13-9370 REPORT [duct.server.http.jetty:13] - :duct.server.http.jetty/starting-server {:port 3000} 
```

但是，我们仍然存在之前遇到的问题，即迁移没有运行。如果您打开浏览器进入`http://localhost:3000/list-films`，您可以演示这一点。您会看到内部服务器错误，并且会在 Docker 中运行应用程序的终端会话中看到一个巨大的堆栈跟踪，该跟踪以以下内容结束:

```
serverErrorMessage: #object[org.postgresql.util.ServerErrorMessage 0x5661cc86 "ERROR: relation \"film\" does not exist\n  Position: 15"] 
```

为了解决这个问题，我们将做一些实际上不建议可伸缩生产服务器做的事情，让迁移在应用程序启动时运行。更好的方法是在生产环境中有一个单独的 Docker 实例，它可以按需启动，可能是通过 [CI](https://circleci.com/continuous-integration/) 管道，在发生变化时运行迁移。为了这篇博客的目的，让我们采用更简单的方法，将我们的主函数改为调用迁移器。打开并编辑`src/film_ratings/main.clj`文件，如下所示:

```
(defn -main [& args]
  (let [keys (or (duct/parse-keys args) [:duct/migrator :duct/daemon])]
    (-> (duct/read-config (io/resource "film_ratings/config.edn"))
        (duct/prep keys)
        (duct/exec keys)))) 
```

这确保了在守护程序启动服务器之前，调用`:duct\migrator` Integrant 键来运行迁移。为了将这一变化加入到我们的 Docker 映像中，我们需要重新运行`lein uberjar`和`docker build . -t film-ratings-app`。然后我们可以在 Docker 中运行我们的应用程序:

```
$ docker run --network=host -e DB_HOST=localhost -e DB_PASSWORD=password film-ratings-app
19-01-22 18:08:23 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["CREATE TABLE ragtime_migrations (id varchar(255), created_at varchar(32))"], :elapsed 2}
19-01-22 18:08:23 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["SELECT id FROM ragtime_migrations ORDER BY created_at"], :elapsed 11}
19-01-22 18:08:23 chris-XPS-13-9370 REPORT [duct.migrator.ragtime:14] - :duct.migrator.ragtime/applying :film-ratings.migrations/create-film#11693a5d
19-01-22 18:08:23 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["CREATE TABLE film (id SERIAL PRIMARY KEY, name TEXT UNIQUE, description TEXT, rating INTEGER)"], :elapsed 13}
19-01-22 18:08:23 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["INSERT INTO ragtime_migrations ( id, created_at ) VALUES ( ?, ? )" ":film-ratings.migrations/create-film#11693a5d" "2019-01-22T18:08:23.146"], :elapsed 3}
19-01-22 18:08:23 chris-XPS-13-9370 INFO [duct.database.sql.hikaricp:31] - :duct.database.sql/batch-query {:queries [], :elapsed 0}
19-01-22 18:08:23 chris-XPS-13-9370 REPORT [duct.server.http.jetty:13] - :duct.server.http.jetty/starting-server {:port 3000} 
```

这一次，我们可以看到在服务器开始添加`film`表之前，迁移正在运行。现在，如果我们打开指向`http://localhost:3000/list-films`的浏览器，我们会看到“找不到电影”消息。您可以通过添加一些电影来试用该应用程序。

让我们在继续之前提交这些更改。

```
$ git add --all .
$ git commit -m "Add Dockerfile & call migrations on startup."
$ git push 
```

## 持久数据

我们还有一些问题。目前，如果我们停止 postgres Docker 容器进程并重新启动它，我们将丢失所有的电影。此外，我们通过主机网络在两个 Docker 容器之间进行通信。这意味着如果您在本地运行 PostgreSQL 服务器，您将会遇到端口冲突。

让我们通过创建 Docker 合成文件来解决这两个问题。我们的 Docker Compose 文件将构建我们的应用程序 Docker 文件，设置适当的环境变量并启动 postgres 实例。Docker Compose 将通过桥接网络促进应用程序和数据库之间的通信，这样我们就不会在本地主机上发生端口冲突，除了应用程序暴露的端口 3000。

在根项目目录中创建一个`docker-compose.yml`文件，并添加以下内容:

```
version: '3.0'
services:
  postgres:
    restart: 'always'
    environment:
      - "POSTGRES_USER=filmuser"
      - "POSTGRES_DB=filmdb"
      - "POSTGRES_PASSWORD=${DB_PASSWORD}"
    volumes:
      - /tmp/postgresdata:/var/lib/postgresql/data
    image: 'postgres:alpine'
  filmapp:
    restart: 'always'
    ports:
      - '3000:3000'
    environment:
      - "DB_PASSWORD=${DB_PASSWORD}"
      - "DB_HOST=postgres"
    build:
      context: .
      dockerfile: Dockerfile 
```

该文件在 docker-compose 中注册了两个服务:一个名为 postgres，它使用`postgres:alpine`映像并设置适当的 postgres 环境变量；另一个名为`filmapp`，它构建我们的 Dockerfile，运行它并公开端口 3000 并设置它的环境变量。

您还可以看到，我们已经定义了一个卷，该卷将本地机器上的目录`/tmp/postgresdata`映射到容器上的`/var/lib/postgresql/data`，该容器是 postgres 的数据目录。

这意味着，当我们运行 docker-compose 进程时，存储在数据库中的任何数据都将被写入本地的`/tmp/postgresdata`,甚至在我们重新启动 docker-compose 进程后，这些数据也将持续存在。

让我们试试这个。首先，我们构建 docker-compose 图像(确保您已经首先设置了`DB_PASSWORD`环境变量)。

```
$ docker-compose build
postgres uses an image, skipping
Building filmapp
Step 1/5 : FROM openjdk:8u181-alpine3.8
 ---> 04060a9dfc39
Step 2/5 : WORKDIR /
 ---> Using cache
 ---> 2752489e606e
Step 3/5 : COPY target/film-ratings.jar film-ratings.jar
 ---> b855626e4a45
Step 4/5 : EXPOSE 3000
 ---> Running in 7721d74eee62
Removing intermediate container 7721d74eee62
 ---> f7caccf63c3b
Step 5/5 : CMD java -jar film-ratings.jar
 ---> Running in 89b75d045897
Removing intermediate container 89b75d045897
 ---> 48303637af01
Successfully built 48303637af01
Successfully tagged film-ratings_filmapp:latest 
```

现在，让我们启动 docker-compose(首先退出任何正在运行的 Docker postgres 或 filmapp 实例)。

```
$ docker-compose up
Starting film-ratings_filmapp_1  ... done
Starting film-ratings_postgres_1 ... done
Attaching to film-ratings_filmapp_1, film-ratings_postgres_1
...
postgres_1  | 2019-01-22 18:35:58.117 UTC [1] LOG:  database system is ready to accept connections
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["CREATE TABLE ragtime_migrations (id varchar(255), created_at varchar(32))"], :elapsed 4}
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["SELECT id FROM ragtime_migrations ORDER BY created_at"], :elapsed 11}
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 REPORT [duct.migrator.ragtime:14] - :duct.migrator.ragtime/applying :film-ratings.migrations/create-film#11693a5d
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["CREATE TABLE film (id SERIAL PRIMARY KEY, name TEXT UNIQUE, description TEXT, rating INTEGER)"], :elapsed 12}
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 INFO [duct.database.sql.hikaricp:30] - :duct.database.sql/query {:query ["INSERT INTO ragtime_migrations ( id, created_at ) VALUES ( ?, ? )" ":film-ratings.migrations/create-film#11693a5d" "2019-01-22T18:36:06.120"], :elapsed 3}
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 INFO [duct.database.sql.hikaricp:31] - :duct.database.sql/batch-query {:queries [], :elapsed 0}
filmapp_1   | 19-01-22 18:36:06 5d9729ccfdd0 REPORT [duct.server.http.jetty:13] - :duct.server.http.jetty/starting-server {:port 3000} 
```

从消息中可以看出，我们已经启动了 postgres 服务，然后是 filmapp 服务。filmapp 服务已经连接到 postgres 服务(通过在`config.edn`中拾取的`DB_HOST=postgres`环境变量)。

filmapp 服务已经运行了迁移以添加`film`表，并且现在正在侦听端口 3000。

您现在可以尝试添加一些电影。然后，通过在另一个终端会话中运行项目根目录中的`docker-compose down`,或者在运行的会话中按 CTRL-C 键，停止 docker-compose 进程。

如果您随后使用`docker-compose up -d`重新启动 docker-compose 服务，您应该会发现您添加的任何电影数据都被保存了下来。

**注意:***`-d`标志运行 docker-compose detached，因此您必须运行`docker-compose logs`来查看日志输出，运行`docker-compose down`来关闭服务。*

如果您想再次从一个空数据库开始，只需删除`/tmp/postgresdata`目录并再次调用 docker-compose 服务。

同样，让我们在继续之前提交 docker-compose 文件。

```
$ git add --all .
$ git commit -m "Added docker-compose"
$ git push 
```

## 将 Docker 映像发布到 Docker Hub

我们几乎已经完成了我们在这个博客中设定的目标。我们想做的最后一件事是将我们的 Docker 映像推送到 Docker Hub，最好作为我们持续集成系统的一部分。

首先，让我们手动操作。

### 手动发布到 Docker Hub

如果你还没有这样做，在 [Docker Hub](https://hub.docker.com/) 上创建一个账户。然后，在您的帐户中创建一个名为`film-ratings-app`的新存储库。

我们只想发布应用程序的 Docker 图像，因为我们不会将 docker-compose 文件用于生产。首先，让我们重新构建 Docker 映像，并用 Docker Hub 存储库 id(在我的例子中为`chrishowejones/film-ratings-app`)标记它:

```
$ docker build . -t chrishowejones/film-ratings-app
Sending build context to Docker daemon  23.72MB
Step 1/5 : FROM openjdk:8u181-alpine3.8
 ---> 04060a9dfc39
Step 2/5 : WORKDIR /
 ---> Using cache
 ---> 2752489e606e
Step 3/5 : COPY target/film-ratings.jar film-ratings.jar
 ---> Using cache
 ---> 60fe31dc32e4
Step 4/5 : EXPOSE 3000
 ---> Using cache
 ---> 672aa852b89a
Step 5/5 : CMD java -jar film-ratings.jar
 ---> Using cache
 ---> 1fdfcd0dc843
Successfully built 1fdfcd0dc843 
```

然后，我们需要像这样将该映像推送到 Docker Hub(记住使用您的 Docker Hub 存储库，而不是`chrishowejones`！):

```
$ docker push chrishowejones/film-ratings-app:latest
The push refers to repository [docker.io/chrishowejones/film-ratings-app]
25a31ca3ed23: Pushed
...
latest: digest: sha256:bcd2d24f7cdb927b4f1bc79c403a33beb43ab5b2395cbb389fb04ea4fa701db2 size: 1159 
```

### 添加 CircleCI 作业以构建 Docker 映像

好了，我们已经证明了可以手动推送。现在，让我们看看如何设置 CircleCI，每当我们标记一个版本的存储库时，circle ci 就会为我们做这件事。

首先，我们想使用 CircleCI 的一个名为 executors 的特性来减少重复。由于这个特性只在 2.1 版本中引入，我们需要打开我们的`.circleci/config.yml`文件，并将版本参考从`2`更改为`2.1`。

```
version: 2.1
jobs:
... 
```

我们需要添加一个作业来为应用程序构建 Docker 图像，并添加另一个作业来发布 Docker 图像。我们将添加两个[工作流](https://circleci.com/docs/workflows/)来控制各个构建步骤的运行时间。

让我们首先在文件的顶部添加一个 executor，它将声明一些我们希望在两个新任务中重用的有用内容。

```
version: 2.1
executors:
    Docker-publisher:
      working_directory: ~/cci-film-ratings # directory where steps will run
      environment:
        IMAGE_NAME: chrishowejones/film-ratings-app
      Docker:
        - image: circleci/buildpack-deps:stretch
jobs:
... 
```

这个执行器声明了工作目录、本地环境变量`IMAGE_NAME`，以及一个 Docker 映像，该映像具有我们支持执行 [Docker 命令](https://circleci.com/docs/building-docker-images/)所需的 buildpack 依赖关系。和以前一样，您需要更改图像名称值，以您的 Docker Hub 用户而不是我的用户作为前缀。

在我们开始添加作业来构建 Docker 映像之前，我们需要确保在`build`作业中构建的应用程序的 Uberjar 持久保存到我们将要编写的新作业中。

所以在`build`作业的底部，我们需要添加 CircleCI 命令来持久化工作空间。

```
 - run: lein do test, uberjar
      - persist_to_workspace:
          root: ~/cci-film-ratings
          paths:
            - target 
```

这将持久保存`target`目录，以便我们可以在新作业中重新附加该目录。现在，让我们将该作业添加到我们的`build`作业之后:

```
 build-docker:
    executor: docker-publisher
    steps:
      - checkout
      - attach_workspace:
          at: .
      - setup_remote_docker
      - run:
          name: Build latest Docker image
          command: docker build . -t $IMAGE_NAME:latest
      - run:
          name: Build tagged Docker image
          command: docker build . -t $IMAGE_NAME:${CIRCLE_TAG}
      - run:
          name: Archive Docker image
          command: docker save -o image.tar $IMAGE_NAME
      - persist_to_workspace:
          root: ~/cci-film-ratings
          paths:
            - ./image.tar 
```

好了，让我们来看看这个`build-docker`任务在做什么。首先，我们引用我们的`docker-publisher`执行器，这样我们就有了工作目录、`IMAGE_NAME`变量和正确的 Docker 映像。

接下来，我们检验我们的项目，以便我们有可用的应用程序的`Dockerfile`。之后，我们将保存的工作空间附加到我们当前的工作目录中，这样我们就可以在 Dockerfile 的正确路径上获得包含 jar 的`target`目录。

`setup_remote_docker`命令创建了一个远离主容器的环境，这是一个 Docker 引擎，我们可以用它来执行 Docker 构建/发布事件(尽管其他 Docker 事件也发生在我们的主容器中)。

接下来的两个 run 命令在 Docker 文件上执行 Docker 构建，并将结果图像分别标记为`chrishowejones/film-ratings-app:latest`和`chrishowejones/film-ratings-app:${CIRCLE_TAG}`。在您的情况下，您应该将`IMAGE_NAME`更改为适合您的 [Docker Hub 帐户](https://circleci.com/blog/build-cicd-piplines-using-docker/)的前缀，而不是`chrishowejones`。

`${CIRCLE_TAG}`变量将被插入到与这个构建相关的 [GitHub 标签](https://circleci.com/blog/publishing-to-github-releases-via-circleci/)中。这里的想法是当我们标记一个 commit 并将其推送到 GitHub 时触发这个作业。出于论证的目的，如果我们将提交标记为`0.1.0`并将其推送到 GitHub，当我们的`build-docker`作业运行时，它将构建一个标记为`latest`的 Docker 映像，但也会构建相同的映像并将其标记为`0.1.0`。

`docker save`命令将`IMAGE_NAME`的所有 Docker 图像保存到一个 tar 存档文件中，然后我们在`persist-to-workspace`命令中保存该文件，这样我们可以在下一个任务中使用它。

**添加 CircleCI 作业以发布到 Docker Hub**

我们现在有一个构建两个 Docker 映像并持久化它们的作业，这样我们就可以在下一个作业中使用它们，下一个作业会将这两个映像推送到 Docker Hub。

让我们将该作业添加到`config.yml`中的`build-docker`作业之后:

```
 publish-docker:
        executor: docker-publisher
        steps:
          - attach_workspace:
              at: .
          - setup_remote_docker
          - run:
              name: Load archived Docker image
              command: docker load -i image.tar
          - run:
              name: Publish Docker Image to Docker Hub
              command: |
                echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USERNAME}" --password-stdin
                docker push $IMAGE_NAME:latest
                docker push $IMAGE_NAME:${CIRCLE_TAG} 
```

让我们来看看这份工作是做什么的。首先，它重用了带有工作区、环境变量和图像的执行器。接下来，它将持久化的工作空间附加到工作目录。然后，我们使用`setup_remote_docker`命令获取远程 Docker 引擎，这样我们就可以推送图像。

之后，我们运行 Docker 命令从持久化的 tar 文件中加载之前存储的 Docker 图像。下一个 run 命令使用两个尚未设置的环境变量`DOCKERHUB_USER`和`DOCKERHUB_PASS`登录到 Docker Hub，然后将前一个作业中构建的两个 Docker 映像推送到 Docker Hub。

在我们继续之前，让我们在 CircleCI 中设置这两个环境变量，这样我们就不会忘记。登录 https://circleci.com/，进入你的仪表板 https://circleci.com/dashboard.为你的项目选择项目设置(点击工作侧边栏中所列项目的图标。)

![](img/15b330a84d4be64b20f07a7eb1b17a72.png)

接下来，从`BUILD SETTINGS`部分选择`Environment Variables`，并添加两个新变量`DOCKERHUB_USER`和`DOCKERHUB_PASS`，分别为您的 Docker Hub 用户名和密码设置适当的值。

![](img/d480da4bdf01e665f6282cd51e42a787.png)

**添加工作流，以便在发布新版本时运行作业**

我们现在有了构建和发布 Docker 映像所需的作业，但是我们还没有指定如何执行这些作业。默认情况下，CircleCI 将在每次推送 GitHub 时运行一个名为`build`的作业，但是额外的作业将被忽略，除非我们设置工作流来运行它们。让我们将工作流添加到`.circleci/config.yml`文件的底部。

```
workflows:
  version: 2.1
  main:
    jobs:
      - build
  build_and_deploy:
    jobs:
      - build:
          filters:
            branches:
              ignore: /.*/
            tags:
              only: /^\d+\.\d+\.\d+$/
      - build-docker:
          requires:
            - build
          filters:
            branches:
              ignore: /.*/
            tags:
              only: /^\d+\.\d+\.\d+$/
      - publish-docker:
          requires:
            - build-docker
          filters:
            branches:
              ignore: /.*/
            tags:
              only: /^\d+\.\d+\.\d+$/ 
```

`main`工作流指定`build`作业应该执行，并且当这发生时没有给出任何特殊的过滤器，因此`build`作业在每次推送到 GitHub 时执行。

`build_and_deploy`工作流程有点复杂。在这个工作流中，我们指定当我们将任何符合指定正则表达式`/^\d+\.\d+\.\d+$/`的标签推送到 GitHub 时，将运行`build`作业。这个正则表达式是符合语义版本的标签的简单匹配，例如`1.0.1`。因此，如果我们向 GitHub 推送一个像`1.0.1`这样的标签，构建工作就会运行。

该工作流接下来指定`build-docker`作业将在相同的条件下运行，但是它要求`build`作业首先成功完成(这就是`requires`命令所做的)。

工作流的最后一步是在将标签推送到 GitHub 的相同条件下运行`publish-docker`作业，但前提是前一个`build-docker`作业成功完成。

我们完成的`.circleci/config.yml`文件应该是这样的(记住将对`chrishowejones`的引用改为您的 Docker Hub 帐户)。

```
 version: 2.1
    executors:
        docker-publisher:
          working_directory: ~/cci-film-ratings # directory where steps will run
          environment:
            IMAGE_NAME: chrishowejones/film-ratings-app
          docker:
            - image: circleci/buildpack-deps:stretch
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
          - persist_to_workspace:
              root: ~/cci-film-ratings
              paths:
                - target
      build-docker:
        executor: docker-publisher
        steps:
          - checkout
          - attach_workspace:
              at: .
          - setup_remote_docker
          - run:
              name: Build latest Docker image
              command: docker build . -t $IMAGE_NAME:latest
          - run:
              name: Build tagged Docker image
              command: docker build . -t $IMAGE_NAME:${CIRCLE_TAG}
          - run:
              name: Archive Docker images
              command: docker save -o image.tar $IMAGE_NAME
          - persist_to_workspace:
              root: ~/cci-film-ratings
              paths:
                - ./image.tar
      publish-docker:
        executor: docker-publisher
        steps:
          - attach_workspace:
              at: .
          - setup_remote_docker
          - run:
              name: Load archived Docker image
              command: docker load -i image.tar
          - run:
              name: Publish Docker Image to Docker Hub
              command: |
                echo "${DOCKERHUB_PASS}" | docker login -u "${DOCKERHUB_USERNAME}" --password-stdin
                docker push $IMAGE_NAME:latest
                docker push $IMAGE_NAME:${CIRCLE_TAG}
    workflows:
      version: 2.1
      main:
        jobs:
          - build
      build_and_deploy:
        jobs:
          - build:
              filters:
                branches:
                  ignore: /.*/
                tags:
                  only: /^\d+\.\d+\.\d+$/
          - build-docker:
              requires:
                - build
              filters:
                branches:
                  ignore: /.*/
                tags:
                  only: /^\d+\.\d+\.\d+$/
          - publish-docker:
              requires:
                - build-docker
              filters:
                branches:
                  ignore: /.*/
                tags:
                  only: /^\d+\.\d+\.\d+$/ 
```

现在可以将这个配置提交给 GitHub 了。

```
$ git add --all .
$ git commit -m "Added workflow & jobs to publish to Docker Hub."
$ git push 
```

现在，您可以标记当前构建并检查新工作流是否运行以构建 Docker 映像并将其推送到 Docker Hub:

```
$ git tag -a 0.1.0 -m "v0.1.0"
$ git push --tags 
```

这将创建一个带注释的标签，并将其推送到您的 GitHub 存储库中。这将触发`build_and_deploy`工作流来构建应用程序 Uberjar，构建 Docker 映像(`latest`和`0.1.0`)，并将这两个映像推送到您的 Docker Hub 存储库用于`film-ratings-app`。你可以在 CircleCI 仪表盘中查看，也可以在 Docker Hub 中浏览你的`film-ratings-app`库。

## 摘要

恭喜你！如果您已经阅读了本系列，那么您已经创建了一个简单的 Clojure web 应用程序，并使用 Docker 和 Docker Compose 对其进行了打包，这样您就可以在类似生产的环境中本地运行它。您还了解了如何让 CircleCI 构建、测试、打包和发布您的应用程序作为 Docker 映像。

在本系列的下一篇博文中，我将逐步介绍如何设置一个相当复杂的 AWS 环境，以便使用 Terraform 在云中运行我们的应用程序。

* * *

Chris Howe-Jones 是顾问 CTO、软件架构师、精益/敏捷蔻驰、开发人员和 DevCycle 的技术导航员。他主要从事 Clojure/ClojureScript、Java 和 Scala 方面的工作，客户从跨国组织到小型创业公司。

[阅读更多克里斯·豪-琼斯的文章](/blog/author/chris-howe-jones/)