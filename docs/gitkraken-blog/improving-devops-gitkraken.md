# 用 GitKraken 提高你的 DevOps

> 原文：<https://www.gitkraken.com/blog/improving-devops-gitkraken>

## **我在 DevOps 中遇到的问题**

我叫詹姆斯·奎励杰，是 Axosoft 的 IT 主管。在 Axosoft，我实施了集中式日志记录基础架构，改进了我们的 Jenkins 构建以提高效率和并行化阶段，在内部实施了更严格的安全要求，利用多阶段 Docker 构建来缩小我们的映像大小，在 Redis 中设置了 auth token 缓存，等等。

可以想象，我在这个过程中遇到了各种各样的问题，并找到了一些有用的解决方案。所以，让我们深入研究一下吧！

### **配置作为代码的重要性**

在*空白即服务*的世界里(用基础设施、功能等来填充空白。)，您的整个系统的状态不仅仅是存储在您的高级开发人员头脑中的机构知识。它可以被编码和重用，以准确地重新创建您的系统。这意味着您可以将您的所有配置存储在一个版本控制系统(VCS)中，比如 Git，这样您就可以恢复到过去的任何一点，并跟踪您配置的整个历史。

我几乎立刻就发现了对 VCS 的需求；在 AWS CloudFormation、Docker 和 Docker-Compose、Jenkins 和 Ansible 之间，快速跳转到一个文件并找到一个配置的能力，而不是试图将它从我的脑海中拉出，是非常有价值的。当您考虑到该文件的价值可被其他团队成员共享或发现，以便他们也知道配置并可以在源代码控制中自由地进行更改时，这种重要性就更大了。

### **上下文切换**

另一件我在开始 DevOps 之旅时绝对低估的事情是，有多少工具存在，以及从一端到另一端，您可能使用多少工具来完成一项任务。在几分钟的时间里，我可能会用到:代码编辑器、源代码控制、 [Jenkins](https://www.gitkraken.com/resources/devops-report-2020#jenkins-build) 来检查我的构建、AWS 来检查基础设施、Kibana 来检查日志、 [Slack](https://www.gitkraken.com/resources/devops-report-2020#slack) 来检查警报和通信，等等。

我有时会开始一项任务，发现一些新的东西，最后却掉进一个完全不同的兔子洞。有时候，这并不是一件坏事，因为我会发现错误或我们可以改进的东西，但其他时候，这是一个巨大的干扰，大大减慢了我的工作流程。

### **跟踪需要完成的事情**

当我深入兔子洞，发现了一堆其他可以改进的地方时，我会在心里记下回头再来。当我发现的时候，我不记得我发现了什么。

即使我在便利贴上给自己做了一个*实际的*笔记，有时当我回过头来看时，缺乏上下文会使我非常不清楚前一天我在谈论什么。即“改进 Dockerfile”只能给你这么多的提示。

无论如何，这不是 DevOps 独有的问题，但这绝对是我在 DevOps 角色中遇到的问题，因为我负责处理许多不同的问题。

## **输入要去的地方**

### **彩虹图美学**

GitKraken 是一个跨平台的 Git GUI。在我开始在 Axosoft 工作之前，我个人使用过 GitKraken。当时，我对 Git 还比较陌生，虽然基本的 Git 操作非常简单，但当涉及到合并冲突和其他更复杂的操作时，我不太确定自己是否有能力正确解决问题。

GitKraken 有一个漂亮的 Git 树图，可以可视化你的分支、合并、隐藏，甚至其他远程操作。

让我立刻爱上 GitKraken 的一个东西是应用内合并工具，它可以省去打开其他工具的麻烦。

另外，如果你的团队正在使用 [Gitflow](/blog/gitflow) ，GitKraken 有一个集成，可以自动为你以正确的顺序合并正确的分支。

**电源编辑**

### 此外，在[版本 4.0](/blog/gitkraken-v4-0) 中，我们添加了摩纳哥代码编辑器，这带来了大量的功能:

在线差分

*   并排视图
*   语法突出显示
*   文件创建/删除
*   文件编辑
*   我发现很多时候，如果我在编辑一个 CloudFormation 模板，Dockerfile，Jenkinsfile 等。实际上，我并不需要做很大的改变，也不需要写完整本书。相反，我只需要对文件做一个快速的单行编辑。或者我在编辑的时候犯了一个错误，当我提交的时候我发现了这一点。现在，不用切换回不同的应用程序，我可以在 GitKraken 中快速进行编辑。

**带 Glo 的 EZ 任务跟踪**

Glo Boards 帮助我们的开发团队更加高效，因为我们从 GitKraken Git 客户端内部跟踪任务和问题。我喜欢还有 VS 代码和 Atom 的插件，如果我在路上，可以从浏览器或移动应用程序访问它！

## Glo 板与 GitHub 问题实时同步，这使得将任务分散到工作流中变得容易，并直观地跟踪项目的完成进度。与大多数[任务跟踪工具](https://www.gitkraken.com/resources/devops-report-2020#project-management)不同，它实际上是为开发者设计的，所以它支持 markdown 并有大量的键盘快捷键。

[Glo Boards](https://www.globoards.com/) help our dev team be more productive because we track tasks and issues from inside the GitKraken Git Client. I love that there are also plugins for VS Code and Atom, and it can be accessed from a browser or the mobile apps if I’m on the go!

**使用 GitKraken 的我的 DevOps 流**

查阅我的 Glo 待办事项，选择一项任务，转到“进行中”

## 假设这是一个更长的任务，打开 VS 代码，进行我的修改。如果没有，就用 [GitKraken 代码编辑器](https://support.gitkraken.com/start-here/tips/#4-create-open-and-edit-files-in-the-built-in-code-editor)！

1.  打开 GitKraken，在并排比较视图中查看我的更改。
2.  使用 GitKraken 代码编辑器修复任何问题。
3.  提交、推送、创建公关。
4.  —切换到 Glo，移动我的卡。
5.  **解决方案**
6.  我使用 GitKraken 工具为 DevOps 建立的工作流确实帮助我解决了我前面谈到的一些问题。

***查看 GitKraken 工具套件在 DevOps 生命周期中的位置。***

[**2020 DevOps 工具报告**](https://www.gitkraken.com/resources/devops-report-2020)

## 有了 Glo 板，我真的可以很快地添加足够上下文的卡片，这样我就知道当我回头再看它们时该做什么。

一旦我开始处理一个特定的任务，我就可以在同一个应用程序中处理基本的文件编辑、审阅、源代码控制和任务跟踪；所以我不需要太多的上下文切换。

GitKraken 显然不会强制您将配置作为代码使用，但它可以增强您在 repos 之间快速切换、查看配置、进行编辑以及将更改存储在 Git 中的能力——提高您的整体工作质量。

[**2020 DevOps Tools Report**](https://www.gitkraken.com/resources/devops-report-2020)

*   With Glo Boards, I can really quickly add cards with enough context so I know what to do when I come back to them later.
*   Once I’ve gotten to work on a specific task, I can handle basic file editing, reviewing, source control, and task tracking all within the same app; so I don’t have to context switch *quite* as much.
*   GitKraken obviously isn’t going to enforce that you use configuration as code, but it can enhance your ability to quickly switch between repos, look at that configuration, make edits, and store changes in Git—improving your quality of work overall.