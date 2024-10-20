# 使用 container runner | CircleCI 在 Kubernetes 中运行自托管 CI 作业

> 原文：<https://circleci.com/blog/deploy-runner-on-kubernetes/>

容器运行器，一个新的容器友好的自托管运行器，现在对所有 CircleCI 用户可用。对于有独特计算或安全需求的客户来说，自托管运行程序是一种受欢迎的解决方案。Container runner 降低了在容器化环境中使用自托管 runner 的门槛，并使中央 DevOps 团队能够更轻松地管理防火墙后的容器化 CI/CD 作业。

## 管理集装箱化的工作

CircleCI 的 Docker executor 允许您在持续集成工作流中充分利用容器化的[速度和灵活性优势。在容器中运行作业是一种快速便捷的方式，可以确保您的构建可以访问所有必需的库和依赖项，而无需启动完整的机器环境所需的额外启动时间。](https://circleci.com/blog/benefits-of-containerization/)

容器运行器消除了用户在他们的`config.yml`文件中使用 [Docker 语法](https://circleci.com/docs/using-docker)。这意味着用户可以自由使用他们自己的自定义图像或 [CircleCI 便利图像](https://circleci.com/developer/images?imageType=docker)，并可以在作业执行期间轻松执行[次级容器](https://circleci.com/docs/using-docker#using-multiple-docker-images)。

## 自动扩展基础设施

对于大多数开发团队来说，基础架构需求整天都在波动，维持固定水平的计算资源以适应峰值工作负载会很快导致云成本失控。为了应对这一挑战，拥有一个能够根据需求动态扩展和缩减基础架构的解决方案是很有帮助的。

Container runner 与 Kubernetes 集成，实现快速、轻松的容器编排。这意味着用户只需管理一个运行器代理，容器运行器将根据需要启动临时 pod 来执行并发 CI 作业。无论您需要同时运行 5 个作业还是 100 个作业，只需一个 container runner 实例就可以确保您拥有足够的基础设施。

## 在您的 Kubernetes 集群中安装容器运行器

安装您的第一个容器运行器只需要几个步骤，并且可以通过 CircleCI UI 轻松完成。只需在 CircleCI web 应用程序中创建一个 [CircleCI 名称空间](https://circleci.com/docs/runner-concepts/#namespaces-and-resource-classes)(如果您的组织还没有)和 [runner 资源类](https://circleci.com/docs/runner-concepts/#namespaces-and-resource-classes)，使用 Helm 在您的 Kubernetes 集群中安装容器运行器，并向您的 CircleCI `config.yml`文件添加一个新作业，该作业使用 Docker 执行器和容器运行器的资源类。

下面是一个为`build`作业使用容器运行器的配置示例:

```
version: 2.1

jobs:
  build:
    docker:
      - image: cimg/base:2021.11
    resource_class: <namespace>/<resource-class>
    steps:
      - checkout
      - run: echo "Hi I'm on Runners!"

workflows:
  build-workflow:
    jobs:
      - build 
```

有关安装容器运行器和从 CircleCI 触发运行器作业的更多信息，请访问[容器运行器安装文档](https://circleci.com/docs/container-runner-installation/)。

## 结论

在防火墙后的容器化环境中运行 CI/CD 作业是许多软件团队日常开发工作的重要部分。在 CircleCI，我们相信支持团队是非常重要的，无论他们在哪里开发软件。无论您是在云中构建还是在自己的基础设施中构建，CircleCI 都能满足您的需求。

如果您有关于如何改善跑步者体验的想法，请访问 Canny 浏览或提交新想法。请访问我们的社区论坛[讨论](https://discuss.circleci.com/t/self-hosted-runners-on-every-plan-draft/43846/)，让我们知道您是如何使用 runner 的。

最初的自托管运行程序 machine runner 主要用于虚拟和物理环境。集装箱转轮是机器转轮的补充，而不是替代。如需了解机器转轮的信息，请访问[文档](https://circleci.com/docs/runner-overview/)。