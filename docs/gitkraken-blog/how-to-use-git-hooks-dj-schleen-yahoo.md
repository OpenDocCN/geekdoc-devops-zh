# 如何使用 Git 钩子| GitKon 2022 | DJ Schleen，Yahoo！

> 原文：<https://www.gitkraken.com/gitkon/how-to-use-git-hooks-dj-schleen-yahoo>

[https://www.youtube.com/embed/JI1WYKfjG1I?feature=oembed](https://www.youtube.com/embed/JI1WYKfjG1I?feature=oembed)

视频

在 DevOps 中讨论反馈循环时，经常使用术语“左移”。本质上，这意味着如果一个人能够以更快的方式向软件工程师提供反馈，他们将有能力更快地做出反应。一般来说，工程师需要了解的信息是质量问题、构建中断、部署问题，更常见的是安全漏洞。

大多数反馈都是在代码被签入到[远程 Git 库](https://www.gitkraken.com/learn/git/git-remote)之后产生的；通常在一个[拉动请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)或发布工作流。工程师很少在提交代码之前被告知他们的代码有问题，除非是通过 IDE 插件在他们编码时提供指导和检查，就像 [GitLens for VS Code](https://www.gitkraken.com/gitlens) 。虽然这些扩展可以帮助工程师在编写代码时学习和修复代码，但它们不提供更复杂的检查和操作。这就是使用 [Git 钩子](https://www.gitkraken.com/learn/git/tutorials/how-to-create-git-hooks)可以有所帮助的地方。

## **Git 挂钩是一个脚本集合**

Git 挂钩只是工程师与 Git 可执行文件交互时调用的[脚本](https://www.gitkraken.com/blog/shell-scripting)的集合。当使用 Git 提交、合并、推送或管理代码时，本地代码存储库中保存的脚本就会运行。这方面的一个例子是在代码提交到存储库之前运行 lint 函数。

当用户在他们的本地机器上创建一个 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)时，会创建一个名为`.git/hooks`、的隐藏文件夹，其中包含工程师可以修改的示例脚本。这些样本的命名约定是*`hooktype`-样本*。

将一个示例重命名为它的钩子类型会让 Git 知道应该运行这个脚本。任何东西都可以放入这些文件；可能性是无限的。不幸的是，这里有一个主要问题。文件夹`.git`永远不会提交到存储库中，所以对文件夹`.git/hooks`内容的任何更改都将保留在本地。

## **管理 Git 挂钩**

为了使 Git 挂钩的管理更容易，并允许定制功能的可移植性，工程师可以利用 DevOps Kung Fu Mafia 的一个名为[“Hookz”的工具。](https://github.com/devops-kung-fu/hookz)

Hookz 是一个应用程序，它允许工程师在一个简单的 yaml 文件中为 Git 调用的任何钩子类型定义将要执行的操作。这个 yaml 文件可以提交给存储库，并与其他存储库用户共享。开发人员可以添加自定义脚本、调用可执行文件，甚至可以引入在钩子执行期间可以使用的应用程序。

一旦在本地存储库的根目录下创建了这个`.hookz.yaml`文件并配置了功能，工程师就运行`hookz`可执行文件，该文件在本地`.git/hooks` 文件夹中构建所有需要的脚本。

## **使用 Git 挂钩添加功能**

Git 挂钩为工程师向 Git 可执行生命周期添加功能提供了无限可能。可以在提交代码之前为`pre-commit`钩子类型定义一些功能的例子:

*   从远程存储库中提取，以确保没有遗漏任何上游更改。
*   将所有代码依赖项升级到最新版本，以便可以测试升级并解决不兼容问题——这有助于减轻技术债务的积累。
*   运行安全工具，即软件组成分析(SAC)工具，可以通知开发人员其依赖关系中的漏洞。
*   执行测试用例并生成测试覆盖度量。
*   确定代码的圈复杂度、林挺，并提供其他质量检查。
*   生成软件物料清单。
*   自动更新 markdown 文档(例如:使用 terraform-docs)或其他文件。
*   在推送到远程设备之前，将预提交阶段更改或添加的任何内容提交回提交阶段。

如果任何任务失败，Hookz 会输出一个非零的返回代码，提交将会停止。这允许开发人员在被推到远程存储库之前解决问题。

Git 挂钩提供了一种强大的方式来收紧反馈循环，比等待工具在远程存储库上运行更快地将信息传递给工程师。这导致了更干净的代码、更快的修复和改进的代码质量。

如果您正在寻找一种工具，使使用 Git 和 Git 挂钩更容易、更安全、更强大，那么 GitKraken Client 是最好的选择，它是世界上最受开发人员和团队欢迎的 Git 客户端。