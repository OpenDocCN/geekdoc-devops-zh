# 日产源代码泄露可以避免吗？

> 原文：<https://www.gitkraken.com/blog/nissan-source-code-leak>

你认为你今天过得很糟糕……你看到日产公司的开发人员发生了什么吗？

日产的移动应用和内部工具的源代码(Git repos)被泄露到互联网上，因为该链接可以公开访问，并且密码很容易猜到。😬呀。

## **位桶上的托管代码**

Bitbucket 服务器通常配置为脱机使用。理想情况下，您应该配置您的 Bitbucket 服务器，以便只有一组目标终端用户可以浏览到该实例，并通过 HTTPS 或 SSH 验证到他们的 Git 存储库。

***了解 GitKraken Git GUI 如何通过 HTTPS 或 SSH 提供到远程 Git 存储库的安全连接。[了解更多关于 GitKraken 认证的信息。](https://support.gitkraken.com/integrations/authentication/#ssh)***

## **日产代码泄露是怎么发生的？**

日产报告称，他们正在对源代码泄露进行调查，希望他们能发现如何避免这样一个简单的错误。当他们挖掘的时候，我们其他人可以享受这一集的两个简单收获:

## **Git 存储库的安全提示**

本质上，Git 存在一些安全漏洞，因为它是由服务器访问控制的，开发人员可以重写历史。然而，建立协作过程和利用工具，如 Git 客户机，帮助您安全地连接到远程数据，将帮助您轻松地避免这些简单的错误。

### **了解你的 Git Repo 的权限设置**

不要让你的自托管 Bitbucket 实例可公开访问(除非这是你的意图)。

### **设置密码**

不要使用默认密码。

这应该适用于目前你在网上设置的任何密码，尤其是专业账户。

考虑使用一种工具，如 [LastPass](https://www.lastpass.com/) ，它可以设置安全密码并管理您的帐户登录。您甚至可以共享协作团队帐户的密码。

## **配合 GitKraken 使用 Bitbucket】**

GitKraken 可以通过个人访问令牌使日产团队的身份验证变得简单而安全。

当设置 [GitKraken 与 Bitbucket 服务器](https://support.gitkraken.com/integrations/bitbucket-server/)的集成时，我们提示用户确认 URL 并提供个人访问令牌。

GitKraken 将引导您使用您的 Bitbucket 服务器凭证登录，以创建访问令牌，包括您可以分配给令牌的权限。这是您可以增强 Git repo 安全性的另一个领域。

我们还让用户能够为 Bitbucket 服务器生成 [SSH 密钥。](https://support.gitkraken.com/integrations/bitbucket-server/)

## **使用 GitKraken 保护您的 Git Repos】**

GitKraken Git GUI 为安全地连接到您的远程 Git 存储库提供了多个选项，并提供了权限设置以满足不断增长的开发团队的需求。