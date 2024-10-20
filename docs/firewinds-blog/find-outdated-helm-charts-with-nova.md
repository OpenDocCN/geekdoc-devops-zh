# K8s 教程:用 Nova 查找过时的舵图

> 原文：<https://www.fairwinds.com/blog/find-outdated-helm-charts-with-nova>

 确保您在 Kubernetes 集群中安装了最新版本的软件，这有助于您避免已知的安全问题，并获得最新的项目功能。如果您使用 Helm 来管理您的版本， [Fairwinds Nova](https://nova.docs.fairwinds.com/#join-the-fairwinds-open-source-community) 可以帮助您找到在您的集群中运行的过时或废弃的 Helm 图表。您可以从命令行运行 Nova，或者将其集成到您的持续集成过程中，并且从 3.1.0 版本开始，您可以 [扫描容器](https://www.fairwinds.com/blog/open-source-project-nova-not-just-for-helm-users-anymorec) 。

## 如何安装

使用 asdf，Homebrew，从源代码或 GitHub 版本安装 Nova。完整的安装说明在 [Nova 文档](https://nova.docs.fairwinds.com/installation/#installation) 中。在本文中，我们演示了如何从 GitHub 发布页面安装 Nova。

首先，访问 [版本页面](https://github.com/FairwindsOps/nova/releases) ，找到适合您环境的版本。例如，在采用 amd64 处理器的 Linux 机器上，您将需要下载适用于 Linux amd64 的版本。

运行以下命令下载并安装 Nova:

```
curl -L "https://github.com/FairwindsOps/nova/releases/download/3.2.0/nova_3.2.0_linux_amd64.tar.gz" > nova.tar.gz
tar -xvf nova.tar.gz
sudo mv nova /usr/local/bin/
```

接下来，通过运行 help 命令检查安装是否正常工作:

```
nova help
```

您应该会看到 Nova 命令和选项的列表，如下所示:

```
fairwinds tool to check for updated chart releases.

Usage:
  nova [flags]
  nova [command]

Available Commands:
  completion      Generate the autocompletion script for the specified shell
  find            Find out-of-date deployed releases.
  generate-config Generate a config file.
  help            Help about any command
  version         Prints the current version.

Flags:
      --alsologtostderr                   log to standard error as well as files (default true)
      --config string                     Config file to use. If empty, flags will be used instead
      --context string                    A context to use in the kubeconfig.
  -d, --desired-versions stringToString   A map of chart=override_version to override the helm repository when checking. (default [])
  -h, --help                              help for nova
  -a, --include-all                       Show all charts even if no latest version is found.
      --logtostderr                       log to standard error instead of files (default true)
      --output-file string                Path on local filesystem to write file output to
      --poll-artifacthub                  When true, polls artifacthub to match against helm releases in the cluster. If false, you must provide a url list via --url/-u. Default is true. (default true)
      --show-old                          Only show charts that are not on the latest version
  -u, --url strings                       URL for a helm chart repo
  -v, --v Level                           number for the log level verbosity
      --wide                              Output chart name and namespace

Use "nova [command] --help" for more information about a command.
```

## 使用 Nova 审计舵图版本

运行 Nova 最快的方法是从命令行。要扫描您当前通过身份验证的 Kubernetes 集群中安装的 Helm 图表，请运行命令:

```
nova find
```

您将看到您的 Helm 版本列表、集群中安装的 Helm 图表版本、最新可用版本以及您的版本是否过期。

```
Release Name             Installed    Latest    Old      Deprecated
============             =========    ======    ===      ==========
kube-prometheus-stack    26.1.0       37.3.0    true     false
metrics-server           3.8.0        3.8.2     true     false
vpa                      1.4.0        1.4.0     false    false
```

有关这些 Helm 版本的更多信息，包括它们在哪个命名空间中运行，请调用以下命令:

```
nova find –wide
```

```
Release Name             Chart Name               Namespace    HelmVersion    Installed    Latest    Old      Deprecated
============             ==========               =========    ===========    =========    ======    ===      ==========
kube-prometheus-stack    kube-prometheus-stack    demo         3              26.1.0       37.3.0    true     false
metrics-server           metrics-server           demo         3              3.8.0        3.8.2     true     false
vpa                      vpa                      demo         3              1.4.0        1.4.0     false    false
```

要更新您的舵版本，请复制最新版本的海图，并运行“舵升级”命令。在我们的示例中，metrics-server 图表版本已经过时，因此要更新它，我们将运行:

```
helm upgrade metrics-server metrics-server/metrics-server --version 3.8.2
```

如果图表被否决，您将看到以下内容:

```
Release Name    Installed    Latest    Old      Deprecated
============    =========    ======    ===      ==========
kubewatch       3.3.4        3.3.4     false    true
```

不推荐使用的图表应该得到特别的关注，因为这意味着项目不再被维护，也不会有其他的更新。弃用的软件部署的时间越长，它就变得越脆弱，越容易受到攻击。被弃用的流行项目通常会更改名称或 Helm repos URLs，被弃用图表的文档会为您指出新项目，或提供您应该考虑的替代方案。

注意:在撰写本文时，Nova 在不推荐使用的图表和 artifacthub.io 方面存在一个有趣的问题。为了生成上面显示的输出，我们必须禁用 artifacthub.io 扫描，并直接在 kubewatch 的图表存储库中指向 Nova。用于执行此操作的命令是:

```
nova find --poll-artifacthub=false --url=https://charts.bitnami.com/bitnami
```

artifacthub.io 的问题在于，我们不知道已安装的 Helm 版本来自哪个实际的源代码库。这是目前正在积极讨论中的对 [头盔改进提案【HIP】](https://github.com/helm/community/pull/224)中头盔的修改。此外，我们正在追踪 Github 中的问题 [。](https://github.com/FairwindsOps/nova/issues/134)

## 使用 Nova 审计容器版本

最近，Nova 增加了一个新的容器扫描功能。通过运行带有`- containers '标志的“nova find”命令，可以看到这一点:

```
nova find --containers
```

```
Container Name                                            Current Version         Old     Latest     Latest Minor            Latest Patch
==============                                            ===============         ===     ======     =============           =============
quay.io/prometheus/alertmanager                           v0.23.0                 true    v0.24.0    v0.23.0                 v0.23.0
quay.io/prometheus-operator/prometheus-config-reloader    v0.53.1                 true    v0.58.0    v0.53.1                 v0.53.1
quay.io/kiwigrid/k8s-sidecar                              1.14.2                  true    1.19.2     1.19.2                  1.14.3
grafana/grafana                                           8.3.3                   true    9.0.4      8.5.9                   8.3.10
k8s.gcr.io/kube-state-metrics/kube-state-metrics          v2.3.0                  true    v2.5.0     v2.5.0                  v2.3.0
quay.io/prometheus-operator/prometheus-operator           v0.53.1                 true    v0.58.0    v0.53.1                 v0.53.1
docker.io/bitnami/kubewatch                               0.1.0-debian-10-r571    true    0.1.0      0.1.0-debian-10-r571    0.1.0
k8s.gcr.io/metrics-server/metrics-server                  v0.6.0                  true    v0.6.1     v0.6.0                  v0.6.1
us.gcr.io/k8s-artifacts-prod/ingress-nginx/controller     v0.34.1                 true    v1.3.0     v0.34.1                 v0.34.1
quay.io/prometheus/prometheus                             v2.32.1                 true    v2.37.0    v2.37.0                 v2.32.1
k8s.gcr.io/autoscaling/vpa-recommender                    0.10.0                  true    0.11.0     0.10.0                  0.10.0
k8s.gcr.io/autoscaling/vpa-updater                        0.10.0                  true    0.11.0     0.10.0                  0.10.0
docker.io/datawire/quote                                  0.4.1                   true    0.5.0      0.4.1                   0.4.1
k8s.gcr.io/coredns/coredns                                v1.8.6                  true    v1.9.3     v1.9.3                  v1.8.6
k8s.gcr.io/etcd                                           3.5.3-0                 true    3.5.4-0    3.5.3-0                 3.5.3-0
k8s.gcr.io/kube-apiserver                                 v1.24.1                 true    v1.24.3    v1.24.3                 v1.24.3
k8s.gcr.io/kube-controller-manager                        v1.24.1                 true    v1.24.3    v1.24.3                 v1.24.3
k8s.gcr.io/kube-proxy                                     v1.24.1                 true    v1.24.3    v1.24.3                 v1.24.3
k8s.gcr.io/kube-scheduler                                 v1.24.1                 true    v1.24.3    v1.24.3                 v1.24.3
```

## 定制 Nova

虽然拥有最新的舵图和容器版本是理想的，但是工程团队选择固定特定版本有很多原因。也许团队还没有准备好升级，因为最新版本引入了突破性的变化。Nova 允许您声明自己的版本集，而不是使用最新的可用版本进行比较，从而适应了这一点。您可以通过命令行或文件设置所需的版本。

例如，在我们上面的 Nova 扫描中，我们发现我们目前运行的是 kube-prometheus-stack Helm chart 的 26.1.0 版本，尽管最新版本是 37.3.0。在这种情况下，我们的工程团队需要使用早期版本。

要将所需版本设置为 26.1.0，请运行命令:

```
nova find --desired-versions='kube-prometheus-stack=26.1.0'
```

如果您希望在文件中设置所需的版本，请使用以下命令生成配置文件:

```
nova generate-config --config=nova.yaml
```

在 Nova 配置文件中，将 kube-prometheus-stack=26.1.0 添加到所需版本列表中，如下所示:

```
containers: false
context: ""
desired-versions: { 
  kube-prometheus-stack: 26.1.0
}
include-all: false
output-file: ""
poll-artifacthub: true
show-errored-containers: false
show-non-semver: false
show-old: false
url: []
wide: false
```

## 大规模应用 Nova 的优势

如果你有多个集群，并希望大规模应用 Nova 的优势，Fairwinds 提供了一个名为 [Insights](https://www.fairwinds.com/insights) 的平台。用户可以跨集群一致地集中管理 Nova，以确保您的 Helm 发布版本和容器是最新的。Nova 也足够灵活，可以集成到持续集成系统中。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！ [在此了解更多](https://www.fairwinds.com/coming-soon) 。

## 资源

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)