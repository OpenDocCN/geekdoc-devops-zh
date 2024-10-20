# 手动作业审批和计划工作流运行- CircleCI

> 原文：<https://circleci.com/blog/manual-job-approval-and-scheduled-workflow-runs/>

在 CircleCI 2.0 中，团队在选择如何运行工作流方面比以往任何时候都更加灵活。您的作业可能很复杂(或者如您所愿很简单)，并且作业不一定按顺序运行。

当我们交付工作流时，我们希望为您提供一种方法来分解您的工作，并选择您想要协调配置的时间和方式。但是你仍然被卡住了——这仍然取决于你去找出一种不破坏任何东西的方式来运行你的工作，或者你没有浪费时间等待它们运行。您被迫就如何以及何时运行作业做出许多单独的决定，甚至是您每天运行的作业。有充分的理由选择手动批准作业，也有同样多的理由您可能希望提前安排工作流:

| 人工批准 | 计划作业 |
| --- | --- |
| 为需要某种程度监控的工作启用控制和风险降低层 | 资源密集型工作流 |
| 确保您团队中的某个人可以在部署到生产环境之前检查和确认工作的细节 | 您希望自动执行的定期运行的任务，例如:生成按计划运行的报告，而不是在每次提交时运行 |
|   | 自动运行作业，无需人工监督 |

因此，现在我们引入了要求手动批准和安排工作流运行的功能。这允许您为您的团队创建最合适的时间表。现在，您可以自定义您的工作流程，使其以最佳方式为您服务。

### 设置计划

按计划运行工作流是自动执行日常任务中重复任务的简单方法。要开始，只需添加一个带有`schedule`触发器的工作流。欲了解更多详细信息，[查看文档](https://circleci.com/docs/workflows/#scheduling-a-workflow)。

示例配置:

```
 version: 2
  commit-workflow:
    jobs:
      - build 
  scheduled-workflow:
    triggers:
      - schedule:
          cron: "0 1 * * *"
          filters:
            branches:
              only: try-schedule-workflow

    jobs:
      - build 
```

### 设置人工审批

当您希望保留手动批准时，可以通过向您的工作流程添加一个包含`type: approval`条目的特殊作业来轻松配置手动批准流程。参见下面的示例，并[查看文档](https://circleci.com/docs/workflows/#holding-a-workflow-for-a-manual-approval)了解更多详情。

示例配置:

```
 version: 2
  release-branch-workflow:
    jobs:
      - build
      - request-testing:
          type: approval
          requires:
            - build
      - deploy-aws:
          requires:
            - request-testing 
```

看不到您的具体用例，或者已经使用这些选项以不同的方式编排您的工作流？在[讨论](https://discuss.circleci.com/)上与我们和 CircleCI 社区的其他人分享吧！