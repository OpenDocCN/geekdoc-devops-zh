# 升级证书管理器-这是值得的！

> 原文：<https://www.fairwinds.com/blog/upgrade-cert-manager-its-worth-it>

 cert-manager 项目通过 Kubernetes 中的 TLS 证书管理自动提供和更新。它支持使用您自己的证书颁发机构、自签名证书、由 Hashicorp Vault PKI 管理的证书，当然还有由 Let's Encrypt 发布的免费证书。如果你想在 0.5 版本之前使用 Helm 升级 cert-manager，而不需要重新创建或重新发布证书，请继续阅读。

如果您还没有使用证书管理器来管理证书，让我们在 Kubernetes 用 Ingresses 加密证书，请看[入门](https://docs.cert-manager.io/en/latest/getting-started/index.html)和 [ACME 发卡行教程](https://docs.cert-manager.io/en/latest/tutorials/acme/index.html)证书管理器文档。

### **升级证书管理器的原因**

以前版本的 cert-manager 可能会淹没 Let's Encrypt API。这在 cert-manager 0.6.0 中有了很大的改进:

*   Kubernetes 为证书订单和验证挑战提供了新的定制资源，通过 Let's Encrypt 简化了流程，并为 Kubernetes 中的调试提供了更多详细信息。您可以通过变更建议阅读[更多关于新订单流程的信息。下面的升级演练包含旧证书对象与新订单和质询对象中可用信息的示例。](https://github.com/jetstack/cert-manager/blob/master/design/acme-orders-challenges-crd.md)
*   一个新的验证 webhook 在提交给 Kubernetes API 时检查新证书资源的错误配置，然后再提交给 Let's Encrypt。这样可以更快地捕捉错误，而不会浪费对 Let's Encrypt API 的调用。
*   与 Let's Encrypt 通信的 ACME 客户端有 Prometheus 指标。在达到加密速率限制之前，您可以使用 ACME APIs 来检测证书管理器的使用情况，以检测问题并了解行为。

如 cert-manager 0.6.0 发行说明中所述:

> 经过广泛的测试，我们发现在最极端的情况下，ACME API 客户端调用减少了 100 倍。这是一个巨大的差异，有助于减少 cert-manager 实例对 Let's Encrypt 等服务的负载。因此，我们强烈建议所有用户尽快升级到 v0.6 版本！

### **头盔升级移除证书对象**

在 Fairwinds，我们使用[稳定舵图表](https://github.com/helm/charts/tree/master/stable/cert-manager)来部署 cert-manager。如果您还没有使用 Helm，它可以帮助您管理多个 Kubernetes 对象的期望状态。当通过[计算器](https://github.com/reactiveops/reckoner)使用时，Helm 更加强大，它使用 YAML 语法一次安装多个 Helm 版本，并允许从 git 存储库中安装图表。

cert-manager Helm chart 在版本 0.5.0 中停止管理自定义资源定义(CRD ),现在应该在安装版本 0.5 以上的 chart 之前手动安装 CRD。这意味着一个典型的`helm upgrade ...`cert-manager 将会从 Kubernetes 中删除你的证书对象。由 cert-manager 填充的机密不会被删除，但是这些机密永远不会被更新，因为没有 cert-manager 证书可以续订。

我原以为重新安装 cert-manager 后，由带注释的入口对象创建的证书最终会被重新创建，但我无法在不停机的情况下做到这一点。

如果你不得不重新创建和重新颁发你的证书，让我们加密有一个每周每个注册域 50 个证书的速率限制。

### **执行升级**

我将介绍 cert-manager 从 0.4.1 到 0.6.2 的升级，演示新的顺序和挑战资源。我的 cert-manager 0.4.1 安装在 kube-system 名称空间中，但是我将把 cert-manager 0.6 安装在它自己的 cert-manager 名称空间中。使用一个专用的名称空间是一个好主意，这个决定是由 cert-manager 在使用新的 cert-manager webhook 时禁用其名称空间验证的要求进一步推动的。

### **查看现有证书**

我有一个“神奇应用”的有效证书，另一个证书未能通过验证。

cert-manager 0.4.1 的证书对象的基本列表不显示证书是否有效:

```
$ kubectl get certificate --all-namespaces
NAMESPACE NAME AGE
default wonder-app-cert 9m
test ivan-test-cert 9m 
```

在测试名称空间中描述 ivan-test 证书，显示其验证失败，因为我没有为 Let's Encrypt ClusterIssuer 正确配置 DNS 验证(在状态中注明。输出底部的消息字段):

```
$ kubectl describe certificate -n test ivan-test
Name: ivan-test-cert
Namespace: test
Labels: <none>
Annotations: <none>
API Version: certmanager.k8s.io/v1alpha1
Kind: Certificate
Metadata:
 Creation Timestamp: 2019-02-28T05:53:09Z
 Generation: 1
 Owner References:
 API Version: extensions/v1beta1
 Block Owner Deletion: true
 Controller: true
 Kind: Ingress
 Name: test-ingress
 UID: 202bc781-3b1d-11e9-9fb0-02096e167e86
 Resource Version: 17919
 Self Link: /apis/certmanager.k8s.io/v1alpha1/namespaces/test/
certificates/ivan-test-cert
 UID: 203105f3-3b1d-11e9-9fb0-02096e167e86
Spec:
 Acme:
 Config:
 Dns 01:
 Provider: clouddns
 Domains:
 ivan-test.example.com
 Common Name: 
 Dns Names:
 ivan-test.example.com
 Issuer Ref:
 Kind: ClusterIssuer
 Name: letsencrypt-prod
 Secret Name: ivan-test-cert
Status:
 Acme:
 Order:
 URL: https://acme-v02.api.letsencrypt.org/acme/order/xxxxxxxx/
yyyyyyyyy
 Conditions:
 Last Transition Time: 2019-02-28T05:53:12Z
 Message: ACME server does not allow selected challenge type or no 
provider is configured for domain "ivan-test.example.com"
 Reason: InvalidConfig
 Status: False
 Type: Ready
Events:
 Type Reason Age From Message
 ---- ------ ---- ---- -------
 Normal CreateOrder 10m cert-manager Created new ACME order, 
attempting validation... 
```

### 为证书管理器创建新的命名空间

如果使用 Kubernetes 1.12 或更早版本，新的 cert-manager web 挂钩要求在其名称空间上禁用验证，以避免有关 ValidatingWebhookConfiguration 资源的 CABundle 字段的错误。cert-manager 将安装在自己的命名空间中，而不是禁用与其他事物共享的命名空间上的验证。如果没有完成这一步，cert-manager 将无法正确地为其新的 webhook 提供证书，从而导致鸡生蛋的局面。有关需要禁用验证的 webhook 的更多详细信息，请参见本期[CABundle Kubernetes](https://github.com/kubernetes/kubernetes/issues/69590)。

```
$ kubectl create namespace cert-manager

$ kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true 
```

### 将机密复制到新的名称空间

cert-manager 为其发行者使用的任何 Kubernetes 秘密都需要存在于新的 cert-manager 名称空间中。这个**不**涉及到持有入侵者使用的 SSL 证书的秘密！

您可能希望复制到新命名空间的机密包括:

*   ClusterIssuer 对象的 issuer.acme.dns01.providers 字段下配置的 DNS 提供程序引用的任何机密。这些秘密提供了对云服务帐户的访问，该帐户用于注册临时 DNS 条目以验证 Let's Encrypt 证书。DNS 可用于验证加密服务器无法访问的私有入口的证书。
*   (可选)由 ClusterIssuer 对象中的 issuer . ACME . privatekeysecretref 引用的 ACME 帐户私钥机密。对于 Let's Encrypt ClusterIssuers，cert-manager 将创建一个新的私钥，并将其存储在一个新的 Secret 中(如果尚不存在的话),到目前为止，这还没有影响到 Let's Encrypt 的现有证书。我不能代表其他发行者使用的秘密——您可能应该将它们复制到新的名称空间。

对于本例，我将从名为 letsencrypt-prod 的 ClusterIssuer 复制私钥秘密名称。首先从集群发布者处获取机密的名称:

`$ kubectl get clusterissuer letsencrypt-prod -o jsonpath='{.spec.acme.privateKeySecretRef.name}'`

这将返回秘密名称 cert-manager-setup-production-private-key。我的秘密是在名称中包含“cert-manager-setup ”,因为我们使用舵图来配置我们的集群发布者。

在`jq`命令的帮助下，将这个秘密复制到 cert-manager 名称空间。

`$ kubectl get secret -o json -n kube-system cert-manager-setup-production-private-key`

`| jq 'del(.metadata.namespace)' | kubectl create -f - -n cert-manager`

稍后将删除 kube-system 名称空间中与证书管理器相关的机密。

记住还要复制集群发行者可能用于 DNS 验证的任何机密！

### 备份相关对象

在使用 Helm 升级的过程中删除自定义资源定义(CRD)时，Issuer、ClusterIssuer 和 Certificate 对象将会消失。安装新版本的 cert-manager 及其附带的 CRD 后，将还原此备份。

```
`$ kubectl get -o yaml \

  --all-namespaces \

  issuer,clusterissuer,certificates > cert-manager-backup.yaml` 
```

请注意，由 Kubernetes Ingresses 使用的 cert-manager 填充的机密不会被触及，也不需要备份。

### 删除旧的证书管理器

此步骤还将删除 CRD 及其对象。不会触及由 Kubernetes Ingresses 使用的 cert-manager 填充的机密。安格斯将继续响应 HTTPS。

`$ helm delete --purge cert-manager`

现在列出证书会返回此错误，因为 CRD 及其对象已经不存在了

```
$ kubectl get certificates
Error from server (NotFound): Unable to list "certmanager.k8s.io/v1alpha1, 
Resource=certificates": the server could not find the requested resource (get 
certificates.certmanager.k8s.io) 
```

### 安装新的自定义资源定义

自定义资源定义(CRD)不再在 Helm chart 中管理，应该在该图表的 0.5+版本之前安装。以下命令适用于 0.6.X 版的证书管理器。

```
$ kubectl apply \
 -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.6/deploy/
manifests/00-crds.yaml
customresourcedefinition.apiextensions.k8s.io/certificates.certmanager.k8s.io created
customresourcedefinition.apiextensions.k8s.io/issuers.certmanager.k8s.io created
customresourcedefinition.apiextensions.k8s.io/clusterissuers.certmanager.k8s.io 
created
customresourcedefinition.apiextensions.k8s.io/orders.certmanager.k8s.io created
customresourcedefinition.apiextensions.k8s.io/challenges.certmanager.k8s.io created 
```

但是，被重新安装的 CRDs 不会恢复证书对象，但是它们将在以后的步骤中从备份中恢复。

```
$ kubectl get certificates --all-namespaces
No resources found. 
```

### 安装证书管理器

使用 Helm 或类似 Reckoner 的辅助工具安装 0.6 版的 cert-manager。这使用 0.6.6 版的 Helm chart，它安装 0.6.2 版的 cert-manager。

#### 使用舵安装

```
$ helm install stable/cert-manager -n cert-manager --namespace cert-manager 
--version 0.6.6
..... Helm output omitted ..... 
```

#### 使用计算器安装

下面是安装 cert-manager 的[计算器](https://github.com/reactiveops/reckoner)的 YAML 代码片段，包括安装 CRDs 和禁用新安装的名称空间验证的预安装挂钩(上面已经完成了):

```
# Use this course.yml file with reckoner to deploy cert-manager,
# For example: reckoner plot course.yml --heading cert-manager
#
# Replace this context with your own.
context: your_actual_kubernetes_context # Get with kubectl config current-context
repositories:
 incubator:
 url: https://kubernetes-charts-incubator.storage.googleapis.com
 stable:
 url: https://kubernetes-charts.storage.googleapis.com
minimum_versions:
 helm: 2.12.3
 reckoner: 1.0.1
charts:
 cert-manager:
 version: "v0.6.6"
 namespace: cert-manager
 set-values:
 # You can place chart values here. . .
 resources:
 requests.cpu: 10m
 requests.memory: 50Mi
 limits.cpu: 15m
 limits.memory: 100Mi
 hooks:
 pre_install:
 - kubectl get namespace cert-manager >/dev/null 2>&1 || kubectl create 
namespace cert-manager
 # The next line is only required with Kubernetes 1.12 or earlier.
 - kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=
true --overwrite=true
 - kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/
release-0.6/deploy/manifests/00-crds.yaml 
```

### 还原对象备份

恢复 Certificate、Issuer 和 ClusterIssuer 对象的备份——这些对象已被 Helm 与以前版本的 cert-manager 发行版一起删除。

```
$ kubectl apply -f cert-manager-backup.yaml
clusterissuer.certmanager.k8s.io/letsencrypt-prod created
clusterissuer.certmanager.k8s.io/letsencrypt-staging created
certificate.certmanager.k8s.io/wonder-app-cert created
certificate.certmanager.k8s.io/ivan-test-cert created 
```

### 还原对象时的潜在错误

如果您收到类似下面的错误，cert-manager web hook 尚未准备好，可能是因为它仍在创建自己的证书，或者是因为上面没有安装 CRDs。如果舵图在 CRDs 完全应用之前安装得太快，网络挂钩可能无法创建其证书。要解决这个问题，请删除 cert-manager-webhook-xxxxxx pod，这样它将重新启动并实现它的证书。再次恢复备份，应该不会出现错误。

更多背景信息见[安装升级文档](https://cert-manager.io/docs/installation/upgrading/)。

```
Error from server (InternalError): Internal error occurred: failed calling admission 
webhook "clusterissuers.admission.certmanager.k8s.io": the server is currently unable 
to handle the request 
```

```
Error from server (InternalError): Internal error occurred: failed calling admission
webhook "certificates.admission.certmanager.k8s.io": the server is currently unable
to handle the request 
```

### 检查证书管理器对象并验证快乐

证书对象现在有一个名为“ready”的字段，对于 wonder-app 证书为真，对于 ivan-test 证书为假，反映了它们在 cert-manager 升级之前的状态。

查看颁发者和证书，我们看到 cert-manager webhook 使用的新证书，以及以前的 ClusterIssuer 和 Certificate 对象:

```
$ kubectl get clusterissuer,issuer,certificates --all-namespaces
NAME AGE
clusterissuer.certmanager.k8s.io/letsencrypt-prod 3m
clusterissuer.certmanager.k8s.io/letsencrypt-staging 3m

NAMESPACE NAME AGE
cert-manager issuer.certmanager.k8s.io/cert-manager-webhook-ca 13m
cert-manager issuer.certmanager.k8s.io/cert-manager-webhook-selfsign 13m

NAMESPACE NAME 
cert-manager certificate.certmanager.k8s.io/cert-manager-webhook-ca 
cert-manager certificate.certmanager.k8s.io/cert-manager-webhook-webhook-tls 
default certificate.certmanager.k8s.io/wonder-app-cert 
test certificate.certmanager.k8s.io/ivan-test-cert 
READY SECRET AGE
True cert-manager-webhook-ca 13m
True cert-manager-webhook-webhook-tls 13m
True wonder-app-cert 3m
False ivan-test-cert 3m 
```

### 查看证书过期时间

描述一个证书现在会在详细信息的底部显示到期时间——下面是一个片段:

```
Status:
 Conditions:
 Last Transition Time: 2019-02-28T06:54:46Z
 Message: Certificate is up to date and has not expired
 Reason: Ready
 Status: True
 Type: Ready
 Not After: 2019-05-29T04:52:06Z
Events: <none> 
```

#### 证书验证失败

在 cert-manager 升级之后，失败的 ivan-test 证书没有**而不是**提供关于 cert-manager 对象中验证失败的足够信息。我必须查看 cert-manager pod 的日志，以发现我没有配置 DNS 验证，这导致无法尝试验证:

```
$ kubectl describe certificate -n test ivan-test-cert 
Name: ivan-test-cert
Namespace: test
Labels: <none>
Annotations: <none>
API Version: certmanager.k8s.io/v1alpha1
Kind: Certificate
Metadata:
 Creation Timestamp: 2019-02-28T07:20:43Z
 Generation: 1
 Owner References:
 API Version: extensions/v1beta1
 Block Owner Deletion: true
 Controller: true
 Kind: Ingress
 Name: test-ingress
 UID: 4ea85e58-3b29-11e9-9fb0-02096e167e86
 Resource Version: 26973
 Self Link: /apis/certmanager.k8s.io/v1alpha1/namespaces/test/
certificates/ivan-test-cert
 UID: 5c06841a-3b29-11e9-9fb0-02096e167e86
Spec:
 Acme:
 Config:
 Dns 01:
 Provider: clouddns
 Domains:
 ivan-test.example.com
 Dns Names:
 ivan-test.example.com
 Issuer Ref:
 Kind: ClusterIssuer
 Name: letsencrypt-prod
 Secret Name: ivan-test-cert
Status:
 Conditions:
 Last Transition Time: 2019-02-28T07:20:43Z
 Message: Certificate does not exist
 Reason: NotFound
 Status: False
 Type: Ready
Events:
 Type Reason Age From Message
 ---- ------ ---- ---- -------
 Normal OrderCreated 115s cert-manager Created Order resource 
"ivan-test-cert-4093320200" 
```

我希望 Order 对象说明一下为什么没有进行验证，但是唯一的细节是它是待定的:

```
$ kubectl describe order -n test ivan-test-cert-4093320200 
Name: ivan-test-cert-4093320200
Namespace: test
Labels: acme.cert-manager.io/certificate-name=ivan-test-cert
Annotations: <none>
API Version: certmanager.k8s.io/v1alpha1
Kind: Order
Metadata:
 Creation Timestamp: 2019-02-28T07:20:43Z
 Generation: 1
 Owner References:
 API Version: certmanager.k8s.io/v1alpha1
 Block Owner Deletion: true
 Controller: true
 Kind: Certificate
 Name: ivan-test-cert
 UID: 5c06841a-3b29-11e9-9fb0-02096e167e86
 Resource Version: 26975
 Self Link: /apis/certmanager.k8s.io/v1alpha1/namespaces/test/orders/
ivan-test-cert-4093320200
 UID: 5c084dcc-3b29-11e9-9fb0-02096e167e86
Spec:
 Config:
 Dns 01:
 Provider: clouddns
 Domains:
 ivan-test.example.com
 Csr: actual CSR omitted
 Dns Names:
 ivan-test.example.com
 Issuer Ref:
 Kind: ClusterIssuer
 Name: letsencrypt-prod
Status:
 Certificate: <nil>
 Finalize URL: https://acme-v02.api.letsencrypt.org/acme/finalize/xxxxxxxx/
yyyyyyyyy
 Reason: 
 State: pending
 URL: https://acme-v02.api.letsencrypt.org/acme/order/xxxxxxxx/yyyyyyyyy
Events: <none> 
```

此证书没有质询对象，因为没有配置 DNS 提供程序，所以没有尝试验证。

```
$ kubectl get challenge -n test
No resources found. 
```

在这种情况下，我没有看到失败的原因(DNS 验证配置不正确)反映在上述任何对象中，正如我在以前版本的 cert-manager 中所做的那样。不过，cert-manager pod 日志确实反映了这一点:

```
cert-manager-6874795dc8-b76kk cert-manager E0228 07:10:22.018889 
1 controller.go:185] orders controller: Re-queuing item 
"test/ivan-test-cert-4093320200" due to error processing: Error constructing 
Challenge resource for Authorization: ACME server does not allow selected 
challenge type or no provider is configured for domain "ivan-test.example.com" 
```

### 清理

删除先前从 kube 系统复制到 cert-manager 名称空间的机密。对于这个例子:

T2`$ kubectl delete secret -n kube-system cert-manager-setup-production-private-key`

从 kube-system 名称空间中删除您可能已经复制到新名称空间的任何其他机密，例如用于 DNS 证书验证的机密。

## 享受证书管理器

享受您的 cert-manager 的升级安装，在它自己的名称空间，以及未来的升级可能会更少的麻烦。

感谢您的阅读！如果你喜欢这篇文章，或者有什么建议，请随时在 Twitter 上联系@IvanFetch。

[![Download the Open Source Guide](img/34ff13f28810d7f376985d433a5db9ed.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/08b823c4-e86d-4c16-b458-823d80c9c090)