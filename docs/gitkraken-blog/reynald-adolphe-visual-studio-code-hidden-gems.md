# VS 代码中隐藏的宝石| GitKon 2022 | Reynald Adolphe，微软

> 原文：<https://www.gitkraken.com/gitkon/reynald-adolphe-visual-studio-code-hidden-gems>

[https://www.youtube.com/embed/9mPBXV7hVA0?feature=oembed](https://www.youtube.com/embed/9mPBXV7hVA0?feature=oembed)

视频

在他的 GitKon 2022 会议上，微软高级开发人员 Reynald Adolphe 探索了一些与 Visual Studio 代码(VS 代码)相关的隐藏宝石，许多人都没有意识到。涵盖了两个主要主题:分支管理和文档管理。

如果你在 VS 代码中使用 Git，尝试使用世界上最流行的扩展，现在有了团队协作特性。

[Download GitLens Free!](https://www.gitkraken.com/gitlens/try-free)

## **Visual Studio 中的分支管理代码**

在分支管理方面，大多数开发人员都知道，他们可以使用 VS Code 的 UI 作为从终端执行 [Git 命令](https://www.gitkraken.com/learn/git/commands)的替代方法来管理分支，包括像[创建 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)、[重命名 Git 分支](https://www.gitkraken.com/learn/git/problems/rename-git-branch)、[删除 Git 分支](https://www.gitkraken.com/learn/git/problems/delete-local-git-branch)等等。

然而，有许多开发人员没有意识到，您可以在幕后观察 Git 命令与代码在使用其 UI 时的执行情况。

为此，导航到终端的输出并从下拉菜单中选择`Git`，如下所示。

下面，您可以看到一些常见的 Git 命令是如何使用 VS 代码完成的，并与 VS 代码实际执行的命令进行了比较。事情并不总是像你想的那样。

例如，对于创建分支和[切换 Git 分支](https://www.gitkraken.com/learn/git/problems/switch-git-branch)，VS 代码分别使用 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 命令，而不是 [Git 分支](https://www.gitkraken.com/learn/git/branch)和 Git 开关。

创建 Git 分支:

*   **Git 终端**中的选项:
    *   `git branch branch-name`
        *   `git checkout branch-name`
        *   **VS 代码执行** : `git checkout -q -b branch-name`
    *   切换 Git 分支:

**Git 终端**中的选项:

*   `git switch branch-name`
    *   `git checkout branch-name`
        *   **VS 代码执行** : `git checkout -q branch-name`
        *   重命名 Git 分支:
    *   Git 终端中的**选项:`git branch -m new-branch-name`**

**VS 代码执行** : `git branch -m new-branch-name`

*   删除 Git 分支:
    *   Git 终端中的**选项:`git branch –d branch-name`**
    *   **VS 代码执行** : `git branch –d branch-name`

*   像这样的信息对于那些刚接触 Git 和不熟悉终端命令，但对 VS 代码在幕后做什么很好奇的人来说非常有用。但是这个输出屏幕对于那些已经熟悉终端并对新命令感兴趣的人来说也很方便。
    *   Git 终端中的**选项:`git branch –d branch-name`**
    *   Git 新手？查看 GitKraken Learn Git 中心，通过教程、文章和备忘单探索初级、中级和高级操作。

**Visual Studio 代码中的文档管理**

VS 代码通常用于文档管理。虽然 VS 代码的文档管理功能不经常被提及，但它是一个非常有用的隐藏的瑰宝。

VS 代码中的文档管理可用于以下任务(但不限于):

[Visit the Learn Git Center](https://www.gitkraken.com/learn/git)

简单记笔记

## 技术 API

跟踪代码片段

**降价**

*   Markdown 是 VS 代码中用于文档管理的主要语法，尽管还有其他的选择。
*   开始使用 markdown 的一些优秀资源包括:
*   跟踪代码片段

Markdown 本质上是 HTML 的一种更简单的形式，所以学习曲线很低，这带来了好处。

### 有很多扩展可以用来简化在 VS 代码中使用 markdown 的过程，比如 [GitDoc](https://marketplace.visualstudio.com/items?itemName=vsls-contrib.gitdoc) ，它通过自动提交到存储库来将您的 markdown 视为 word 文档。这只是它的众多特性之一。

其他 feutres GitDoc 产品包括:

自动拉动

挤压版本

撤销版本

还原版本

VS 代码中文档管理需要考虑的其他扩展包括:

*   自动拉动
*   您可以将文档保存到一个私有的存储库中，这样就可以在任何地方访问它。更好的是，如果你使用的是 [vscode.dev](https://vscode.dev/) ，你可以在任何浏览器上修改你的文档和代码，甚至是在 iPad 上。
*   文档管理的重要性，尤其是 VS 代码的易用性，是不应该被忽视的瑰宝。
*   还原版本

**用 Visual Studio 代码**管理分支和文档

缺乏适当的分支和文档管理会使开发人员的生活变得困难，而掌握这些技术，并利用像 VS Code 的 [GitLens 和本文中提到的其他工具可以使您的工作生活更加顺利。](https://www.gitkraken.com/gitlens)

您可以将文档保存到一个私有的存储库中，这样就可以在任何地方访问它。更好的是，如果你使用的是 [vscode.dev](https://vscode.dev/) ，你可以在任何浏览器上修改你的文档和代码，甚至是在 iPad 上。

文档管理的重要性，尤其是 VS 代码的易用性，是不应该被忽视的瑰宝。

## **用 Visual Studio 代码**管理分支和文档

缺乏适当的分支和文档管理会使开发人员的生活变得困难，而掌握这些技术，并利用像 VS Code 的 [GitLens 和本文中提到的其他工具可以使您的工作生活更加顺利。](https://www.gitkraken.com/gitlens)