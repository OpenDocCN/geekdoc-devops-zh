# 使用 GitLens 获取 VS 代码|责备、代码透镜等 Git 信息

> 原文：<https://www.gitkraken.com/blog/get-git-info-gitlens>

*这篇文章是由* *通过* *同步带给你的。*

GitLens 是一个为 Visual Studio 代码构建的流行扩展，帮助开发人员提取隐藏在 [Git 代码库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中的强大洞察力。这个 VS 代码扩展通过提供快速浏览、比较和跟踪每一行代码的工具，使开发变得毫不费力。它还有助于解决大规模项目中本机 Git 特性的缺点和侵扰性。

本文将概述单独使用 Git 时很难发现的洞察，以及 GitLens 如何在您的 IDE 中神奇地发现它们。

GitLens+解锁了强大的团队功能，如可视化文件历史，它向您显示何时、由谁以及它们的重要性进行了更改。

[Sign up for GitLens+ for free!](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)

在本文中，我们将回顾 GitLens 和 GitLens+中提供的一些特性，这些特性帮助用户在工作流程中的多个点快速获取 Git 信息。

Git 的“黑暗”面

## 尽管 Git 有许多开发人员友好的原生特性，比如查看当前的变更日志和遍历提交，但是毫不费力的变更跟踪并不在其中。因为使用 Git 和 GitLens 这样的工具缺乏做出明智决策所需的环境，开发人员经常被迫切换到 [Git GUI](https://www.gitkraken.com/git-client) 或其他工具，而不是直接在他们已经工作的 IDE 中比较和跟踪变化。

虽然这可能不是一个噩梦，但是缺乏对项目的适当的可见性会给开发人员带来一些不适。事实上，让我们看看在没有 GitLens 的情况下使用 Git 和 VS 代码时缺少的一些重要部分。

Git 中的修订导航

Git 没有提供一种简单的方法来浏览 ide 中的多个版本。开发人员必须切换到在托管服务或 web 浏览器上查看他们的 Git 存储库，并手动浏览各种提交，以便跟踪更改。

## 这种转换中断了开发工作流，因为它要求开发人员切换应用程序。此外，即使查看了特定文件上的提交，开发人员也必须检查每个提交，以检查提交影响的行。

Git 没有提供一种简单的方法来浏览 ide 中的多个版本。开发人员必须切换到在托管服务或 web 浏览器上查看他们的 Git 存储库，并手动浏览各种提交，以便跟踪更改。

这种转换中断了开发工作流，因为它要求开发人员切换应用程序。此外，即使查看了特定文件上的提交，开发人员也必须检查每个提交，以检查提交影响的行。

Git 中的逐行差异跟踪

当使用 Git 和没有 GitLens 的 IDE 时，开发人员不得不走很长的路来跟踪多个文件修订中的变化。通过多次修订检查一行代码的更改的唯一方法是检查每次提交并手动比较差异。

## 如果没有像 GitLens 这样的工具，这个过程会非常耗时，在开发人员可以解决关键问题的时候会分散他们的注意力。

跟踪最近的 Git 文件更改

当生产环境中出现关键问题时，快速找到问题的根源至关重要。如果您认为它是由于最近的更改而发生的，那么识别更改和作者是第一步。

尽管 Git 提供了作者及其更改的视图，但它不允许开发人员快速浏览多个开发人员所做的数千个更改。这很困难，并且需要花费大量的精力，尤其是当所讨论的更改来自多次提交时。

## 跟踪最近的 Git 文件更改

GitLens 改进了 VS 代码和 Git 的使用

GitLens 将 Git 存储库增加的洞察力直接引入到最流行的 IDE VS 代码中。这非常有益的原因有很多，包括通过防止需要在不同任务的多个应用程序之间切换来保持平稳完整的工作流。

下面的 [GitLens 特性](https://www.gitkraken.com/gitlens/features)通过让使用 Git 和 VS 代码的开发工作流更加高效，提高了开发人员的工作效率。

## GitLens 责备

GitLens 责备在每一行的末尾插入一个清晰的注释，显示最后一次提交及其作者:

下面的 [GitLens 特性](https://www.gitkraken.com/gitlens/features)通过让使用 Git 和 VS 代码的开发工作流更加高效，提高了开发人员的工作效率。

通过将鼠标悬停在注释上，您可以很容易地看到关于更改的更多细节。

## GitLens 责备

GitLens 责备在每一行的末尾插入一个清晰的注释，显示最后一次提交及其作者:

作为一个特性，GitLens 责备为开发人员提供了更多的可见性，并允许他们直接从他们的 IDE 中查看信息。他们只需将光标移动到某一行，而无需输入任何 Git 命令。

git 无代码

Git CodeLens 在识别文件或代码块的最新变更时非常有用。这对于确定哪个修改破坏了应用程序至关重要。CodeLens 在文件或特定代码块的顶部显示最近的更改和作者。

在 GitLens 中，CodeLens 还允许开发人员查看关于提交的更多信息，并让他们通过点击文件或代码块顶部的作者姓名来跟踪每一行。

CodeLens 在代码的当前版本旁边显示每个 [Git commit](https://www.gitkraken.com/learn/git/commit) ，这使得跟踪更改变得容易。

## git 无代码

Git CodeLens 在识别文件或代码块的最新变更时非常有用。这对于确定哪个修改破坏了应用程序至关重要。CodeLens 在文件或特定代码块的顶部显示最近的更改和作者。

使用 GitLens 中的 CodeLens 跟踪每次提交的更改再简单不过了。将鼠标悬停在提交上，更改会神奇地出现在代码中！

在 GitLens 中，CodeLens 还允许开发人员查看关于提交的更多信息，并让他们通过点击文件或代码块顶部的作者姓名来跟踪每一行。

当您单击单个提交时，CodeLens 更进一步。它显示了一次提交中所做的所有更改，并为它们着色以便于查看。

Git 装订线更改

Git gutter changes 特性添加了一个颜色编码的热图，用于标识特定文件的最新更改。此外，它提供了对文件所做更改的视图，允许开发人员缩小最近编辑的代码行。

使用 GitLens 中的 CodeLens 跟踪每次提交的更改再简单不过了。将鼠标悬停在提交上，更改会神奇地出现在代码中！

GitLens 的这个特性还允许开发人员根据自己的喜好定制注释。

Git Gutter 热图

Git gutter 热图类似于 Git gutter changes 特性，但是它跟踪最近的更改，并在一行代码变冷时分配不同的颜色。默认情况下，90 天后的线路被认为是冷的，但是这可以在设置中自定义。

更好的版本导航

## GitLens 提供了强大的能力，通过并排比较修订版和文件的当前版本，可以在一个 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中的所有可用修订版之间跳转。

单独使用您的 IDE，这将需要相当大的努力。然而，使用 GitLens，只需几秒钟。

GitLens+功能

虽然 GitLens 提供的大多数功能都是完全免费的，不需要帐户，但也有一个免费的基于帐户的版本，名为 [GitLens+](https://www.gitkraken.com/gitlens/plus-features) ，它包括工作树和可视文件历史等附加功能，以进一步提高生产力和改善团队协作。GitLens+对于公共存储库是完全免费的。用户还可以[升级到 GitLens+ Pro](https://www.gitkraken.com/gitlens/pricing) ，以很低的价格访问工作树和私人回购上的可视文件历史等功能。

GitLens 的这个特性还允许开发人员根据自己的喜好定制注释。

GitLens+具有强大的功能，如可视化文件历史，让您快速浏览项目变更的极有价值的信息。

## 可视文件历史

GitLens+中的可视化文件历史视图允许开发人员可视化项目中单个文件的演变，包括每个作者的更改和贡献。

可视化表示使开发人员可以很容易地通过彩色编码的插图快速识别所有更改。变化的幅度由图中圆圈的大小表示。

## 更好的版本导航

GitLens 提供了强大的能力，通过并排比较修订版和文件的当前版本，可以在一个 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中的所有可用修订版之间跳转。

VS 代码+ Git 用 GitLens 更好

我们已经讨论了在 IDE 中使用 Git 与在同一个工作流中使用 vs 代码、Git 和 GitLens 在开发人员体验上的区别。本文中重点介绍的 GitLens 特性将通过将繁琐的步骤简化为几个毫不费力的鼠标点击来提高开发人员的工作效率，所有这些都不需要离开您的 IDE。

## 这篇博客是由 Syncfusion 的一位作者写的。 [Syncfusion](https://www.syncfusion.com/) 拥有超过 1700 个组件和框架，用于 [WinForms](https://www.syncfusion.com/winforms-ui-controls) 、 [WPF](https://www.syncfusion.com/wpf-ui-controls) 、 [WinUI](https://www.syncfusion.com/winui-controls) 、[。NET MAUI(预览版)](https://www.syncfusion.com/maui-controls)、ASP.NET([Web Forms](https://www.syncfusion.com/jquery/aspnet-web-forms-ui-controls)、 [MVC](https://www.syncfusion.com/aspnet-mvc-ui-controls) 、 [Core](https://www.syncfusion.com/aspnet-core-ui-controls) )、 [UWP](https://www.syncfusion.com/uwp-ui-controls) 、 [Xamarin](https://www.syncfusion.com/xamarin-ui-controls/) 、 [Flutter](https://www.syncfusion.com/flutter-widgets) 、 [JavaScript](https://www.syncfusion.com/javascript-ui-controls) 、 [Angular](https://www.syncfusion.com/angular-ui-components) 、 [Blazor](https://www.syncfusion.com/blazor-components) 、 [Vue](https://www.syncfusion.com/vue-ui-components) 和[考虑使用它们来加速您的应用程序开发。](https://www.syncfusion.com/react-ui-components)

虽然 GitLens 提供的大多数功能都是完全免费的，不需要帐户，但也有一个免费的基于帐户的版本，名为 [GitLens+](https://www.gitkraken.com/gitlens/plus-features) ，它包括工作树和可视文件历史等附加功能，以进一步提高生产力和改善团队协作。GitLens+对于公共存储库是完全免费的。用户还可以[升级到 GitLens+ Pro](https://www.gitkraken.com/gitlens/pricing) ，以很低的价格访问工作树和私人回购上的可视文件历史等功能。

如果您正在使用 VS Code + Git，您可以通过有用的见解增强您的工作流程并做出更好的决策——Git lens+完全免费。

GitLens+具有强大的功能，如可视化文件历史，让您快速浏览项目变更的极有价值的信息。

[Sign up for GitLens+ for free!](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)

可视文件历史

## GitLens+中的可视化文件历史视图允许开发人员可视化项目中单个文件的演变，包括每个作者的更改和贡献。

可视化表示使开发人员可以很容易地通过彩色编码的插图快速识别所有更改。变化的幅度由图中圆圈的大小表示。

The visual representation makes it easy for developers to identify all changes quickly with color-coded illustrations. The magnitude of the change is represented by the size of the circle in the graph.

VS 代码+ Git 用 GitLens 更好

## 我们已经讨论了在 IDE 中使用 Git 与在同一个工作流中使用 vs 代码、Git 和 GitLens 在开发人员体验上的区别。本文中重点介绍的 GitLens 特性将通过将繁琐的步骤简化为几个毫不费力的鼠标点击来提高开发人员的工作效率，所有这些都不需要离开您的 IDE。

这篇博客是由 Syncfusion 的一位作者写的。 [Syncfusion](https://www.syncfusion.com/) 拥有超过 1700 个组件和框架，用于 [WinForms](https://www.syncfusion.com/winforms-ui-controls) 、 [WPF](https://www.syncfusion.com/wpf-ui-controls) 、 [WinUI](https://www.syncfusion.com/winui-controls) 、[。NET MAUI(预览版)](https://www.syncfusion.com/maui-controls)、ASP.NET([Web Forms](https://www.syncfusion.com/jquery/aspnet-web-forms-ui-controls)、 [MVC](https://www.syncfusion.com/aspnet-mvc-ui-controls) 、 [Core](https://www.syncfusion.com/aspnet-core-ui-controls) )、 [UWP](https://www.syncfusion.com/uwp-ui-controls) 、 [Xamarin](https://www.syncfusion.com/xamarin-ui-controls/) 、 [Flutter](https://www.syncfusion.com/flutter-widgets) 、 [JavaScript](https://www.syncfusion.com/javascript-ui-controls) 、 [Angular](https://www.syncfusion.com/angular-ui-components) 、 [Blazor](https://www.syncfusion.com/blazor-components) 、 [Vue](https://www.syncfusion.com/vue-ui-components) 和[考虑使用它们来加速您的应用程序开发。](https://www.syncfusion.com/react-ui-components)

This blog was written by an author from Syncfusion. [Syncfusion](https://www.syncfusion.com/) has over 1,700 components and frameworks for [WinForms](https://www.syncfusion.com/winforms-ui-controls), [WPF](https://www.syncfusion.com/wpf-ui-controls), [WinUI](https://www.syncfusion.com/winui-controls), [.NET MAUI (Preview)](https://www.syncfusion.com/maui-controls), ASP.NET ([Web Forms](https://www.syncfusion.com/jquery/aspnet-web-forms-ui-controls), [MVC](https://www.syncfusion.com/aspnet-mvc-ui-controls), [Core](https://www.syncfusion.com/aspnet-core-ui-controls)), [UWP](https://www.syncfusion.com/uwp-ui-controls), [Xamarin](https://www.syncfusion.com/xamarin-ui-controls/), [Flutter](https://www.syncfusion.com/flutter-widgets), [JavaScript](https://www.syncfusion.com/javascript-ui-controls), [Angular](https://www.syncfusion.com/angular-ui-components), [Blazor](https://www.syncfusion.com/blazor-components), [Vue](https://www.syncfusion.com/vue-ui-components), and [React](https://www.syncfusion.com/react-ui-components). Considering using them to speed up your application development.

如果您正在使用 VS Code + Git，您可以通过有用的见解增强您的工作流程并做出更好的决策——Git lens+完全免费。

If you’re using VS Code + Git, you can enhance your workflow and make better decisions with helpful insights – completely free with GitLens+.

[Sign up for GitLens+ for free!](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)