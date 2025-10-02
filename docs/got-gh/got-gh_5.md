# 5\. 付费服务

GitHub 神奇的协同工具使得开源项目的创建和协同更加简单、高效。有些人可能会提出疑问：能否把 GitHub 用于私有项目呢？即能否只允许指定的用户访问项目和版本库，而其他人不能访问呢？能否在企业内部架设一个一模一样的 GitHub 服务呢？GitHub 针对这些需求提供了解决方案，这些解决方案需要或多或少地付出一定费用。

# 5.1\. GitHub 收费方案

访问网址 [`github.com/plans`](https://github.com/plans) 可以看到 GitHub 提供的不同的服务方案列表。

![../images/available-plans.png](http://box.kancloud.cn/2015-07-09_559de5f79adbd.png)

图 5-1：GitHub 服务方案列表

图 5-1 中显示了 GitHub 的三类（8 种）服务方案：

*   第一类是免费方案。免费用户账号可以创建任意数量的开放式项目（版本库），并且可以为开放式项目设置任意数量的协同者。
*   第二类是需要付费的个人账号方案。付费的个人账号允许托管私有版本库，即可以创建只有自己及指定的私有协同者才能够访问的版本库，而其他人不能访问。根据允许创建的私有版本库数量及私有版本库协同者数量，提供了三种收费标准（7 美元/月、12 美元/月 和 22 美元/月）。
*   第三类是需要付费的组织账号方案。使用付费的组织账号，可以突破私有项目的协同者数量限制，并使用更易管理的团队（Team）对项目进行授权。关于如何通过团队配置授权参见“4.3\. 组织和团队”。组织账号的付费标准较个人账号更高（有 25 美元/月、50 美元/月、100 美元/月 和 200 美元/月），但同时也可以创建更多的私有版本库和拥有更大的托管空间。

用户可以随时升级或降级自己在 GitHub 上的服务方案。点击菜单中的“Account Settings”可以看到当前所选方案，如图 5-2 所示。

![../images/plan-status.png](http://box.kancloud.cn/2015-07-09_559de5fc793b3.png)

图 5-2：用户所选方案及状态

点击图 5-2 中的“Change plan”按钮，进入到更换 GitHub 服务方案页面，如图 5-3 所示。

![../images/change-plan.png](http://box.kancloud.cn/2015-07-09_559de5fdad82e.png)

图 5-3：更换方案

选择适合的收费方案并付款后，即可完成服务方案的升级。

当 gotgithub 用户升级为付费账号后，创建新版本库时就可以通过新的选项创建私有版本库了。即在创建版本库时，如果不选择默认的“Anyone”，而是选择“Only the people I specify”可以创建私有版本库，如图 5-4 所示。

![../images/new-private-repository.png](http://box.kancloud.cn/2015-07-09_559de60295e38.png)

图 5-4：创建私有版本库

通过版本库的管理界面，可以随时将版本库的状态在公开和私有之间切换，如图 5-5 所示。

![../images/private-repos-settings.png](http://box.kancloud.cn/2015-07-09_559de60482351.png)

图 5-5：私有版本库管理界面

付费账号的公开版本库没有协同者数量上的限制，但是私有版本库却存在协同者数量上的限制。如图 5-6 所示，当私有版本库的协同者数量超出所选 GitHub 付费方案的限额后，会显示“OVERLIMIT”的警告，不过超出限额的协同者依然可以操作私有版本库。

![../images/add-private-collaborators.png](http://box.kancloud.cn/2015-07-09_559de6058239a.png)

图 5-6：添加私有协同者

组织是一类特殊的不能登录的用户账号。如果要对组织账号进行配置，需要先以组织所有者的用户账号登录，再通过切换上下文的方式访问组织账号。图 5-7 就是以 gotgithub 用户账号登录后，切换到 GotGitOrg 组织账号的管理界面。

![../images/org-plan-status.png](http://box.kancloud.cn/2015-07-09_559de606d0dc3.png)

图 5-7：团队账号的所选方案及状态

在图 5-7 所示的组织账号管理界面中显示了组织账号当前的 GitHub 方案，点击其中的“Change plan”按钮，显示如图 5-8 所示界面，可对组织账号的 GitHub 方案进行升级或降级。

![../images/org-change-plan.png](http://box.kancloud.cn/2015-07-09_559de6086ee10.png)

图 5-8：团队账号更换方案

为组织账号选择一个付费方案后，就可以在组织的账号下创建私有版本库，并以团队方式管理该私有版本库的授权。图 5-9 就是一个私有版本库 GotGitOrg/NonPublicRepo 的设置界面。

![../images/org-private-repo-settings.png](http://box.kancloud.cn/2015-07-09_559de60a89c0c.png)

图 5-9：团队的私有版本库设置

# 5.2\. GitHub 企业版

出于隐私或法律原因而不能将代码托管到第三方平台的企业，可能希望在企业内部架设专有的 GitHub 服务，能做到么？答案就是 GitHub 企业版（GitHub Enterprise）。

网址：[`enterprise.github.com/`](https://enterprise.github.com/)。

GitHub 企业版搭建在企业本地网络中，因此企业拥有对版本库和项目完整的控制权限。GitHub 企业版包含了 GitHub 上所有的好东西：提交历史、代码浏览、比较视图、Pull Requests、问题追踪、维基、Gists、组织和团队管理、强大的 API，和漂亮的界面，因此会使用 GitHub 就会使用 GitHub 企业版。此外企业版还增加了对 LDAP 和 CAS 支持功能，以便和企业现有的认证系统整合等等[[1]](https://github.com/blog/978-introducing-github-enterprise)。

GitHub 企业版不像它的前身 GitHub:FI(GitHub:FI 是 GitHub Firewall Install 的缩写，含义为在企业的防火墙内部架设 GitHub 服务，现已升级为 GitHub Enterprise。)那样通过下载软件包进行安装和部署，而是提供基于虚拟机的解决方案。即 GitHub 企业版以 OVF 虚拟机文件格式发布，可以运行在多种虚拟机平台，如：VMWare、VirtualBox、Oracle VM、Red Hat Enterprise Virtualization 和 IBM POWER。使用 OVF 格式让 GitHub 企业版的部署更加轻松。

GitHub 企业版根据用户数量收取年费，入门级的价格为 20 用户每年 5,000 美金。如果用户数少，建议采用付费的 GitHub 托管账号。购买更多用户许可，访问 GitHub 企业版网站，那儿有一个报价生成器，如图 5-10 所示。

![../images/github-enterprise-pricing.png](http://www.worldhello.net/gotgithub/images/github-enterprise-pricing.png)

图 5-10：GitHub 企业版报价生成器