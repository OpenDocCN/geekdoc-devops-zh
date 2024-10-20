# GitHub 添加 SSH 密钥|如何给 GitHub 添加 SSH 密钥？

> 原文：<https://www.gitkraken.com/learn/git/problems/github-add-ssh-key>

如果你还不熟悉， [SSH](https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works) 或 secure shell 是一种网络协议，它允许你安全地加密你通过互联网从本地计算机推送到服务器的任何数据。

GitKraken 使得生成新的 SSH 密钥并将它们添加到 GitHub 的过程变得简单、安全和直观。

## **GitHub 如何添加 SSH 密钥**

为什么要给 GitHub 添加 SSH 密钥？每次从 GitHub 存储库中推送和提取更改时，不需要使用用户名和密码，而是可以使用 SSH 密钥。

在 Git 中使用 SSH 密钥很方便，因为它们避免了记住复杂密码的需要，但是您可以使用非常长的安全密码来保护您的帐户。SSH 密钥的工作方式就像只有您拥有的实际物理密钥一样。

## **GitHub 在 GitKraken 中添加 SSH 密钥**

如果您正在使用 GitKraken 的健壮的 [GitHub 集成](https://www.gitkraken.com/integrations/github)，您将能够从 GitHub 提供到您现有的 SSH 密钥对的路径，或者要求 GitKraken 为您生成一个新的 SSH 密钥对。

要开始向 GitHub 添加 SSH 密钥，您需要首先连接 GitKraken 的 GitHub 集成。您可以通过点击齿轮⚙️图标访问您的`Preferences` → `Integrations` → `GitHub`来完成此操作。接下来，点击`Connect to GitHub`。

这将打开您的默认浏览器到 GitHub.com，在那里您将完成 GitHub 集成过程。

在 GitKraken 中成功连接 GitHub 集成后，就可以在 GitKraken 中点击`Generate SSH key and add to GitHub`。几秒钟后，GitKraken 将自动生成一个新的 SSH 密钥，并将其添加到您的 GitHub.com 帐户中！

如果在 GitKraken 中没有使用 GitHub 集成会怎样？永远不要害怕；您仍然可以在 GitKraken 中使用 SSH 连接到 GitHub。为此，您将再次点击齿轮⚙️图标来访问您的`Preferences` → `SSH`。从这里，你可以浏览到你已经有的一个现有的 GitHub SSH 密钥，或者你可以点击`Generate`让 GitKraken 给 GitHub 添加一个 SSH 密钥。

为 GitHub 创建 SSH 密钥后，您需要手动将其添加到您的 GitHub.com 帐户中。要添加 SSH 密钥，您需要在浏览器上导航到 GitHub.com 并登录。接下来，点击你右上角的头像，进入`Settings` → `SSH and GPG keys` → `New SSH key`。

接下来，您将从 GitKraken 复制您的公共 SSH 密钥，并粘贴到 GitHub 帐户中上面显示的`Key`字段中，以及在`Title`中最有意义的任何标题。

ProTip:可以在 GitKraken 中点击`Copy file contents to clipboard`按钮，快速方便地将整个 SSH 密钥复制到 GitHub 中。

在`Title`和`Key`字段中添加信息后，点击 GitHub 中的`Add SSH Key`，完成将 SSH 密钥添加到 GitHub 的过程。就这么简单！

GitKraken 的安全 GitHub 集成使快速向 GitHub 添加 SSH 密钥以安全加密您的数据变得清晰而方便。

## **GitHub 在 CLI 中添加 SSH 密钥**

要开始使用 CLI 向 GitHub 添加 SSH 密钥，您将打开您选择的终端，如 [GitKraken CLI](https://www.gitkraken.com/cli) 。

接下来，您将使用以下命令，使用与您的 GitHub 帐户关联的电子邮件。

`ssh-keygen -t rsa -b 4096 -C “you _email@example.com”`

但是，如果您喜欢使用更新的`Ed25519`算法来添加 GitHub SSH 密钥，您可以使用:

`ssh-keygen -t ed25519 -C “your_email@example.com”`

然后 Git 会抛出几个提示。首先是选择文件位置。它应该是这样的:

`Enter file in which to save the key (/c/Users/alexl/.ssh/id_rsa): `

从这里，您可以点击`Enter`来接受默认位置。或者，如果在默认位置已经有一个 SSH 密钥，系统会询问您是否要覆盖 GitHub 中使用的现有 SSH 密钥。

您将看到的下一个提示是输入与 GitHub 相关的 SSH 密钥的密码。它应该是这样的:

`Enter passphrase (empty for no passphrase) `

您可以单击此处的`Enter`跳过添加密码短语。

几秒钟后，您将看到您的新 SSH 密钥已经生成！下面的例子是用用户`alexl`在 Windows 10 上给 GitHub 添加一个 SSH 密钥。

现在，您可以浏览到之前指定的文件位置，找到您的公共和私有 SSH 密钥。以下是 Windows 计算机上默认位置的一个示例:

生成 SSH 密钥后，您需要手动将 SSH 密钥添加到您的 GitHub.com 帐户中。你可以去 GitHub.com，登录，然后点击右上角的你的账户头像。这将带您到`Settings`，然后您可以导航到`SSH and GPG keys` → `New SSH key`。

接下来，您将打开您的公共 SSH 密钥文件(本例中的`.pub`文件)，突出显示所有文本，并将文本复制/粘贴到 GitHub 中上面显示的`Key`字段中。

最后，您将点击`Add SSH Key`来完成向 GitHub 添加 SSH 密钥的过程。