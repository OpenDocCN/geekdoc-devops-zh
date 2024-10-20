# 将 ASP.NET 核心应用程序构建并部署到 Azure | CircleCI

> 原文：<https://circleci.com/blog/deploy-dotnetcore-azure/>

> 1.  创建一个 Azure Web 应用程序和一个配置文件来构建它
> 2.  在 GitHub 和 CircleCI 上设置项目
> 3.  将项目部署到 Azure Web 应用程序

自动化新 web 应用程序的部署和特性更新的发布可以提高开发团队的生产力和效率。自动化还最大限度地减少甚至消除了重复的手动部署，手动部署会在开发过程的这一关键部分引入人为错误的风险。自动化是一个有价值的目标，但是开发人员使用的各种框架和语言会使自动化部署成为一个挑战。

微软 Azure Web Apps 是一个平台即服务 [(PaaS)](https://searchcloudcomputing.techtarget.com/definition/Platform-as-a-Service-PaaS) ，让你发布运行在多个框架上、用不同编程语言编写的 Web 应用。然后，您可以使用 CI/CD 工具来构建、测试和部署 web 应用程序，以获得更快的发布周期、更高效的开发和更高质量的代码。

在本教程中，我将向您展示如何建立一个连续的部署管道来将 ASP.NET 核心应用程序部署到 Azure Web App 服务。我们将使用 [Azure CLI orb](https://circleci.com/developer/orbs/orb/circleci/azure-cli) 进行身份验证，并直接部署我们的应用程序。

## 先决条件

您将需要这些来从本教程中获得最大收益:

> 我们的教程是平台无关的，但是使用 CircleCI 作为例子。如果你没有 CircleCI 账号，请在 注册一个免费的 [**。**](https://circleci.com/signup/)

## 克隆演示项目

您的第一步是使用 Git 克隆一个用 ASP.NET 核心构建的 API。运行以下命令:

```
git clone https://github.com/CIRCLECI-GWP/deploy-aspdotnetcore-to-azure.git 
```

接下来，转到上一个命令创建的新文件夹的根目录。运行应用程序:

```
cd deploy-aspdotnetcore-to-azure

dotnet watch run 
```

您可能会得到一个关于框架版本兼容性的错误。

![ASP.NET Core version error](img/08d1b5feba67530aa16c982863d3fe08.png)

默认情况下。NET framework 应用程序在构建它的版本上运行。演示应用程序是用 ASP.NET 核心版本`3.1`构建的。如果您运行的是不同的版本，将会出现该错误。使用以下命令找出您的计算机上安装的版本:

```
dotnet --version 
```

如果输出的版本不是版本`3.1`，您可以修复它。转到应用程序的根目录，打开`dotnet-core-sample.csproj`文件。像这样更新:

```
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp6.0</TargetFramework>
  </PropertyGroup>
</Project> 
```

将`6.0`替换为之前在您的终端上显示的版本。然后，用`dotnet watch run`命令再次运行项目。

![dotnet run watch results](img/9bb329aa6acae04c09512dd1a568cd15.png)

该应用将于`http://localhost:5000`上线。导航到这个端点`http://localhost:5000/weatherforecast`来查看 JSON 响应。

![API Response](img/cd6ed14383f478f1efb859e1bfc7d634.png)

## 审查部署策略

既然您已经在本地运行了应用程序，那么回顾一下您的部署策略是一个好主意。除了包含配置文件来设置 CircleCI 的部署之外，不需要对项目代码进行任何更改。

按照时间顺序，我们将:

1.  创建 Azure web 应用程序
2.  创建一个配置文件以在 CircleCI 上构建应用程序
3.  将项目推送到 GitHub 上的存储库中
4.  在 CircleCI 建立项目
5.  提供 Azure 凭据并使用 Azure CLI orb 进行身份验证
6.  使用 Git 设置部署 URL
7.  将我们的项目部署到 Azure Web App

## 创建 Azure web 应用程序

如果您还没有帐户，[在 Azure 上创建一个帐户](https://azure.microsoft.com/en-us/free/)。然后转到你的 Azure 门户仪表板，点击**创建资源**来创建一个新的服务实例。

![Click create resources](img/dbd8d84924e79347bb6dc4f2df9e886c.png)

接下来，从热门列表中点击 **Web 应用**。

![Select Web App](img/577e0cc7c33fde2f3c4cd392afc2eaee.png)

您将被重定向到创建 Web 应用程序页面。选择 Azure 订阅和资源组。如果尚未创建新的资源组，请创建一个。

![Save Web App](img/bc2631e736a0b427507ea60ad1a16e48.png)

现在，为您的 web 应用程序输入一个友好的、唯一的名称。对于本教程，我使用了`dotnet-core-sample`。如上图所示，更新其他字段。

点击**审核+创建**，然后**创建**。Azure 正在设置 web 应用环境，请稍候。

![Deployment In Progress](img/d3e5f6e15efc2dca57467a6564ed5773.png)

一旦流程完成，您将拥有一个 web 应用程序来托管您的 ASP.NET 核心应用程序。

![Application in App Services](img/5b9d83ea5990245e71ecb3e8b316c348.png)

## 添加 CircleCI 配置文件

在这一步，我们将使用 [Windows orb](https://circleci.com/developer/orbs/orb/circleci/windows) 和 [Azure CLI orb](https://circleci.com/developer/orbs/orb/circleci/azure-cli) 为 CircleCI 创建管道配置。

首先，在项目的根目录下创建一个名为`.circleci`的文件夹。在新文件夹中，创建一个名为`config.yml`的文件。在新文件中，添加:

```
version: 2.1
orbs:
  azure-cli: circleci/azure-cli@1.2.0
  windows: circleci/windows@2.2.0
jobs:
  build:
    description: Build application with Release configuration
    executor:
      name: windows/default
    steps:
      - checkout
      - restore_cache:
          keys:
            - dotnet-packages-v1-{{ checksum "dotnet-core-sample.csproj" }}
      - run:
          name: "Install project dependencies"
          command: dotnet.exe restore
      - run:
          name: "Build Application according to some given configuration"
          command: dotnet.exe build --configuration Release
      - run:
          name: "Publish to a subfolder"
          command: |
            dotnet.exe publish --configuration Release --output WebAPIFolder
      - persist_to_workspace:
          root: ~/project
          paths:
            - WebAPIFolder
  login-to-azure-and-deploy:
    executor: azure-cli/azure-docker
    steps:
      - azure-cli/install
      - azure-cli/login-with-user:
          alternate-tenant: false
      - attach_workspace:
          at: ~/project
      - run:
          command: az webapp deployment source show --resource-group sample-dotnet-core --name sample-dotnet-core-application
      - run:
          name: "Deploy Web App to Azure"
          command: |
            az webapp deployment source config-local-git --resource-group sample-dotnet-core --name sample-dotnet-core-application
workflows:
  build-and-deploy:
    jobs:
      - build
      - login-to-azure-and-deploy:
          requires:
            - build 
```

这种配置可能看起来有点复杂，所以我将尝试为您分解它。

首先，它定义并引入我们在这个项目中使用的 orb:

*   `circleci/azure-cli@1.2.0` orb 用于安装、初始化和登录 Azure 命令行界面。Azure CLI 让您可以访问一组命令来创建和管理 Azure 资源。
*   orb 为您提供了构建 Windows 项目的工具，例如。Net 框架。

配置的下一部分指定了两个不同的作业:

*   `build`
*   `login-to-azure-and-deploy`

`build`作业使用`windows/default`执行器，它从 GitHub 中检查我们的项目，安装项目的依赖项，并将应用程序发布到一个名为`WebAPIFolder`的新的子文件夹中。这个作业的另一个步骤是`persist_to_workspace`，它使用[工作区特性](/blog/deep-diving-into-circleci-workspaces/)将数据从工作流中的特定作业移动到可能需要相同数据的后续作业。

在这种情况下，我们希望包含项目`WebAPIFolder`的已发布版本的文件夹在下一个作业中可用，而不需要重新构建项目。

`login-to-azure-and-deploy`作业使用`azure-cli/default`来安装 Azure CLI，并提供认证 Azure 用户的步骤。然后，它会附加先前存储的文件夹。然后，该作业使用 Azure CLI 来配置将与我们的 Azure web 应用程序连接的 Git URL。

工作流的最后一部分指定在`build`任务完成之前`login-to-azure-and-deploy`不应该运行。

## 将应用程序连接到 CircleCI

下一步是在 GitHub 上建立一个资源库，并将项目链接到 CircleCI。查看[将项目推送到 GitHub](/blog/pushing-a-project-to-github/) 以获取指示。

登录您的 CircleCI 帐户。如果你注册了你的 GitHub 账户，你所有的库都可以在你项目的仪表盘上看到。

点击`dotnet-core-sample`项目旁边的**设置项目**。

用您的资源组替换资源组。

![Set up project](img/c091e8850c43c90a5e67c00b90a2eff2.png)

系统将提示您编写新的配置文件，或者在项目中使用现有的配置文件。选择现有的分支，并输入您的代码在 GitHub 上所在的分支的名称。点击**走吧**。

![Select Config](img/3f442cc7d8abf5179bc8611f39b4d8cd.png)

您的第一个工作流将开始运行:

*   工作会过去的
*   `login-to-azure-and-deploy`将会失败

![The complete steps for the build job](img/496c1585601bba82adffa6c3c33f5a28.png)

因为我们还没有指定 Azure 凭证，所以`login-to-azure-and-deploy`作业将会失败。

![Login build job](img/0551c7ef3687acc8795bab6eeddf61c5.png)

要解决这个问题，创建一个 Azure 服务主体并添加输出作为环境变量。点击**项目设置**。

![Project Settings](img/e20c6697c0fac892406ef400953575b5.png)

点击左侧工具条上的**环境变量**按钮，创建这些变量:

*   `AZURE_SP_TENANT`是存储服务主体的租户 ID 的环境变量的名称。
*   `AZURE_SP`是存储服务主体全名的环境变量的名称。
*   `AZURE_SP_PASSWORD`是存储服务主体密码的环境变量的名称。

![AZ CLI create service principal](img/5b20974f5c539f07c32df92bdfc547fe.png)

了解如何使用 Azure CLI 创建 Azure 服务主体。

回到仪表板。点击**从失败的**重新运行工作流程。

![Rerun job](img/628d229611a9e1335cd494f7eb2e5b02.png)

当构建完成运行时，您可以查看输出。

![Remote URL](img/bd19bff5bca1638657c7b1b708bf2fd8.png)

这是为您自动生成的本地 URL，用于部署到您的 Azure web 应用。网址的格式是`https://<username>:<password>@sample-dotnet-core-application.scm.azurewebsites.net/sample-dotnet-core-application.git`。

下一步是创建您的部署凭证并更新配置文件。请注意，部署凭据与 Azure 身份验证凭据不同。

## 更新配置文件

在这一步中，您将更新配置文件，并使用 Git 部署到生成的 URL。首先，返回门户为部署创建一个新的凭证，这样 Git 就不会提示输入密码。

从侧面菜单栏中，点击**部署中心**，然后点击**本地 Git / FTP 凭证**。滚动到用户范围部分。

![Set deploy credential](img/d4a9bbf66df35e4328b8e558d5d79c8e.png)

输入用户名或使用默认用户名。输入密码并再次输入以确认。

点击顶部的**保存**按钮。

将`.circleci/config.yml`的内容替换为:

```
version: 2.1
orbs:
  azure-cli: circleci/azure-cli@1.2.2
  windows: circleci/windows@2.2.0
jobs:
  build:
    description: Build application with Release configuration
    executor:
      name: windows/default
    steps:
      - checkout
      - restore_cache:
          keys:
            - dotnet-packages-v1-{{ checksum "dotnet-core-sample.csproj" }}
      - run:
          name: "Install project dependencies"
          command: dotnet.exe restore
      - run:
          name: "Build Application according to some given configuration"
          command: dotnet.exe build --configuration Release
      - run:
          name: "Publish to a subfolder"
          command: |
            dotnet.exe publish --configuration Release --output WebAPIFolder
      - persist_to_workspace:
          root: ~/project
          paths:
            - WebAPIFolder
  login-to-azure-and-deploy:
    executor: azure-cli/azure-docker
    steps:
      - azure-cli/install
      - azure-cli/login-with-user:
          alternate-tenant: false
      - azure-cli/login-with-service-principal
      - attach_workspace:
          at: ~/project
      - run:
           command: az webapp deployment source show --resource-group sample-dotnet-core --name sample-dotnet-core-application
    # - run:
      #     name: "Deploy Web App to Azure"
      #     command: |
      #       az webapp deployment source config-local-git --resource-group sample-dotnet-core --name sample-dotnet-core-application

      - run:
          name: "Deploy Web App to Azure"
          command: |
            az webapp deployment source show --resource-group sample-dotnet-core --name sample-dotnet-core-application
            cd ~/project/WebAPIFolder/
            git init
            git config --global user.email "motukadominic@gmail.com"
            git config --global user.name "dominicmotuka"
            git add .
            git commit -m "Deploy Azure Web Application"
            git remote add azure https://$GIT_USERNAME:$GIT_PASSWORD@sample-dotnet-core-application.scm.azurewebsites.net/sample-dotnet-core-application.git
            git push azure master --force
workflows:
  build-and-deploy:
    jobs:
      - build
      - login-to-azure-and-deploy:
          requires:
            - build 
```

对配置文件的更新包括 Git，设置一个远程 URL 来引用我们之前生成的链接，并推送到 Azure。

用您之前在 Azure 中为本地 Git 创建的凭证替换`GIT_USERNAME`和`GIT_PASSWORD`占位符。

**注意:** *如果您在密码中使用了任何特殊字符(如`@`或`$`)，将会出现错误。为了防止这种错误，请使用与其等效的 [HTML 编码引用](https://www.w3schools.com/tags/ref_urlencode.ASP)。*

快好了！再一次把你的代码推给 GitHub。

![It is a successful build!](img/5ed3ec7684e0e82656dfb694500e4394.png)

## 测试应用程序

在`https://dotnet-core-sample.azurewebsites.net/weatherforecast`访问您的应用程序。您的应用程序已部署。

![Your successfully deployed application](img/9828ef648ca28135f4287001b4e07062.png)

## 结论

您和您的团队所使用的越来越多的框架和语言可能会阻止您迁移到完全自动化的部署过程。在本教程中，我们展示了您可以使用 Windows 和 Azure orbs 设置功能部署管道。该管道成功地将 ASP.NET 核心应用程序部署到 Azure Web Apps 托管平台。

与您的开发团队分享您的工作成果，开始关于用自动化取代手动部署的对话。即使部署一个复杂的应用程序也是可以完成的，更低的人为错误风险、更高的效率和更高质量的代码都是值得投资的。

我希望这篇教程对你有所帮助。完整的源代码可以在 GitHub 上找到。

* * *

Oluyemi 是一名拥有电信工程背景的技术爱好者。出于对解决用户日常遇到的问题的浓厚兴趣，他冒险进入编程领域，并从那时起将他的问题解决技能用于构建 web 和移动软件。Oluyemi 是一名热衷于分享知识的全栈软件工程师，他在世界各地的几个博客上发表了大量技术文章和博客文章。作为技术专家，他的爱好包括尝试新的编程语言和框架。

[阅读更多 Olususi Oluyemi 的帖子](/blog/author/olususi-oluyemi/)