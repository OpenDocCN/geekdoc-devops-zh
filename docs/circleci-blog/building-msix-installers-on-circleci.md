# 使用 MSIX orb | CircleCI 为 Windows 构建

> 原文：<https://circleci.com/blog/building-msix-installers-on-circleci/>

MSIX orb 是 CircleCI 的第一个“Windows 专用”orb。当微软向我们提出建立一个 orb 来帮助 Windows 开发人员在我们的平台上进行开发的机会时，我们非常热情。我们的大多数 orb 和一般工作负载都围绕着 Linux，并利用 Bash。然而，我们认识到为在 Windows 上构建应用程序提供良好的 [CI/CD 解决方案的迫切需要，并且随着 PowerShell 在 Linux 中的使用稳步增长，是时候冒险了。](https://circleci.com/blog/cicd-for-dotnet/)

MSIX orb 有两种不同的方法来构建新的 MSIX/Appx 安装程序格式:使用 Visual Studio 和手动创建文件。Visual Studio 解决方案文件是 Visual Studio 中的一个项目设置，它链接到其他 Visual Studio 项目，并包含使用正确的元数据构建安装程序包[所需的配置。这是使用这个球体和技术最直接的方式。手动选项更麻烦，因为它涉及创建必要的元数据和映射文件，以正确打包预编译的二进制文件。](https://docs.microsoft.com/en-us/windows/msix/desktop/vs-package-overview)

在本教程中，我们将介绍使用 Visual Studio 解决方案文件构建包，并在最后为手动方法提供一些资源。

## 关于代码签名，您需要知道什么

代码签名允许用户和操作系统验证特定二进制代码或代码位的作者。有些东西是否被签名并不明显，所以操作系统警告未签名的二进制文件是很常见的。根据您的系统，您可能还会收到关于未签名的软件包和应用程序的警告。该警告提示您确认是否要继续安装或使用该特定软件包。

代码签名是 MSIX 和 Appx 包的必需步骤。它通常涉及一个昂贵但被广泛信任的证书或一个较小的本地创建的证书。除非明确安装，否则本地创建的证书可能不可信。对于本教程，我们将使用一个较小的本地创建的证书，原因有两个:

*   上传广泛信任的私有证书的私有部分不是一种好的做法。
*   本地创建的证书更容易获得并用于测试。

虽然有安全的方法可以做到这一点，但对私有证书最常见的建议是要么选择可信的专用服务，要么完全远离互联网。本地创建的证书是自签名的，如果它被泄露或被第三方获得，也不会成为重大事件。

您可以按照微软文档中的步骤[生成自签名证书](https://docs.microsoft.com/en-us/windows/msix/package/create-certificate-package-signing)。创建证书后，使用同一链接中的说明导出证书。我们需要将导出的证书上传到 CircleCI。

我们的下一步是将证书转换成基于文本的格式，因为环境变量是显式文本。我们可以使用 base64 编码来做到这一点。把 base64 编解码想象成脱水的食物，后期再补水。这种编码保留了文件内容，同时也将其视为文本。当我们需要原始文件时，可以对其进行解码。

使用以下 PowerShell 代码片段:

```
[Convert]::ToBase64String([IO.File]::ReadAllBytes($FileName)) 
```

用导出的证书文件的名称替换`$FileName`。这个代码片段返回一个字符串，您可以将它添加到 CircleCI 上下文中。您将在稍后的管道中使用它。把这个命名为`MSIX_SIGNING_CERTIFICATE`。

您还需要添加用于将证书导出到相同安全上下文的密码。因为这个证书仅用于测试目的，只要它没有被安装在任何其他机器上，你就不需要担心它被泄露*。把这个命名为`MSIX_CERTIFICATE_PASSWORD`。*

## 使用 MSIX 球体

MSIX orb 极大地简化了构建基于解决方案的项目。这种极简的配置应该足以让你开始:

```
# .circleci/config.yml
version: 2.1

orbs:
  # We pin the minor version but leave out the patch version
  # to automatically accept non-breaking improvements.
  msix: circleci/microsoft-msix@1.1
  # Gives us easy access to Windows execution environments.
  win: circleci/windows@2.4.0
jobs:
  using-sln:
    executor:
      name: win/default
    steps:
      - checkout
      - msix/pack

workflows:
  demo:
    jobs:
      - using-sln 
```

`msix/pack`命令获取您的`.sln`项目并构建它，自动引入您的签名证书。它将最终结果发布为 CircleCI 工件，您可以下载并手动验证。如果您需要调整行为，有几个参数可用，但我们已经确定了大多数情况下的最佳默认值。

因为这是一个命令，所以您还可以通过将生成的文件保存到工作区或者上传到您自己的工件服务器来重用它。

## 不使用 Visual Studio 构建 MSIX

可以使用通过其他方式构建的、具有 MSIX 可执行文件格式的现有可执行文件。然而，它需要手动创建元数据，你可以在微软的文档中了解更多关于[的信息。](https://docs.microsoft.com/en-us/windows/msix/desktop/desktop-to-uwp-manual-conversion)

当您使用这种方法时，在配置上有几个关键的区别:

*   您需要手动定义清单和映射文件，因为这两者都没有标准的文件名。
*   您的安装程序需要使用`msix/sign`命令手动签名。

```
version: 2.1

orbs:
  msix: circleci/microsoft-msix@1.0
  win: circleci/windows@2.4.0
jobs:
  using-sln:
    executor:
      name: win/default
    steps:
      - checkout
      - msix/pack:
          using-sln: false
          mapping-file: mapping.txt
          manifest-file: appxmanifest.xml
      - msix/sign:
          import-cert: true

workflows:
  build:
    jobs:
      - using-sln 
```

在这个代码片段中，打包一个非 Visual Studio 解决方案项目需要更多的手动操作，但是它受 orb 支持。

对于这两种方法，在 orb 源代码库中有一些可用的资源。这些资源主要用于集成测试，但是它们也展示了 orb 的功能。

## 继续使用 CircleCI 构建 Windows

我和我的工程师同事对使用 PowerShell 编写脚本的体验非常满意。对我们来说，默认情况下，它被证明比 Bash 更健壮。我们的团队对 MSIX 球体的最终结果感到满意，我们希望您和您的团队会发现它很有用。它不是一个通用的执行环境，所以我们不依赖它来运行其他 orb。然而，我们将继续投入时间，并希望建立更多的 orb，为 Windows 执行器提供更好的原生支持。我们致力于改善团队在我们的平台上构建 Windows 操作系统的体验。