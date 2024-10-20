# 软件供应链:它是什么以及如何保证它的安全

> 原文：<https://circleci.com/blog/secure-software-supply-chain/>

随着国际供应网络瓶颈导致的消费品短缺和价格上涨变得越来越普遍，全球供应链及其脆弱性已成为许多人最关心的问题。对于开发者来说，最近几个引人注目的软件安全漏洞凸显了类似供应商网络的固有风险:软件供应链。

软件应用程序不再完全由定制代码构建。相反，它们是由开放源码组件和库的复杂网络组成的，其中大部分继承了其他第三方来源的功能。这种依赖链使开发人员能够利用他们喜欢的工具，并使团队能够以令人难以置信的速度将功能软件交付给他们的用户，但它也使组织和他们的客户暴露于他们直接控制之外的变化所带来的漏洞。

在本文中，我们将仔细研究什么是软件供应链，它给软件生产商带来了什么风险，以及您的组织如何利用[持续集成](https://circleci.com/continuous-integration/)来自动化安全性和合规性检查，这可以让您充分利用开源生态系统，同时降低供应链风险。

使用新的触发器和权限控制来管理您的生态系统中的变化

[Learn More](/blog/announcing-gitlab-support/)

## 什么是软件供应链？

很难想象今天有哪个组织的日常运作不依赖于多个软件。在几乎所有的情况下，软件都依赖于预先构建的外部组件。因此，默认情况下，组织继承了其软件所有部分的软件供应链。

软件供应链由代码、配置、专有和开源二进制文件、库、插件和容器依赖项组成。它还包括构建编排器和工具，如汇编器、编译器、代码分析器和存储库、安全、监控和日志操作工具。软件供应链还包括软件开发中涉及的人员、组织和过程。

由于软件供应链可以变得如此庞大和复杂，规避风险的企业和政府通常会要求一份描述部分或大部分供应链的[软件物料清单](https://circleci.com/blog/what-is-a-software-bill-of-materials/) (SBOM)。例如，2021 年 5 月，拜登政府[发布了一项行政命令](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)，要求与联邦政府合作的软件供应商为他们的项目提供一个 SBOM。

考虑到内部、外包、专有或开源软件通常使用外部组件，坚持使用 SBOM 是可以理解的。虽然以这种方式开发软件的明显好处——例如加快开发、降低生产成本和缩短上市时间——是可取的，但是利用这些组件中的常见漏洞和暴露(CVE)的不良行为者的威胁却是不可取的。

### 软件供应链攻击的例子

过去两年中对软件供应链的大量网络攻击说明了软件供应链可能带来的风险增加。仅在 2021 年就发生了至少四起著名的袭击事件——网络安全管理软件产品、Codecov、Kaseya 和 Log4j。

在 2020 年 3 月至 6 月的网络安全管理软件产品攻击中，Orion 平台上估计有 [18，000 名客户](https://www.zdnet.com/article/sec-filings-solarwinds-says-18000-customers-are-impacted-by-recent-hack/)下载了被注入恶意代码的更新，其中包括一些美国政府机构。该代码允许未经授权的后门访问系统和专用网络。网络安全管理软件产品直到 2020 年 12 月才发现漏洞，促使召回、修补和向客户发出[咨询](https://www.solarwinds.com/sa-overview/securityadvisory)。

几周后，2021 年 1 月，由于构建过程中的错误，攻击者获得了 Codecov 的 Docker 映像创建过程中使用的[凭据。凭证允许修改](https://about.codecov.io/security-update/) [bash 上传脚本](https://docs.codecov.com/docs/about-the-codecov-bash-uploader)。通过该脚本，他们窃取了信任客户的持续集成(CI)环境中的凭据和额外资源。Codecov 直到几个月后的 4 月份才发现漏洞。

2021 年 7 月 2 日，大约 90 天后，一个复杂的勒索软件组织[利用了 Kaseya 虚拟系统管理员(VSA)](https://www.cisa.gov/uscert/kaseya-ransomware-attack) 服务器中的一个漏洞——影响了[大约 1500 家小企业](https://www.washingtonpost.com/business/2021/07/06/kaseya-ransomware-attack-victims/)。与上面提到的案例不同，Kaseya 当天就发现了漏洞——可能是因为攻击者要求受影响方支付 45，000 美元到 500 万美元不等的赎金。Kaseya 建议客户在修复问题时关闭他们的 VSA 服务器。幸运的是，补丁早在两天后就可用了，直到 7 月 22 日 Kaseya 从第三方获得了一个工作解密工具。技术支持仍在继续，一个[咨询](https://helpdesk.kaseya.com/hc/en-gb/articles/4403440684689)随后在下个月发布。

2021 年 12 月，Kaseya 事件几个月后，发生了可以说是最简单但最普遍的软件供应链攻击。在流行的 Java 日志框架 Apache Log4j 中关于远程代码执行(RCE)的[概念证明(POC)](https://twitter.com/dzikoysk/status/1469091718867951618?s%3D20%26t%3DeHVpd6MJPdyuBwsH4EG8LQ) 被披露后，攻击者开始大规模利用一个漏洞。该漏洞名为 [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228#vulnCurrentDescriptionTitle) ，允许攻击者将恶意软件推送到易受攻击的轻量级目录访问协议(LDAP)服务器上。尽管解决该问题的更新相对较快，但[挥之不去的 log4j 漏洞](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Log4j)仍不断涌现，促使最近的[网络安全和基础设施安全局(CISA)于 2022 年 4 月发布咨询](https://www.cisa.gov/uscert/apache-log4j-vulnerability-guidance)。

## 为什么软件供应链容易受到攻击？

在它的核心，软件供应链是一个越来越大、越来越复杂、越来越互联的技术、人员和过程接触点系统，呈现出多种攻击媒介。坏演员可以利用这些接触点渗透到软件供应链中。

“技术”接触点通常由基础设施、软件和代码库组成。

基础设施是指用于运行软件的虚拟或物理设备。服务器、虚拟机、存储和网络设备等基础设施易受[错误配置错误](https://circleci.com/blog/cloud-misconfiguration/)的影响，从而使关键资源暴露在网络攻击之下。根据 Aqua Security 的 [2021 年云安全报告](https://www.aquasec.com/news/cloud-misconfigurations-on-the-rise-2021-cloud-security-report/)，90%的组织由于错误配置的云基础架构而面临安全漏洞的风险。

由专有代码组成的软件，尤其是开源库和第三方工具，很容易被插入[恶意代码](https://www.cisa.gov/uscert/ncas/current-activity/2021/04/15/namewreck-dns-vulnerabilities)或[利用代码中的错误](https://www.coindesk.com/business/2022/02/12/coinbase-trading-vulnerability-exposed-by-repeat-white-hat-hacker/)，精心设计软件包[依赖性混乱](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)，劫持更新，破坏代码签名过程。

存放这些程序的代码库也容易受到攻击。Synopsys 报告称，多达 88%的包含开源软件的商业代码库的组件比用户更新晚了两年。

“人”接触点由开发人员和其他人组成，他们恶意地或无意地在软件供应链中引入漏洞。例如，2017 年的 [crossenv 域名抢注](https://medium.com/@ceejbot/crossenv-malware-on-the-npm-registry-45c7dc29f6f5)事件是对 npm 注册表的恶意攻击，目的是欺骗不知情的开发人员安装包含恶意软件的 crossenv(而不是 cross-env)包。

“进程”是攻击者感兴趣的接触点，尤其是身份和访问管理(IAM)进程。攻击者可以通过员工或系统破坏 IAM 控制，从而引入间谍软件和勒索软件。

## 如何提高软件供应链的安全性

保护软件供应链的第一步是实现组件的可见性。供应商和最终用户可以通过列出您分发和使用的软件中所有第三方组件和依赖项的 SBOM 来实现这一点。

SBOM 概述了当前发生的情况，展示了安全意识和许可证合规性，并可作为影响软件组件的最新建议的指南。通过采用各种自动化漏洞扫描技术，您可以获得软件供应链的进一步可见性和安全性。

仅仅扫描或跟踪常见漏洞是不够的。您修复漏洞的速度和彻底程度会影响您的暴露程度。因此，有一个专门的事件响应团队来根据需要提供及时的补丁或更新会有所帮助。确保您拥有编写良好的故障转移流程，并定期进行严格的测试。

仅对供应链中的供应商使用可信的存储库和经过验证的来源，并对库、框架和供应商进行定期风险评估。一定要经常用你的独立测试来补充供应商的测试。作为供应商，您可以实施强有力的 IAM 政策和控制，同时牢记最低特权原则，包括数据治理指南，以保护您的软件供应链中的数据和基础设施。

## 如何利用 CI/CD 实现供应链安全自动化

实现供应链安全自动化的最佳方式是拥有强大的[持续集成和持续交付(CI/CD)管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)。借助 CircleCI，您可以无缝集成应用安全(AppSec)和开发安全运营(DevSecOps)工具，以便在管道的构建、测试和部署阶段检查来自版本控制系统(VCS)的漏洞。

在 VCS 阶段，开发人员可能会犯以纯文本形式将秘密提交给存储库的错误，攻击者可以在 Git 历史中发现这一点。CircleCI 与您首选的 VCS 提供商集成，使您能够使用相关的[orb](https://circleci.com/orbs/)，如 [GitGuardian orb](https://circleci.com/developer/orbs/orb/gitguardian/ggshield) ，扫描提交的秘密。您可以使用静态加密[环境变量](https://circleci.com/docs/env-vars/)或容器存储[上下文](https://circleci.com/docs/contexts/)在 CircleCI 中安全地存储和管理您的秘密，以便跨项目使用。

或者，您可以从第三方解决方案中动态获取存储的机密，或者提交加密版本并将解密工具保存在上下文中，以便您可以在 CI/CD 管道的构建和测试阶段执行解密作业来检索机密。

在 VCS 阶段，您还可以使用 [Lightspin orb](https://circleci.com/developer/orbs/orb/lightspin-tech/lightspin) 扫描 IAM 错误配置、暴露的凭据和基础架构中不安全的配置，并提供建议，作为代码库中的代码(IaC)模板。

在管道的构建阶段，您可以使用相关的 orb 对您的代码和开源库运行[静态应用安全测试(SAST)](https://circleci.com/blog/sast-vs-dast-when-to-use-them/) 作业。例如，你可以使用 [Snyk orb](https://circleci.com/developer/orbs/orb/snyk/snyk) 来[扫描你的代码库寻找依赖漏洞](https://circleci.com/blog/security-with-snyk-in-the-circleci-workflow/)。如果扫描揭示了软件供应链中的潜在威胁，您的构建将会失败，Snyk 将会输出提高代码安全性的建议。

在部署阶段，您可以运行动态应用程序安全测试(DAST)作业，以在生产运行时捕获漏洞。例如， [DeepFactor orb](https://circleci.com/developer/orbs/orb/deepfactor/deepfactor) 基于应用程序行为提供对应用程序代码、包依赖关系、web APIs 和合规性 [CVEs](https://owasp.org/www-project-top-ten/) 的优先洞察。

## 结论

软件供应链攻击日益猖獗。随着现代商业的相互联系，加上软件生态系统的变化速度越来越快，坏人有很多攻击点可以瞄准。这强调了尽可能防止成功利用漏洞的必要性。

做到这一点的一个方法是在 CI/CD 的软件交付过程的每一点添加自动化的供应链安全扫描。

使用 CircleCI，您可以创建带有作业的工作流，这些作业执行漏洞扫描并提供关于代码库、开源库、依赖项和其他第三方工具的建议。您可以检测基础架构中的 [IaC 错误配置](https://circleci.com/blog/cloud-misconfiguration/)在基础架构中，安全地构建和部署工件，并使用各种自动管道触发器验证 CI/CD 管道中的合规性规定。

为了领先于攻击者并了解更多有关将自动化供应链安全添加到您的 CI/CD 渠道的信息，现在就开始使用[免费 CircleCI 帐户](https://circleci.com/signup/)。