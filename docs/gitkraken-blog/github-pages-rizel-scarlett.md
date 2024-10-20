# GitHub 页面重新设计| GitKon 2022 | Rizel Scarlett，GitHub

> 原文：<https://www.gitkraken.com/gitkon/github-pages-rizel-scarlett>

[https://www.youtube.com/embed/CzJhgq9cvMY?feature=oembed](https://www.youtube.com/embed/CzJhgq9cvMY?feature=oembed)

视频

*这是 GitHub 开发者倡导者 Rizel Scarlett 写的一篇客座博文*

我的父母出生在圭亚那，这个位于南美洲的国家受到了美洲印第安人、非洲人、印度人、中国人、英国人、葡萄牙人和荷兰人文化的影响。尽管有如此多的文化影响，圭亚那是南美洲唯一一个官方语言是英语的国家。然而，他们经常说一种叫做圭亚那克里奥尔语的英语方言。

我喜欢圭亚那文化的一切，包括音乐、食物和有趣的谚语。我妈妈总是说:“猴子 mek 他 pickney 直到他宠坏他们，”意思是“保持简单。不要加多了把事情搞砸了。”当我做饭时加了太多调料，或者我在一个艺术和工艺项目中加了太多亮片时，她会对我说这句话。我们在科技界也有类似的说法。这是首字母缩写:K.I.S.S .，代表“保持简单，笨蛋。”我同意这个缩写，但更喜欢省略“愚蠢”这个词

尽管科技行业鼓励我们保持简单，但我们真的遵循了自己的建议吗？当我反思 90 年代末到 21 世纪初的互联网和计算时，我对我曾经经历的简单感到怀旧。我记得我用过 Neopets，Millsberry，Limewire，AIM，The Doll Palace，我的场景炫目钉游戏，还有最重要的 Myspace。Myspace 提供了如此多的简单性，以至于鼓励 11 岁的我学习 HTML 和 CSS 作为自我表达的手段。

我怀念那些日子！自然，随着新工具和更快的 web 应用程序的发展，我们引入了更多的复杂性。科技行业已经从 HTML 和 CSS 发展到对 CSS 做出反应和调整。

用 Node.js 在后端写 JavaScript 不再是革命性的，令人惊讶；很常见。现在，我们有了更新、更快的 JavaScript 运行时，比如 Deno 和 Bun。虽然这些技术有趣、时尚，有时还很有帮助，但我想挑战你问自己这些问题:

1.  我的项目需要这种复杂程度吗？
2.  有我可以简化的地方吗？

当我第一次以开发者倡导者的身份加入 GitHub 时，我制作了一个[网络应用](http://s3-website-bucket-43d5014.s3-website-us-east-1.amazonaws.com/)，你可以通过名字搜索表情符号。为了构建它，我使用了以下技术:

*   Next.js
*   尾翼 CSS
*   普鲁米
*   AWS S3
*   Fuse.js
*   GitHub 的 API

事后看来，我并不需要使用所有这些技术来构建网站，尤其是因为它只做了一件事。相反，我可以通过使用 HTML、CSS、JavaScript 并将其托管在 [GitHub 页面](https://pages.github.com/)上来保持简单。

github pages-github 页面

GitHub Pages 是 GitHub 在 2008 年推出的首批产品之一，同年 GitHub 成立。它使开发者能够免费托管静态网站。GitHub Pages 是存储静态内容的强大选项，原因如下:

它是免费的。

## 它使协作变得容易。任何人都可以打开一个[拉请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)来更新站点。

您的存储库会与您对站点所做的任何更改同步。

*   虽然 GitHub Pages 带有类似于`https://YOUR_USER_NAME.github.io/`的默认域名，但它支持自定义域名。
*   它使用可定制的 GitHub 动作工作流进行构建和部署。
*   您的存储库会与您对站点所做的任何更改同步。
*   说了这么多，我意识到我没有充分利用 GitHub 页面，所以我开始探究为什么我停止使用它。
*   它使用可定制的 GitHub 动作工作流进行构建和部署。

**作为初级开发人员使用 GitHub Pages**

当我还是一名初级开发人员时，我只使用 GitHub 页面来托管我的第一个作品集；那是我最后一次使用它。我的理由:GitHub 平台吓到了我这个新开发者，所以我避开了。

为了解决新开发人员可能会遇到的问题，我开发了一个 VS 代码扩展，可以在不离开 IDE 的情况下将您的网站部署到 GitHub 页面。我写了一篇关于如何安装和使用它的博客，如果你感兴趣的话，我还写了一篇关于我如何构建应用程序的博客。

## **作为初级开发人员使用 GitHub Pages**

**作为一名经验丰富的开发人员使用 GitHub Pages**

我不再害怕使用 GitHub 了。我从 2018 年开始做开发者，对 GitHub 和各种 JavaScript 框架有了更多的熟悉。然而，GitHub Pages 以托管 HTML 或 Jekyll 支持的网站而闻名。

配置 GitHub 页面来托管 Next.js 或 Astro 站点是一件痛苦的事情。然而，最近，GitHub 通过 GitHub 动作使得在 GitHub 页面上托管任何静态框架或静态站点生成器变得更加容易。

## **作为一名经验丰富的开发人员使用 GitHub Pages**

**将任何静态站点生成器部署到 GitHub 页面**

GitHub Pages 现在使用可定制的 GitHub 动作工作流来构建和部署代码，以便开发人员可以控制他们的创作框架和部署。

了解更多关于如何[将任何静态站点生成器或前端框架部署到 GitHub 页面](https://dev.to/github/how-to-host-a-static-nextjs-site-on-github-pages-4pe0)的信息。

## **将任何静态站点生成器部署到 GitHub 页面**

**使用 GitHub 页面的更多资源**

以下是更多关于 GitHub 页面部署静态站点的入门资源:

## 如果您正在使用 GitHub 页面和 VS 代码来部署您的网站，请考虑将 GitLens 添加到您的工具箱中，以增强您的项目历史在 IDE 中的可见性。

以下是更多关于 GitHub 页面部署静态站点的入门资源:

如果您正在使用 GitHub 页面和 VS 代码来部署您的网站，请考虑将 GitLens 添加到您的工具箱中，以增强您的项目历史在 IDE 中的可见性。

[Sign up for GitLens+ for free](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)