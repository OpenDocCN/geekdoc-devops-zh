# 如何使用 Husky 和 Git 挂钩来自动化操作

> 原文：<https://www.gitkraken.com/blog/husky-git-hooks>

Git 挂钩是一个适合的解决方案，适用于寻求在团队中实现一致的策略和过程的组织。首先，它们使团队能够根据特定的 Git 操作运行定制脚本。将自动化构建到您的 [Git 客户端](https://www.gitkraken.com/git-client)中有助于消除人为错误的可能性，并且不费吹灰之力就能改进工作流程。

这里我们将探索 Husky 如何在 JS 项目中使用 Git 钩子。首先，让我们从基础开始:

什么是 Git 挂钩？

## [Git 挂钩](https://www.gitkraken.com/learn/git/tutorials/how-to-create-git-hooks)是开发人员[可以设置的脚本，当 Git 生命周期中发生某些事件时运行](https://git-scm.com/docs/githooks)。有一些流行的方法可以利用 Git 挂钩来通过自动化来简化工作流。

对于开发人员来说，在自动化的帮助下[运行定制代码任务的脚本](https://www.gitkraken.com/blog/shell-scripting)并执行标准可能是有用的，因为它简化了操作。提交的某些阶段，比如之前和之后，是开始使用 Git 挂钩的好方法。

对于开发人员来说，在自动化的帮助下[运行定制代码任务的脚本](https://www.gitkraken.com/blog/shell-scripting)并执行标准可能是有用的，因为它简化了操作。提交的某些阶段，比如之前和之后，是开始使用 Git 挂钩的好方法。

GitKraken 客户端支持许多流行的 Git 挂钩，包括提交、检验、合并、重置等挂钩。

什么是哈士奇？

许多使用 Git 挂钩的开发人员也使用 Husky 来帮助管理脚本。Husky 是一个工具，[帮助开发人员更有效地使用 Git 挂钩工作](https://www.npmjs.com/package/husky)，并运行各个阶段需要工作的所有脚本。

## 通过简化设置 Git 挂钩的过程，开发人员[可以更快地创建有效的解决方案](https://www.waveapps.com/freelancing/back-end-development-resources)。Husky 在 package.json 文件中工作，包括一个对象，该对象配置 Husky 运行某些脚本，然后 Husky 在 Git 生命周期的特定点管理脚本。

安装 Husky

如果您想使用 Git 挂钩以这种方式优化您的项目，您需要从安装 Husky 开始。您可以通过输入以下命令来完成此操作:

`$ npm install husky --save-dev`

## 在 Husky 完成安装后，运行一个允许启用 Git 挂钩的命令:

`$ npx husky install`

接下来，您将需要调整 package.json 文件。最后一个命令在安装过程结束后运行:

`// package.json`

`{`

`  "scripts": {`

`    "prepare": "husky install"`

`  }`

`}`

如果您在 GUI 中收到错误响应，您必须添加并设置环境路径。

根据您选择的环境，位置会有所不同。在这种情况下，我们将使用 asdf-nodejs，因此我们的 environment.plist 文件将如下所示:

`<?xml version="1.0" encoding="UTF-8"?>`

`<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">`

`<plist version="1.0">`

`  <dict>`

`    <key>PATH</key>`

`<string>/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin:/bin:/sbin:/Users/brunobrito/.asdf/shims</string>`

` </dict>`

`</plist>`

现在，是时候保存文件了。为此，重启 Git 客户端，并返回到命令行来创建一个钩子。

创建 Git 挂钩

虽然 Git 挂钩可以用来创建许多自动化，但是有几个挂钩在开发人员中一直很受欢迎。让我们进一步了解如何验证分支名称、林挺提交消息、林挺代码、压缩分段映像，以及使用 Husky 和 Git 挂钩运行测试。

现在，是时候保存文件了。为此，重启 Git 客户端，并返回到命令行来创建一个钩子。

用 Git 挂钩验证分支名称

## 这对于确保每个分支[遵循特定的模式](https://www.gitkraken.com/blog/trunk-based-development)尤其方便。Validate-branch-name 是一个 npm 包。如果您了解 regex，这个包带有可以定制的预设默认值。

使用以下命令开始安装此软件包:

`$ npm install validate-branch-name --save-dev`

### 接下来，您可以利用预提交挂钩将它添加到 Husky:

`$ npx husky add .husky/pre-commit "npx validate-branch-name"`

使用以下命令开始安装此软件包:

用 Git 钩子压缩图像

一些框架比其他框架更好地呈现图像，并且每次都很难记住运行 ImageOptim。幸运的是，开发人员可以使用一个脚本来自动压缩您添加到项目中的图像。此示例将用于 ImageOptim-CLI。

您将使用以下命令开始安装过程:

`$ npm install -g imageoptim-cli`

### 接下来，将 imageoptim 命令添加到 lint-stage，因为您只想压缩暂存文件。

`// package.json`

`{`

`  "scripts": {`

`    "prepare": "husky install"`

`  },`

`  "lint-staged": {`

`    "**/*.js": "prettier --write --ignore-unknown",`

`    "**/*": "imageoptim"`

`  },`

`  ...`

`}`

带 Git 挂钩的林挺代码

要用 Git 挂钩链接代码，开发人员需要使用两个节点包:lint-staged 和 appearlier。

Lint-staged 非常适合跨暂存文件运行检查。后来，漂亮的负责林挺代码。

首先使用以下命令安装这两个软件包:

`$ npm install lint-staged prettier --save-dev`

### `$ npx husky add .husky/pre-commit "npx lint-staged"`

接下来，在 package.json 文件中设置 lint-staged:

`// package.json`

`{`

`  "scripts": {`

`    "prepare": "husky install"`

`  },`

`  "lint-staged": {`

`    "**/*.js": "prettier --write --ignore-unknown"`

`  },`

`  ...`

`}`

用 Git 挂钩运行测试

您可以使用 Husky 和 Git 挂钩添加的另一个简单的自动化功能是运行测试，这会让您的生活更加轻松。对于这个例子，我们将安装 Jest，这是一个经常被[质量控制工程师](https://www.gitkraken.com/blog/quality-assurance-engineer)用于运行测试的工具:

`$ npm install --save-dev jest`

然后，向 package.json 文件添加一个测试脚本:

`// package.json`

`{`

### `  "scripts": {`

`    "prepare": "husky install",`

`    "test": "jest"`

`  },`

`  "lint-staged": {`

`    "**/*.js": "prettier --write --ignore-unknown",`

`    "**/*": "imageoptim"`

`  },`

`  ...`

要使用 Git 挂钩完成自动化设置，您必须使用以下命令将 npm 测试脚本添加到预提交挂钩:

`$ npx husky add .husky/pre-commit "npm test"`

跳过 Git 挂钩

自动化对于 Git 开发人员来说非常重要，原因有几个。首先，内置的自动化功能有助于保护您的环境免受入侵。自动化使团队能够轻松地跟踪提交，并确保在存储库中只进行允许的更改，防止恶意代码被注入到项目中。

虽然开发人员不太可能跳过 Git 钩子，但是当代码需要一点人类的 TLC 时，跳过钩子是绕过这些自动化的一种方式。

您可以使用以下命令绕过 Git 挂钩:

`$ git commit -m "skipping hooks" --no-verify`

现在，您可以手动执行任务，而无需脚本自动运行。

## Git Hooks 和 Husky 入门

开始使用 Husky 和 Git 挂钩是一个简单的过程，可以节省开发人员不必要的击键和大量时间。

Git 挂钩可以被配置为创建许多自动化功能，以改进工作流并在开发人员之间强制执行代码标准。这是在使用 Git 时，开始使用 Git 钩子和 Husky 自动化常见任务的五种方法。

您可以使用以下命令绕过 Git 挂钩:

您可以使用 GitKraken 客户端通过内置的终端开始使用 Husky 和 Git 挂钩。今天就自己试试吧。

现在，您可以手动执行任务，而无需脚本自动运行。

## Git Hooks 和 Husky 入门

开始使用 Husky 和 Git 挂钩是一个简单的过程，可以节省开发人员不必要的击键和大量时间。

Git 挂钩可以被配置为创建许多自动化功能，以改进工作流并在开发人员之间强制执行代码标准。这是在使用 Git 时，开始使用 Git 钩子和 Husky 自动化常见任务的五种方法。

您可以使用 GitKraken 客户端通过内置的终端开始使用 Husky 和 Git 挂钩。今天就自己试试吧。