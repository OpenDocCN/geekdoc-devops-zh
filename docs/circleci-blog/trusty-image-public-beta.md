# 可信的图像现在在公共测试版 CircleCI 中

> 原文：<https://circleci.com/blog/trusty-image-public-beta/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

我们很自豪地宣布可靠形象的公开测试。如果你是 CircleCI 的测试程序“内部圈子”的一员，你已经有权限了！

## 什么是可信的图像？

Ubuntu 云映像是由 Ubuntu engineering 定制的预装磁盘映像，可在 Amazon EC2、Openstack、Windows 和 LXC 等云平台上运行。测试版程序使你能够在 Ubuntu 14.04(可信的)映像上运行构建。目前，构建运行在 Ubuntu 12.04 (Precise)上。

## 使用可信图像的好处？

以下是为什么您应该使用 Trusty 的一些好处:

*   你现在可以安装更新的 Debian 软件包了
*   最新主要和次要版本的语言支持

## 如何启用可信图像？

要启用可信的图像，导航到:“项目设置”->“调整”->“实验设置”->“Ubuntu 14.04 可信容器”

## 运营重点

由于可信图像仍处于测试阶段，可能会有某些性能问题。在某些情况下，您可能会遇到很长的构建队列。然而，我们的团队将尽最大努力确保所有构建的成功完成。

## 预安装的软件包

新的可靠映像包括大多数语言的更新版本，具有最新的路径级别。如需完整列表，请点击[此处](https://discuss.circleci.com/t/trusty-container-update-16-02-01/1914)。例如:Python 2.7.11 excludes2.7.10

您仍然可以使用未预安装的版本，但是安装将在构建时进行。以前，如果你想使用新版本的软件包/语言，你需要手动安装它们，有了新的可信任的镜像，很多都是预装的。

## 频繁更新

您的反馈对我们非常重要。当我们收到反馈时，我们将继续更新可靠的图片。请随时通过[beta@circleci.com](mailto:beta@circleci.com)联系我们。

更多详情，您可以[访问我们的主题为](https://discuss.circleci.com/t/ubuntu-14-04-trusty-container/2260)的讨论论坛。