# Git SSH 如何工作| Git 初学者教程

> 原文：<https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works>

#### ******初学饭桶教程******

[https://www.youtube.com/embed/z7jVOenqFYk?feature=oembed](https://www.youtube.com/embed/z7jVOenqFYk?feature=oembed)

视频

GitKraken 使认证 Git SSH 的过程更加顺利，因此您的整个团队可以安全地为您的项目做贡献。

Git SSH 或 secure shell 是一种网络协议，用于安全加密通过互联网从计算机推送到服务器的任何数据。

观看这个 Git 初学者教程视频，了解 Git SSH 如何安全地登录到远程机器、安全地传输文件以及安全地发出远程 Git 命令。此外，看看 Git SSH 密钥背后的科学以及它们如何加密数据。

您还将在 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 中看到 Git SSH 如何工作的示例，因为 GitKraken 作为 SSH 代理，通过代表您与 SSH 服务器进行通信来提供额外的安全性和便利性。

## **Git SSH 在 GitKraken 是如何工作的？**

您的机器可以使用 SSH 代理与 SSH 服务器进行协商，作为额外的安全层，而不是使用本地机器作为 SSH 客户机。

GitKraken 虽然不是可以被其他应用程序使用的传统 SSH 代理，但它可以代表任何 GitKraken 用户与 SSH 服务器进行协商。

## 如何设置 Git SSH？

要在 GitKraken 中设置 Git SSH，请导航到 preferences，然后单击 Authentication 来访问您的 Git SSH 设置。

如果您已经有了一对 Git SSH 密钥，那么您可以在这里设置您的公钥和私钥的路径。否则，您可以要求 GitKraken 为您生成一个新的密钥对。

如果您正在使用任何可用的 GitKraken 远程存储库托管集成，如 [GitHub](https://www.gitkraken.com/integrations/github) 您可以提供现有 GitHub SSH 密钥对的路径，或者让 GitKraken 为您生成 Git SSH 密钥对。

GitKraken 将使用这些 Git SSH 密钥与 SSH 服务器通信。本地存储库和远程存储库之间的所有操作都将通过 SSH 连接安全地传输。

借助传奇的跨平台 GitKraken Git GUI(可免费下载，并连续 5 年被评为 Git 客户端第一名)简化 Git SSH 流程。