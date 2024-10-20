# 使用 Codecov CircleCI orb 轻松覆盖代码

> 原文：<https://circleci.com/blog/making-code-coverage-easy-to-see-with-the-codecov-orb/>

我们在 [Codecov](https://codecov.io/) 的目标是提供相关的、及时的数据来帮助开发人员和团队理解并提高他们代码的质量。我们通过提供高度集成的工具来分组、合并、归档和比较覆盖率报告来做到这一点。使用 Codecov，工程团队可以创建良好的测试文化，同时看到每个新提交对整体覆盖度量的影响。

Codecov 最初是一个项目，旨在解决一个开发人员无法快速、轻松地看到正在测试的代码的个人挫折。从那时起，它已经发展成为一种工具，从独立开发人员到开源项目，从小机构到大型企业，每个人都可以使用它。无论项目或团队规模如何，每个人都可以从更好地理解他们的代码质量中受益，我们相信代码覆盖率是这种理解的重要部分。

## 我们的球体是做什么的？

将 Codecov 与 CI/CD 提供者集成在一起，可以确保代码覆盖率成为管理和部署软件的基础。它将代码覆盖从单个开发人员可以在他们的终端上运行的活动，扩展到团队级别的共享活动，该活动可以由团队中的每个开发人员发起、查看和评论。团队还可以依靠代码覆盖率来对每个 PR 执行检查，确保没有 PR 会被合并到您的主分支中，除非覆盖率需求得到满足。

在将 Codecov 集成到 CI/CD 工作流中之后，团队可以依赖代码覆盖率作为工具来帮助提高他们的测试覆盖率和整体代码库的质量。Codecov 的特性提供了对一个项目的测试覆盖和可操作路径的深入了解(拉请求注释，检查，等等)。)来提高代码质量。此外，当团队将 Codecov 合并到他们的 CI/CD 管道中时，很自然地会看到代码覆盖率随着时间的推移而提高。

Codecov 的主要目的是获取您的覆盖率报告，并提供易于查看、易于理解的指标。CircleCI 用户现在有了 [Codecov orb](https://github.com/codecov/codecov-circleci-orb) ,这使得将覆盖报告上传到 Codecov 变得简单，从而让您更快地获得您的指标。

## 向 CircleCI 工作流添加您的覆盖率报告的上传

使用 Codecov orb 就像在 CircleCI 配置文件(`.circleci/config.yml`)中添加几行代码一样简单。

下面是一个将代码覆盖率报告上传到 Codecov 的例子:

```
version: 2.1
orbs:
  codecov: codecov/codecov@1.0.2
jobs:
  build:
    steps:
      - codecov/upload:
          file: {{ coverage_report_filepath }} 
```

请注意，如果您有多个文件，您将需要为每个文件创建一个步骤。您还可以指定以下其他可选参数:

*   **conf** -用于指定`.codecov.yml`配置文件的位置
*   **flags** -标记上传到组覆盖度量(例如，单元测试、集成、ui、chrome)
*   **令牌**——设置私有存储库令牌(默认为环境变量`$CODECOV_TOKEN`)
*   **上传名称** -自定义上传的名称

## 包扎

我们非常高兴能够与 CircleCI 的[合作伙伴](https://circleci.com/blog/announcing-orbs-technology-partner-program/),并快速方便地为 CircleCI 用户带来代码覆盖。在 Codecov，我们相信开发人员日常使用的工具之间的集成可以让开发人员专注于真正重要的事情，而不是试图争论不同的平台。为此，我们非常自豪能与 CircleCI 合作并成为这一新计划的一部分。

在 2019 年 1 月 17 日上午 11:00 PST/19:00 UTC 举行的我们的网络研讨会“Codecov 和 CircleCI Orbs:使代码覆盖变得简单”中，了解更多关于 CircleCI 和 Codecov 的信息。在这里报名[。](https://www2.circleci.com/CircleCI-Codecov-Webinar.html)

* * *

Tom Hu 与开发人员一起构建和维护高质量的代码，并在他们的组织和网络中成为代码覆盖率的捍卫者。在 Codecov 之前，Tom 在各种公司担任了 7 年的软件工程师，从预融资的初创公司到财富 500 强公司。在他的个人生活中，汤姆是一个狂热的攀岩者和鸡尾酒爱好者。