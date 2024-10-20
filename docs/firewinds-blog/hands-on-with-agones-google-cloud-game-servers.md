# 实践 Agones 和 Google 云游戏服务器

> 原文：<https://www.fairwinds.com/blog/hands-on-with-agones-google-cloud-game-servers>

 我最近有幸探索了 [Agones](https://agones.dev/site/) 和 [Google 云游戏服务器(GCGS)](https://cloud.google.com/game-servers/) ，我想分享一下我的经验。

Agones 是一个在 Kubernetes 中运行多人游戏服务器的开源平台。在这次探索中，我从未运行过任何类型的游戏服务器，但是我在 Kubernetes 中处理过实时通信工作负载。我看到了这两种经历之间潜在的相似问题集，并好奇 Agones 将如何解决我在过去看到的问题。

在 Agones 之上，GCGS 是一个多集群管理层，使得跨多个集群部署游戏服务器更加容易。在我更详细地介绍 GCGS 之前，有必要对 Agones 有一个更深入的了解，它是如何安装的，以及它有什么作用。

## **Agones**

### **安装**

关于 Agones，我注意到的第一件事是，它完全是库伯内特斯土著。我是这个的忠实粉丝，因为这些天我在 Kubernetes 上运行一切；有[安装与舵](https://agones.dev/site/docs/installation/install-agones/helm/)指令，我立即跳转。在我的 GKE 集群上的初始 Helm 安装进展顺利，几分钟内我就有了一些自定义资源定义(CRD)和一个控制平面在 agones-system 名称空间中运行。

```
▶ kubectl get deploy -n agones-system -oname
deployment.extensions/agones-allocator
deployment.extensions/agones-controller
deployment.extensions/agones-ping 
```

```
▶ kubectl get crd -oname | grep agones
fleetautoscalers.autoscaling.agones.dev
fleets.agones.dev
gameserverallocationpolicies.multicluster.agones.dev
gameservers.agones.dev
gameserversets.agones.dev 
```

### **创建一个游戏服务器**

我有一个控制器、一个分配器、一个 ping 服务器和一些看起来有用的 CRD，但是我现在该做什么呢？遵循文档中的[，下一步似乎是创建一个游戏服务器。](https://agones.dev/site/docs/getting-started/create-gameserver/)

所以我按照文档从提供的 [yaml 清单](https://raw.githubusercontent.com/googleforgames/agones/release-1.8.0/examples/simple-udp/gameserver.yaml)中创建了一个游戏服务器。这个游戏服务器使用一个名为 `simple-udp` 的示例服务来展示 Agones 是如何工作的，这是我们开始了解 Agones 真正在做什么的地方。

如果我看一下 gameserver 规范，我会发现它包含一个容器规范，就像 pod 规范一样。它还通过他们所谓的 `portSpecification:` 来声明端口

```
spec:
  ports:
  - name: default
    portPolicy: Dynamic
    containerPort: 7654 
```

这意味着它将使用一个 `Dynamic` 外部端口，并且容器正在监听端口 7654。很酷，到目前为止很简单。

进一步看，我可以检查 gameserver 以查看分配了哪个端口:

```
▶ kubectl get gameserver                                                                                            
NAME               STATE   ADDRESS         PORT      AGE    
simple-udp-4pgls   Ready   35.224.97.238   7063 
```

看起来我有一个运行 pod 并监听端口 `7063;` 的游戏服务器，现在我将尝试使用 netcat 通过 UDP 连接。

```
▶ nc -u 35.224.97.238 7063   
HI                           
ACK: HI                      
EXIT                         
ACK: EXIT 
```

`simple-udp` 服务器完成了它的工作，用我发送的任何东西来响应我，然后我给出了退出命令。这个服务器的设置方式是，当它接收到一个 `EXIT` 命令时，它告诉 Agones 它完成了，然后 Agones 关闭游戏服务器。我们可以在实践中看到这一点:

```
▶ kubectl get po
NAME               READY   STATUS        RESTARTS   AGE
simple-udp-4pgls   0/2     Terminating   0          30s 
```

```
▶ kubectl get gameserver
NAME               STATE      ADDRESS         PORT                                  
simple-udp-4pgls   Shutdown   35.224.97.238   7063 
```

pod 终止，游戏服务器状态为 `Shutdown.`

### **关于网络和集群创建的简要说明**

我第一次尝试连接游戏服务器时，没有任何反应。就我而言，这是因为我没有遵循 Agones 安装指南中所有精彩的文档。我跳过了[集群创建](https://agones.dev/site/docs/installation/creating-cluster/gke/)部分，因为我认为我已经知道如何创建一个集群。原来这里有一个步骤有点不同寻常。由于这些游戏服务器都需要监听 UDP 端口，我们需要在 GCP 防火墙上打开一组端口。请注意，本例中的端口范围是 udp:7000-8000。此端口范围由 Agones 控制面板安装控制，并且可以通过舵图表中的值进行配置:

```
▶ gcloud compute firewall-rules create game-server-firewall \
  --allow udp:7000-8000 \
  --target-tags game-server \
  --description "Firewall to allow game server udp traffic" 
```

完成之后，我们需要用 `game-server` 标记节点，以便应用这个规则。从一开始就遵循集群创建说明要容易得多，这会让您在开始时添加标记并创建防火墙规则。

### **艾格尼丝建筑**

你可能会问自己为什么 gameserver 这个东西很重要，所以我要离开探索一步，谈谈 Agones 实际上在做什么。

游戏服务器在理论上只是一个进程，多个客户端可以连接到这个进程来玩游戏。这有许多不同的实现方式，但是这些服务器必须做几件关键的事情，这会影响它们在 Kubernetes 中的运行方式:

1\. Gameservers must remain available during the time that the game is being played.

事实上，gameserver pod 必须在指定的时间内不间断，这意味着我们不能因为自动缩放、耗尽或任何其他原因而杀死 pod。为了解决这个问题，gameserver 以一种部署无法做到的方式管理 pod 生命周期。Agones 为游戏服务器引入了几种状态，游戏代码本身能够通过 Agones API 更新这种状态。Agones 在每个游戏服务器中运行一个 sidecar 来接收流量。

2\. Gameservers must accept connections on some specified port from multiple clients.

由于端口耗尽的可能性，当在 Kubernetes 中运行多个游戏服务器时，这也是一个问题。我们需要一种方法来分配游戏服务器可以使用的许多不同的端口。Agones 通过其动态端口分配策略无缝处理这一问题。每个游戏服务器都被分配了一个端口和一个 IP 组合，客户端可以使用它们进行连接。

## **谷歌云游戏服务器(GCGS)**

既然我们已经掌握了 Agones 如何安装和游戏服务器如何运行的基本知识，我们可以开始看看 GCGS 在此基础上提供了什么。我从他们文档中的[快速入门](https://cloud.google.com/game-servers/docs/quickstart)指南开始。本指南要求您做的第一件事是创建一个集群并向其部署 Agones。我已经完成了，所以我跳到了创建 GCGS 王国的部分。

### **领域**

GCGS 的领域是一个组织结构。在这个探索过程中，我试图找出将一大群全球集群组织成领域的最佳方式。我最终在 Agones public Slack 和一个谷歌人聊了聊。(有一个 `#google-cloud-game-servers` 频道在那里)。在这里，我不会太深入地讨论领域组织，但我得到的最佳建议是:

*   *一个很好的经验法则是“从玩家的角度来看，集群组之间的延迟差异无关紧要”*

反正我跑题了。一旦您有一个或一组运行 Agones 的集群，为了使用 GCGS，您必须将它们添加到一个领域中。这很简单，用几个 `gcloud` 命令就可以做到:

```
▶ gcloud game servers realms create agones-blog --time-zone EST --location us-central1
Create request issued for: [agones-blog]
Waiting for operation [projects/gcp-prime/locations/us-central1/operations/operation-1598980635305-5ae43b0c52589-34a097b6-d0659fc7] to c
omplete...done.
Created realm [agones-blog]. 
```

```
▶ gcloud game servers clusters create agones-blog --realm=agones-blog --gke-cluster locations/us-central1/clusters/agones-blog --namespace=default --location us-central1 --no-dry-run
Create request issued for: [agones-blog]
Waiting for [operation-1598980727087-5ae43b63da1ee-88b19318-22a70d4f] to finish...done.
Created game server cluster: [agones-blog] 
```

第一个命令创建领域，第二个命令将运行 Agones 的集群连接到该领域。

现在我们已经在一个领域中有了一个集群，下一步是创建一个部署。部署基本上是一组配置的容器，这些配置将描述一组游戏服务器。因此，我们创建部署，然后在其中创建配置:

```
▶ gcloud game servers deployments create agones-blog
Create request issued for: [agones-blog]
Waiting for operation [projects/gcp-prime/locations/global/operations/operation-1598980944523-5ae43c3336ffd-4eb7fce4-fd872d08] to complete...done.
Created deployment [agones-blog]. 
```

```
▶ gcloud game servers configs create config-1 --deployment agones-blog --fleet-configs-file fleet_configs.yaml
Create request issued for: [config-1]
Waiting for operation [projects/gcp-prime/locations/global/operations/operation-1598981023478-5ae43c7e83334-58008b9c-8df141c0] to complete...done.
Created game server config [config-1]. 
```

注意，在创建配置时，我指定了一个包含车队规范的 yaml 文件。车队规范看起来很像我们之前部署的 gameserver 规范，但是具有模板和副本字段:

```
- name: fleet-spec-1
  fleetSpec:
    replicas: 2
    template:
      metadata:
        labels:
          foo: bar
      spec:
        ports:
        - name: default
          portPolicy: Dynamic
          containerPort: 7654
        template:
          spec:
            containers:
            - name: simple-udp
              image: gcr.io/agones-images/udp-server:0.17 
```

这表明我们希望创建两个游戏服务器，并保持副本数量为 2。如果 gameserver 规范类似于 pod 规范，那么舰队就很像 Kubernetes 部署。

最后要做的事情是将该部署部署到领域中的集群。

```
▶ gcloud game servers deployments update-rollout agones-blog --default-config config-1 --no-dry-run
Update rollout request issued for: [agones-blog]
Waiting for [operation-1598981253616-5ae43d59fd30b-b841d131-f1822e0c] to finish...done.
Updated rollout for: [agones-blog]
createTime: '2020-09-01T17:22:24.587136253Z'
defaultGameServerConfig: projects/gcp-prime/locations/global/gameServerDeployments/agones-blog/configs/config-1
etag: fHXlfY2MivvPraKyPJEseJF5SqjaBfUrnaWMGT1aCb8
name: projects/gcp-prime/locations/global/gameServerDeployments/agones-blog/rollout
updateTime: '2020-09-01T17:22:25.547699385Z' 
```

完成后，我们看到在我们的集群中部署了 agones 车队，配备了我们要求的两台游戏服务器:

```
▶ kubectl get fleet
NAME                         SCHEDULING   DESIRED   CURRENT
fleet-agones-blog-config-1   Packed       2         2 
```

```
▶ kubectl get gameserver
NAME                                     STATE   ADDRESS         PORT
fleet-agones-blog-config-1-st55c-8gbd2   Ready   34.123.40.127   7212
fleet-agones-blog-config-1-st55c-rnxjh   Ready   34.123.40.127   7839 
```

### 为什么使用 GCGS？

到目前为止，看起来你可以使用 Agones CRD 将舰队部署到你的集群，这是完全正确的。GCGS 的真正优势在于这些车队的多集群管理。

在进一步的探索中，我启动了另一个集群，安装了 Agones，并将该集群添加到领域中。当我添加第二个集群时，我看到舰队也被部署到新的集群。这告诉我，集群现在由 GCGS 集中管理。我可以在领域中随意添加和删除集群，并且我的游戏服务器部署保持不变。这是一个非常强大的概念，它将使管理游戏服务器的大规模部署变得更加容易。

## **游戏服务器分配**

到目前为止，我们已经看到了如何使用 Agones + GCGS 将游戏服务器部署到多个集群，但是我们实际上如何使用这些游戏服务器呢？我们知道每个游戏服务器都处于 `Ready` 状态，并且每个游戏服务器都可以在指定的端口和 IP 地址上接收 UDP 流量。现在让我们探索另一个强大的 Agones 概念:分配。

您可能已经注意到，其中一个控制平面部署名为 `agones-allocator.` 默认情况下，Helm chart 会部署这个控制平面和一个相应的服务，但是在您可以使用它之前，它需要进一步的配置。分配服务的设置超出了本文的范围，但是在 **[高级 Agones 文档中有详细介绍。](https://cloud.google.com/game-servers/docs/how-to/configuring-multicluster-allocation)**

一旦配置了分配器服务，您就可以使用 mTLS 和 gRPC 向分配器发出请求。该请求可以指定选择器，该选择器将通过标签来限制游戏服务器的选择。您得到的响应是一个 IP 地址和一个游戏服务器的端口。真正酷的部分是这个游戏服务器的状态现在在 Kubernetes API 中变成了 `Allocated` 。这意味着不允许分配器服务再次分配那个游戏服务器，并且在节点耗尽的情况下，这个游戏服务器不能再被 Kubernetes 关闭。实际上，这种 pod 现在是一种长寿服务，只要它还在使用，Agones 和 Kubernetes 就会努力让它保持活力。

此外，GCGS 还可以设置多集群分配，这样，每当您向领域中的集群发出分配请求时，您都可以被分配到该领域中任何集群中的任何可用游戏服务器。这是涵盖在[【GCGS】文档中的](https://cloud.google.com/game-servers/docs/how-to/configuring-multicluster-allocation)。

## **总之**

Agones 是一个开源平台，提供 CRDs 和一个控制器，用于在 Kubernetes 中运行游戏服务器。运行起来很简单，文档也很棒。添加 Google 云游戏服务器可以让你从一个中心点管理多个运行 Agones 的集群，并使跨所有集群部署游戏服务器变得更加简单。

在我探索这些产品的过程中，我越来越对它们让运行游戏服务器变得更容易的潜力感到兴奋。我遇到的障碍很小，Agones public Slack 中的谷歌员工总是乐于助人，提供丰富的信息。我很少探索一个新的谷歌产品(当我第一次遇到它时，它还处于测试阶段)并有这种很好的体验。向在 Agones 工作的团队大声欢呼。

[![Free Download: Kubernetes Best Practices Whitepaper](img/b53e4ee22b6ef19bc06a035649ea1dc6.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/4f92c7e1-1646-4985-9a0a-b1091903dddb)