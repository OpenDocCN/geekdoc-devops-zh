# 线束产品更新| 2020 年 1 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-january-2020>

我们在成长，而且发展得很快——我们已经正式拥有超过 200 名员工！你知道还有什么在生长吗？关键线束特性和功能。你知道谁会在这种增长中最终胜出吗？你当然知道。让我们来看看我们在 1 月份发布的产品。

## Azure DevOps -工件源

如果你正在 Azure 上部署并使用他们的集成 DevOps 工具套件，你会很高兴知道我们现在支持 Azure 工件。作为 Azure DevOps 工具包的一部分，Azure Artifacts 是团队公开或私下共享打包应用程序的一种很好的方式。

当使用 Harness 进行部署时，您只需使用您的 Azure DevOps URL 和个人访问令牌将 Azure 工件作为工件源进行连接([阅读更多信息](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/add-azure-dev-ops-artifact-servers)):

然后，将 Azure 工件作为工件源添加到您的服务概述配置中([阅读更多信息](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/add-azure-dev-ops-artifact-servers#step-1-select-azure-devops-artifact-server)):

## 验证徽章

在任何部署过程中，持续验证都是至关重要的一步。如果您的管道包括任何种类的验证，我们现在可以更容易地查看管道中验证成功或失败的摘要。

当查看*连续部署*选项卡时，如果该管道中包含验证，现在会出现一个验证徽章。如果您点击它，您可以立即查看您所有部署阶段的测试状态:

当您访问管道本身时，您可以查看覆盖在其工作流中包含验证的阶段上的验证标记，以获得另一个快速摘要:

您可以[在此](https://developer.harness.io/docs/first-gen/firstgen-fa-qs/continuous-verification-faqs/)阅读更多关于验证徽章的信息。

## 谷歌云 KMS 支持

如果你正在使用谷歌云 KMS 来管理你的加密密钥，Harness 爱你！尽管 Harness 有一个内置的机密管理特性，但是您仍然可以选择使用自己的特性。我们支持 HashiCorp Vault、AWS、Azure Key Vault、CyberArk，现在还支持谷歌云 KMS！

按照这些说明来设置。

## 给我们你的马具技巧和窍门！

任何对如何使用安全带有独特建议或技巧的顾客都将获得一张价值 100 美元的礼品卡。(免责声明:每人限用一个，必须是一个独特的使用案例，不在我们的任何材料中销售，并适用于所有线束，而不是一家公司的独特使用案例)。将您的故事发送至电子邮件 marketing@harness.io。