# 如何 Kubernetes:使用 Conftest 审计基础设施即代码

> 原文：<https://www.fairwinds.com/blog/how-to-kubernetes-use-conftest-to-audit-infrastructure-as-code>

 在  Fairwinds，我们在我们的开源产品  [Polaris](https://github.com/fairwindsops/polaris "Fairwinds Polaris") 以及我们的 SaaS 产品[fair winds Insights](/insights "Fairwinds Insights")中利用 OPA ( [开放策略代理](https://www.openpolicyagent.org/ "Open Policy Agent"))。随着我们的开发团队实现这些功能，我开始思考我们可以利用 Fairwinds 的 OPA 来增强我们 Kubernetes 环境的可靠性和安全性的其他方法。

与此同时，我试图想出一种方法来更好地审计我们的许多基础设施即代码库。对于我们的每个客户，都有一个回购协议，我们为该客户所做的一切都会进入回购协议。可以想象，跨客户的标准会逐渐过时，我们需要知道他们什么时候会过时。目前我们有一个基于 Python 的内部解决方案来做这件事，但是它变得有点笨拙和过时了。计划是重写它，但 EJ(我们的前首席技术官)有一个更好的主意:“为什么我们不使用 OPA？”我的回应当然是 facepalm。我怎么没想到呢？因此，我四处查看了 OPA 资料库，重温了我对  `rego,` 的一点点知识，并整理了一份概念证明。我需要一种运行和测试策略的方法，所以我更新了我的版本[conftest](https://github.com/open-policy-agent/conftest "conftest")，事实证明，它被设计来做我需要做的事情:审计配置文件。

## 撰写政策

下一步，写保单，是最难的部分。作为一个经常使用 Python 和 Go 等语言进行开发的人，这比我想象的要困难得多。  `Rego` 不是一种编程语言，而是一种查询语言，这意味着我需要思考这一切的方式与我通常的思考方式不太一样。一旦你进入了它的最佳状态，它就会变得非常强大，但这花了我相当长的时间。我决定从检查我们正在使用的 Terraform 模块的版本开始，并将其与批准的模块列表进行比较。在野外有很好的例子，所以我能够找到这些例子并开始行动。我能写的最基本的策略是这样的:

```
package terraform

import data

violation[{"msg": msg}] {
    m := input.module[name]
    source := m.source
    not source == "git::https://github.com/FairwindsOps/terraform-vpc.git?ref=v5.0.1"
    msg = sprintf("%s module version %s is out of date", [name, source])
}
```

第一行，  `package terraform,` 只是允许我将策略分成多个部分。最终，我要用一堆不同的模块来结束，比如  `terraform,` `kops,` 和 `kubernetes.` 当使用此策略与 `conftest,` 时，我们将使用`--namespace=terraform`标志来定位 terraform 模块。策略的下一部分读入传递给它的 terraform 数据中的所有模块，并将每个模块与一个静态字符串进行比较。如果有匹配，则返回  `msg` 作为违规。

这个政策是有作用的，但是并没有完全达到我们想要的效果。我们需要添加针对不同字符串检查不同模块名称的功能。这需要一个映射变量，我们可以用它来指定多个模块名，我们称它为  `module_allowlist.` ，这给了我们以下内容:

```
package terraform

import data

module_allowlist = {
    "vpc": "git::https://github.com/FairwindsOps/terraform-vpc.git?ref=v5.0.1",
    "bastion": "git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.6.0",
}

violation[{"msg": msg}] {
    m := input.module[name]
    source = m.source
    not source == module_allowlist[name]
    msg = sprintf("%s module version %s is out of date", [name, source])
} 
```

这里我们将实际模块源与  `module_allowlist` 图中的对应源进行比较。这样更好，但是我们有另一个问题:有多个允许版本的模块怎么办？上面的`bastion`模块就是一个很好的例子；我们期待不同版本的`bastion`取决于你使用的云提供商。让我们将地图格式更改为  `module_name: [list of possible versions].` 新的策略看起来是这样的:

```
package terraform

import data

module_allowlist = {
    "vpc": ["git::https://github.com/FairwindsOps/terraform-vpc.git?ref=v5.0.1"],
    "bastion": [
        "git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.6.0",
        "git::https://github.com/fairwindsops/terraform-bastion.git//gcp?ref=gcp-v0.1.1",
    ],
}

violation[{
    "msg": msg,
}] {
    m := input.module[name]
    source = m.source
    not contains(module_allowlist[name], source)
    msg = sprintf("%s module version %s is out of date", [name, source])
}

contains(sources, elem) {
    source := sources[_]
    source == elem
} 
```

现在我们有了一个可能版本的列表，我们引入一个函数  `contains` 来检查我们的源模块是否在该模块允许的源列表中。这意味着通过用更多的信息填充映射，我们可以检查所有种类的模块版本，而不仅仅是一个。

我们最不希望这个策略在将结果输出为 JSON 时返回更多的数据。我们的目标是能够存储、呈现或分析这些数据，因此它有助于添加更多的细节。幸运的是，机制已经存在，我们只需要利用它。

这个文件的最终产品，叫做  `terraform.reg:`

```
package terraform

import data

module_allowlist = {
        "vpc": ["git::https://github.com/FairwindsOps/terraform-vpc.git?ref=v5.0.1"],
        "bastion": [
                "git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.6.0",
                "git::https://github.com/fairwindsops/terraform-bastion.git//gcp?ref=gcp-v0.1.1",
        ],
}

violation[{
        "msg": msg,
        "family": "terraform",
        "check": "module version",
        "module": name,
}] {
        m := input.module[name]
        source = m.source
        not contains(module_allowlist[name], source)
        msg = sprintf("%s module version %s is out of date", [name, source])
}

contains(sources, elem) {
        source := sources[_]
        source == elem
} 
```

在违例的定义中，我们增加了几条数据:

*   `"msg"`——变量 `msg`中我要返回的消息。这已经到位了

*   `"family"` -表示数据来源的字符串，本例中为  `terraform`

*   `"check"`——我希望能够说出我在检查什么。在这里，是  `"module version"`

*   `"module"` -这将通过变量  `name`设置为触发违规的模块的名称

最终结果是我们现在可以运行:

```
conftest test <path-to-terraform-file(s)> --policy terraform.rego -ojson --namespace  terraform
```

得到这样的输出:

```
[
  {
    "filename": "bastion.tf",
    "namespace": "terraform",
    "successes": 0,
    "failures": [
      {
        "msg": "bastion module version git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.5.0 is out of date",
        "metadata": {
          "check": "module version",
          "family": "terraform",
          "module": "bastion"
        }
      }
    ] }
] 
```

或者，如果您喜欢在 CLI 上查看结果，请放下  `-o json` 标志:

```
FAIL - bastion.tf - terraform - bastion module version git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.5.0 is out of date 
```

## 添加测试

`rego` 还有另一个我想利用的非常棒的功能:  单元测试。这对于制定政策以及长期维护政策来说是非常强大的。为了测试这个功能，我为上一节中的`terraform.rego`策略编写了一个测试。下面是我新的`terraform_test.rego`文件:

```
package terraform

empty(value) {
    count(value) == 0
}

no_violations {
    empty(violation)
}

test_module_source {
    violation[{"check": "module version", "family": "terraform", "module": "bastion", "msg": "bastion module version git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.0.0 is out of date"}] with input as {"module": {"bastion": {"source": "git::https://github.com/fairwindsops/terraform-bastion.git//aws?ref=aws-v0.0.0"}}}
} 
```

本质上，如果我传入一个名为  `bastion` 的模块，而这个模块的版本不在我的列表中，我认为会有一个违例。该测试可以使用命令  `conftest verify.` 运行

## 结论

现在，我有能力编写一整套审计策略，针对我们所有的基础设施代码运行它，并生成结构化的 JSON 数据，这将允许我分析结果并报告发现。更好的是，由于[fair winds Insights](https://www.fairwinds.com/insights "Fairwinds Insights")支持 OPA，我可以插入这些策略，让 Insights 为我收集和管理这些数据。这可能会成为我们工作流程中不可或缺的一部分，使我们的服务代表能够更好地管理许多客户，并在此过程中收集有用的信息。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)