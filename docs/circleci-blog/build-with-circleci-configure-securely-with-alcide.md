# 安全 Kubernetes -安全 CI/CD | CircleCI & Alcide

> 原文：<https://circleci.com/blog/build-with-circleci-configure-securely-with-alcide/>

DevOps 已经不是什么新概念了。许多公司已经将 DevOps 集成到他们的软件开发流程中，以改进和加速软件开发，并帮助推动他们的数字化转型。现在有完整的工具生态系统、方法和转换模型，以及无尽的资源，可用于指导公司走上 DevOps 之旅。

但是 DevOps 的成功有时很难衡量，因为它不是一个正式的框架；它更多的是一种文化和一套做法。你能得到的指导是有限的，以确保你做得正确，或者你能准确地衡量你的成功和失败。DevOps 在每个组织中看起来也不同，导致没有两个 DevOps 管道是相同的。

DevOps 最突出的目标之一是确保无摩擦、尽可能自动化的 [CI/CD](https://circleci.com/continuous-integration/) 管道。让我们从安全的角度来看看这意味着什么。

## 持续 Kubernetes 部署:Alcide 方式

确保安全性符合性的最简单的方法是在开发阶段向左移动并解决安全性问题。通常，安全性是在生产阶段应用的，这意味着它不是环境端到端流程的一部分。在开发级别保护应用程序和网络将使您更有信心，您的应用程序将在生产级别正确地互操作。

左移后，确保以安全的方式持续部署和监控集群、节点和单元。理想情况下，您应该有一个[工具](https://www.alcide.io/kubernetes-advisor)，它通过查看工作负载安全和治理检查、集群工作节点检查、集群入口控制器和[等等](https://www.alcide.io/secured-ci-cd-pipeline)来提供集群配置和安全状态的实时摘要，但是该工具最重要的特性是它会使未能通过策略检查的资源上的管道失败。

在最近我们在阿尔西德进行的一项分析中，我们查看了 5000 多份跟踪扫描，发现 DevOps 团队在遵循 Kubernetes 的最佳实践(如秘密处理和网络策略)时面临着巨大的挑战和差距。具体来说，我们发现 89%的部署扫描显示，公司没有使用 Kubernetes 的秘密资源，秘密写在公开。我们还发现，超过 75%的扫描部署使用装载高漏洞主机文件系统(如`/proc`)的工作负载，而没有一个被调查的环境显示使用 Kubernetes 网络策略的分段实施。

埃尔希德的 Kubernetes 顾问做的一切，甚至更多。旨在快速安全地增加 K8s 集群，使用 DevSecOps 策略进行开发的团队可以享受自动化的、无需人工干预的 Kubernetes 体验，并让 Alcide 来完成所有繁重的工作。

## 底线

DevSecOps 要么成功地提高您的团队的速度、敏捷性和安全性，要么您的组织将遭受损失。正确创建您的管道，并确保不要让错误配置转变为安全风险。必须通过在每个阶段集成安全性来保护整个应用程序管道。通过 CircleCI 和 Alcide 的本机集成，这使得整个过程变得容易得多。现在，您可以通过添加 [Alcide Advisor orb](https://circleci.com/developer/orbs/orb/alcideio/alcide-advisor) ，只需几行配置就可以将 Alcide 与 CircleCI 一起使用。[orb](https://circleci.com/orbs/)是 CircleCI config 的可重用、可共享的开源包，支持服务的即时集成。完整的 CircleCI 球体列表见[球体注册表](https://circleci.com/developer/orbs)。

* * *

这篇文章是我们制作的关于 DevSecOps 的系列文章的一部分。要阅读本系列的更多文章，请单击下面的链接之一。