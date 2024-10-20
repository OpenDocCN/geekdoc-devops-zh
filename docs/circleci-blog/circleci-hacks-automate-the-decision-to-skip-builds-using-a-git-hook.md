# 使用 Git 钩子自动决定跳过构建

> 原文：<https://circleci.com/blog/circleci-hacks-automate-the-decision-to-skip-builds-using-a-git-hook/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

开发人员使用 CircleCI 构建各种项目。这些项目中的许多都遵循典型的一个项目一个回购的代码组织方法。有些包括文档、日志、供应商包、部署脚本等等。还有谷歌等公司使用的单边回购协议。这些回购带来的额外复杂性导致我们许多客户的工作流程不同。

！[快进 _ 图标 _- *宏 _ 摄影 _of_a_remote_control.jpg](/blog/media/快进 _ 图标*-_ 宏 _ 摄影 _ of _ a _ remote _ control . jpg)

有时候，客户不希望 CircleCI 构建一个微不足道的提交。也许他们只是在自述文件中添加了注释，或者更新了日志文件。不构建可以为实际需要的提交节省容器时间。CircleCI 通过支持提交消息中的`[ci skip]`或`[skip ci]`标志来解决这个问题。如果经常需要这样做，每次在提交消息前加上`[skip ci]`会很快变得令人讨厌。这可以用 Git 钩子自动完成。

在这篇博文中，我们将使用`commit-msg` Git 钩子创建我们的自动构建跳过特性。

## 我们将使用什么

### Git 挂钩

对于那些不熟悉的人，Git 有一个钩子系统。这些是时间点，在 Git 中可以运行脚本的特定操作之前或之后。例如，假设每次提交时，我们都希望确保使用最新的上游代码。在 Git 保存提交之前，可以利用 Git 挂钩来更新主服务器，并在主服务器上重置当前分支。

### [跳过配置项]

如前所述，CircleCI 支持 CI 标准提交标志`[skip ci]`。当该标志出现在提交消息中时，CircleCI 不会运行构建。这可以节省运行的容器数量和使用的总构建时间。

### 使用. ciignore 文件

为了模仿 Git 对`.gitignore`文件的使用，我们将为这篇博文创建一个`.ciignore`文件的概念。我们正在创建的 Git 钩子将使用这个文件来知道在运行构建时应该忽略哪些文件、目录和模式

## 正在实施。回购中的 ci 忽略

本文中我们使用的场景是根目录中的一个`.ciignore`文件，它将告诉 Git 我们不想在 CircleCI 上构建，因为提交中唯一的更改是在`logs/`目录中。我们回购的根源可能是这样的:

```
$ ls -la
total 9
drwxrwxr-x 4 felicianotech felicianotech 4096 Jun 15 00:44 ./
drwxrwxr-x 4 felicianotech felicianotech 4096 Jun 15 00:44 ../
-rw-rw-r-- 1 felicianotech felicianotech    7 Jun 15 00:32 .ciignore
-rw-rw-r-- 1 felicianotech felicianotech 1077 Jun  1 20:29 circle.yml
-rw-rw-r-- 1 felicianotech felicianotech  135 Jun  1 20:23 Dockerfile
drwxrwxr-x 8 felicianotech felicianotech 4096 Jun 15 00:52 .git/
drwxrwxr-x 2 felicianotech felicianotech 4096 Jun 15 00:45 logs/
-rw-rw-r-- 1 felicianotech felicianotech   43 Apr 29 22:23 main.go
-rw-rw-r-- 1 felicianotech felicianotech   20 Apr  1 16:40 README.md
-rw-rw-r-- 1 felicianotech felicianotech   31 Jun 15 00:44 test.txt 
```

`.ciignore`文件放在回购的根目录中，而我们的 Git 钩子的文件路径将是`.git/hooks/commit-msg`。这些文件的内容可以通过下面的 GitHub Gists 看到。

**。ci ignore**——这个文件包含一个单一的模式，`logs/*`。我们希望跳过日志目录中的变更构建。

**。这是我们的钩子，一个简单的 Bash 脚本。这里有一个快速分类:**

1.  脚本首先检查`.ciignore`是否存在。如果它不存在，那很酷。一切照旧。
2.  然后，我们调用`git diff`来获取一个机器可读的文件列表，这些文件是暂存的(将在这次提交中被更改)。
3.  我们为要忽略的模式列表加载`.ciignore`。
4.  我们循环遍历这些更改，每当它匹配一个忽略模式时就删除它。
5.  如果我们仍然有更改，这意味着我们需要实际构建提交，这样我们就退出了。
6.  如果我们到达这一步，我们将在提交消息(存储在文件$1 中)前面加上`[skip ci]`，这样可以节省一些构建时间。

第 4 步和第 5 步很重要，因为即使在我们想要忽略的目录中发生了变化，如果主要的源代码也发生了变化，我们仍然想要构建。否则，我们可能会在一个场景中结束，在这个场景中，我们在同一次提交中添加了一个特性并更新了一个日志文件，而它永远不会被构建，这意味着该特性的推出现在被延迟了。

## 笔记

需要记住的一点是，钩子不会随着回购而移动。这意味着客户端钩子，我们在这里使用的，不在 Git 的版本控制之下，当一个回购被克隆或推送时，它们也不会被复制。为了绕过这一点，有些人在回购本身的目录中保留挂钩，然后在克隆回购后将它们符号链接到位。

使用 Git 钩子创建一个`.ciignore`文件只是如何利用 Git 钩子改进 CircleCI 日常工作流程的一个例子。其他的想法可以是使用 Git 钩子在提交时修改`circle.yml`,只运行特定的测试。

你用 Git 钩子和 CircleCI 吗？我们很想在[讨论](https://discuss.circleci.com/)上听到它。

想加入才华横溢的 CircleCI 团队吗？我们正在招聘！