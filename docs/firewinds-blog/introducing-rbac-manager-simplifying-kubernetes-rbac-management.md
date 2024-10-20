# 介绍 RBAC 经理:简化 Kubernetes RBAC 管理

> 原文：<https://www.fairwinds.com/blog/introducing-rbac-manager-simplifying-kubernetes-rbac-management>

 在 [Fairwinds](/) ，我们用 Kubernetes 帮助一些大公司。与这么多不同的组织合作让我们对大规模维护 Kubernetes 所涉及的一些挑战有了独特的看法。

我们看到组织与 Kubernetes 斗争的最常见领域之一是管理 RBAC。对于许多组织来说，这是如此的困难和令人困惑，以至于 RBAC 经常甚至没有实现，或者只实现了一半。对于设法正确锁定 RBAC 配置的组织来说，他们经常发现配置很难维护。

> *缺乏适当的 RBAC 配置通常会导致太多的人过多地访问太多的 Kubernetes 集群。*

在亲眼目睹了 RBAC 配置在规模上的挑战性之后，我们构建了一个开源的 Kubernetes 操作器来提供帮助。今天我们介绍的是  [RBAC 经理](https://github.com/reactiveops/rbac-manager)。我们希望这个项目能够帮助那些正在努力管理他们当前的 RBAC 配置的组织，并且鼓励更多的组织在他们的 Kubernetes 实现中完全采用 RBAC。

## RBAC 管理当前面临的挑战

为了充分理解我们对 RBAC 管理器的目标，有必要花一些时间来理解 RBAC 配置目前如何与 Kubernetes 一起工作。让我们从一个非常简单的单用户例子开始。我们称这个用户为“A”，他们需要  `edit` 访问集群中的  `web` 和  `api`名称空间。为此，我们将创建 2 个新的角色绑定:

```
 kind: RoleBinding 
apiVersion: rbac.authorization.k8s.io/v1 
metadata: 
    name: example 
    namespace: web 
subjects: 
  - kind: User 
    name: A 
    apiGroup: rbac.authorization.k8s.io 
roleRef: 
    kind: ClusterRole 
    name: edit 
    apiGroup: rbac.authorization.k8s.io 
--- 
kind: RoleBinding 
apiVersion: rbac.authorization.k8s.io/v1 
metadata: 
    name: example 
    namespace: api 
subjects: 
  - kind: 
    User name: A 
    apiGroup: rbac.authorization.k8s.io 
roleRef: 
    kind: ClusterRole 
    name: edit 
    apiGroup: rbac.authorization.k8s.io
```

当然，这并不特别复杂，但是对于单个用户来说，这是一个非常简单的配置。当我们引入更多的用户、名称空间或集群时，这变得更加复杂。

随着规模的扩大，不可避免地需要所有的 YAML 配置，很快就很难对其进行跟踪。获取用户列表并查看他们每个人在集群中的访问级别是一个令人望而生畏的过程。

此外，如果我们想要在其中一个名称空间中更改该用户的访问级别，这可能不像您希望的那么简单。没有办法把一个  `roleRef` 换成一个  `RoleBinding`。该方法包括删除一个  `RoleBinding` 并代之以用更新的  `roleRef`创建一个新的。

在越来越多地由自动化驱动的工作流中，作为代码的基础设施通常被视为黄金标准，这并不十分合适。这还不包括在任何规模上管理删除角色绑定会有多困难。你不能只是从回购协议中删除一个  `RoleBinding` ，然后指望某种自动化任务来为你管理变更。

现在当然可以修改 Kubernetes 角色和集群角色。为了获得最细粒度的结果，可以为每个角色绑定创建唯一的角色和集群角色，然后修改角色，而不是角色绑定。尽管在某些情况下这无疑是一个很好的解决方案，但是在任何规模上管理这些角色都会变得非常复杂。我们的目标是简化 RBAC 管理，因此，我们希望利用 Kubernetes 为我们的工作流提供的  [优秀默认角色](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#default-roles-and-role-bindings) 。根据我的经验，在 RBAC 用户管理方面，这些默认的集群角色(查看、编辑、管理和集群管理)涵盖了大多数组织的绝大多数用例。

## 尝试用 RBAC 管理器简化这一过程

我们对 RBAC 管理器的目标一直是让 RBAC 配置更容易使用。我们想要一种方法，它允许更简单的配置以及更直接的自动化途径。

## 更简单的配置

RBAC 管理器是一个 Kubernetes 操作员，监视新的或更新的  `RBACDefinition` 定制资源。为了创建上述场景的两个角色绑定，  `RBACDefinition` 需要大约一半的 YAML 配置:

```
 apiVersion: rbacmanager.reactiveops.io/v1beta1 
kind: RBACDefinition 
metadata: 
    name: rbac-manager-config 
rbacBindings:
  - name: user-a-example 
    subjects: 
      - kind: User 
        name: A 
    roleBindings: 
      - clusterRole: edit 
        namespace: web 
      - clusterRole: edit 
        namespace: api
```

随着更多用户、名称空间或集群的加入，配置的减少只会变得更加显著。当然，RBAC 还有比这个例子更多的东西。RBAC 管理器支持在命名空间或集群级别将用户、组或服务帐户绑定到角色或集群角色的任意组合。

根据我们的经验，这种用单个定制资源分配多个角色绑定的能力非常强大。您不再需要寻找跨多个名称空间的角色绑定来理解您的 RBAC 配置的当前状态，取而代之的是它被整齐地总结在一个单一的 RBAC 定义中。

## 自动化之路

RBAC 管理器的设计理念是，我们希望在 Kubernetes 中实现 RBAC 管理的自动化。手动进行这些更改很容易出错，而且根本无法很好地扩展。不幸的是，这里现有的 Kubernetes 资源不能像部署那样自动更新。

考虑到这一点，RBAC 定义的功能更像是部署，而不是角色绑定。就像部署简化了更新 pod 一样，RBAC 定义旨在简化更新角色绑定和集群角色绑定。对 RBAC 定义的更改会触发其拥有的资源的更改，在本例中是角色绑定和集群角色绑定。如果 RBAC 定义中不再引用角色绑定，RBAC 管理器会将其删除。同样，如果 RBAC 定义中请求的角色发生更改，RBAC 管理器将删除并重新创建适当的角色绑定。最后，如果删除了 RBAC 定义，所有关联的角色绑定和集群角色绑定也将自动删除。

这种新方法打开了自动化的大门，并允许您使用 CI 工作流管理 RBAC 配置。与部署如何简化 CI 工作流中的 pod 更新类似，RBAC 定义可以简化角色绑定和集群角色绑定的更新。

在过去的几个月里，我们发现 RBAC 管理器对我们在 [Fairwinds](/) 的工作流程特别有用，我们希望您能像我们一样发现它的帮助。

这是一个完全开源的项目，我们很乐意接受你的任何想法或贡献。欲了解更多技术信息，或安装它，请访问我们的  [GitHub](https://github.com/reactiveops/rbac-manager) 。请继续关注一些后续帖子，内容涉及我们如何使用 RBAC 管理器来管理 AWS 和 GKE 集群上的 RBAC 配置。