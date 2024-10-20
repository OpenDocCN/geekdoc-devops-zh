# CircleCI 安全警报:轮换存储在 CircleCI 的任何秘密(1 月 13 日更新)

> 原文：<https://circleci.com/blog/january-4-2023-security-alert/>

## 安全更新 01/13/2023 - 21:25 UTC

我们完整的事故报告
现已发布

[Read the Report](/blog/jan-4-2023-incident-report/)

## 安全更新 01/12/2023 - 00:30 UTC

我们已与 AWS 合作，帮助通知其 AWS 令牌可能在此次安全事件中受到影响的所有 CircleCI 客户。今天，AWS 开始通过电子邮件提醒客户潜在受影响令牌的列表。此电子邮件的主题行是[需要采取的措施] CircleCI 安全警报以循环访问密钥。

我们与 AWS 在这一额外沟通层面上合作的目标是帮助客户更轻松地识别、撤销或轮换任何可能受影响的密钥。如需帮助，请参见关于旋转访问键的 [AWS 文档或](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html#Using_RotateAccessKey)[联系亚马逊支持](https://aws.amazon.com/support)。

**您可能有的其他问题:**

*   ***如果我收到了电子邮件，这是否意味着有人未经授权访问了列出的 AWS 帐户？*** 此时，没有任何迹象表明您的 AWS 帐户被访问过，只有存储在 CircleCI 中的令牌有可能被泄露，因此应该从 AWS 中删除并轮换。

*   ***circle ci 1 月 4 日披露以来这里有什么新鲜事？还发生了什么事吗？*** 这是 CircleCI 在 2023 年 1 月 4 日所做的原始披露的一部分的额外警告。没有新的信息或新的发展出现。本说明旨在帮助客户识别和轮换 AWS 上的 AWS 令牌。

## 安全更新 01/10/2023 - 21:10 UTC

这是一个简短的更新，传达我们的事件报告的状态。我们预计在 1 月 17 日(太平洋标准时间)星期二向我们的客户提供事故报告。

我们对 CircleCI 平台的安全性有信心，客户可以继续构建。我们的支持工程、客户成功和安全团队将继续在袖手旁观协助您解决任何问题或顾虑。我们还将继续[在我们的论坛上与我们的客户和社区保持联系](https://discuss.circleci.com/t/circleci-security-alert-rotate-any-secrets-stored-in-circleci/46479)。谢谢你。

## 安全更新 01/07/2023 - 07:30 UTC

昨天，我们让客户知道我们正在代表客户轮换 GitHub OAuth 令牌。这个过程现在已经完成，所有的 GitHub OAuth 令牌都已经被轮换。

希望轮换自己的 OAuth 令牌的客户仍然可以按照下面概述的说明进行。

在这一点上，我们不期望有更多的实质性更新来分享，直到我们完成了我们正在进行的调查与我们的第三方法医团队。我们对 CircleCI 平台的安全性有信心，客户可以继续构建。

我们想继续表达我们对顾客的感激和关心。我们知道秘密和可变的轮换可能是费时费力的工作。感谢您对保护所有系统安全的支持和关心。我们在 CircleCI 的团队夜以继日地工作，尽可能多地采取缓解措施来支持我们的客户。我们的支持工程、客户成功和安全团队可以提供帮助。我们还将继续[在我们的论坛上与我们的客户和社区保持联系](https://discuss.circleci.com/t/circleci-security-alert-rotate-any-secrets-stored-in-circleci/46479)。谢谢你。

## 安全更新 01/06/2023 - 23:00 UTC

这是一个简短的更新，提供我们的 GitHub OAuth 令牌轮换的状态。截至 2023 年 1 月 6 日 23:00 UTC，我们完成了 99%的令牌轮换。我们预计将在未来几个小时内完成，并将在完成后提供额外的更新。

作为一个澄清点，对于处理旋转机密和密钥的客户，您应该在源(他们提供访问的系统)上旋转密钥，然后在 CircleCI 上存储新的机密。仅仅把它们从切尔莱西移走是不够的。

感谢您对缓解这一问题的支持和关心。我们的支持工程、客户成功和安全团队将继续在袖手旁观协助您解决任何问题或顾虑。我们还将继续[在我们的论坛上与我们的客户和社区保持联系](https://discuss.circleci.com/t/circleci-security-alert-rotate-any-secrets-stored-in-circleci/46479)。谢谢你。

## 安全更新 01/06/2023 - 17:52 UTC

我们的团队正在努力采取一切可能的行动来帮助客户缓解这一事件。

自上次更新以来，我们的团队代表客户解决了以下问题:

*   **个人和项目 API 令牌**:我们已经移除了 2023 年 1 月 5 日 00:00 UTC 之前创建的所有个人和项目 API 令牌。
*   **Bitbucket OAuth** :截至世界协调时 2023 年 1 月 6 日 10:00，我们在 Atlassian 的合作伙伴已终止 Bitbucket 用户的所有 OAuth 令牌。用户登录时将刷新位桶令牌，此处无需任何其他操作。Bitbucket 用户仍然需要替换 SSH 令牌。
*   **GitHub OAuth:** 截至世界协调时 1 月 7 日 07:30，所有 GitHub OAuth 令牌已代表 CircleCI 客户进行轮换。希望轮换自己的 GitHub OAuth 令牌的客户可以遵循以下说明。

**注意:**该安全事件与使用 [CircleCI 服务器](https://circleci.com/docs/server/overview/circleci-server-v4-overview/)的客户无关。

## 截至 1 月 10 日 2:00 UTC 的更新说明

1.  **OAuth 令牌**:
    1.  对于 GitHub:截至 1 月 7 日 07:30 UTC，所有 GitHub OAuth 令牌已经代表 CircleCI 客户进行了轮换。希望这样做的客户可以通过注销 CircleCI 应用程序，转到[https://github.com/settings/applications](https://github.com/settings/applications)，选择“授权的 OAuth 应用程序”，然后撤销 CircleCI 条目来轮换他们自己的 OAuth 令牌。完成后，重新登录 CircleCI 以触发重新授权。
    2.  对于 Bitbucket:截至 2023 年 1 月 6 日 10:00 UTC，我们在 Atlassian 的合作伙伴已终止 Bitbucket 用户的所有 OAuth 令牌。用户登录时将刷新位桶令牌，此处无需任何其他操作。Bitbucket 用户仍然需要替换 SSH 令牌。
    3.  对于 GitLab: GitLab 用户不需要重新授权他们的应用程序访问。作为预防措施，我们仍然建议 GitLab 用户轮换他们的环境变量、个人和项目 API 令牌以及所有 SSH 密钥。
2.  **项目 API 令牌**:要旋转它们，进入项目设置> API 权限>添加 API 令牌。**更新:CircleCI 已撤销 2023 年 1 月 5 日 00:00 UTC 之前创建的所有代币。**
3.  **项目环境变量**:进入项目设置>环境变量，然后创建一个同名的环境变量来替换现有的值。
4.  **上下文变量**:转到组织设置>上下文，为每个上下文执行与项目环境变量相同的操作。
5.  **注意:截止到 1 月 9 日 22:50 UTC，我们已经更新了上下文 API，以包含最后的“updated_at”日期和时间戳。这为确定秘密轮换是否成功完成提供了必要的信息。我们将推出额外的更改，以确保除 API 之外，UI 中也包含 updated_at 日期。[你可以在 API 文档上下文和环境变量](https://circleci.com/docs/api/v2/index.html#tag/Context)中了解更多信息。**
6.  **用户 API 令牌**:进入用户设置>个人 API 令牌，然后删除并重新创建您可能正在使用的任何令牌。**更新:CircleCI 已撤销 2023 年 1 月 5 日 00:00 UTC 之前创建的所有代币。**
7.  **项目 SSH 密钥**:
    1.  前往项目设置> SSH 密钥。
    2.  删除部署密钥并再次添加它。
    3.  如果您使用了任何额外的键，那么这些键需要被删除并重新创建。
    4.  **注意:SSH 密钥也需要从目标环境中轮换。**
8.  **Runner 令牌**:使用 CircleCI CLI，运行以下命令:
    1.  `circleci runner token list <resource-class name>`
    2.  `circleci runner token delete "<token identifier>"`
    3.  `circleci runner token create <resource-class-name> "<nickname>"`
    4.  按照这些命令，您需要将创建的令牌添加到您的 launch-agent-config.yml 中，并重新启动您的 runner 服务

**注意:**在 CircleCI 上还有一个[工具可以发现你所有的秘密，可以用来找到一个可操作的物品列表进行轮换。](https://github.com/CircleCI-Public/CircleCI-Env-Inspector)

2023 年 1 月 9 日更新:我们已经添加了使用[我们的`get-checkout-key` API V1.1 端点](https://circleci.com/docs/api/v1/index.html#get-checkout-key)返回结帐键的 SHA256 签名的功能。

请参见下面的 API 调用示例:

*   curl-H " Circle-Token:<circle-token>" https://Circle ci . com/API/v 1.1/project/:VCS-type/:username/:project/check out-key？digest=sha256</circle-token>
*   请注意这里的`sha256`查询参数

我们感谢您的适应性和保持系统安全的承诺。随着更多细节的出现，我们将继续在这里提供更新和信息。我们还将继续[在我们的论坛上与我们的客户和社区保持联系](https://discuss.circleci.com/t/circleci-security-alert-rotate-any-secrets-stored-in-circleci/46479)。谢谢你。

## 安全更新 2023 年 5 月 1 日

我们希望向客户介绍我们昨天披露的安全事件的最新情况，并进一步澄清客户的常见问题。我们理解新年伊始就中断您的工作是不理想的。我们感谢您的耐心和理解，因为我们共同努力，以保持所有系统的安全。

## CircleCI 客户可以构建

我们从客户那里收到的第一个问题是，“我能建造吗？”答案是肯定的。

我们相信我们已经消除了导致此次事故的风险。我们采取了以下措施来确保我们平台的完整性:

*   我们已经轮换了所有生产机器并循环使用了所有访问密钥。
*   我们已经完成了对所有系统访问的审计。
*   我们正积极与第三方调查人员和我们的合作伙伴合作，以验证我们调查的步骤和行动。

## 你现在应该继续做什么

我们还想为所有客户提供更多有关我们建议行动的详细信息。

请轮换 CircleCI 储存的所有秘密。有多种方法可以做到这一点，我们鼓励你和你的团队使用你喜欢的方法。这里有一个你可以遵循的方法:

1.  **OAuth 令牌**:
    1.  对于 GitHub，这意味着去[https://github.com/settings/applications](https://github.com/settings/applications)，选择“授权 OAuth 应用”，然后撤销 CircleCI 条目。完成后，注销并重新登录 CircleCI 以触发重新授权。
    2.  对于比特桶:[https://bitbucket.org/account/settings/app-authorizations/](https://bitbucket.org/account/settings/app-authorizations/)。
    3.  对于 GitLab: GitLab 用户不需要重新授权他们的应用程序访问。作为预防措施，我们仍然建议 GitLab 用户轮换他们的环境变量、个人和项目 API 令牌以及所有 SSH 密钥。
2.  **项目 API 令牌**:要旋转它们，进入项目设置> API 权限>添加 API 令牌。
3.  **项目环境变量**:进入项目设置>环境变量，然后创建一个同名的环境变量来替换现有的值。
4.  **上下文变量**:转到组织设置>上下文，为每个上下文执行与项目环境变量相同的操作。
5.  **用户 API 令牌**:进入用户设置>个人 API 令牌，然后删除并重新创建您可能正在使用的任何令牌。
6.  **项目 SSH 按键**:进入项目设置> SSH 按键。删除部署密钥并再次添加它。如果您使用了任何额外的键，那么这些键需要被删除并重新创建。
7.  **Runner 令牌**:使用 CircleCI CLI，运行以下命令:
    1.  `circleci runner token list <resource-class name>`
    2.  `circleci runner token delete <token identifier>`
    3.  `circleci runner token create <resource-class-name> "<nickname>"`
    4.  按照这些命令，您需要将创建的令牌添加到您的 launch-agent-config.yml 中，并重新启动您的 runner 服务

**注意:**我们建议对所有项目(转到“项目设置”)、组织(转到“组织设置”)和用户(转到“用户设置”)都这样做。

除了这些说明，今天，我们创造了一个[工具，用于在 CircleCI](https://github.com/CircleCI-Public/CircleCI-Env-Inspector) 上发现你的所有秘密。这将有助于您创建一个可操作的物品轮换列表。

## 其他安全建议

当客户正在轮换密钥、机密和变量时，为您的 CI/CD 管道配置添加额外的保护层可能会有所帮助。

所有客户都可以利用以下几点来提高管道安全性:

*   尽可能使用 [OIDC 令牌](https://circleci.com/docs/openid-connect-tokens/)以避免在 CircleCI 中存储长期有效的凭证。
*   利用 [IP 范围](https://circleci.com/docs/ip-ranges/)将到您系统的入站连接限制到已知的 IP 地址。
*   使用[上下文](https://circleci.com/docs/contexts/)实现跨项目的环境变量共享，然后可以通过 API 自动[旋转。](https://circleci.com/docs/contexts/#rotating-environment-variables)
*   对于特权访问和附加控制，您可以选择使用 [runners](https://circleci.com/docs/runner-overview/#circleci-runner-use-cases) ，它允许您将 CircleCI 平台连接到您自己的计算和环境，包括 IP 限制和 IAM 管理。

## 解决客户关注的问题

我们还想解决一些客户对事件处理方式的担忧。作为一家深度投资于迭代和改进的公司，我们始终欢迎并感谢客户对事件管理的反馈。

以下是客户分享的几个问题:

*   如何访问 CircleCI 的审计日志？ 我们已将自助审计日志的访问权限扩展至所有客户，包括免费客户。客户可以通过我们的 UI 访问[自助审计日志。客户最多可以查询 30 天的数据，并有 30 天的时间下载生成的日志。虽然我们理解访问 CircleCI 审计日志的要求，但我们对所有客户的建议是将您的审计和调查重点放在 CircleCI 中存储了机密的任何系统的日志上。](https://circleci.com/docs/audit-logs/#request-audit-logs-from-the-web-app)

*   ***为什么周三这么晚才发出这个？*** 我们了解到，1 月 4 日(星期三)太平洋时间下午 6:30/美国东部时间晚上 9:30 发布我们的轮换机密指南后，我们的许多北美客户经历了深夜和随叫随到的轮换。我们错在尽可能快地获取信息，以尽量减少任何潜在的暴露时间。我们也知道，作为一家几乎在每个国家都有客户的全球性公司，除了“尽快”之外，没有披露安全事件的好时机

*   ***此事件与您在博客上发布的[12 月 21 日可靠性更新](https://circleci.com/blog/an-update-on-circleci-reliability/)有关吗？*** 不是。作为我们客户沟通的一部分，我们会定期提供可靠性更新。作为此次事件的一部分，我们建议在 12 月 21 日至 1 月 4 日之间查看日志，而我们在 12 月 21 日更新可靠性的时间只是一个简单的巧合。

## 结论

我们要感谢我们的客户和社区对我们的支持，因为我们都在努力保持我们系统的安全。我们知道安全事故会给所有相关人员带来压力和负担，为此，我们深表歉意。随着更多细节的出现，我们将继续在这里提供更新和信息。我们还将继续[在我们的论坛上与我们的客户和社区保持联系](https://discuss.circleci.com/t/circleci-security-alert-rotate-any-secrets-stored-in-circleci/46479)。

## 2023 年 4 月 1 日安全警报

我们想让你知道，我们目前正在调查一个安全事件，我们的调查正在进行中。我们将向您提供有关此事件的最新消息，以及我们的回应。此时，我们确信在我们的系统中没有未授权的参与者；然而，出于谨慎，我们希望确保所有客户也采取某些预防措施来保护您的数据。

## 行动请求

出于谨慎，我们强烈建议所有客户采取以下措施:

*   立即轮换 CircleCI 中存储的所有秘密。这些可能存储在项目环境变量或上下文中。
*   我们还建议客户在 2022 年 12 月 21 日至 2023 年 1 月 4 日期间，或在您的机密轮换结束后，查看其系统的内部日志，查看是否有任何未经授权的访问。

此外，如果您的项目使用项目 API 令牌，我们已经使它们无效，您将需要替换它们。你可以在我们的文档中找到关于如何做的更多信息。

我们对您工作的任何中断表示歉意。我们非常重视我们系统和客户系统的安全性。虽然我们正在积极调查这一事件，但我们承诺在未来几天与客户分享更多细节。

**感谢您紧急关注旋转秘笈。**