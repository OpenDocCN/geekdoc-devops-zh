# 3 种通过执行构建标准来改进过程的方法

> 原文：<https://circleci.com/blog/enforce-build-standards/>

当单独开发软件时，你不必花时间向任何人传达你的决定，而且你很可能像昨天那样格式化你今天的代码。然而，当在共享代码库中工作时，创建和遵循行为标准变得更加重要。对“我们做事的方式”有一个共同的理解，通过消除不必要的来回，混乱的代码审查，以及不得不在不正确的分支上重新做 PRs，最终节省了时间。

然而，新的规则和标准可能难以采用。CircleCI 可以帮助实现过程自动化，并在违反标准时创建内置警报。请继续阅读 CircleCI 帮助您在团队中实施构建标准的三种方式。

## 1:强制代码格式

当与团队一起开发软件时，确保参与者之间的代码格式一致很重要。

不同开发人员使用的不同风格会导致更困难的代码审查，并增加以后维护软件的复杂性。

有一些文本编辑器插件，甚至 CLI 工具可以用来强制代码格式化，但是它们的成功依赖于开发者是否记得使用它们。CircleCI 可以自动执行代码格式化。为了说明这是如何工作的，没有比 Go (Golang)编程语言更好的例子了。

Go 工具链附带了一个名为`go fmt`(一个`gofmt`的包装器)的便利命令，它根据社区认可的标准格式化 Go 代码。

在 CircleCI 构建步骤中运行这个命令，加上一点 Bash，可以在代码格式不正确时故意使构建失败。

下面是一个示例`.circleci/config.yml`步骤:

```
#...
      - run:
          name: "Enforce Go Formatted Code"
          command: "! go fmt ./... 2>&1 | read"
#... 
```

出于好奇，下面是正在发生的事情的分类:

*   `go fmt`是检查代码格式并在必要时返回格式正确的代码的命令。
*   `./...`是一个特殊的语法，告诉 Go 工具检查这个包(通常是`main`)以及在那里找到的任何子包。
*   `2>&1`将错误消息重定向到标准输出。对下一步有用。
*   `| read`读取标准输出的输出。如果`go fmt`发现任何需要纠正的地方或有错误，将输出给`read`。`read`如果没有得到任何要读取的输出，将返回一个错误。
*   `!`这将翻转错误代码。`read`有输出时为 0，表示有格式化要做。我们将此反转为失败，因为这意味着开发人员没有格式化他们的代码。相反，当没有输出时，我们希望退出代码为 0；这意味着代码已经格式化并准备好了。

并非所有语言都有代码格式化工具，但幸运的是，第三方工具是存在的。

## 2:强制实施构建时间限制

对于较大的公司，构建时间是一个关键指标。

跟踪构建时间不仅有助于管理人员跟踪他们的软件管道的效率，还可以暴露构建时间中随机出现的问题。

这些问题可以通过浏览 [CircleCI Insights](https://circleci.com/docs/insights/) 或第三方仪表板来发现，但是，实际上，这样做的频率有多高呢？

相反，我们可以通过明确地导致构建在超过预先确定的时间量时失败来更直接地暴露这些问题。

例如，如果一个项目似乎有 2 分 10 秒的平均构建时间(使用 CircleCI Insights 可以很容易地确定这一点)，我们可以设置一个 5 分钟的硬限制。在两倍于我们平均构建时间的情况下，如果一个构建突然花了超过 5 分钟，我们就让它失败，以便让一个人检查这个构建，看看发生了什么。

这里有一个例子`.circleci/config.yml`片段来实现这一点。第一步应该在工作的最开始:

```
#...
      - run:
          name: "Set 5m Build Time Limit"
          command: sleep 300; touch .fail-build; exit 1
          background: true
#... 
```

而第二步应该在最后:

```
#...
      - run:
          name: "Enforce Build Time Limit"
          command: if [ -f .fail-build ]; then echo "Build ran too long."; exit 1; fi
          when: always
#... 
```

第一步，`sleep`在我们希望构建失败的时间内在后台进程中运行。在这种情况下，时间是 5 分钟，即 300 秒。如果`sleep`命令完成了，这意味着构建仍在运行，因此会创建一个文件`.fail-build`。

第二步寻找`.fail-build`文件。如果找到了，构建运行得太长了，我们故意返回`exit 1`以使构建失败。

如果没有找到，构建时间在 5 分钟以内，我们可以正常退出。

这个技巧对于 PRs 也很有用，在 PRs 中，您希望确保更改不会显著增加构建时间。

## 3:实施公关目标分支

许多项目都有代码贡献的过程。特别是在开源项目中，通常需要签署 CLA、代码格式化(参见提示 1)以及 PRs 应该基于和/或针对哪个分支的规则。后者在 CircleCI 很容易实施。

时不时地，我会遇到一个项目，在他们的`CONTRIBUTING.md`文件中声明，所有的 pr 必须针对`develop`分支，而不是`master`。下面是一个 CircleCI 配置步骤，它执行了这个规则:

```
#...
      - run:
          name: "Enforce PR Target Branch"
          command: |
            if [[ -n ${CIRCLE_PR_NUMBER} ]]; then
              url="https://api.github.com/repos/$CIRCLE_PROJECT_USERNAME/$CIRCLE_PROJECT_REPONAME/pulls/$CIRCLE_PR_NUMBER?access_token=$GITHUB_TOKEN"
              target_branch=$( curl "$url" | jq '.base.ref' | tr -d '"' )

              if [[ $target_branch != "develop" ]]; then
                echo "This PR is targeting ${target_branch} instead of 'develop'. Failing build."
                exit 1
              fi
            fi
#... 
```

在本例中，检查`$CIRCLE_PR_NUMBER`以查看是否正在构建 PR。如果这是一个 PR，那么使用`jq`命令来解析 GitHub API 并确定这个 PR 的目标分支。

然后将新创建的`$target_branch`变量与所需的分支字符串进行比较，在本例中为`develop`，以确保它们匹配。如果他们不这样做，我们的构建就失败了。

一些注意事项:

1.  将“开发”更改为您的目标分支。
2.  在这种情况下，需要创建一个 GitHub API 令牌，并将其存储为一个名为`$GITHUB_TOKEN`的私有环境变量。如果这是一个公共项目，请确保将这个 API 令牌的范围明确限定在这个项目上，并且尽可能地减少权限(仅读取权限)。
3.  这个例子使用了`jq`命令。该命令预安装在所有 [CircleCI 便利映像](https://circleci.com/docs/circleci-images/)以及所有 [CI 构建](https://hub.docker.com/cibuilds/)(第二方)Docker 映像上。如果你使用的是没有安装`jq`的镜像，他们的 [GitHub 自述文件](https://github.com/stedolan/jq)提供了说明。

这些绝不是在你的团队中标准化构建的详尽列表。在这篇文章中，我的目的是让你了解什么样的实践是可能的，以及用 CircleCI 自动化这些实践的方法。你的团队实施标准化的一些方法是什么？在[讨论](https://discuss.circleci.com)或[推特](https://twitter.com/FelicianoTech)上让我知道。