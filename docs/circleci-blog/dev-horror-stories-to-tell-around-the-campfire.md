# 围着篝火讲述开发人员的恐怖故事

> 原文：<https://circleci.com/blog/dev-horror-stories-to-tell-around-the-campfire/>

作为一名开发人员，生活可能会有许多令人毛骨悚然的变化。

![I have never felt so close to another soul, and yet so hopelessly alone, as when I Google an error, and there’s one result: A thread by someone with the same problem, and no answer, last posted in 2003\. Stick figure shaking their computer monitor: ‘Who were you, DenverCoder9? What did you see?!’](img/b1d120b6735f299eb7ed875d7bbb7eec.png "All long help threads should have a sticky globally-editable post at the top saying ‘DEAR PEOPLE FROM THE FUTURE: Here’s what we’ve figured out so far ...’")

从长期困扰您的论坛难题，到您发现潜伏在遗留代码中的偷偷摸摸的 bug，作为开发人员，您遇到的事情可能会非常令人震惊。为了庆祝万圣节，请继续阅读我们 CircleCI 开发者的一些恐怖故事:

> “我曾经为一家 VPS 提供商提供技术支持，我告诉一位客户，他们的磁盘映像被破坏了，因此他们应该删除它并从备份中恢复。备份也遭到破坏，我们无法尝试修复他们的映像，因为他们删除了它……”

> “让一切以根运行。说够了。”

> “我曾经在运行 Ansible playbook 时删除了我们车队中每台机器上的/home，因为 Jinja 模板的规则以及过滤和循环的操作顺序不清楚，所以没有删除/home/$employee，$employee 是一个空字符串。”

> “我曾在一家初创公司工作，当时只有 4 个人，包括我自己，老板/自认为是本周名人的人在泡沫破裂前的 90 年代赚了钱，他擅长营销，但几乎没有技术知识。有一天，这款应用发布了一个 bug，那天晚些时候，当我告诉他 bug 的根本原因时，他的反应是‘为什么没有一个备份功能可以在出现 bug 的情况下工作？我那天提出了辞呈。"

> “我曾经在一家网络保险公司工作，该公司举行了一次发布会/聚会，所有高管花了一个多小时谈论公司的表现，然后第二天就解雇了三分之一的员工。”

> “我最近的一家公司有大约 3 年的建筑师旋转门，产品从纯 Clojure/ClojureScript 发展到 Python、Go、Clojure、ClojureScript、Java、JavaScript，以及一些 C++的加入。”

> "让我们不要使用 Kubernetes，并尝试推出我们自己的无状态 Kubernetes . "

> “一家老公司从主值文件和 jinja 模板呈现每个微服务配置文件，然后将配置部署到 zookeeper 并直接部署到运行各种服务的容器中。这至少达到了你预期的 1/4。”

> “我曾经继承运行一个 webservice，它是 lighttp，使用 CGI 运行 bash 脚本，使用 sed/awk/cut 解析 env 变量，如查询字符串和路径，以提取参数，然后使用 touch 和 rm 加上一些在文件系统上精心维护的状态，而不是使用真正的 DB -他们甚至没有使用 SQLite！”

> "将一个整体作为一个依赖，重构为微服务."

有你自己可怕的故事？将你的#devhorror 故事分享到@CircleCI