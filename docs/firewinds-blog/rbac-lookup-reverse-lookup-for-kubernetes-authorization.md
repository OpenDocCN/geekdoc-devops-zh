# RBAC-Lookup:Kubernetes 授权的反向查找

> 原文：<https://www.fairwinds.com/blog/rbac-lookup-reverse-lookup-for-kubernetes-authorization>

 如果您曾经使用过 Kubernetes authorization，那么您可能想知道一个非常简单的问题的答案。"这个用户对这个群集有多少访问权限？"不幸的是，这个问题总是很难回答。所有相关的 Kubernetes APIs 都允许您列出角色绑定和集群角色绑定，但是从来没有像什么角色绑定到用户这样简单的事情。

考虑到这一点，我们构建了一个简单的 Go CLI， [rbac-lookup](https://github.com/reactiveops/rbac-lookup) ，来帮助回答这个问题。要开始，您可以直接从 GitHub 下载最新版本，或者用 Homebrew 安装它:

```
brew install reactiveops/tap/rbac-lookup
```

在那里，您可以使用 rbac-lookup 来轻松查看谁可以访问哪些角色。这里有一个简单的例子:

```
rbac-lookup rob

SUBJECT                   SCOPE             ROLE
rob@example.com           cluster-wide      ClusterRole/view
rob@example.com           nginx-ingress     ClusterRole/edit
```

这表明“rob@example.com”除了在 nginx-ingress 名称空间中具有编辑权限之外，还具有集群范围的查看权限。为了获得这个结果，rbac-lookup 遍历集群中的所有 RoleBindings 和 ClusterRoleBindings，并返回主题(用户、服务帐户或组)名称与查询匹配的任何结果。

作为一个更完整的示例，您可以使用“wide”输出标志运行更广泛的查询:

```
rbac-lookup ro -owide

SUBJECT                   SCOPE             ROLE                SOURCE
User/rob@example.com      cluster-wide      ClusterRole/view    ClusterRoleBinding/rob-cluster-view
User/rob@example.com      nginx-ingress     ClusterRole/edit    RoleBinding/rob-edit
User/ross@example.com     cluster-wide      ClusterRole/admin   ClusterRoleBinding/ross-admin
User/ron@example.com      web               ClusterRole/edit    RoleBinding/ron-edit
ServiceAccount/rops       infra             ClusterRole/admin   RoleBinding/rops-admin
```

在这种情况下，我们看到有许多用户甚至一个服务帐户与“ro”查询相匹配。这个广泛的输出为我们提供了额外的信息，比如主题的类型和授予访问权限的特定源(RoleBinding 或 ClusterRoleBinding)。

希望这个工具对你和我们一样有用。你可以在 [GitHub](https://github.com/reactiveops/rbac-lookup) 上找到这个项目。如果你有任何问题，请直接在 Twitter 或 Kubernetes Slack (@robertjscott)上联系我。

如果你已经走了这么远，你可能真的很喜欢库伯内特和 RBAC。如果是这样，你可能想看看我们的相关项目， [rbac-manager](https://github.com/reactiveops/rbac-manager) ，一个旨在简化 rbac 管理的操作器。

[![Download the Open Source Guide](img/34ff13f28810d7f376985d433a5db9ed.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/08b823c4-e86d-4c16-b458-823d80c9c090)