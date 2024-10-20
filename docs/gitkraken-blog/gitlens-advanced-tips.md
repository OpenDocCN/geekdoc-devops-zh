# 如何在 VS 代码中使用 GitLens 的 5 个高级技巧

> 原文：<https://www.gitkraken.com/blog/gitlens-advanced-tips>

GitLens 是一个 Git 扩展，与 [Visual Studio 代码](https://code.visualstudio.com/)无缝集成。事实上，有超过 1400 万的安装量，它是 VS 代码市场上最受欢迎的 Git 扩展之一。GitLens 之所以广受欢迎，是因为它能够解锁有价值的作者信息，帮助用户浏览项目的修订历史，等等。

即使你是 GitLens 的长期用户，你可能会发现本文中的提示将揭示你以前不知道的 GitLens 的功能。如果你是 GitLens 新手，不用担心！虽然这些都是高级技巧，但你可以在今天的工作流程中应用这些技巧。[免费注册 git lens+](https://www.gitkraken.com/gitlens/plus-features),这样你就可以遵循本文中的提示。

[https://www.youtube.com/embed/bOoLr3_8pco?feature=oembed](https://www.youtube.com/embed/bOoLr3_8pco?feature=oembed)

视频

在 VS 代码中释放 Git 的全部力量——注册 GitLens+以访问强大的功能，如工作树和可视化文件历史！

[Sign up for GitLens+ for free!](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)

## **变怪怪怪**

GitLens 在每一行代码上公开注释，通过 hovers 提供更多信息。悬停可以显示详细的责备信息，如[提交消息](https://www.gitkraken.com/learn/git/best-practices/git-commit-message)、作者信息、提交 ID 等等。所有这些信息都可以通过将光标悬停在代码的特定部分来访问。

要修改 GitLens 中的指责悬停行为，请在 VS 代码中通过键入`cmd+shift+P`打开命令面板，然后在命令面板中键入`GitLens settings`，并点击`enter`。

接下来，向下滚动到`Hovers`部分。您可以通过选择和取消选择紧靠`Hovers`标题左侧的复选框来打开或关闭整个功能。此外，您可以通过选择`Hovers`部分后面的复选框来定制您的悬停显示内容以及它们的行为方式。

要定制当鼠标悬停在一行文本而不仅仅是注释上时出现的光标，选择`Show hovers for the current line`下的下拉菜单并选择`line & annotation`。

现在，您可以将光标悬停在注释或一行代码的任何部分上，以访问 GitLens 悬停信息。

## **GitLens 设置:配置日期和时间格式**

GitLens 允许您修改日期和时间设置，使您非常容易地本地化时间和显示您喜欢的格式。要调整 GitLens 的日期和时间设置，请在 VS 代码中打开命令面板，键入`GitLens settings`，然后点击`enter`。然后，导航到位于屏幕右侧的`Jump to menu`部分，并选择右下角附近的`Dates & Times`。

从这里开始，您可以使用各种格式选项。只需点击`Allow relative date formatting`复选框，就可以将日期格式化为相对时间，类似于:`1 day ago`，或者绝对时间，类似于:`July 25th, 2018 7:18 PM`。

您也可以在这里设置您的`Date locale`。您可以选择默认 VS 代码区域设置，或者手动设置您自己的区域设置。

GitLens 中默认的日期格式表示为`MMMM Do, YYY h:hmma`，看起来像是`July 25th, 2018 7:18 PM`。您可以通过点击`Date format`框并输入您想要的格式来轻松更新格式。例如，您可以选择这样反映日期:`YYYY-MM-DD`，如下所示:`2018-07-25`。

您可以按照如下所示的过程设置所需的日期格式，以自定义`Short Date`和`Time`格式。如果您后来发现自己进行了想要恢复或更改的更改，您可以随时返回 GitLens 设置并进行这些更新。

## **为任何 GitLens 命令添加按键绑定**

您可以为任何可以从命令调板访问的 GitLens 命令添加按键绑定。键绑定或热键是一组执行特定操作的自定义键击。如果您发现自己一遍又一遍地在命令面板上寻找同一个命令，这个特性会非常有用。

要创建一个 keybinding，在 VS 代码中打开命令面板并输入`>GitLens:`。这将显示所有可用的 GitLens 命令列表。接下来，将鼠标悬停在要为其创建热键的命令上，并选择出现在右侧的齿轮图标。这将带您进入所选命令的设置。

在这里，选择屏幕顶部的`Keybinding`。将出现一个文本框，提示您创建自定义的组合键。输入与所选 GitLens 命令相关联的所需组合键，然后点击`enter`。现在，只要你输入你创建的组合键，GitLens 就会运行相关的命令。

按照这个过程为任何 GitLens 命令创建一个键绑定。但是，如果您试图为另一个 GitLens 或 VS Code 原生命令创建一个已经存在的 keybinding，您会得到一个警告消息，建议您创建一个新的、唯一的 keybinding。

## **定制 CodeLens**

GitLens 利用了 [CodeLens](https://code.visualstudio.com/blogs/2017/02/12/code-lens-roundup) ，这是另一个暴露 VS 代码内部信息的工具。使用 GitLens，您可以定制 CodeLens 操作的行为方式。

要定制 GitLens 如何利用 CodeLens，请执行以下操作:打开 VS 代码命令面板，键入`GitLens settings`，然后点击`enter`。接下来，向下滚动到标题为`Git CodeLens`的部分。从这里，您可以完全控制 CodeLens，包括通过选择标题`Git CodeLens`左侧的复选框来打开和关闭整个功能，以及其他细粒度的控制。

当您打开和关闭 CodeLens 功能时，您会注意到中间的预览窗口显示了您的自定义如何影响 CodeLens 显示的信息。

有很多方法可以定制 CodeLens 以适应您的特定工作流，所以不要害怕探索这些选项并真正使它成为您自己的。

## **使用令牌定制 GitLens 注释**

您可以完全控制 GitLens 公开注释的方式，并且可以使用一整套标记来完全个性化该信息在工具中的呈现方式。

要开始定制 GitLens 注释，打开 VS 代码命令面板，输入`GitLens settings`，点击`enter`。在 GitLens 设置的顶部，你会发现一个标题为`Current Line Blame`的部分。像我们讨论过的其他功能一样，您可以通过`Current Line Blame`标题左侧的复选框打开或关闭整个功能。

您会注意到在这一部分的底部，有一个定制 GitLens 注释格式的选项。默认的注释格式是`${author,}${agoOrDate}${via`pullRequest}${  • message|50?}`。但是，如果您单击注释格式框，将会显示几个可用的标记，允许您修改注释格式。

例如，您可以通过用`|10`替换`|50`来修改`• message|50?}`字段，使其只显示 10 个字符。您还可以对序列中某些 GitLens 责备注释出现的位置进行完全重新排序。默认情况下，GitLens 责备注释首先显示`${author,}`信息。但是，您可以将该字段移动到注释序列中的任何位置，只需简单地剪切和粘贴它以适应您想要的顺序。

此外，您可以自定义注释在当前行责备和状态栏责备中的显示方式。要访问状态栏责备，只需在 GitLens 设置中向下滚动到标题为`Status Bar Blame`的部分。您可以选择配置两个选项的设置，以提供相同的 GitLens 责备信息，或者您可以调整它们以显示稍微不同的信息。

## **定制 GitLens 以适应您独特的工作流程**

这 5 个技巧只是 GitLens 可定制性的一瞥。这个工具不仅功能极其丰富，而且您还可以通过裁剪它来满足您的开发需求。如果你正在寻找更多的方法来释放 Git 在 VS 代码中的全部力量，并增强你的 GitLens 体验，请查看这些额外的 [11 GitLens 技巧](https://www.gitkraken.com/blog/gitlens-tips)。

GitLens+特性减少了用 VS 代码完成复杂任务的时间。尝试可视化文件历史，以显示项目的贡献者和您的贡献的图形表示。

[Sign up for GitLens+ for free!](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)