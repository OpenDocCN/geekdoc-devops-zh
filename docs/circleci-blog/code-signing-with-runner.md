# 使用 runner | CircleCI 管理 CircleCI 上的代码签名

> 原文：<https://circleci.com/blog/code-signing-with-runner/>

代码签名是测试和分发桌面和移动应用程序的重要部分。它确保最终用户的系统可以验证您的应用程序的合法性。由于对签名证书安全性的需要，它们存储在本地，而不上传到云中。这个约束可能会阻止您的团队完全自动化您的 CI/CD 管道。幸运的是，使用 runner，您的团队可以在您控制的机器上保持证书安全，同时仍然允许 CircleCI 作业对您的包进行签名。

## 什么是代码签名？

代码签名在移动应用领域很常见，谷歌 Play 商店和 App Store 要求每个发布的应用都使用它。代码签名向用户保证他们下载的应用程序将被他们想要安装的设备信任。

### 桌面应用程序的代码签名

代码签名可用于桌面二进制文件，但不太常见。在 Windows 上运行未签名的二进制文件时，您可能会得到一个提示，询问您是否信任发行者。操作系统会提醒您这是一个未签名的应用程序。该应用程序可能值得信任，也可能不值得信任，因此您可以将该提示用作警告标志。

有两种方法可以管理应用程序的代码签名:

*   将签名嵌入到二进制文件中，以便操作系统可以对其进行验证；这是 macOS、Windows 和移动操作系统使用的。
*   提供二进制文件的*散列*的签名；用于 Linux。

这两种方法都可以用来验证下载的文件是否正确。

## 保护私有证书的安全

最佳实践是尽可能保证这个私有证书的安全，不要把它放在网上或云中。因此，在 CircleCI 中存储私有证书不是一个好主意，无论是在安全上下文中还是在环境变量中。不建议将证书或私有 GPG 密钥上载到 CircleCI。

然而，当涉及到测试时，有一个例外。在 CI/CD 过程中，本地创建的证书可以用于上传和测试，只要该证书没有被其他机器分发和信任。当您使用 runner 来保证您的团队控制的机器上的证书安全时，这个异常使得对您的 CI/CD 管道执行端到端测试成为可能。

## 在您的机器上设置转轮

在你自己的机器上安装 runner 很简单，目前在 Ubuntu、macOS 和 Windows 上都支持。您可以在[安装 CircleCI 转轮](https://circleci.com/docs/runner-installation/index.html/)中找到多个平台的说明。

您还需要将私有证书安装到您用来签名的平台所要求的位置。以下是说明:

完成这些步骤后，您就可以开始使用 CircleCI runner 为您的应用程序签名了。

## 使用签名哈希在 Linux 上签名

Linux 不使用签名二进制文件的概念。相反，用户依赖于文件散列和签名。

第一步是计算要分发的文件的文件哈希。您可以使用各种强度的 SHA 和，包括 SHA1(不推荐)、SHA256 或 SHA512 哈希。对于本教程，我们将使用 SHA512。

```
$ sha512sum xyz.exe
cf83e...927da3e  xyz.exe 
```

将结果保存为`SHA512SUMS`。如果您想在一个签名中包含多个文件，只需将它们的文件名传递给`sha512sum`命令。每个文件的散列将被打印到单独的一行。不要编辑输出，否则以后将无法验证散列。

现在你可以签名了。如果您已经生成了一个私有 GPG 密钥，那么签名非常简单:

```
$ gpg --sign SHA512SUMS --output SHA512SUMS.sig 
```

这段代码输出文件`SHA512SUMS.sig`，该文件已经使用您的密钥进行了签名。用您的二进制文件分发这个`.sig`文件，并指示用户在安装您的公钥后验证输出。您可以在这里查看[的说明](https://www.gnupg.org/gph/en/manual/x56.html)。

```
$ gpg --decrypt SHA512SUMS.sig > SHA512SUMS
gpg: Signature made Tue 22 Jun 13:53:56 2021 BST
gpg:                using RSA key 1234XYZ
gpg: Good signature from "CircleCI <contact@circleci.com>" [ultimate]
$ sha512sum --check SHA512SUMS
xyz.exe: OK 
```

## macOS 上的代码签名

和 Linux 一样，macOS 也可以使用文件哈希和签名。与签署团队的 macOS 或 iOS 应用程序文件相比，这有点乏味。反过来，macOS 会自动验证它们。

确保构建应用程序所需的所有证书和配置文件都安装在本地 runner 机器上。这些证书和配置文件必须安装在为运行程序守护程序配置的同一用户下。最简单的方法是在机器上用 Xcode 构建一次项目。这允许 Xcode 创建和安装它需要的证书和描述文件。

runner launch 守护进程需要添加一个额外的选项，以便可以通过作业访问 keychains:

```
<key>SessionCreate</key>
<true/> 
```

另一个关键步骤是确保新的苹果全球开发者关系证书在 runner 机器上可用。作为跑步者用户，你可以在这里下载它

以在 CircleCI 上构建应用程序但不创建签名的`.ipa`文件的方式构建您的工作流程。相反，它应该创建一个供以后使用的`.xcarchive`文件。尝试构建`.ipa`文件将会失败，因为在云作业中没有代码签名资产。

该归档保存到运行程序作业中拉入的工作区。runner 作业获取这个归档文件，跳过再次构建项目，并通过 gym 运行它，gym 使用安装在 runner 机器上的代码签名资产将它导出为一个签名的`.ipa`文件。这将代码签名资产安全地保存在您的团队控制的机器上。

## Windows 上的代码签名

如果你安装了 Visual Studio，你可以使用微软的一个特殊的签名工具`SignTool.exe`。签署文件只需要一个有效的证书(可以是自签名的)并执行`SignTool.exe`:

```
SignTool.exe sign /a /fd SHA256 xyz.exe 
```

不像在 Linux 进程中，不会创建其他文件。相反，签名嵌入在文件本身中。

如果要对文件进行签名以进行更广泛的分发，请确保用于签名文件的证书受到广泛信任。要么购买由可信机构签名的证书，要么自己生成并分发证书。但是，除了内部使用之外，不建议使用自签名。正如我在前面的教程中所描述的，Windows 用一个提示来标记未签名的二进制文件的执行，以继续处理一个不可信的文件。如果找到有效的签名，操作系统会显示详细信息，以便用户查看。

## 使用 runner 将签名添加到 CircleCI 配置中

您知道如何对二进制文件进行签名，或者提供一种可信的方法来验证二进制文件是否正确。现在，您可以将它整合到您的 CircleCI 配置中。第一步是用证书或私有 GPG 密钥验证运行者是否正确安装在机器上。

在本例中，我们将使用 Windows Server 2019 虚拟机和 runner 通过 CircleCI 创建可执行文件。如果你愿意，你可以为其他用例修改这个过程。

以下是配置示例:

```
version: 2.1
orbs:
  msix: circleci/microsoft-msix@1.1
jobs:
  build:
  	docker:
  		- image: cimg/go:stable
  	steps:
  		- checkout
  		- run:
  			command: GOOS=windows GOARCH=amd64 go build -o your-package.exe
  			name: Build application for Windows
  		- persist_to_workspace:
  			root: .
  			paths: your-package.exe
  sign:
	shell: powershell
    machine: true
    resource_class: your-namespace/windows-vm
    steps:
      - attach_workspace:
      		at: .
      - msix/sign:
          package-name: your-package
workflows:
  demo:
    jobs:
      - build
      - sign:
      		requires: ["build"] 
```

这个工作流程的第一步是在 CircleCI 上执行密集的构建过程。保存到工作区，将二进制文件传输到 runner 机器。这种方法使您自己的基础结构很小，但是足够强大，可以控制您的签名密钥或证书。

## 结论

代码签名对于分发应用程序变得越来越重要。一些系统拒绝执行未经可信方签名的二进制文件。由于有了 runner，很容易将 CircleCI Cloud 的速度与在自己控制的机器上运行敏感工作负载的安全性结合起来。我希望这篇教程对你有所帮助，我也希望你能受到启发，带领你的团队完成更多与跑步者的代码签名的应用。