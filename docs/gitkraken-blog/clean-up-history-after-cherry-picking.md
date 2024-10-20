# 在 Git | Git 最佳实践中挑选后清除回购历史记录

> 原文：<https://www.gitkraken.com/learn/git/best-practices/clean-up-history-after-cherry-picking>

在您利用 Git 中的 cherry pick 命令将提交的更改从一个分支转移到另一个分支之后，请确保返回并清理您的回购历史。返回并签出原始分支，对父提交进行硬重置。这将删除重复提交。