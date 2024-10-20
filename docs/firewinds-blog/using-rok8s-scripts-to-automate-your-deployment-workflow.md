# 使用 rok8s 脚本自动化您的部署工作流程

> 原文：<https://www.fairwinds.com/blog/using-rok8s-scripts-to-automate-your-deployment-workflow>

[rok8s 脚本](https://github.com/reactiveops/rok8s-scripts) 是 Kubernetes 脚本，通过持续集成环境帮助管理应用程序到 Kubernetes 的部署。rok8s-scripts 是由  [罗斯·库库林斯基](https://twitter.com/rosskukulinski)创建的原 k8s 知识库的 Fairwinds 分支。

在这里，我将提供更多关于 rok8s 脚本是什么、它们做什么以及为什么你应该考虑使用它们的上下文。

## rok8s 脚本概述

Kubernetes 部署涉及大量文件。例如，您必须安装 Kubernetes 命令行实用程序，将该实用程序授权给您正在使用的集群，并添加决定您的部署配置的所有文件。

自动化部署遵循类似的模式，这些模式已经被集成到高效的脚本中，从而有助于部署到 Kubernetes 中。开源 rok8s 脚本使得管理应用程序开发和部署生命周期以及从持续集成环境中部署到 Kubernetes 变得容易。(rok8s 脚本与 CircleCI 和 Jenkins 一起工作。Docker 1.10 或更高版本目前不支持缓存，但在不久的将来会支持。)

## rok8s 脚本做什么

rok8s 脚本可用于验证部署、验证数据库迁移、处理机密和组织 YAML 文件。

***部署验证***
使用 Kubernetes，您可以描述您想要部署的映像，并为 Kubernetes 如何将该部署部署到 Kubernetes 集群中设置参数。因为 Kubernetes 没有内置的方法来通知您代码部署是否失败，所以 rok8s 脚本可以用来验证部署。

***数据库迁移***
当您部署代码时，代码和数据库模式需要对齐。rok8s 脚本包括一个包装 Kubernetes 作业(一次性可执行代码集)的包装器，称为阻塞作业。阻塞作业会将作业部署到集群中，并提供验证。如果迁移成功，您可以推出部署。如果失败，部署将在您的代码被放入 Kubernetes 之前停止，并给您解决问题的机会。该过程允许自动检入、检查和应用迁移代码。

***Kubernetes Secrets***
应用程序需要运行大量敏感信息，比如数据库的密码和其他服务的 API 密匙。当这些信息与您的源代码一起签入时，您实际上是在让其他人轻松访问利用您的系统所需的敏感凭据。rok8s 脚本包括 Kubernetes 秘密的包装器，允许在亚马逊 S3 中存储秘密，并对文件设置适当的权限。当您在 CircleCI 或 Jenkins 中构建部署时，您从 S3 下载秘密，将它们放入 Kubernetes 所需的文件格式，然后将它们推入 Kubernetes。这样，秘密的改变可以被同步，而不需要在源代码中保留秘密。

***YAML 文件***
单个应用程序部署涉及多个 YAML 文件的情况并不少见，因此目标之一就是在存储库中组织这些文件。rok8s 脚本可以帮助管理部署(如何将代码放入 Kubernetes)、服务和入口(如何部署 Kubernetes 中的应用程序)以及 ConfigMap(如何将配置推入 Kubernetes，以便部署在不同环境中保持不变)。

## 为什么应该考虑使用 rok8s 脚本

设置 Kubernetes 很简单，但是管理 Kubernetes 可能很复杂。(关于概述，请查看[kops 101-Kubernetes 部署游戏规则改变者](http://blog.reactiveops.com/kops-the-kubernetes-deployment-game-changer)。)

通过自动化标准工作流和简化常见任务，rok8s 脚本为部署到 Kubernetes 中的应用程序提供了一个固执的指导方针，确保所有部署遵循相同的模式并快速跟踪常见功能。虽然 rok8s 脚本不会取代 Kubernetes 和 kops 中的专业知识，但它们将大大减少快速有效地部署到 Kubernetes 所需遵循的步骤。