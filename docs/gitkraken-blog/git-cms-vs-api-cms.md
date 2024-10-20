# 基于 Git 的 CMS 与 API 驱动的 CMS

> 原文：<https://www.gitkraken.com/blog/git-cms-vs-api-cms>

内容管理正转向两种无头选项:基于 Git 的和 API 驱动的。Headless CMS 解决方案允许开发人员轻松地编辑和管理内容，同时仍然使用他们熟悉和喜爱的前端和后端工具和平台。这些基于 Git 和 API 驱动的 CMS 解决方案为开发人员提供了一种新的方式来加快 CI/CD 流程，而不会影响产品质量。

Git 是初学者和经验丰富的软件开发人员的必备工具。根据我们最近的调查，95%的受访者在他们当前的项目中积极使用 Git。因此，基于 Git 的 CMS 可以对开发人员和团队产生巨大的影响。希望学习 Git 并快速发展可行的技能吗？查看我们学习 Git 的[顶级资源。](https://www.gitkraken.com/blog/best-git-training)

但是 API 驱动的 CMS 对于新老开发者也有优势。基于 Git 的 CMS 充当 Git 之上的一层，直接与 Git 存储库交互，而 API 驱动的 CMS 使用户能够选择他们想要使用的框架和语言。哪个适合你？让我们讨论一下每种类型的优点、缺点、好处和挑战，这样你就可以决定哪种最适合你。

什么是无头 CMS？

## 像 WordPress 和 Drupal 这样的传统 CMS 被构建在一个单一的 web 优先的应用程序中，具有后端和前端功能。由于全球注册了超过 3 . 6 亿个域名，传统的 CMS 通常足以满足初露头角的开发者和那些几乎没有编码知识的人的需求。

像 WordPress 和 Drupal 这样的传统 CMS 被构建在一个单一的 web 优先的应用程序中，具有后端和前端功能。由于全球注册了超过 3 . 6 亿个域名，传统的 CMS 通常足以满足初露头角的开发者和那些几乎没有编码知识的人的需求。

[图像来源](https://www.sanity.io/headless-cms)

[图像来源](https://www.sanity.io/headless-cms)

无头解决方案为专业开发人员提供了对其实现的更多控制，并使团队能够创建更加一致的 CI/CD 管道。它还使团队能够跨多个前端重新调整内容的用途。Headless CMS 将内容管理与前端表示层分离，因此开发人员可以交付应用和网站之外的内容。

基于 Git 的内容管理系统

当使用基于 Git 的 CMS 时，开发人员实际上是在使用已经保存到他们的存储库中的文件。基于 Git 的 CMS 解决方案的行为就像 Git 之上的一个层，直接与 Git 存储库交互。一些最流行的基于 Git 的 CMS 包括[林业](https://forestry.io/)、[公共](https://getpublii.com/)和[散文](http://prose.io/)，但是还有更多可供选择。

## 基于 Git 的内容管理系统

[图像来源](https://craftercms.org/blog/2020/09/git-based-cms-what-it-is-and-why-you-need-it)

在我们深入探讨好处和挑战之前，让我们从高层次上看一下使用基于 Git 的 CMS 的利弊:

赞成的意见

内容的完整版本控制

内容以平面文件的形式存在，因此开发人员可以使用他们常用的工具

### 易于回滚

*   基于 Git 的工作流的同质方法
*   没有数据或带宽限制
*   易于设置
*   基于 Git 的 CMS 与 Git 存储库和开发人员已经在使用的工具无缝融合。因为它本质上是 Git 层之上的一层，所以除了写入代码的内容之外，实际上没有任何数据限制。
*   骗局
*   无法从使用相同 CMS 的多个应用程序和网站获取内容

有限的发布能力

不适合包含大量内容的网站

对内容格式的有限控制

### 存储库大小的限制意味着开发人员可能需要为媒体文件提供单独的服务

*   无法从使用相同 CMS 的多个应用程序和网站获取内容
*   尽管基于 Git 的 CMS 对于使用 Git 库进行开发的团队很有帮助，但是对于那些经常向他们的项目发布大型媒体文件的团队来说，它可能会稍微复杂一些。此外，内容格式化严格受限于 Git，这意味着它不能跨多个演示提取内容。
*   基于 Git 的 CMS 的优势
*   现在我们已经了解了一些基础知识，让我们了解更多关于使用基于 Git 的 CMSs 的好处。
*   多对象版本控制

传统的 CMS 平台在其构建能力方面受到限制。有限的版本意味着传统的解决方案必须维护繁琐的数据结构或跟踪单对象图。对于普通开发人员来说，有限的版本控制并不是一个障碍。

基于 Git 的 CMS 的多对象版本控制给组织带来了优势。在基于 Git 的解决方案的支持下，它允许在文件级别跟踪特定的内容变化。因为所有内容都存储为平面文件，所以可以很容易地从测试环境转移到开发和生产环境以及系统之间。

## 分布式存储库和分支

有了基于 Git 的 CMS，分布式版本管理和工作流管理都得到了简化。传统的 CMS 要求开发者直接在 CMS 中工作。但是基于 Git 的 CMS 开发人员可以在本地工作，并且仍然可以在 CMS 中进行修改。开发人员不用被迫使用中介工具，仍然可以不受限制地使用他们日常使用的工具。

通过分支，团队可以创建无限的阶段环境和预览，以可视化开发过程，并使团队工作更加容易。开发人员和作者可以使用基于 Git 的 CMS 来完成这一切。他们可以尝试新的功能，对网站进行重大改进，并管理内容分发。

### 所有权

最后，开发人员使用基于 Git 的 CMS 的最后一个主要好处是所有权。内容数据都存储在本地存储库文件中，因此默认情况下，开发人员是内容的唯一所有者。这意味着开发者可以自由地将内容从 CMS 转移到 CMS，而不会产生法律后果。事实所有权解决了许多常见的 Git 错误，这些错误是经验丰富的专业人员和新手开发者在传输和分发内容时经常犯的。

基于 Git 的 CMS 面临的挑战

基于 Git 的 CMS 允许开发人员处理他们的内容，但也有一些挑战需要注意。

### 基于 Git 的 CMS 中的关系

关系对于建模和检查内容非常重要。与当前基于 Git 的 CMS 解决方案相关的主要问题是，如果不加强监督和定期维护，它们往往太简单，容易出问题。

通过分支，团队可以创建无限的阶段环境和预览，以可视化开发过程，并使团队工作更加容易。开发人员和作者可以使用基于 Git 的 CMS 来完成这一切。他们可以尝试新的功能，对网站进行重大改进，并管理内容分发。

搜索引擎优化的特殊措施是必需的

### SEO 对于在线品牌来说也是一个至关重要的因素，但是 Git 没有提供任何具体的支持来帮助改进技术 SEO。使用基于 Git 的 CMS 的开发人员想要提高他们的在线可见性，需要生成站点地图，有效地使用元标签，开放图形标签，以及使用移动友好的主题。

GitHub 不运行插件

开发人员在使用基于 Get 的 CMSs 时遇到的最大问题是 GitHub 不运行插件。虽然许多用 Git 编码的开发人员意识到了这一限制，并推出本地生成的内容来定制某些功能，但这确实会推迟发布时间。

## API 驱动的 CMS

API 驱动的 CMS 是另一种通过 API 提供内容的无头 CMS。这允许开发人员将内容与表示完全分离。这意味着数据是结构化的，但如何处理这些内容取决于您自己。一些最流行的 API 驱动的 CMS 包括 [Prismic](https://prismic.io/) 、 [Ghost](https://ghost.org/) 和 [Strapi](https://strapi.io/) 。但是正如基于 Git 的 CMSs 一样，有许多 API 优先的解决方案可供选择。

### [图像来源](https://buttercms.com/blog/api-driven-cms-a-win-for-both-marketers-and-developers/)

关系对于建模和检查内容非常重要。与当前基于 Git 的 CMS 解决方案相关的主要问题是，如果不加强监督和定期维护，它们往往太简单，容易出问题。

以下是使用 API 驱动的 CMS 的利与弊的简要概述:

### 赞成的意见

在不同的应用程序和网站之间分发相同的托管内容

能够在多个前端使用内容

### 大多数是完全可定制的

轻松处理大量数据

包括资产管理功能

## 骗局

未集成到开发人员工作流中

存储和使用限制

重大变更严重依赖开发人员

API 驱动的 CMS 的优势

正如您所看到的，API 驱动的 CMS 比基于 Git 的 CMS 拥有更多的特性，但是对于某些实现来说，缺点可能大于优点。让我们仔细看看使用 API 驱动的 CMS 的好处。

现有生态系统

### API 驱动的 CMS 解决方案越来越受欢迎的原因之一是它们与现有的生态系统集成得很好。最佳解决方案还通过使用 API 中适当的端点来帮助团队实施最佳实践并最小化错误。这意味着开发人员可以自由地集成 CRM、分析和自动化等工具，并随时删除它们。通过这种方式，一旦客户情绪发生变化，品牌就可以改变他们的数字体验和平台。

*   在不同的应用程序和网站之间分发相同的托管内容
*   通过几种不同的设备接触消费者
*   API 驱动的 CMSs 还使组织能够连接到几个不同设备上的消费者。平板电脑、智能手机和其他支持物联网的设备[正变得越来越普遍](https://www.zdnet.com/article/what-is-the-internet-of-things-everything-you-need-to-know-about-the-iot-right-now/)，因为它们通常比电脑和个人电脑便宜。但传统的 CMS 平台无法在设备层面提供定制化的体验。然而，当 API 被开发用于特定的接触点时，API 驱动的 CMSs 可以，使得定制实际上是无限的。
*   更快的内容再利用
*   API 驱动的 CMS 将内容存储在一个中心位置，使其易于传递到任何渠道。内容创建者可以创建一个内容片段，然后使用 CMS 将其重新用于新的渠道、不同的设备和不同的受众。这对于采用全渠道营销方式、以数字为先的公司来说是一个巨大的优势。它节省了时间，并且消除了为每个频道创建全新内容的需要。

API 驱动的 CMS 面临的挑战

### API 驱动的 CMS 有很多好处，但是也有一些挑战，开发者应该小心。

*   过于依赖网络开发人员
*   API 驱动的 CMS 解决方案严重依赖开发人员来创建定制的前端。这意味着团队需要做更多的工作，并且需要改进营销和开发团队之间的沟通。
*   内容预览是个问题

API 驱动的 CMS 并不总是允许开发者在内容发布前预览内容。后端编辑器允许内容创建者将他们的内容直接输入到 API 中，但是在没有从用户的角度看到页面行为的情况下发布内容是具有挑战性的。这给错误留下了很小的空间，因此开发人员和创建者必须确保他们正在进行准确的更改。

## 费用

API 驱动的 CMSs 可能比传统的和基于 Git 的 CMS 解决方案更昂贵。即使开源 API 驱动的 CMS 也需要组织支付基础设施成本，如托管和 cdn，人员成本，如开发人员和质量保证工程师，以及生产成本，如维护和安全。

### 基于 Git 的 CMS 与 API 驱动的 CMS

关于基于 Git 的 CMS 和 API 驱动的 CMS，没有一个比另一个更好。这完全取决于您作为开发人员的需求、您所工作的团队的才能，以及您的客户所期望的产品结果。如果你独自工作或者和一个固定的团队一起工作，做出选择会很简单。但是涉及的人员越多，就越困难。

许多组织和开发人员在时间紧张和最后期限临近时选择外包软件项目。在这种情况下，成本可能是一个重要因素。大多数自由软件开发人员要求 150-275 美元一小时的报酬，这个价格会随着经验和专业知识而上涨。有许多不同价位的基于 Git 和基于 API 的 CMS 选项，因此您一定会找到一个符合您预算的选项。

### 然而，考虑你的内容是在两个选项之间做出决定的最好方法。这些无头 CMS 类型之间的主要区别在于内容的存储和使用方式。在决定哪种解决方案最适合您时，这是一个至关重要的驱动力。

如何选择

切换到无头 CMS 意味着在基于 Git 的解决方案和 API 驱动的解决方案之间做出选择。虽然每个项目都需要不同级别的定制，但是您的整个开发过程应该是相同的。如果您仍然试图决定哪个选项适合您和您的团队，请确保您有一个可靠的开发策略，该策略足够灵活以适应独特的项目需求，但又足够严格以提供 CI/CD 过程的结构。

### 扪心自问，我的技术水平如何？我的团队能有效地使用 API 工具吗？客户期望迭代多快？你最常用的存储库是什么？你的团队对各种编码语言的理解程度如何？现在再来看看这两种无头 CMS 选项的优缺点、好处和挑战。在您最终决定之前，在采用基于 Git 或 API 驱动的解决方案之前，请确保您理解了工具背后的技术。

API 驱动的 CMS 将内容存储在一个中心位置，使其易于传递到任何渠道。内容创建者可以创建一个内容片段，然后使用 CMS 将其重新用于新的渠道、不同的设备和不同的受众。这对于采用全渠道营销方式、以数字为先的公司来说是一个巨大的优势。它节省了时间，并且消除了为每个频道创建全新内容的需要。

API 驱动的 CMS 面临的挑战

## API 驱动的 CMS 有很多好处，但是也有一些挑战，开发者应该小心。

过于依赖网络开发人员

API 驱动的 CMS 解决方案严重依赖开发人员来创建定制的前端。这意味着团队需要做更多的工作，并且需要改进营销和开发团队之间的沟通。

### 内容预览是个问题

API 驱动的 CMS 并不总是允许开发者在内容发布前预览内容。后端编辑器允许内容创建者将他们的内容直接输入到 API 中，但是在没有从用户的角度看到页面行为的情况下发布内容是具有挑战性的。这给错误留下了很小的空间，因此开发人员和创建者必须确保他们正在进行准确的更改。

费用

### API 驱动的 CMSs 可能比传统的和基于 Git 的 CMS 解决方案更昂贵。即使开源 API 驱动的 CMS 也需要组织支付基础设施成本，如托管和 cdn，人员成本，如开发人员和质量保证工程师，以及生产成本，如维护和安全。

API-driven CMSs don’t always allow developers to preview content before it’s published. The back-end editor allows content creators to input their content directly into the API, but it’s challenging to publish content without seeing how the pages behave from the user’s perspective. This leaves little room for mistakes, so developers and creators must be sure that they are making accurate changes. 

基于 Git 的 CMS 与 API 驱动的 CMS

### 关于基于 Git 的 CMS 和 API 驱动的 CMS，没有一个比另一个更好。这完全取决于您作为开发人员的需求、您所工作的团队的才能，以及您的客户所期望的产品结果。如果你独自工作或者和一个固定的团队一起工作，做出选择会很简单。但是涉及的人员越多，就越困难。

许多组织和开发人员在时间紧张和最后期限临近时选择外包软件项目。在这种情况下，成本可能是一个重要因素。大多数自由软件开发人员要求 150-275 美元一小时的报酬，这个价格会随着经验和专业知识而上涨。有许多不同价位的基于 Git 和基于 API 的 CMS 选项，因此您一定会找到一个符合您预算的选项。

然而，考虑你的内容是在两个选项之间做出决定的最好方法。这些无头 CMS 类型之间的主要区别在于内容的存储和使用方式。在决定哪种解决方案最适合您时，这是一个至关重要的驱动力。

## 如何选择

切换到无头 CMS 意味着在基于 Git 的解决方案和 API 驱动的解决方案之间做出选择。虽然每个项目都需要不同级别的定制，但是您的整个开发过程应该是相同的。如果您仍然试图决定哪个选项适合您和您的团队，请确保您有一个可靠的开发策略，该策略足够灵活以适应独特的项目需求，但又足够严格以提供 CI/CD 过程的结构。

扪心自问，我的技术水平如何？我的团队能有效地使用 API 工具吗？客户期望迭代多快？你最常用的存储库是什么？你的团队对各种编码语言的理解程度如何？现在再来看看这两种无头 CMS 选项的优缺点、好处和挑战。在您最终决定之前，在采用基于 Git 或 API 驱动的解决方案之前，请确保您理解了工具背后的技术。

However, considering your content is the best way to decide between the two options. The main difference between these headless CMS types is how content is stored and consumed. This is a crucial driving force when deciding which solution is best for you. 

## How to Choose

Switching to a headless CMS means making a choice between [Git-based](https://www.gitkraken.com/blog/best-git-gui-client) and API-driven solutions. While each project will require a different level of customization, your overall development process should stay the same. If you’re still trying to decide which option is right for you and your teams, make sure that you have a solid development strategy in place that is flexible enough to accommodate unique project requirements but rigid enough to provide structure for CI/CD processes.

Ask yourself, what is my skill level? Can my teams efficiently use API tools? How quickly do clients expect iterations? What repositories do you use the most? How well does your team understand various coding languages? Now take a second look at the pros, cons, benefits, and challenges of these two headless CMS options. And before you finally settle, be sure that you understand the tech behind the tools before landing on either a Git-based or API-driven solution.