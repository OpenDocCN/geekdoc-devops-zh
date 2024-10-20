# 在 CircleCI 2.0 中设置复杂的容器

> 原文：<https://circleci.com/blog/setting-up-tricky-containers-in-circle-2-0-multi-image/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

有些 Docker 容器非常适合 CircleCI 2.0。例如，Postgresql 只需传入几个变量就能完成你需要的一切:

```
version: 2.0
jobs:
  build:
    docker:
      - image: clojure:alpine
      - image: postgres:9.6
        environment:
          POSTGRES_USER: username
          POSTGRES_DB: db
          ... 
```

但是有时候你会遇到第三方容器，它玩起来不太好。您将需要访问一些资源，这些资源存在于容器内部，而不存在于其他地方。您将遇到的问题是 CircleCI 将您的执行环境作为您的主要映像。如上所述，虽然我可以访问 Postgres 公开的端口，但是`psql`不在我的`$PATH`中。只有 Clojure 的 Alpine 容器的内容是。哈希公司的金库就是这样一个容器。每次引导时，Vault 将选择一个新的根令牌，任何对它的访问调用都需要该令牌。现在，如果 Vault 是您的主映像，这真的没有问题，因为存储这个密钥的文件位于`$HOME/.vault-token`。但是如果这个图像是这样的:

```
version: 2.0
jobs:
  build:
    docker:
      - image: clojure:alpine
      - image: postgres:9.6
        environment:
          POSTGRES_USER: username
          POSTGRES_DB: db
      - image: vault:0.7.3 
```

你会过得很糟糕。

## 简单的变通方法

在对 Vault 进行一些测试时，我决定实际调用资源，而不是模仿对它的调用。当地的事情进展得或多或少很顺利，但是进入切尔莱西的`vault-token`被证明是一件苦差事。

### Python 简单服务器拯救世界

如果您需要访问不透明容器中的资源，我强烈推荐使用用`python -m SimpleHTTPServer`包装容器的方法。`SimpleHTTPServer`通过 http 服务于它所在的文件系统，这意味着你可以通过一个端口遍历和访问你的容器上的文件系统。

最后，我这样做只是为了让我的测试正常工作(下面的所有代码都可以在这里找到):

*Dockerfile*

```
 FROM vault:0.7.3

RUN apk add --update python

ADD ./vault/ /vault
WORKDIR /vault

ENV VAULT_ADDR=http://127.0.0.1:8200
ENV SKIP_SETCAP=skip

EXPOSE 8201

ENTRYPOINT ["./docker-entrypoint.sh"] 
```

*坞站-entrypoint.sh*

```
python_http_server() {
  # we want to be able to server the VAULT_TOKEN for testing
  cd $HOME/
  python -m SimpleHTTPServer 8201
}

vault_server () {
  vault server -dev
}

vault_server & python_http_server 
```

将容器发布到 [Dockerhub](https://hub.docker.com/r/mannimal/vault-cci/) ，然后将其包含在我的多重图像层中:

*。circi/config . yml〔t1〕*

```
version: 2.0
jobs:
  build:
    docker:
      - image: clojure:alpine
      - image: postgres:9.6
        environment:
          POSTGRES_USER: contexts
          POSTGRES_DB: contexts_service
      - image: mannimal/vault-cci:latest 
```

现在在我的`.circleci/config.yml`中我有这样一句台词:

```
 - run:
          name: Setup Vault
          command: |
            VAULT__CLIENT_TOKEN=`curl localhost:8201/.vault-token`

            curl --fail -v -X POST -H "X-Vault-Token:$VAULT__CLIENT_TOKEN" -d '{"type":"transit"}' localhost:8200/v1/sys/mounts/transit
      - run:
          name: Run Tests
          command: |
            export VAULT__CLIENT_TOKEN=`curl localhost:8201/.vault-token`
            lein test 
```

然后，使用 CCI 将上述所有内容持续部署到 Dockerhub。

黑客快乐！