# 什么是 PaaS(平台即服务)？装具

> 原文：<https://www.harness.io/blog/what-is-paas>

在我看来，马达加斯加的猴面包树是地球上最迷人的树之一。如果你想享受你自己的猴面包树，有很多事情要做，甚至要适应气候和土壤，然后等待很长时间，才能拥有你的神奇猴面包树时刻。如果马达加斯加太远，你可以去佛罗里达中部，和家人一起拍几张值得在 Instagram 上观看的照片，然后继续前行，享受迪士尼动物王国的生命树。

由于这不是(很大程度上不是)一个旅游博客，拥有一个平台即服务(PaaS)可以让我们专注于软件开发中令人愉快的部分，而不必担心选择的巨大生态系统和获得正确环境的组合。PaaS 在过去几年的兴起带来了主流和固执己见的软件运行方式。

## 什么是 PaaS？

平台即服务旨在提供您的应用程序运行所需的所有应用程序基础设施。应用程序基础架构不同于物理基础架构[例如存储、计算、网络]，因为应用程序基础架构是支持您的软件的软件。著名的[“披萨即服务”图](https://medium.com/@pkerrison/pizza-as-a-service-2-0-5085cd4c365e)很好地辨别了 IaaS/PaaS/SaaS 之间的区别。

在 Heroku 之前有很多 PaaS，但是对很多人来说 Heroku 是第一个主流的、面向大众的 PaaS。Heroku 的联合创始人之一 Adam Wiggins 也因十二要素应用程序的[而闻名，该应用程序一直是许多云原生应用程序的创建和开发的支柱。在 Heroku 于 2010 年被 SalesForce 收购后，PaaS 提供商开始涌现并成为主流。](https://www.12factor.net/)

如今，主要的 PaaS 提供商都围绕着传统的 PaaS 产品，从 [OpenShift](https://www.openshift.com/) 和 [Cloud Foundry](https://www.cloudfoundry.org/) 阵营到抽象平台，如 DC/操作系统。公共云提供商可以辩称，他们的服务集合可以构成 PaaS，但出于可移植性的考虑，我们可以专注于更传统的 PaaS 产品。Heroku 最初是一个在 PaaS 平台上开发 Ruby 的平台，类似于 PaaS 平台，现在已经变得更加多元化，可以处理更多的工作负载。

## 当我看到一个 PaaS 时，我如何识别它？

平台即服务与开发者体验息息相关。您正在使用某种 PaaS 的第一个重要标志是应用程序目录。将应用程序目录想象成一个菜单，您可以通过自助服务订购您和您的应用程序所需要的内容。 [Pivotal Marketplace](https://pivotal.io/platform/services-marketplace) 组织得很好，展示了 Pivotal 产品中可以运行的内容。供应商也可以发布到应用程序目录。大多数时候，如果在公共目录中，您会得到一个操作性更强的软件包版本。

PaaS 产品具有不同级别的意见和可配置的防护栏。比方说，您的组织强化了某些应用程序基础架构的版本，例如 Apache Kafka。您的平台工程团队可以轻松打包一个强化版本，并在应用程序目录中提供该强化版本，如[DC/操作系统宇宙](https://kafka.apache.org/)。如果您的应用程序需要某种 Kafka 基础设施，构建基础设施就像点击一样简单。不需要批准，批准的版本就在那里，允许的容量会很快增加。

对于企业而言，缩小应用程序基础架构的选择范围是一个明智之举。你需要支持的技术越少，专注的能力就越强。PaaS 还可以充当通向混合/多云世界的桥梁。PaaS 平台最有可能跨基础设施提供商运行，即便携平台；几年前，这是一个很大的采购点。如果您的应用程序可以在 PaaS 中运行，并且 PaaS 可以安装在通用基础架构上，那么实现混合/多云架构就指日可待了。

与应用程序需要维护的方式类似，PaaS 仍然是一个需要维护的平台。Cloud Foundry 组织拥有 [Bosch](https://www.cloudfoundry.org/bosh/) ，可以通过多种方式升级/维护平台，并可能升级/维护在 Cloud Foundry PaaS 中运行的应用。平台即服务为您提供了轻松运行应用程序的所有工具。通过在后端运行应用程序的 PaaS，向组合中添加 Harness 可以使您的连续交付体验无缝化。

## PaaS 和 Harness——一起使用更好

在 Harness，我们支持 OpenShift 和 Pivotal Cloud Foundry 部署。潜在地，您的 PaaS 对如何构建/打包/推送可部署的具有某种持续集成的观点。如你所知，这只是你申请之旅的一部分。部署/连续交付场景是构建 Harness 平台的目的；成功和失败的所有信心建立步骤的编排和自动化，例如回滚。耦合线束与 PaaS 是天作之合。

像任何快速发展的技术一样，PaaS 前景一直面临挑战。随着 Kubernetes 迅速成为应用程序操作系统和公共云供应商争夺您的工作负载，传统 PaaS 的前景如何？

## 平台即服务实施中的困难

PaaS 供应商目前面临着很多竞争。选择不等于意见的信念有时是为了远离/选择不那么固执己见的平台。

随着 Kubernetes 和 Kubernetes 生态系统(也称为云原生应用的操作系统)的迅速崛起，为什么还要利用“臃肿的 PaaS”呢？

## 库伯内特是国王

PaaS 供应商和任何人一样都在拥抱 Kubernetes。[自 2016 年以来，PaaS 公司 Red Hat OpenShift](https://www.openshift.com/) 一直将 Kubernetes 列为管弦乐队。反对利用 PaaS 的现代观点是 Kubernetes 生态系统的爆炸。

PaaS 的早期争论是从[源到映像](https://github.com/openshift/source-to-image)【用 OpenShift 术语来说就是 s2i】过程。有了 source to image，开发人员就不必知道错综复杂的 [Docker Compose](https://docs.docker.com/compose/) 。虽然随着时间的推移，Docker/Kubernetes 生态系统已经变得更加成熟，拥有技能的人越来越多，例如[经过认证的 Kubernetes 管理员](https://www.cncf.io/certification/cka/) / [应用程序开发人员](https://www.cncf.io/certification/ckad/)【CKA/CKD】正在增长。

通过 CKA/CKD 认证，深入了解课程设置，它们与 [PaaS 供应商提供的认证](https://pivotal.io/training/certification/platform-operator-certification)非常相似。CKA/CKD 认证中的监控、记录和故障排除等非功能性要求代表了市场的成熟。

PaaS 供应商也将可移植性作为一个重要的差异化因素。如果您在本地运行 [Pivotal Cloud Foundry](https://pivotal.io/platform) ，您可以在 AWS/Azure/etc 上运行 PCF。另一方面，如果您的工作负载在 Kubernetes 上运行，那么一旦您有了一个一致的 Kubernetes 集群，您应该能够在另一个集群上运行您的工作负载。对于 PaaS vs PaaS 供应商来说，情况就不一样了。

## 互用性

从一个平台即服务到另一个平台即服务的工作流通常不会以 1:1 的方式运行。例如，您在 Pivotal Cloud Foundry 上的工作负载不会转移到 OpenShift，反之亦然。这是一个既定的事实，因为这两个平台是如此不同。两种平台即服务的应用程序目录项目存在重叠。

尽管在 PaaS 上运行的应用基础设施的配置[例如，扩展和部署规则]在不同的平台上会有所不同。更糟糕的是， [IBM + Red Hat 的收购](https://techcrunch.com/2019/09/12/together-at-last-ibm-brings-cloud-foundry-to-red-hat-openshift/)将 [Cloud Foundry](https://www.cloudfoundry.org/) 的最大贡献者之一【IBM】和 OpenShift 背后的供应商 Red Hat 带到了一起。两家公司将如何围绕 PaaS 合并战略还有待观察。无论您的组织选择什么，Harness 都会支持您。

## 不要害怕挽具

在 Harness，我们可以帮助您和您的组织最大限度地发挥技术决策和平台的价值。有了 PaaS，让您的应用进入自然环境的步骤编排变得越来越容易。向等式中加入一些 Harness 魔法，Harness 可以为您的 Kubernetes 和基于非 Kubernetes 的 PaaS 提供持续交付的能力。免费试用我们的[连续交付平台](https://app.harness.io/auth/#/signup/)。

干杯，

拉维