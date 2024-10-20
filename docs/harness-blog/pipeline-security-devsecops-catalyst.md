# CI/CD 管道安全性:DevSecOps 催化剂|线束

> 原文：<https://www.harness.io/blog/pipeline-security-devsecops-catalyst>

Harness 平台在您的管道中编排安全步骤。利用 Harness 平台提升您的持续交付协议。

在我们之前的比较[持续交付和持续部署](https://harness.io/blog/continuous-delivery-vs-continuous-deployment/)的帖子中，我们了解到持续交付侧重于信心。随着越来越多的用户信任我们的系统，正如 [DevSecOps](https://www.redhat.com/en/topics/devops/what-is-devsecops) 运动教导我们的那样，今天的安全性不是 SDLC 期间的事后想法。

我们的应用程序和平台继续变得更加分布式和复杂，但是我们的变化速度也在增加。随着攻击媒介的不断扩大，应用程序安全性对我们组织的成功至关重要，DevSecOps 运动正在走向成熟，以帮助应对复杂的攻击媒介。

### 充足的安全

安全无疑是一项大生意。作为一个现代社会，我们依赖于我们的信息基础设施。随着行业转向基于标准和组件的开发，当存在漏洞时，攻击者有很大的攻击面。根据最近的[微软入侵](https://www.infoq.com/news/2020/01/microsoft-elasticsearch-breach/)，2019 年数十亿记录被突破。

总体而言，工业安全标准在不断提高。在应用领域。DevSecOps 一直鼓吹向左转，也就是向开发者靠拢。总之一个名字，和 DevOps 类似的是开发和运营的合并，DevSecOps 是开发、安全和运营的合并。有了更好的信息和实践，工程师可以做出更好的开发和维护时间决策。

像任何领域一样，安全性可以有许多术语。随着我的职责开始增加，并开始研究应用程序安全性，我必须熟悉一些新术语。NIST，即国家标准与技术研究所(是的，就是度量衡部门)在网络安全方面相当突出。NIST 是 T2 NVD 或国家漏洞数据库的所在地，这是安全信息的黄金标准之一。NVD 主持 [CVEs](https://en.wikipedia.org/wiki/Common_Vulnerabilities_and_Exposures) 或用于公开描述安全漏洞的常见漏洞和暴露。学习和研究场所的首字母缩略词是 OWASP，或者开放 Web 应用程序安全项目是一个帮助推进应用程序安全实践的组织。OWASP 首页 [OWASP 十大](https://owasp.org/www-project-top-ten/)为软件工程师展示了常见的陷阱和补救措施。

随着这些主要术语的消失，在安全性方面已经有了很大的进步。有几类工具有助于以自动化的方式向工程团队传播信息。

### 自动化应用程序安全性

应用程序安全性无疑已经左移，允许开发团队继续提高他们的安全能力。对于正在转向 DevSecOps 模型的组织来说，您的持续交付管道是一个包含安全覆盖的自然区域。下面是几类以安全为中心的技术，它们可以集成到一个连续的交付管道中。

第一个是 [SAST](https://www.gartner.com/en/information-technology/glossary/static-application-security-testing-sast) 或者静态应用安全测试。SAST 工具通常会检查应用程序代码和配置中已知的好模式和坏模式。就测试覆盖率而言，拥有代码覆盖率并不是一个新概念，但更重要的是，安全性可以被添加到代码覆盖率/应用程序整体质量的组合中。OWASP 维护着一份非常全面的 SAST 工具清单。一个 SAST 工具如何帮助的简单例子是，例如，如果您有未整理的输入，例如，允许某种类型的注入，SAST 工具可以提醒您缺少对输入的过滤。我知道这一点，因为我是未经过滤的 Web XML 之王，我们的 SAST 工具会让我知道这一点。

随着复杂性的发展，代码级分析只能告诉我们应用程序代码是生态系统和环境(也称为基础设施)的一部分。 [DAST](https://www.gartner.com/en/information-technology/glossary/dynamic-application-security-testing-dast) 或动态应用安全测试。随着我们的应用程序变得越来越复杂和分散，生态系统问题开始发挥作用。一个 DAST 工具将试图在一个安全和可控的环境中代表你进行攻击。虽然 DAST 工具将需要某种部署的应用程序来运行，理论上这将在管道中捕捉问题，但将摆脱环境问题。

依赖性和工件/图像扫描工具在今天很流行，对于开源开发来说是理所当然的。在 2017 年 [Equifax 泄露](https://blog.sonatype.com/wsj-on-struts-companies-still-downloading-flaw-linked-to-equifax-breach)之后，开源依赖扫描【将包与 CVE 匹配】受到了极大的重视。我们当然不会永远停留在项目上，能够检查软件材料清单是很重要的，知道我们的平台里面有什么是很重要的，因为粒度部落知识很难转移。

[RASPs](https://www.gartner.com/en/information-technology/glossary/runtime-application-self-protection-rasp) 或者说运行时应用程序自我保护平台是新出现的。想象一下，当新的威胁出现时，RASP 可以满足您的应用程序安全需求。RASP 通常是应用程序中部署的一个依赖项，它将分析应用程序中的调用，例如方法调用。与部署在网络边缘的 Web 应用程序 firewalll[WAF]类似，RASP 会针对可疑呼叫进行调整，但与 WAF 不同的是，它不仅限于 HTTP/S。

需要协调几个工具，因为有些同类最佳的专用工具专注于应用程序安全体系中的某些领域。连续交付管道是协调这些工具的好地方，但是如何保护您的 CD 管道本身呢？

### 你的实际管道呢？

几年过去了，你的管道不再重要，因为对我们大多数人来说，持续交付管道只是一个概念，并不具体。时至今日，许多组织依靠他们的持续交付渠道将他们的想法付诸实践。通常，在您的所有环境中，安全性都至关重要。

作为任何健壮的应用程序基础设施，限制谁有权访问是第一步。从设计上来说，应该将谁可以执行管道/部署到更高的环境限制在那些需要的人。让所有人都能看到管道的健康状况也是一个平衡的无线电拨号盘，也就是最小特权原则。这就是[基于角色的访问(RBAC)](https://developer.harness.io/docs/feature-flags/ff-onboarding/ff-security-compliance/manage-access-control/) 允许成功实现最小特权的原因。

根据法规要求，能够看到谁以及何时进行了更改是许多组织的重要支柱。即使您的组织不在法规要求之下，现代企业软件的期望是产生某种[审计跟踪](https://docs.harness.io/article/kihlcbcnll-audit-trail)，这样至少我们可以看到谁做了打破管道的改变。

显然，我们大多数人(希望是所有人)都没有使用明文密码。利用秘密管理器不仅是为了在管道中利用凭证，而且是为了在密钥过期时交换密钥等，这一点至关重要。Harness 平台有一个[健壮的秘密管理器](https://developer.harness.io/docs/platform/security/harness-secret-manager-overview/)，如果你以前没有使用过秘密管理器，这是一个很好的启动方式。Harness 在这里帮助协调组织中不同学科之间的信心。

### 驾驭-建立信心的平台

Harness 平台是在管道中编排安全步骤的平台。随着 DevSecOps 运动的不断成熟，编排安全覆盖将成为大多数组织的标准。

线束平台能够对管道标准的符合性进行评分。随着您的组织在 DevSecOps 实践中变得越来越成熟，这对于确保诸如安全测试之类的方面不被忽视非常有帮助。

今天，您可以随意使用 Harness [连续交付平台](https://harness.io/platform/continuous-delivery/)来体验一下，看看您如何在今天的管道中加入以安全为中心的步骤，并作为您自己的 DevSecOps 运动的催化剂。

干杯！

-拉维