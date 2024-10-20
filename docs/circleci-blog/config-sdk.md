# CircleCI 配置 SDK | CircleCI 简介

> 原文：<https://circleci.com/blog/config-sdk/>

我们很高兴地宣布，新的 CircleCI Config SDK 现已作为开源类型脚本库提供。开发人员现在可以使用 TypeScript 和 JavaScript 编写和管理他们的 CircleCI `config.yml`文件。

对于习惯了成熟编程语言的生态系统和灵活性的开发人员来说，YAML 有时会感到受限或令人生畏。使用 Config SDK，您可以从类型安全和带注释的 JavaScript 定义和生成您的 YAML 配置。您甚至可以利用包管理来模块化配置代码的任何部分，以便重用。

当与 CircleCI 的[动态配置](https://circleci.com/docs/dynamic-config/)配合使用时，Config SDK 可以在运行时动态构建您的 CI 配置，允许您根据任意数量的因素选择要执行的内容，例如 Git repo 的状态、外部 API 或只是一周中的某一天。

## 入门指南

对于我们的例子，假设我们管理几个 Node.js 项目，它们都是使用相同的框架构建的，并且通常需要相同的 CI 配置。因此，我们决定构建一个所有项目都将使用的配置“模板”,并且可以集中管理和更新。我们将创建并发布一个 NPM 包，它将为我们所有的节点项目生成完美的配置文件。

 </blog/media/2022-09-19-config-sdk.mp4> 

让我们构建配置模板包，然后构建将使用它的管道。

### 设置

我们将从创建一个标准的 NPM 包开始。您可以使用 TypeScript 或 JavaScript，但是为了加快速度，我们将在本例中使用 JavaScript。此处显示的示例基于回购 wiki 中的[这一页。](https://github.com/CircleCI-Public/circleci-config-sdk-ts/wiki/Write-Package)

首先在一个新目录中初始化一个 JavaScript 项目。

```
mkdir <your-package-name>

cd <your-package-name>

npm init -y

npm i --save @circleci/circleci-config-sdk 
```

`@circleci/circleci-config-sdk`包将允许我们用 JavaScript 定义一个 CircleCI 配置文件。虽然我们可以简单地定义一个配置并导出它，但是我们也可以利用动态配置并导出一个函数。在我们的示例中，我们将保持简单，创建一个配置生成函数，它将为我们的部署采用一个`tag`参数，并采用一个`path`参数来选择配置文件将被导出到的位置。

## 创建应用程序

创建一个`index.js`文件并导入 CircleCI 配置 SDK 包和节点的`fs`包，这样我们就可以将配置写到一个文件中。

```
const CircleCI = require("@circleci/circleci-config-sdk");
const fs = require('fs'); 
```

接下来，我们将开始使用 Config SDK 构建配置文件的组件。您会注意到，因为我们正在使用基于类型脚本的库，所以我们能够接收代码提示、类型定义、文档和自动完成。

### 创建执行者

假设我们正在为 Node.js 项目构建一个配置，我们将从定义我们的作业将使用的 [Docker 执行器](https://circleci.com/docs/using-docker/)开始。您可以传入 Docker 映像、资源类和您想要配置的任何其他参数。

```
// Node executor
const dockerNode = new CircleCI.executors.DockerExecutor(
  "cimg/node:lts"
); 
```

### 创造就业机会

我们正在构建一个工作流，它将在每次提交时测试我们的应用程序，并在我们提供某个标签时部署它。像我们的执行器一样，我们将定义这两个作业，都使用我们刚刚定义的执行器，并且每个都有一组用于各自目的的独特步骤。

```
// Test Job
const testJob = new CircleCI.Job("test", dockerNode);
testJob.addStep(new CircleCI.commands.Checkout());
testJob.addStep(new CircleCI.commands.Run({ command: "npm install && npm run test" }));

//Deploy Job
const deployJob = new CircleCI.Job("deploy", dockerNode);
deployJob.addStep(new CircleCI.commands.Checkout());
deployJob.addStep(new CircleCI.commands.Run({ command: "npm run deploy" })); 
```

可以使用步骤实例化作业，或者将作业动态添加到现有作业中，如上所示。在这个过于简化的例子中，我们缺少了一个[缓存](https://circleci.com/docs/caching/)步骤，但是您可以看到我们是如何构建配置元素的。

## 创建工作流

定义了我们的工作之后，是时候在工作流中实现它们，并定义它们应该如何运行了。我们之前提到过，我们希望`test`作业在所有提交时运行，而`deploy`作业只在给定的标签上运行。

现在我们正在使用配置文件的顶级组件，让我们最后定义一个新的 CircleCI 配置对象，并命名我们将向其中添加作业的工作流。

```
//Instantiate Config and Workflow
const nodeConfig = new CircleCI.Config();
const nodeWorkflow = new CircleCI.Workflow("node-test-deploy");
nodeConfig.addWorkflow(nodeWorkflow); 
```

我们没有向测试作业添加任何参数，因为我们希望在所有提交时运行它，所以我们可以直接将它添加到我们的 config 对象中。

```
nodeWorkflow.addJob(testJob); 
```

对于部署作业，我们需要首先定义工作流作业，以便我们可以向它添加过滤器。我们现在要添加一个过滤器，告诉 CircleCI 忽略所有分支的这个作业，这样它就不会在每次提交时都执行。我们一会儿将处理为标签启用它。

```
const wfDeployJob = new CircleCI.workflow.WorkflowJob(deployJob, {requires: ["test"], filters: {branches: {ignore: ".*"}}});
nodeWorkflow.jobs.push(wfDeployJob); 
```

## 导出配置生成器功能

现在我们已经定义好了所有的东西，我们准备好创建和导出最终的部分。我们将创建一个函数，它接受我们前面提到的标签和路径参数，并将我们定义的内容写入一个新文件。

```
/**
* Exports a CircleCI config for a node project
*/
export default function writeNodeConfig(deployTag, configPath) {
 // next step
} 
```

在新创建的`writeNodeConfig`函数中，我们将把这里传递的标记过滤器添加到我们工作流中的部署作业中，最后使用 config 对象上的`generate`函数将配置写到由`path`参数提供的文件中。

```
/**
* Exports a CircleCI config for a node project
*/
export default function writeNodeConfig(deployTag, configPath) {
  wfDeployJob.parameters.filters.tags = {only: deployTag}
  fs.writeFile(configPath, nodeConfig.generate(), (err) => {
    if (err) {
      console.error(err);
      return
    }
  })
} 
```

这里是完整的源代码，你也可以在 wiki 中找到[:](https://github.com/CircleCI-Public/circleci-config-sdk-ts/wiki/Write-Package)

```
const CircleCI = require("@circleci/circleci-config-sdk");
const fs = require('fs');

// Node executor
const dockerNode = new CircleCI.executors.DockerExecutor(
  "cimg/node:lts"
);

// Test Job
const testJob = new CircleCI.Job("test", dockerNode);
testJob.addStep(new CircleCI.commands.Checkout());
testJob.addStep(new CircleCI.commands.Run({ command: "npm install && npm run test" }));

//Deploy Job
const deployJob = new CircleCI.Job("deploy", dockerNode);
deployJob.addStep(new CircleCI.commands.Checkout());
deployJob.addStep(new CircleCI.commands.Run({ command: "npm run deploy" }));

//Instantiate Config and Workflow
const nodeConfig = new CircleCI.Config();
const nodeWorkflow = new CircleCI.Workflow("node-test-deploy");
nodeConfig.addWorkflow(nodeWorkflow);

//Add Jobs. Add filters to deploy job
nodeWorkflow.addJob(testJob);
const wfDeployJob = new CircleCI.workflow.WorkflowJob(deployJob, {requires: ["test"], filters: {branches: {ignore: ".*"}}});
nodeWorkflow.jobs.push(wfDeployJob);

/**
* Exports a CircleCI config for a node project
*/
export default function writeNodeConfig(deployTag, configPath) {
  wfDeployJob.parameters.filters.tags = {only: deployTag};
  fs.writeFile(configPath, nodeConfig.generate(), (err) => {
    if (err) {
      console.error(err);
      return
    }
  });
} 
```

## 发布包

随着您的`index.js`文件的完成和`writeNodeConfig`函数的导出，是时候[将包](https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages)发布到您选择的包存储库，比如 NPM 或 GitHub。

完成后，您应该能够将您的包导入到其他项目中，就像我们之前导入`@circleci/circleci-config-sdk`一样。

## 创建 CI 渠道

您现在有了一个可以生成 CircleCI 配置文件的已发布的 NPM 包。我们可以使用 CircleCI 的动态配置在运行时拉入这个包，并动态运行我们生成的配置文件。我们可以在许多类似的 NodeJS 项目中复制这个基本模板，当我们想要更新或更改我们的配置时，我们将能够简单地更新我们创建的包。

### 创建 config.yml

像往常一样，我们的 CircleCI 项目需要一个`.circleci`目录和一个`config.yml`文件。在这种情况下，配置文件将是我们所有项目中使用的基本模板，它只是告诉 CircleCI 为当前管道启用动态配置特性，生成新的配置文件，并运行它。我们还将创建一个`dynamic`目录，供以后使用。

```
└── .circleci/
    ├── dynamic/
    └── config.yml` \ 
```

使用下面的[示例配置文件](https://github.com/CircleCI-Public/circleci-config-sdk-ts/blob/main/sample/01-dynamic-workflow-javascript/.circleci/config.yml):

```
version: 2.1
orbs:
  continuation: circleci/continuation@0.3.1
  node: circleci/node@5.0.2
setup: true
jobs:
  generate-config:
    executor: node/default
    steps:
      - checkout
      - node/install-packages:
          app-dir: .circleci/dynamic
      - run:
          name: Generate config
          command: node .circleci/dynamic/index.js
      - continuation/continue:
          configuration_path: ./dynamicConfig.yml
workflows:
  dynamic-workflow:
    jobs:
      - generate-config 
```

您可以在 GitHub 上的 [Wiki](https://github.com/CircleCI-Public/circleci-config-sdk-ts/wiki/Write-Dynamic-Config-App) 中找到这个配置文件和其他示例。

这个配置是一个样板文件，负责执行我们的包，其中包含我们想要执行的“真正的”逻辑。

请注意，`setup`键被设置为`true`，启用了流水线的动态配置。使用 Node orb，我们安装一个位于`.circleci/dynamic`的节点应用程序(我们将回到这一点)，并运行 Node 中的`.circleci/dynamic/index.js`。这将使用我们之前写的包并在`./dynamicConfig.yml`创建一个新的配置文件，最终由`continuation` orb 执行。

我们将在所有的节点项目中使用这样的配置，它不太可能需要经常更新或更改。我们没有修改这个配置，而是更新了我们之前创建的包。

### 创建配置应用程序

最后要做的是构建我们的“配置应用程序”。这是负责实现我们的包的应用程序，并作为我们计划执行的“真实”配置的来源。因为在这个例子中，我们将配置的大部分逻辑外包给了一个外部包，所以在这个例子中，我们的配置应用程序大部分也是样板文件。

将目录更改为`.circleci/dynamic`，我们将在这里设置我们的应用程序。初始化一个新的存储库并安装您之前发布的包。

```
npm init -y

npm i <your-package-name> 
```

运行这两个命令后，您应该有一个显示您的包的依赖关系的`package.json`文件。

```
"dependencies": {
  "my-circleci-node-config": "^1.0.0",
} 
```

您可以[修改语义版本字符串](https://docs.npmjs.com/about-semantic-versioning#using-semantic-versioning-to-specify-update-types-your-package-can-accept)来指定在运行时您的包的版本。这意味着，您可以更新您的`my-circleci-node-config`包，并且，如果您愿意，让所有使用这个包的 CircleCI 项目在下次您的 CI 管道被触发时立即获得这些更改。

**不推荐:**假设您一直想获取您创建的自定义依赖项的最新版本，您可以使用:

```
"dependencies": {
  "my-circleci-node-config": "x",
}, 
```

为了安全起见，只引入次要的补丁更新，而不是主要版本。

```
"dependencies": {
  "my-circleci-node-config": "1.x",
}, 
```

最后，在`.circleci/dynamic.index.js.`中使用您的定制包

我们的包导出了一个名为`writeNodeConfig`的函数，它接受我们想要触发部署的`tag`的值，以及我们想要将配置导出到的`path`。我们知道之前在 config.yml 中的路径，我们设置为`./dynamicConfig.yml`，因为我们在`dynamic`目录中，我们将前置`..`。对于标签，我们将使用通用的正则表达式字符串`v.*`

```
import writeNodeConfig from '<your/package>';

writeNodeConfig("v.*", "../dynamicConfig.yml") 
```

这是我们的整个应用程序。我们只需要取出我们想要的包的任何版本，并调用它从我们在包中创建的模板生成我们的配置文件。

## 运行管道

概括地说，我们有一个样板文件`config.yml`指示 CircleCI 启用动态配置，并使用 Node.js 在运行时构建一个新的配置文件。负责构建我们的新配置的节点应用程序使用了一个包依赖项，它包含了我们想要的配置的“模板”。我们现在可以在许多不同的项目中使用这个包，并集中更新它。我们的项目既可以通过在 package.json 中指定一个`x`来获取这个包的最新版本，也可以使用像 dependabot 这样的工具，在这个包更新时，自动向所有使用这个包的项目打开一个获取请求。

有了动态配置和 Config SDK，可能性是无限的。上面的教程是基于 GitHub 上 wiki 的[这一页。查看我们的](https://github.com/CircleCI-Public/circleci-config-sdk-ts/wiki/Write-Package) [wiki](https://github.com/CircleCI-Public/circleci-config-sdk-ts/wiki) 和[文档](https://circleci-public.github.io/circleci-config-sdk-ts/)的剩余部分，获得更多关于 Config SDK 的有趣例子。

我们希望听到您的意见，并了解您如何在自己的管道中利用 Config SDK。在 [Twitter](https://twitter.com/CircleCI) 上与我们联系，在我们的[论坛](https://discuss.circleci.com/t/circleci-config-sdk-now-g-a/45457)上问好，或者在 [Discord](https://discord.gg/5dUX4FstW2) 上与我们聊天。