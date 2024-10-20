# 用 Jamstack & Git 部署快速静态站点

> 原文：<https://www.gitkraken.com/gitkon/jamstack-cms>

[https://www.youtube.com/embed/moZWRGon9fk?feature=oembed](https://www.youtube.com/embed/moZWRGon9fk?feature=oembed)

视频

基本上有两种方式将网站交付给用户。通过静态网页或动态网页。

静态网页被交付给用户的浏览器，就像它们被存储在 web 服务器上一样。静态页面以同样的方式向每个访问者提供 HTML 标记的内容、CSS 和 Javascript，确保无论谁访问网站都有统一的体验。

另一方面，网站可以作为动态网页交付，其中一些内容是在请求时在 web 服务器上动态生成的。例如，当用户从 WordPress 网站请求网页时，服务器需要使用模板引擎生成内容，从数据库或第三方 API 提取数据，以创建所需的 HTML 呈现给用户。

GitKraken 包括一个 GUI 和 CLI，让您的 Git 体验更加动态，并提高工作效率，因此您可以真正拥有两个世界的精华。

## **静态网页的好处**

### **1。速度**

静态网页提供了一系列的优势，首先是速度。静态网页加载速度非常快，因为它们传送的文件就像存储在 web 服务器上一样。当发出请求时，不需要生成任何东西。这与许多动态站点采用缓存的原因相同。

### **2。实惠**

静态站点运行起来很便宜，因为底层 web 服务器不需要主动运行任何代码，也不需要任何高级配置。服务器只需要根据其文件系统的需求交付静态资产。也不需要维护昂贵的数据库层。这使得静态站点更容易维护。

**3。安全性**

### 静态网站也更安全。构建站点不需要执行任何代码，因此恶意代码注入或跨脚本攻击的可能性要小得多。因为没有运行活动的管理系统，所以不良行为者不能获得管理特权。俗话说:“没有代码比没有代码更安全。”

Static sites are also more secure. No code needs to be executed to build the site, so there is much less chance of malicious code injection or cross-scripting attacks. Because there is no active administration system running, there is no admin privilege a bad actor can gain. As the saying goes: “there’s no code more secure than no code.” 

**4。可扩展性**

### 很容易扩展静态站点。由于需要更多的容量来满足额外的请求，可以在几秒钟内添加更多的服务器来服务静态映像。这可以由许多系统在运行中完成，而不需要预先提供额外的资源，因为您必须使用动态 web 服务器。

**5。稳定性**

静态站点也带来了稳定性。没有执行任何代码来为用户生成内容，因此交付过程中出错的风险有限。使用当前的缓存设置，即使某个特定的 web 服务器完全从 Internet 上消失，也很可能有一个站点的副本缓存在某个地方，准备处理传入的请求。

### **5\. Stability**

**jam stack 的原理**

Jamstack 一词指的是一种构建网站和应用程序的方法，这种方法提供了更好的性能、更高的安全性和更好的开发人员体验，同时还降低了维护和扩展的成本。Jamstack 更像是开发人员遵循的一套核心原则，而不是正式的技术栈。

## **The Principles of Jamstack**

1.**在构建时生成静态资产。**

Jamstack 的第一个原则是静态资产应该尽可能在构建时生成。在提供给浏览器之前，可以构建的静态资产越多越好。理想情况下，整个项目可以由静态网页提供服务。这并不总是可能的，但像 Next.js 这样的工具已经发展到可以帮助开发人员遵守 Jamstack 原则，同时在绝对需要时仍然使用动态生成。

1\. **Generate static assets at build time. **

2.**将静态资产部署到内容交付网络(CDN)。**

Jamstack 的下一个原则是将这些静态资产部署到内容交付网络(CDN)。CDN 是一组地理上分散的服务器，它们将内容复制到世界各地的不同点。某人离 CDN 服务器网络中的一个节点越近，内容就能越快地提供给他们。例如，如果一个静态站点来自阿根廷，但使用全球 CDN 网络，欧盟的某个人将从他们最近的 CDN 节点获取所需的资产，从而避免往返地球的另一端来服务页面。因此，页面加载速度更快。

2\. **Deploy static assets to a Content Delivery Network (CDN).**

**3。JavaScript 将用于调用 API 来为动态内容生成资产。**

Jamstack 的最后一个首要原则是，如果需要动态内容，将使用 JavaScript 调用 API 来生成这些资产。最初，Jamstack 的名字大写为 JAMstack，前三个字母代表 Javascript、API 和标记。

例如，如果您想在 Jamstack 站点上显示一个 Twitter 提要，您可以使用 JavaScript 调用 Twitter API 来获取所需的 tweet，然后将 tweet 呈现到屏幕上，同样使用 JavaScript。

The final overarching principle of Jamstack is that if dynamic content is needed, JavaScript will be used to call APIs to generate those assets. Originally, the name Jamstack was capitalized as JAMstack, with the first three letters standing for Javascript, APIs, and Markup. 

For example, if you would like to display a Twitter feed on a Jamstack site, you would use JavaScript to call the Twitter API to get the needed Tweets, and then render the Tweets to the screen, again using JavaScript.

**jam stack 的工具**

与所有构建网站和应用程序的方法一样，与 Jamstack 相关的各种工具和平台已经出现，以抽象出一些开发人员的挑战，例如减轻他们对项目的每个元素进行手工编码的负担。这些工具和平台主要属于静态站点生成器、无头 CMS 和无服务器平台领域。

## **The Tools of Jamstack**

**静态现场发电机**

静态网站生成器(SSG)，顾名思义，是一个使用各种数据源和样式模板生成静态网站的软件。基于许多编程语言，有许多选项。几个例子包括使用 Ruby 的 [Jekyll](https://jekyllrb.com/) 、使用 GoLang 构建的 [Hugo](https://gohugo.io/) 和利用 React 的 [Gatsby](https://www.gatsbyjs.com/) 。Next.js 可以算作这一类，尽管它也可以处理动态内容生成。所有这些 SSG 程序从指定来源获取数据，并创建将部署到 CDN 的风格化静态资产。

## **Static Site Generator **

Jamstack CMS:无头 CMS

Jamstack 开发需要理解的下一项技术是 Headless 内容管理系统(CMS)。一般来说，CMS 是你存储和管理网站内容的地方。你可能已经熟悉了传统的 CMS，比如 [WordPress](https://wordpress.org/) 、 [Drupal](https://www.drupal.org/) 和 [Joomla](https://www.joomla.org/) 。这些 CMS 不仅存储内容，还负责显示内容，处理作为应用程序一部分的表示层。

## 无头 CMS 不关心内容最终将如何呈现；这是 SSG 的工作。

在一个无头 CMS 中有多种方法来存储和检索内容，有时也称为解耦 CMS。总的来说，一个好的 headless CMS 将会有一个管理界面，内容编辑可以在那里写他们的副本和修改预设字段中的文本。CMS 管理界面也是开发人员可以在称为“内容建模”的过程中定义各种字段和内容类型的地方。从智能手表到亚马逊的 Alexa，许多应用程序都在利用 headless CMSs 向用户提供内容。传统的 CMSs 可以用来满足 Jamstack 站点的需求，但是已经出现了许多系统，如 [Storyblok](https://www.storyblok.com/home) 和 [NetlifyCMS](https://www.netlifycms.org/) ，它们都是为这个用例定制的。

Headless CMSs are not concerned with how the content will eventually be presented; that is the job of the SSG. 

**无服务器平台**

Jamstack 开发所需的最后一套工具是无服务器平台。这是指构建、集成、部署和托管应用程序的在线服务。每当代码被更改并被推送到一个[远程 Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)，或者一个新的 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)被发出，这就触发了一个生成站点所需静态资产的构建过程。相同的服务将获取这些静态资产，并将其部署到 CDN，通常由相同的服务运行。 [Netlify](https://www.netlify.com/) 、 [Vercel](https://vercel.com/) 、 [Azure Static Web Apps](https://azure.microsoft.com/en-us/services/app-service/static/) ，甚至 [GitHub Pages](https://pages.github.com/) 结合 [GitHub Actions](https://github.com/features/actions) ，都是无服务器平台。

## **Serverless Platforms**

**Jamstack:将所有这些放在一起**

构建 Jamstack 站点的典型工作流如下所示:

## **Jamstack: Pushing it All Together**

在您的 headless CMS 中添加内容，例如 Storyblok。

在 SSG 中添加对该内容的引用，例如 Next.js

1.  使用类似 TailwindCSS 的东西，用你的 SSG 来设计内容的样式。
2.  使用 **[GitKraken](https://www.gitkraken.com/git-client)** ，将那些提交的代码变更推送到您的在线存储库中。
3.  观看构建过程在您的无服务器平台上工作，将最终呈现的站点交付到您的 URL。
4.  Push those committed code changes to your online repository, using **[GitKraken](https://www.gitkraken.com/git-client)**.
5.  Watch the build process work on your serverless platform, delivering the final rendered site to your URL. 

点击此处，查看 Facundo 使用 Next.js 作为 SSG，Vercel 作为无服务器平台，Storyblok 作为 headless CMS，向 Jamstack 站点添加内容类型的示例。观看他使用 GitKraken 提交更改并推送到他的遥控器。使用 GitKraken，您可以通过从中心图中拖放一个分支到另一个分支上来快速地推动变更。

无论您正在构建什么，GitKraken 都可以帮助您降低代码管理和协作过程的复杂性。现在就下载 GitKraken [**Git 客户端**](https://www.gitkraken.com/git-client) ，它包括一个 GUI 和 [**CLI**](https://www.gitkraken.com/cli) ，通过强大的可视化和 Git 增强的终端体验让您的开发人员生活更加轻松，让您专注于交付下一个令人惊叹的项目，而不是努力管理您的代码库。

Click here to see Facundo walk through an example of [adding content types to a Jamstack site using Next.js](https://youtu.be/moZWRGon9fk?t=857) as the SSG, Vercel as the serverless platform, and Storyblok as the headless CMS. Watch as he uses GitKraken to commit changes and push to his remote. With GitKraken, you can quickly push changes by dragging-and-dropping one branch onto another from the central graph.

No matter what you are building, GitKraken can help take the complexity out of code management and the collaboration process.  Download the GitKraken [**Git client**](https://www.gitkraken.com/git-client) today, which includes a GUI and [**CLI**](https://www.gitkraken.com/cli) to make your life as a developer easier with powerful visualizations and a Git-enhanced terminal experience, letting you focus on delivering your next awesome project instead of fighting to manage your codebase.