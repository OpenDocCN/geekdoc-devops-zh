# CircleCI Hacks:在每次提交时用 Git 钩子验证 CircleCI 配置

> 原文：<https://circleci.com/blog/circleci-hacks-validate-circleci-config-on-every-commit-with-a-git-hook/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

CircleCI 2.0 开创了持续集成和交付的新时代。在 2.0 的新特性中(包括新的[配置文件格式](https://circleci.com/docs/sample-config/)，对 Docker 映像和工作流的强调)有了通过 CircleCI CLI 本地构建[的能力。我们可以使用本地构建和 Git 在每次提交时轻松地验证我们的配置文件。](https://circleci.com/docs/local-jobs/)

CircleCI CLI 非常适合通过本地构建来解决问题构建，并作为运行[支持 SSH 的构建](https://circleci.com/docs/ssh-access-jobs/)的替代方法。

这篇文章的重点是 CLI 的验证功能。

## 问题是

你有没有提交一些修改，把它们上传到 GitHub，然后在失败前运行 CircleCI 构建 3 秒钟？这可能是由于一些简单的原因，比如在第 7 行隐藏了一个分散的制表符，而不是正常的 2 个空格的缩进。

当你的配置文件中有几个这样的小错误时，可能会有一个持续几分钟的“修复、提交、推送、失败、重新开始”的循环。当处理代码中的重要特性时，这可能是可以接受的，但对于配置语法来说就不是了。

本地构建可以用一行代码来验证您的配置:`circleci config validate -c .circleci/config.yml`。有了快速 Git 挂钩，我们甚至可以实现自动化。

## 解决方案

我们将在 git `pre-commit`钩子中运行`validate`命令，以确保我们的配置文件按预期工作(通常是`.circleci/config.yml`)。

1.  首先，确保在本地机器上安装了构建工具。在 [CircleCI 文档](https://circleci.com/docs/local-jobs/#installing-the-cli-locally)中有相关说明。
2.  接下来，您需要在 repo 的`.git`目录中创建以下文件来创建 Git 挂钩。这可以用:`vim .git/hooks/pre-commit`来完成。如果你没有使用`vim`，用`nano`或者任何你喜欢使用的编辑器替换。
3.  对自动化系统的自动化部分自我感觉良好。

Git 挂钩:

```
#!/usr/bin/env bash

# The following line is needed by the CircleCI Local Build Tool (due to Docker interactivity)
exec < /dev/tty

# If validation fails, tell Git to stop and provide error message. Otherwise, continue.
if ! eMSG=$(circleci config validate -c .circleci/config.yml); then
	echo "CircleCI Configuration Failed Validation."
	echo $eMSG
	exit 1
fi 
```

仅此而已。一旦 git hook Bash 脚本被正确地放置在存储库中，CLI 中的构建工具将在每次提交之前验证您的配置。如果有效，您的提交将照常进行。否则，您将看到错误消息，提交将被取消。

## 更多信息