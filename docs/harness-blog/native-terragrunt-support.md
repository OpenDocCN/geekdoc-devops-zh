# 挽具+地形地貌|挽具

> 原文：<https://www.harness.io/blog/native-terragrunt-support>

立即观看网上研讨会:Tenable 的 Dheeraj Khanna 基于挑战现状而领先。他努力成为一名敢于直面任何挑战的颠覆性领导者。

Harness 现在提供了原生的 Terragrunt 支持！

Terragrunt 是一个围绕 Terraform 的包装工具。它允许用户在他们的 Terraform 代码中实践 DRY(不要重复自己)原则。使用 Terragrunt，用户只需定义一次他们的 Terraform 代码，无需为多种环境重复定义。Terraform 代码在各种环境中通常都是相同的，只是在输入、输出和状态文件中存在潜在差异。管理状态文件、模块、输入变量和输出变量是 Terraform 用户的维护工作。开发人员被 Terragrunt 所吸引，因为它很容易维护组件，同时还能帮助他们一次性定义代码。

Harness 允许用户在产品中配置 Terragrunt。这种体验类似于在 Harness 中配置 Terraform 或 CloudFormation。

https://youtu.be/HYSi2LAaYdc

在创建这个特性之前，Harness 用户必须利用 shell 脚本供应程序来执行 Terragrunt 基础设施供应。但是现在，Harness 用户可以获得一流的集成。