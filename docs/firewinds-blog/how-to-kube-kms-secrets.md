# 如何 Kube: KMS 的秘密

> 原文：<https://www.fairwinds.com/blog/how-to-kube-kms-secrets>

 安全性是网络上所有人最关心的问题，尤其是那些使用 Kubernetes 的人。由于平台上没有任何预先配置的安全设置，因此由您来确保您的节点和集群是安全的。这就是加密在保护您最敏感的数据方面如此重要的原因。

密钥管理服务(KMS)是一种在 Kubernetes 中加密秘密的廉价方式。使用 KMS 可以通过控制加密密钥和元数据的机密性、完整性、可用性和来源验证来确保它们受到保护。

请继续阅读如何设置亚马逊提供的密钥管理服务的分步指南。

## **构建 AWS 加密提供程序 Docker 容器**

1.  首先，前往 Kubernetes sigs AWS 加密提供者存储库。在存储库中，运行“make docker build”来构建 docker 容器。
2.  用您控制的存储库的名称标记 docker 容器，并将其推送到该存储库。这确保了 docker 容器在 Kubernetes 集群上——特别是主节点上——是可用的。
3.  既然 docker 容器被推送到存储库，那么您需要创建一个 KMS 键。

## **创建一个 KMS 键**

1.  要创建 KMS 键，请执行以下操作:
    *   打开 AWS 控制台，
    *   点击“创建密钥”
    *   选择“对称密钥”
    *   给它起个你能记住的名字
    *   为您的密钥分配默认选项(在此阶段没有必要分配使用权限)。
2.  创建密钥后，在“客户管理的密钥”下找到它，并记录 ARN，以便在后面的步骤中使用。

## **配置节点策略**

1.  接下来，您需要创建一个节点策略，允许主节点使用您刚刚创建的密钥进行加密和解密。
2.  将此策略添加到“附加策略”主部分的 to kops 集群规范中，然后保存文件。
3.  然后，在 kops 集群规范下，添加“encryptionConfig”并将其设置为 true。
4.  最后，您需要添加一个文件资产，在每个主节点上创建一个静态 pod。静态 pod 的清单包括:
    *   您之前推送的图像名称
    *   您创建的 KMS 键的 ARN
    *   您在其中创建密钥的区域

## **创建加密配置**

1.  接下来，您需要创建一个加密配置 yaml 文件。该文件指定应该使用 AWS 加密提供程序在您在 static pod 中创建的 unix 套接字上加密机密。
2.  使用“kops create secret”命令将此加密配置创建为 kops secret。
3.  既然集群配置已经就绪，并且加密配置被保存为一个秘密，那么是时候替换集群 yaml 并输入“kops update cluster - yes”了。
4.  Kops 随后将指示需要滚动重启主设备。继续执行滚动更新。

一旦滚动更新完成，您的集群将被配置为使用密钥管理服务加密所有新创建的秘密！这确保您可以在不暴露主密钥的情况下加密/解密数据，并且您的加密参数总是很强。

关于如何解决 Kubernetes 集群安全性的更多技巧，  [查看这篇博文](https://www.fairwinds.com/blog/cluster-health-checkup-security)。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)