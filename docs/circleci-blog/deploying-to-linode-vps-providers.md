# 从 CircleCI 部署到 Linode 和其他 VPS 提供商- CircleCI

> 原文：<https://circleci.com/blog/deploying-to-linode-vps-providers/>

构建和测试软件的最终目标通常是部署该软件。CircleCI 官方博客和许多社区博客展示了部署到 Kubernetes 集群、推送 Docker 注册中心、上传到 AWS S3，甚至新的无服务器技术的例子。然而，对于个人开发者甚至小公司来说，使用这样的大型部署平台并不总是可行的。

但是还有其他选择。

在这篇博文中，我们将介绍几种将软件部署到传统虚拟专用服务器(VPS)的方法，例如由 [Linode](https://www.linode.com/) 、 [DigitalOcean](https://www.digitalocean.com/) 或 [Vultr](https://www.vultr.com/) 提供的那些。

### SSH 密钥

我们将要介绍的所有工具都将通过互联网发送您的数据，以便部署您的软件。既然如此，拥有一个安全的连接是非常重要的。这篇文章中的许多例子将使用 SSH 密钥来加密连接。我建议专门为 CircleCI 创建一个全新的 SSH keypair。我最喜欢的创建 SSH 密钥的资源是 GitHub 的[。](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/)

私钥应该通过 web 应用程序存储在 CircleCI 上。我们有指示你可以跟着[到这里](https://circleci.com/docs/add-ssh-key/)。对于您想要部署到的远程服务器上的用户，公钥应该存储在`authorized_keys`文件中。例如，如果您正在部署一个 Linux 服务器`prod@example.com`，那么公钥应该被附加到远程机器上的文件`/home/prod/.ssh/authorized_keys`中。

### Rsync

`rsync`是一个已经存在了一段时间的文件传输工具。该名称来自“远程同步”，这意味着它允许您将文件从一个目录或机器传输到另一个目录或机器，但同时保持它们同步。这很有用，因为这个目的意味着`rsync`可以比较远程服务器上的文件，如果它们已经存在，就不要传输它们，这有助于更快和更有效的部署。

当你有几个文件要传输时，比如用 Hugo 或 Jekyll 构建一个静态生成的网站时，这种方法非常有效。基本的 Rails、Flask 和 React 应用程序也可以这样部署。

### 使用

```
rsync -va --delete ./my-files/ user@example.com:/var/www/html 
```

对于`rsync`，您提供选项，您想要传输的文件或目录，然后您想要传输到哪里。

*   “v”表示打开详细度，以便我们可以在出现问题时看到发生了什么，而“a”表示存档模式。存档模式打开了许多有用的选项，如递归、复制文件权限和所有权等等。

*   `--delete` -这告诉`rsync`如果一个文件在 CircleCI 上被删除，它也应该从远程服务器上被删除。闲置的旧文件可能会导致不必要的问题，比如生产与您的本地环境不匹配，从而导致不必要的影响。

*   `./my-files/` -我们要复制其内容的目录。请注意，有一个尾随斜线，尽管对于`rsync`，没有尾随斜线。`./my-files`没有尾随斜线意味着“复制整个目录，包括其中的所有内容。”`./my-files/`末尾的斜杠表示“只复制目录的内容，而不复制目录本身。”

*   `user@example.com`--“用户”是我们要部署到的远程服务器“example.com”上的用户名。

*   `:/var/www/html` -这是远程服务器上的文件路径，`rsync`应该将文件发送到该路径。对于 web 服务器，这应该是 DocumentRoot。

### 安装

虽然一些 Docker 镜像可能已经安装了`rsync`，但是[官方 CircleCI 镜像](https://hub.docker.com/r/circleci)和第三方 [CI 构建镜像](https://hub.docker.com/r/cibuilds)没有。然而，安装它相当简单:

```
# Ubuntu & Debian based Docker images
sudo apt-get update && sudo apt-get install rsync

# Alpine based Docker images
apk update && apk add rsync 
```

### CircleCI 示例

这是我的个人网站(Feliciano)的 CircleCI 配置片段。Tech)，通过`rsync`部署:

```
 deploy:
    docker:
      - image: cibuilds/hugo:0.42.1
    steps:
      - attach_workspace:
          at: src
      - add_ssh_keys
      - run: |
          echo 'web01.revidian.net ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJiGRY6N9WYQ0vy6cTiwAgNbc6ueJmVo/EafBtmT7bcD6cQMbipYM/KfYQ2lCn2TxqWepZKYoyoVQXgArycCOns=' >> ~/.ssh/known_hosts
          rsync -va --delete src/public/ staticweb@web01.revidian.net:www/feliciano.tech/public_html 
```

### 相关工具

除了`rsync`之外，还有其他类似的选项。被称为“安全复制”的“T1”和被称为“安全 FTP”的“T2”是两种工具，它们都利用 SSH 进行加密，并处理基本的文件传输过程，您可以使用它们部署到 VPS。
记下最后一条。
老式的 FTP 是完全不安全的，不应该使用。每当你想到 FTP，就想到 SFTP。

## 停靠点(SSH)

如果你在 Linode 或类似的提供商上运行 Dockerized 应用，你可能没有复杂的编排工具，如 Kubernetes 或 Docker Swarm running。当然这并不意味着你不能，但是这对于 VPS 用户来说不太常见。那么我们如何在不手动登录服务器的情况下，从 CircleCI“部署”一个 Docker 应用呢？让我们看一个例子:

```
 deploy:
    docker:
      - image: circleci/golang:1.10
    steps:
      - attach_workspace:
          at: src
      - add_ssh_keys
      - run: |
          docker build .
          docker login -u $DOCKER_USER -p $DOCKER_PASS
          docker push myuser/myapp
          echo 'example.com ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJiGRY6N9WYQ0vy6cTiwAgNbc6ueJmVo/EafBtmT7bcD6cQMbipYM/KfYQ2lCn2TxqWepZKYoyoVQXgArycCOns=' >> ~/.ssh/known_hosts
          ssh root@example.com <<'ENDSSH'
          docker pull myuser/myapp
          docker stop my-app
          docker rm my-app
          docker run --name=my-app --restart=always -v $PWD:/app -d myuser/myapp
          ENDSSH 
```

在这个部署作业中，我们为示例应用程序构建了一个新的 Docker 映像，并将其推送到 Docker Hub。这使得该应用程序可以在 CircleCI 之外使用。然后，我们使用 SSH 命令的一个特性，该特性允许我们在远程机器上运行命令，在本例中是我们的生产机器。我们使用 SSH 登录，将新的 Docker 映像拖到生产机器上，停止、删除并使用更新后的映像重新启动应用程序。

为了简化 CircleCI 配置，我们还可以将远程 Docker 命令保存到名为`update-app.sh`或类似的 Bash 脚本中，然后从 CircleCI 运行该脚本。看起来会像这样:

```
ssh root@example.com '/var/app/update-app.sh` 
```

如果你尝试一下，[让我知道](https://twitter.com/FelicianoTech)你做得怎么样！

总而言之，如果您不需要像 Kubernetes 和 AWS S3 这样的大型提供商的灵活编排，使用 VPS 来部署您的应用程序是一个经常被忽略但可行的选择。我写这篇文章的目的是展示一些可以和 CircleCI 一起用于发布应用程序的其他部署选项。