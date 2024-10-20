# 如何在 Kubernetes 环境中运行日志条目

> 原文：<https://www.fairwinds.com/blog/how-to-run-logentries-in-a-kubernetes-environment>

 原木很棒。当你在一个或两个盒子上运行时，它们很容易。然而，当您在一个适度的分布式环境中运行时，它们会变得更加困难，如果您已经采用了微服务和容器，情况就更是如此。我收集了一些关于在 Kubernetes 环境中运行 Logentries 的笔记和想法。

在 [Fairwinds](/) ，我们为客户使用[Kubernetes](https://kubernetes.io/)。集中式日志对于他们了解其环境以及我们帮助进行故障排除至关重要。有时，客户会有他们希望我们使用的现有日志工具或服务。我当前的客户就是这种情况，他使用  [日志条目](https://logentries.com/)。

让它在 Kubernetes 运行起来并不困难，但有些东西是学来的。因为 Kubernetes 完全是关于容器的，所以 Logentries 的日志收集器也应该作为容器运行是有意义的。Logentries 的人显然也有同样的感觉，并在 docker hub 上提供了[log entries/docker-log entries](https://hub.docker.com/r/logentries/docker-logentries/)图像。说明书讨论了如何在容器中运行，但仅限于直  [docker](https://www.docker.com/) 环境。Kubernetes 是基于 docker 的，但是你的运行方式确实不同。

## 启动并运行日志条目

我将 Logentries 收集器作为  `DaemonSet` 运行在它自己的`ServiceAccount``kube-system`名称空间下。将它作为  `DaemonSet` 运行的原因是为了确保我们在每个节点上运行这些容器中的一个。一个专用的  `ServiceAccount` 不是必须的，而是有助于隔离事物。如果您不想添加到  `kube-system`中，您也可以使用不同的名称空间，但是它非常适合，因为这是用来记录所有内容的。

Logentries 允许您将日志发送到名为日志集的不同位置。为了让日志显示在日志集中，您需要创建日志集，然后创建一个访问令牌。因为这是一个秘密，我们将这样保存它。下面是一个例子  `secret.yml` 在 Kubernetes 中创建  `logentries_token` :

```
---
apiVersion: v1
data:
  logentries_token: <your token | base64 -w0>
kind: Secret
metadata:
  name: logentries
  namespace: kube-system 
```

用敷此`kubectl apply -f secret.yml`。接下来我们将创建  `ServiceAccount`。  `serviceaccount.yml` 看起来是这样的:

```
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: logentries
  namespace: kube-system 
```

用敷此`kubectl apply -f serviceaccount.yml`。现在我们已经准备好创建真正的工作马了。  `daemonset.yml` 看起来是这样的:

```
---
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: logentries
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app: logentries
  template:
    metadata:
      name: logentries
      labels:
        app: logentries
    spec:
      serviceAccount: logentries
      containers:
      - name: logentries
        image: logentries/docker-logentries
        imagePullPolicy: Always
        securityContext:
          privileged: true
        args:
          - "-t $LOGENTRIES_TOKEN"
          - "-j"
          # - "--no-stats"
          # - "--no-logs"
          # - "--no-dockerEvents"
          # - "-a <a special string>"
          # - "--stats-interval <STATSINTERVAL>
          # - "--matchByName REGEXP"
          # - "--matchByImage REGEXP"
          # - "--skipByName REGEXP"
          # - "--skipByImage REGEXP"
        volumeMounts:
        - name: socket
          mountPath: /var/run/docker.sock
        env:
        - name: LOGENTRIES_TOKEN
          valueFrom:
            secretKeyRef:
              name: logentries
              key: logentries_token
      volumes:
        - name: socket
          hostPath:
            path: /var/run/docker.sock 
```

其中你用  `kubectl apply -f daemonset.yml`。

如果你有一个标准的类型设置，类似于你从 kops 那里得到的，上面的应该可以工作。如果您已经完成了定制安装工作，您可能需要调整一些东西，如插座的位置等。

以上是默认配置，它将把容器日志、容器统计数据和 docker 事件发送到 Logentries。如果您希望通过标志来改变日志记录，我已经以注释的方式添加了它们。关于这些标志的详细信息可以在  [docker hub 上找到:log entries/docker-log entries/](https://hub.docker.com/r/logentries/docker-logentries/)。

## 观察

总的来说，它像宣传的那样工作。一旦  `DaemonSet` 运行，日志就会流动。每个容器或 Pod 的内存利用率都在 100MB 以下，到目前为止 CPU 消耗也不多。

容器生成的日志是 JSON 格式的，而  `-j` 开关确保事情正常通过。到达的 JSON 看起来干净且功能齐全。

我注意到的第一件事是，查找单个  `Deployment`的日志可能很棘手。例如，根据图像或容器的命名方式，您可能需要使用正则表达式。简单搜索不会找到子字符串。如果我们用一个名字  `pod-staging_nginx-worker` 搜索  `nginx`，它不会返回结果，但  `name=/.*nginx.*/` 会。

既然我们只获得了感兴趣的 pod 或 container 的日志，那么还有一个问题，那就是所有的统计数据也是搜索结果的一部分。要将结果减少到仅来自容器本身的日志，您可以添加另一个子句:  `AND line`。记录的每一行都将存储在一个名为  `line` `,` 的 JSON 属性中，从而强制搜索也只找到那些我们需要的项目:  `name=/.*nginx.*/ AND line`。

另一个仍在寻找答案的问题是您想要为  `application X` 和  `application Y`设置日志的用例。使用直接访问日志文件的经典 Logentries 收集器，您可以配置来自给定文件的日志将进入哪个日志集。在 Kubernetes 环境中，日志是通过 docker API 获取的，这是不可能的。不幸的是，一旦从 docker 接收到日志，Logentries 不提供切片和标记的方法。一种可能的选择是在每个 pod 旁边运行一个 Logentries 容器，并利用  `--matchByName` 或  `--matchByImage` 和  `--no-stats` 和  `--no-dockerEvents`。显然，这将导致大量的日志容器。我将对此进行更深入的研究，但默认可能是目前唯一合理的方法。

总之，Logentries 完全可用，并且很容易用 Kubernetes 设置。