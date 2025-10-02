# Mercurial 使用教程

> 来源：[Mercurial 使用教程](https://www.mercurial-scm.org/wiki/ChineseTutorial)

本教程是介绍如何使用 Mercurial。我们不假定你有使用 源代码控制管理(SCM) 软件的背景。 本教程有法文 FrenchTutorial，西班牙文 SpanishTutorial，日文 JapaneseTutorial，和中文 ChineseTutorial

当研究完本教程后，你应该对以下几点有所领会：

*   你将要使用的 Mercurial 的概念和命令

*   如何用简单的方法在软件项目中使用 Mercurial

我们强烈建议你研读 Mercurial 手册 [hg(1)](http://www.selenic.com/mercurial/hg.1.html) 和[hgrc(5)](http://www.selenic.com/mercurial/hgrc.5.html)，它们也在源代码树 `doc/hg.1.txt` 和 `doc/hgrc.5.txt` 中

## 如何阅读本教程

格式约定很简单。命令名和参数名以 `fixed font` 显示。

你需要在 shell 或 命令行中输入的行用 fixed 字体显示，该行以 `$` 字符开头。

你希望 Mercurial 或 shell 输出的行以 fixed 字体显示，但开头没有字符。

```
$ this is a line of user input
this is a line of program output 
```

我们在所有例子中使用 `bash` shell。在其它 Unix shell 和 Windows `command.exe` 中的概念是一样的，但操作上的某些语法需要改动。例如，`ls` 在 Unix shell 中与 Windows 中的 `dir` 大致相当，Unix 的 `vi` 和 Windows 的 `edit` 相似。

## 目录

*   ChineseTutorialInstall - 安装 Mercurial

*   ChineseTutorialClone - 为现有的 仓库 作一个复本

*   ChineseTutorialHistory - 浏览仓库的历史

*   ChineseTutorialFirstChange - 生成你的第一个改变

*   ChineseTutorialShareChange - 与其它仓库分享改变

*   ChineseTutorialExport - 与其它人分享改变

*   ChineseTutorialMerge - 处理一个文件中独立的改变

*   ChineseTutorialConflict - 处理需要人工解决的合并

*   ChineseTutorialConclusion - 结束