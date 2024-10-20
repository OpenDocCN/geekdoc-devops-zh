# Kubernetes emptyDir 与 Docker 的 volumes 不同-from

> 原文：<https://www.fairwinds.com/blog/kubernetes-emptydir-not-the-same-as-dockers-volumes-from>

 我当前的客户有一个与 Nginx 紧密耦合的 Rails 应用程序。这种耦合非常常见，用于避免静态文件由 Rails 堆栈提供服务。在这方面最值得注意的文件是应用程序资产。使用类似  `rake assets:precompile`的命令将资产创建为应用程序的一部分。那些资产是特定于代码的特定版本的。

我之前使用过 docker 的  `--volumes-from` 在 sidecar 风格的容器间共享文件，效果相当好。令我惊讶的是，我发现 Kubernetes 没有这个特性的直接模拟。Kubernetes 提供各种类型的卷，其中最接近的是 Kubernetes emptyDir。然而，Kubernetes 中的  `emptyDir` 并不等同于 docker 的  `--volumes-from`。

## 它有什么不同

在 Docker 中，  `--volumes-from` 通过  `--volume`直接连接到另一个容器共享的空间。确切地说，如果您用 docker run`--name app --volume /app/assets <app image>`运行一个容器，那么您可以从另一个容器访问  `/app/assets`中的数据，如下所示  `docker run --name nginx --volumes-from app:/app/assets <nginx image>`。这将允许  `nginx` 容器直接访问  `app` 容器中的资产。库伯内特的  `emptyDir` 并没有创造出这样的直接联系。相反，它创建了一个空目录(是的，这个名字暗示了这一点)，然后这个目录在容器中变得可用。它看起来是这样的:

```
---
kind: Deployment
apiVersion: extensions/v1beta1
metadata:
  labels:
    app: test
  name: test
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: test
    spec:
      # here we set up the empty directory
      volumes:
      - name: shared-assets
        emptyDir: {}
      containers:
      - name: app
        image: app
        volumeMounts:
        - name: shared-assets
          mountPath: /app/assets
      # the nginx container
      - name: nginx
        image: nginx
        volumeMounts:
        - name: shared-assets
          mountPath: /app/assets 
```

从表面上看，这非常相似。事实上，Kubernetes emptyDir 方法似乎有一个额外的好处，即允许挂载位于容器中的不同位置。例如，您可以将  `nginx` 改为在  `/flubble/monkey` 中挂载共享资产。这对于 Docker 方法是不可行的。

如果在连接  `emptyDir`的位置有数据，问题就出现了。假设你已经在  `/app/assets`中有了要分享的文件。当您创建  `emptyDir` 并将其附加到 app 容器时，它将具有与在包含数据的目录顶部挂载相同的效果。这与您将在 Linux 命令行上看到的行为相同。实际上，现有文件被屏蔽并隐藏起来。这显然是不可取的。

## 如何让它工作

绕过这个限制的一个简单方法是将共享文件复制到  `emptyDir` 位置。上述部署将如下所示:

```
---
kind: Deployment
apiVersion: extensions/v1beta1
metadata:
  labels:
    app: test
  name: test
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: test
    spec:
      # here we set up the empty directory
      volumes:
      - name: shared-assets
        emptyDir: {}
      containers:
      - name: app
        image: app
        volumeMounts:
        - name: shared-assets
          ### the new location
          mountPath: /app/shared-assets
      # the nginx container
      - name: nginx
        image: nginx
        volumeMounts:
        - name: shared-assets
          mountPath: /app/assets 
```

这样会将  `/app/shared-assets` 链接到  `app` 容器中的  `emptyDir` ，这样就可见  `nginx` 容器中的  `/app/assets`了。这将仍然是空的。要填充这个空间，您需要将  `app` 容器中的资产从它们自然存在的位置复制到链接到  `emptyDir`的位置。对此，以下内容就足够了:

```
cp -r /app/assets/ /app/shared-assets/ 
```

## 潜在的缺点和替代方案

这是创建 sidecar 的一个非常简单的方法，它从另一个容器中访问文件。然而，它确实给容器启动增加了额外的步骤和时间。根据需要复制的空间大小，对于您的用例来说，这可能是也可能不是一种合理的方法。还有其他几种选择。

1.  网络资产可以被转移到另一个服务上，比如 S3(T2)或 T4()的 CDN。这可以完全消除对  `nginx` 容器的需求，这取决于用例。
2.  可以扩展构建过程，将文件直接放入另一个容器。这将消除部署集装箱边车风格的需要，并开辟了单独扩展每个部分的可能性。

在我的紧耦合用例中，除了用例之外，还需要额外的重写等等，这是最简单的方法。它还为应用程序在 Kubernetes 之外的运行方式提供了很好的对等性。当许多其他重大变化正在发生时，很难夸大这种熟悉的价值。资源消耗也非常低，这使它成为一个简单的选择。我们可能会在未来考虑替代方案，但目前这是一个可靠的方法。