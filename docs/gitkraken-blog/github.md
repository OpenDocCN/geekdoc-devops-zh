# GitHub 集成|使用 GitHub 和 GitKraken

> 原文：<https://www.gitkraken.com/git-client-integrations/github>

将 GitKraken 与 GitHub 集成连接后，您将能够生成并添加 GitHub SSH 密钥。

导航到`Preferences` → `SSH`，然后点击 魔法 ✨ `Generate SSH key and add to GitHub`按钮。

GitKraken 使与 GitHub 的连接变得快速而简单，甚至让您直接从 GUI 登录到您的 GitHub 帐户。 登录 GitKraken 时，可以选择`Sign in with GitHub`按钮，输入凭证。这将自动连接您的 GitKraken 帐户到 GitHub 集成。

为了开始添加 GitHub 遥控器，你仍然需要 通过 GitHub 认证 GitKraken。不过不用担心，那个过程也一样简单！ 只需导航至 GitKraken UI 的右上角，点击 gear ⚙️图标，即可进入您的 GitKraken 账户偏好设置，然后点击`Integrations`。从这里，你可以选择左边的`GitHub`，然后点击`Connect to GitHub`按钮。

GitKraken 允许 GitHub.com 或 GitHub 企业集成的用户从 GitHub 派生存储库。 要开始派生 GitHub 存储库，首先要在 GitKraken 中打开 GitHub 存储库。然后，点击左侧`Remote`面板的绿色`+`图标，选择`GitHub.com`或`GitHub Enterprise`选项卡。 如果 GitKraken 在远程存储库上没有检测到任何现有的分支，Git GUI 将为您提供分支存储库的选项，然后将其添加为远程存储库。

点击`Fork and Add Remote`按钮，分叉 GitHub 库，在 GitKraken 的左侧面板中添加为 remote。

在 GitKraken 中添加 GitHub remote 的现有 fork 的过程非常类似。您将遵循如上所示的第一步，但是 GitKraken 将检测现有的分叉，从这里，您可以简单地单击`Add this Remote`按钮。

GitKraken 允许您通过将一个分支拖放到另一个 分支 上并选择`Start a pull request...`选项，轻松地直接在应用程序内创建 GitHub pull 请求。这将在 GitKraken 中打开一个 pull request 模板，在这里您可以向您的 PR 添加审核人、受托人和标签，然后 GitKraken 会将其传递给 GitHub。

GitKraken 还允许您直接在 Git 客户端创建和保存 GitHub pull 请求草案。只需选中 pull request 模板中`Submit as draft`旁边的复选框，就可以创建 GitHub draft pull request。

在 GitKraken 中，GitHub remote repo 和 GitHub Issues 集成共享同一个连接，在 导航 到`Preferences` → `Integrations`后，您可以从`ISSUES`窗格中选择 GitHub 或 GitHub Enterprise。

在 GitKraken 中连接 GitHub 问题集成后，您的问题将出现在左侧面板中。将鼠标悬停在左侧面板中的任何问题上可获得预览，包括问题标题、描述、状态、标签、受托人和报告者。

要在 GitKraken 中创建一个新的 GitHub 问题，首先要点击左侧面板中问题跟踪器旁边的绿色`+`按钮。

GitKraken 提供的另一个伟大特性是能够创建与 GitHub 问题相关的 分支 。 这使您能够简化问题跟踪并直接开始工作，而无需在两种工具之间切换上下文。 您可以通过点击`Create a branch for this issue`按钮，直接从问题详细信息视图创建与问题相关联的分支。或者，您可以单击左侧面板中的某个问题来访问相同的选项。