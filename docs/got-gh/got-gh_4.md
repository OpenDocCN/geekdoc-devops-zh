# 4\. 工作协同

项目落户 GitHub 后，一定希望有越来越多的人能参与其中。GitHub 提供了包括传统的问题追踪系统、维基，还包括了分布式版本控制系统特有的协同工具。

# 4.1\. Fork + Pull 模式

参与 GitHub 中的项目开发，最常用和推荐的首选方式是“Fork + Pull”模式。在“Fork + Pull”模式下，项目参与者不必向项目创建者申请提交权限，而是在自己的托管空间下建立项目的派生（Fork）。

如果一个开源项目派生出另外的项目，通常意味着项目的分裂和开发团队的削弱，而 GitHub 中的项目派生则不会，而且正好相反，GitHub 中的项目派生是项目壮大的体现。所有的派生项目都会有链接指向原始项目，派生项目没有独立的缺陷追踪系统（ISSUE），而是必须利用创建者本人的项目中的缺陷追踪系统。至于在派生项目中创建的提交，可以非常方便地利用 GitHub 的 Pull Request 工具向原始项目的维护者发送 Pull Request。

下面以 GotGit 版本库为例，介绍如何利用 GitHub 提供的 Fork 和 Pull Request 工具实现工作协同。

## 4.1.1\. 版本库派生

GotGit 版本库[[1]](https://github.com/gotgit/gotgit/)用于维护《Git 权威指南》一书的官网和勘误，下面演示的勘误表修改是由王胜[[2]](https://github.com/wangsheng/)通过 GitHub 之外的一个缺陷追踪平台报告的[[3]](http://redmine.ossxp.com/redmine/issues/161)。他在报告中，甚至直接用 GNU diff 格式告诉我该如何修改。

下面就以用户 gotgithub 身份，访问版本库 [`github.com/gotgit/gotgit/`](https://github.com/gotgit/gotgit/) ，添加新的勘误。如图 4-1 所示，gotgit 项目在之前的示例中已经被我们关注但尚未 Fork。

![../images/gotgit-repo-before-fork.png](http://box.kancloud.cn/2015-07-09_559de342e98b2.png)

图 4-1：原 gotgit 项目

点击项目名称右侧的 Fork 按钮，便在 gotgithub 用户自己的托管空间下创建项目派生，派生项目版本库出现在版本库列表中，如图 4-2。

![../images/gotgit-in-repo-list.png](http://box.kancloud.cn/2015-07-09_559de344b3dce.png)

图 4-2：gotgithub 用户的项目列表

访问派生后的版本库，会发现和派生前的几乎相同，除了没有缺陷跟踪（ISSUE），以及标识了该项目派生之前的原路径等。如图 4-3 所示。

![../images/gotgit-repo-forked.png](http://box.kancloud.cn/2015-07-09_559de34550c4e.png)

图 4-3：派生的 gotgit 项目

现在 gotgithub 用户就在本地派生的版本库中提交。

*   克隆 gotgithub/gotgit 版本库。

```
$ git clone git@github.com:gotgithub/gotgit.git
$ cd gotgit 
```

*   为了向问题的发现者致敬，并经王胜同意，以他的身份进行提交。

```
$ git config user.name "Wang Sheng"
$ git config user.email wangsheng@ossxp.com 
```

*   编辑 errata.mkd 文件(版本库 gotgit/gotgit 已将勘误文件重命名为 errata.md。)，录入新发现的书中的文字错误。

```
$ vi errata.mkd 
```

*   对 error.mkd 的改动如下：

```
$ git diff
diff --git a/errata.mkd b/errata.mkd
index b0b68fb..29e40cf 100644
--- a/errata.mkd
+++ b/errata.mkd
@@ -14,5 +14,6 @@
 |     66 | 倒数第 11 行                | Author（提交者）             |  Author（作者）              | [Github#2](http://github.com/gotgit/gotgit/issues/2)    |
 |    144 | 第 1 行                     | \`$ **git rev-parse  A^{tree}  A:**  | $ **git rev-parse  A^{tree}  A:**              | [#153](http://redmine.ossxp.com/redmine/issues/153)  |
 |    218 | 第 8 行                     | 况下，Gits 标识出合并冲突，           | 况下，Git 标识出合并冲突，                      | [#159](http://redmine.ossxp.com/redmine/issues/159)  |
+|    369 | 第 21 行                    | 但 `-i` 参数仅当对一个项执行时才有效。 | 但 `-i` 参数仅当对一个项目执行时才有效。     | [Github#3](http://github.com/gotgit/gotgit/issues/3)    |
 |    516 | 倒数第 15 行                | **oldtag="cat"**             | **oldtag=\`cat\`**           | [#151](http://redmine.ossxp.com/redmine/issues/151)  | 
```

*   提交修改。至于提交说明中出现的编号，是为了和缺陷跟踪系统关联，会在后面章节介绍。

```
$ git add -u
$ git commit -m "Fixed #3: should be 项目, not 项." 
```

*   推送提交到 GitHub。

```
$ git push 
```

访问 GitHub 上的派生项目页面，会看到以用户 whangsheng 在 master 分支[[5]](版本库 gotgit/gotgit 原 master 分支内容已转移至 gh-pages 分支，通过 GitHub 提供的网站部署机制完成网页的编译和部署。)创建的提交。如图 4-4 所示。

![../images/gotgit-new-commit.png](http://box.kancloud.cn/2015-07-09_559de3466960d.png)

图 4-4：派生版本库中的新提交

## 4.1.2\. Pull Request

那么如何能够让 gotgit 原始项目的创建者知道这个派生项目及新的提交呢？GitHub 提供的工具就是“Pull Request”。注意到图 4-3 右上方“Pull Request”按钮了么？点击该按钮进入 Pull Request 创建界面。

在弹出的 Pull Request 创建界面中，点击菜单中的“Commits”，查看所包含的提交。如图 4-5 所示。

![../images/pull-request-form-commit.png](http://box.kancloud.cn/2015-07-09_559de347923e1.png)

图 4-5：Pull Request 包含的提交

点击菜单中的“Files Changed”，查看所包含的提交。如图 4-6 所示。

![../images/pull-request-form-file.png](http://box.kancloud.cn/2015-07-09_559de3490d978.png)

图 4-6：Pull Request 包含的改动差异

点击菜单中的“Preview Discussion”，填写 Pull Request 的标题和内容，完成 Pull Request 的创建。如图 4-7 所示。

![../images/pull-request-form-discuss.png](http://box.kancloud.cn/2015-07-09_559de34ce80cb.png)

图 4-7：Pull Request 的提交界面

当 Pull Request 发出后，项目 gotgit 的开发者会收到通知邮件，如图 4-8 所示。

![../images/pull-request-email.png](http://box.kancloud.cn/2015-07-09_559de34f7043b.png)

图 4-8：Pull Request 的通知邮件

点击邮件中的 URL 链接，以项目 gotgit 的开发者（如 ossxp-com）身份登录，看到如图 4-9 的视图。之所以看到有两个用户参与到此 Pull Request，是因为 Pull Request 创建者和提交的作者是不同的用户。图 4-9 下方的表单可以向 Pull Request 追加评论，或者关闭此 Pull Request。

![../images/pull-request-owner-view.png](http://box.kancloud.cn/2015-07-09_559de35462d3d.png)

图 4-9：Pull Request 接收者视图

GitHub 如果检测到 Pull Request 中包含的提交可以直接合并，会显示自动合并的提示信息，点击图 4-9 中提示信息中的自动合并按钮，显示图 4-10 的自动合并对话框。

![../images/pull-request-auto-merge.png](http://box.kancloud.cn/2015-07-09_559de35a0c812.png)

图 4-10：Pull Request 的通知邮件

点击“Confirm Merge”按钮即完成 Pull Request 中所含提交的自动合并。自动合并完成后，Pull Request 页面下方会以评论的形式出现相关提示，并自动关闭 Pull Request。如图 4-11 所示。

![../images/pull-request-closed.png](http://box.kancloud.cn/2015-07-09_559de35b1cb6d.png)

图 4-11：Pull Request 关闭

## 4.1.3\. 手工合并

Pull Request 提供的自动合并显示在提交日志中是什么样子的呢？以用户 ossxp-com 身份检出版本库，会看到用户 wangsheng 的提交已经合并到版本库中。

```
$ git clone git@github.com:gotgit/gotgit.git
$ cd gotgit
$ git log --graph -3
*   commit 6c1f1ee152629fd2f8d00ebe92c27a32d068d756
|\  Merge: 00c6c4b 7ecdfe7
| | Author: OpenSourceXpress 
| | Date:   Tue Aug 16 01:23:47 2011 -0700
| |
| |     Merge pull request #4 from gotgithub/master
| |
| |     Find a typo in the book
| |
| * commit 7ecdfe7451412cfb2e65bb47c12cf2162e21c841
|/  Author: Wang Sheng 
|   Date:   Tue Aug 16 10:17:53 2011 +0800
|
|       Fixed #3: should be 项目, not 项.
|
* commit 00c6c4bfab9824bd967440902ce87440f9e87852
| Author: Jiang Xin 
| Date:   Wed Aug 3 11:50:31 2011 +0800
|
|     Change font color for stronger text from red to brown. 
```

可以看出 GitHub 的自动合并产生了一个合并提交，类似执行 git merge --no-ff 命令。也就是说即使用户 wangsheng 的提交是一个“快进式提交”（基于 gotgit/gotgit 版本库最新提交所做的提交），也要产生一个合并提交。

可能有人并不喜欢这种用--no-ff 参数的非标准的合并方式，因为这种合并产生了一个多余的提交，可能增加代码评审的负担。若要取消 GitHub 的自动合并也很简单，因为 Git 无所不能：

```
$ git reset --hard HEAD^  # 回退一个提交，即回退到当前提交的第一个父提交
$ git rev-parse HEAD      # 检查是否正确的回退
00c6c4bfab9824bd967440902ce87440f9e87852
$ git push -f             # 强制推送回退的 master 分支 
```

下面就演示一下当收到他人的 Pull Request 后，该如何手动合并。实际上在很多情况下，Pull Request 所含提交有可能造成合并冲突，那样的话 GitHub 不再、也不能提供自动合并功能，就必须采用手工合并的方式。

*   将 Pull Request 发出者的派生版本库添加为一个新的源。

    例如收到来自 gotgithub 用户的 Pull Request，不妨以 gotgithub 为名添加新的源。

```
$ git remote add gotgithub https://github.com/gotgithub/gotgit.git 
```

*   此时版本库中有两个源，一个克隆时自动建立的 origin，另外一个就是新增加的 gotgithub。

```
$ git remote -v
gotgithub       https://github.com/gotgithub/gotgit.git (fetch)
gotgithub       https://github.com/gotgithub/gotgit.git (push)
origin  git@github.com:gotgit/gotgit.git (fetch)
origin  git@github.com:gotgit/gotgit.git (push) 
```

*   获取远程版本库 gotgithub 的分支和提交。

```
$ git fetch gotgithub
From https://github.com/gotgithub/gotgit
 * [new branch]      gh-pages   -> gotgithub/gh-pages
 * [new branch]      master     -> gotgithub/master 
```

*   现在除了本地分支 master 外，还有若干远程分支，如下：

```
$ git branch -a
* master
  remotes/gotgithub/gh-pages
  remotes/gotgithub/master
  remotes/origin/HEAD -> origin/master
  remotes/origin/gh-pages
  remotes/origin/master 
```

*   将远程分支 remotes/gotgithub/master（可简写为 gotgithub/master）合并到当前分支中。

```
$ git merge gotgithub/master
Updating 00c6c4b..7ecdfe7
Fast-forward
 errata.mkd |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-) 
```

*   查看提交说明，看到此次合并没有产生不必要的合并提交。

```
$ git log --graph -2
* commit 7ecdfe7451412cfb2e65bb47c12cf2162e21c841
| Author: Wang Sheng 
| Date:   Tue Aug 16 10:17:53 2011 +0800
|
|     Fixed #3: should be 项目, not 项.
|
* commit 00c6c4bfab9824bd967440902ce87440f9e87852
| Author: Jiang Xin 
| Date:   Wed Aug 3 11:50:31 2011 +0800
|
|     Change font color for stronger text from red to brown. 
```

*   将合并推送到 GitHub 版本库中。

```
$ git push 
```

## 4.1.4\. 在线编辑

GitHub 提供了在线编辑功能，这样可以无需克隆版本库、无需使用 Git 即可完成对版本库中文件的修改，甚至可以在你的 iPad 甚至 iPhone 上完成对文件的修改。

以 gotgithub 账户身份登录 GitHub，访问之前派生而来的版本库 gotgithub/gotgit 中的文件，例如文件 errata.md(版本库 gotgit/gotgit 已重构。分支 gh-pages 中文件 errata.md 文件来自于原 master 分支的 errata.mkd 文件，地址：[`github.com/gotgithub/gotgit/blob/gh-pages/errata.md`](https://github.com/gotgithub/gotgit/blob/gh-pages/errata.md))，会看到其中一个“Edit this file”的按钮，如图 4-12 所示。

![../images/edit-this-file-btn.png](http://box.kancloud.cn/2015-07-09_559de35c55d5d.png)

图 4-12：浏览自己版本库中文件

点击图 4-12 中的“Edit this file”按钮，开始在线编辑文件 errata.md，编辑器还支持语法加亮，如图 4-13 所示。

![../images/edit-this-file-form.png](http://box.kancloud.cn/2015-07-09_559de35ede088.png)

图 4-13：编辑文件

## 4.1.5\. 简化的 Fork + Pull Request

到目前，我们已经了解了 GitHub 的三大武器：Fork、Pull Request 和在线编辑。对于最常用的“Fork + Pull Request”操作，GitHub 还提供了一个快捷模式。即 GitHub 对于无权更改的他人版本库中的文件，提供了一个类似在线编辑的按钮，名为“Fork and edit this file”按钮，自动完成版本库派生和在线编辑，即将三大武器一勺烩。

访问他人版本库（尚未在自己空间派生）中的文件，例如访问下面地址：[`git.io/hello-world-makefile`](http://git.io/hello-world-makefile)(即地址 [`github.com/ossxp-com/hello-world/blob/master/src/Makefile`](https://github.com/ossxp-com/hello-world/blob/master/src/Makefile))。显示他人（ossxp-com）版本库 hello-world 中的 src/Makefile 文件，如图 4-14 所示。

![../images/fork-and-edit-btn.png](http://box.kancloud.cn/2015-07-09_559de3666e915.png)

图 4-14：浏览他人版本库中文件

点击图 4-14 中的“Fork and edit this file”按钮，会自动在自己托管空间创建派生版本库，并开始在线编辑文件 src/Makefile，如图 4-15 所示。

![../images/fork-and-edit-form.png](http://box.kancloud.cn/2015-07-09_559de36a50c35.png)

图 4-15：派生并编辑文件

文件修改完毕，点击“Propose File Change”按钮，会将改动作提交到派生的版本库中，并马上开启一个新的 Pull Request。如图 4-16 所示。

![../images/fork-and-edit-pull-request.png](http://box.kancloud.cn/2015-07-09_559de37807da4.png)

图 4-16：编辑完毕自动开启 Pull Request

点击“Send pull request”按钮完成 Pull Request 的创建。如果仔细查看图 4-16，会发现 Pull Request 所包含的修改发生在 gotgithub/hello-world 派生版本库中的 patch-1 分支中，并非通常的 master 分支。

原版本库 ossxp-com/hello-world 的开发者会收到一封邮件，通知有新的 Pull Request，如下所示（前四行为信头）：

```
From: GotGitHub 
Date: 2011/12/17
Subject: [hello-world] Bugfix: build target when version.h changed.  (#1)
To: Jiang Xin 

Without this fix, when version changed only version.h update, target rebuild needs a second `make`.

You can merge this Pull Request by running:

 git pull https://github.com/gotgithub/hello-world patch-1

Or you can view, comment on it, or merge it online at:

 https://github.com/ossxp-com/hello-world/pull/1

-- Commit Summary --

* Bugfix: build target when version.h changed.

-- File Changes --

M src/Makefile (3)

-- Patch Links --

 https://github.com/ossxp-com/hello-world/pull/1.patch
 https://github.com/ossxp-com/hello-world/pull/1.diff

---
Reply to this email directly or view it on GitHub:
https://github.com/ossxp-com/hello-world/pull/1 
```

版本库 ossxp-com/hello-world 的管理员既可以通过 GitHub 提供的图形化界面完成对 Pull Request 的审核和合并，也可以在命令行下完成。正如邮件中所述若使用命令行，操作如下：

```
$ git pull https://github.com/gotgithub/hello-world patch-1 
```

# 4.2\. 共享版本库

除了独具特色的“Fork + Pull”的分布式工作模式，GitHub 同样支持传统的集中式协同工作模式。

## 4.2.1\. 版本库授权

GitHub 可以通过多种途径为版本库授权，让版本库成为多人共享的版本库，从而让项目管理者围绕项目创建一个核心开发团队。下面以 gotgithub 账号下的 helloworld 版本库为例，介绍如何设置版本库的多人共享。

进入 gotgithub/helloworld 项目的管理界面，点击左侧导航条中的“Collaborators”，可以查看及添加项目的合作者，如图 4-17 所示。

![../images/collaborators.png](http://box.kancloud.cn/2015-07-09_559de3b2167d2.png)

图 4-17：添加项目合作者

图 4-17 为项目添加了两个合作者：supergirl 和 incredible。添加到项目当中的合作者会收到通知邮件，告知已经被加入到相应的项目当中。当新加入的项目合作者，如 incredible，登录 GitHub，在仪表板的版本库列表中会看到源自 gotgithub 用户的版本库，如图 4-18 所示。

![../images/prj-list-of-collab.png](http://box.kancloud.cn/2015-07-09_559de3b46988d.png)

图 4-18：合作者项目列表

从图 4-18 可以看出 GitHub 用户 incredible 自己创建的项目托管在自己的空间下，而作为合作者参与的项目仍然托管在原创建者（gotgithub）的空间下，这一点明显和派生（Fork）而来的项目不同。图 4-19 是以 incredible 登录 GitHub 访问 gotgithub/helloworld 的界面。

![../images/collab-view-helloworld.png](http://box.kancloud.cn/2015-07-09_559de3bab97ed.png)

图 4-19：以合作者身份访问项目

用户 incredible 对版本库 gotgithub/helloworld 拥有写入权限。和 gotgithub 用户稍有区别的是没有管理员权限。

## 4.2.2\. 与传统集中式工作模式的异同

传统的集中式版本控制系统，如 CVS、SVN，所有用户都访问同一个版本库。采用集中式工作模式的 GitHub 用户也同样是访问同一版本库。

对于由用户 gotgithub 创建的 helloworld 版本库，添加了合作者 supergirl 和 incredible，三个人克隆版本库使用如下命令。

*   用户 gotgithub 克隆版本库。

```
gotgithub$ git clone https://gotgithub@github.com/gotgithub/helloworld.git 
```

*   用户 supergirl 克隆版本库。

```
supergirl$ git clone https://supergirl@github.com/gotgithub/helloworld.git 
```

*   用户 incredible 克隆版本库。

```
incredible$ git clone https://incredible@github.com/gotgithub/helloworld.git 
```

传统集中式版本控制系统，所有的提交历史数据都在唯一的版本库中，各个提交是顺序进行的。为了防止多用户在提交时相互覆盖，集中式版本控制系统发明了很多方法 。有的采用编辑锁的形式（如 VSS），更改文件前对文件锁定，其他人禁止对文件进行访问（甚至无法读取），完成修改并提交后，文件解锁。有的如 SVN，允许多人同时编辑同一文件，但只有先进行提交的才能成功，后提交的会遇到“过时”错误，必须先获取版本库中的新增提交并和本地修改进行合并。即传统集中式版本控制系统，提交时必须和唯一的版本库所在的服务器保持连接，而且提交有可能会失败。

对于像 Git 这样的分布式版本控制系统，提交总是会成功，这是因为提交并不涉及和共享服务器的交互，是针对本地克隆版本库进行的本地操作。采用集中式的工作模式，共享版本库作为各个用户各自本地版本库数据交换、沟通的中介，只有在本地克隆版本库需要和共享版本库同步的时候才要和服务器建立连接。例如将本地所做的一个或多个提交推送到共享服务器，或者将服务器上新的提交获取到本地克隆版本库。

实际上无论采用分布式还是集中式的工作模式，Git 都好像工作在一个独立的分支上（克隆即分支），即使共享版本库和本地克隆版本库的分支名都叫做 master。如图 4-20，三个用户克隆 gitgithub/helloworld 版本库后，各自在本地执行了一次或多次提交。

![../images/workflow-commit.png](http://box.kancloud.cn/2015-07-09_559de3c7342f4.png)

图 4-20：在本地版本库中的提交

三个用户各自的提交都会非常顺利，但是一旦决定将本地提交推送到共享服务器时就可能遇到麻烦。用户 gotgithub 先执行推送，会非常顺利。如图 4-21 所示。

```
gotgithub$ git push 
```

![../images/workflow-commit-push.png](http://box.kancloud.cn/2015-07-09_559de3d282ab3.png)

图 4-21：用户 gotgithub 完成推送

而其他人就没有这么幸运了，会报告错误：遇到非快进式推送。Git 的推送操作就像 SVN 等集中式版本控制系统的提交操作那样，先执行者成功，后执行者糟糕（要解决冲突，自动或手动）。

## 4.2.3\. 合并后推送

当用户 gotgithub 完成推送后，共享版本库以及三个用户的本地版本库如图 4-21 所示。其中共享版本库变得和 gotgithub 用户的本地版本库相一致。此时如果用户 supergirl 执行推送，会遇到错误：非快进式推送。

```
supergirl$ git push
To https://supergirl@github.com/gotgithub/helloworld.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://supergirl@github.com/gotgithub/helloworld.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details. 
```

GitHub 并不对强制推送进行限制，但是用户 supergirl 不要用 git push -f 命令强制推送，因为那样会覆盖掉共享版本库中用户 gotgithub 的推送，正确的做法是获取共享版本库中新提交，并在本地版本库中和本地提交合并。即执行：

```
supergirl$ git fetch
supergirl$ git merge 
```

获取和合并操作过程如图 4-22 所示。

![../images/workflow-fetch-merge.png](http://box.kancloud.cn/2015-07-09_559de3dd73137.png)

图 4-22：合并操作示意图

实际上用户 supergirl 只需执行一条命令便可完成所有的操作：

```
supergirl$ git pull 
```

即：git pull = git fetch + git merge。

但是合并操作并不总是会成功，如果自动合并失败，会在暂存区对合并前后文件进行标识，工作区进入冲突解决状态，在冲突解决完成之前不能提交。Git 支持多种图形工具帮助完成冲突解决，执行如下命令，即可自动调用已安装的冲突解决工具。

```
supergirl$ git mergetool 
```

冲突解决完毕，执行提交即完成冲突解决。如果在冲突解决过程把本地文件搞得一团糟，随时可以取消合并操作。执行命令 git reset --hard 会取消冲突的合并让本地版本库回到合并之前的状态。

成功完成合并后将本地版本库中的提交推送到共享版本库：

```
supergirl$ git push 
```

完成推送后的版本库示意图如图 4-23 所示。

![../images/workflow-merge-push.png](http://box.kancloud.cn/2015-07-09_559de3df8d8d4.png)

图 4-23：完成合并后推送

## 4.2.4\. 合并还是变基

合并并非多个开发者的工作成果融合的唯一选择，有时甚至并非最佳选择。一方面合并会产生除了合并双方（或多方）所有提交外的一个新提交，增加了代码审核的负担，另一方面本地多个提交混杂一起与远程分支合并会更困难。在特定情况下，变基是合并之外的另一个选择。

图 4-24 展示用户 incredible 采用合并和变基两种不同解决方案的操作结果。图中右上是合并操作后的结果，右下是变基操作后的结果。

![../images/workflow-merge-or-rebase.png](http://box.kancloud.cn/2015-07-09_559de3e13b28d.png)

图 4-24：合并和变基结果比较

若用户 incredible 选择变基操作，执行命令如下：

*   获取远程版本库的提交到本地的远程分支。

```
incredible$ git fetch origin 
```

*   执行变基操作，将本地 master 分支的提交变基到新的远程分支中。

```
incredible$ git rebase origin/master 
```

如果一切顺利，变基后推送到共享版本库。

```
incredible$ git push 
```

推送后的版本库状态如图 4-25 所示。

![../images/workflow-rebase-push.png](http://box.kancloud.cn/2015-07-09_559de3e2b8a4f.png)

图 4-25：变基后推送

如果希望在执行 git pull 时自动使用 git rebase 取代默认的 git merge 操作，可以在 git pull 命令行添加参数--rebase 如下：

```
$ git pull --rebase 
```

或者通过配置变量设置当前分支使用变基策略，即每次执行 git pull 命令时对于 master 分支，采用变基操作取代默认的合并操作。

```
$ git config branch.master.rebase true 
```

如果希望本地所有克隆版本库在执行 git pull 时都改变默认行为，将变基作为首选，则如下设置全局变量。

```
$ git config --global branch.autosetuprebase true 
```

# 4.3\. 组织和团队

GitHub 在早期没有专门为组织提供账号，很多企业用户或大型开源组织只好使用普通用户账号作为组织的共享账号来使用。后来，GitHub 推出了组织这一新的账号管理模式，满足大型开发团队的需要。

*   组织账号是不能用来登录的，它包含一个 Owner（拥有者）用户组，只有属于这个组的用户在登录后，才能切换为组织的管理者。
*   可以创建任意多的团队（Team）即角色，对属于组织的用户进行管理。Owner Team 就是组织中权限最高的角色。
*   组织和用户一样可以创建项目，但是组织没有 SSH 公钥配置，也不能以组织的身份操作版本库。
*   组织没有工作描述之类的个人账号才拥有的属性。

## 4.3.1\. 创建新组织

组织是非登录账号，不能像创建普通登录账号那样直接创建，而是需要以 GitHub 用户身份登录，然后再创建自己的组织，创建者成为组织天然的管理者。

图 4-26 就是用户 gotgithub 登录后，通过点击右上角的账号设置图标进入账号设置界面，然后再点击菜单中的“Organizations”进入组织管理界面。

![../images/new-org-1.png](http://box.kancloud.cn/2015-07-09_559de41c2eb2e.png)

图 4-26：账号设置中的组织管理

在初始的组织管理界面中组织列表为空，即尚不属于任何组织。可以选择把当前用户 gotgithub 的账号转换为一个组织账号（前提是 gotgithub 的账号不属于任何组织）。提供这一账号迁移功能是因为在 GitHub 提供组织这一新功能之前，很多公司或团队以个人身份创建 GitHub 账号，但是以组织的形象出现，对于这类账号，GitHub 提供了由个人账号向组织账号迁移的途径。

在这里我们不进行这一迁移，而是以用户 gotgithub 的身份创建一个新的组织。点击“Create New Organization”按钮，显示创建组织表单，如图 4-27 所示。

![../images/new-org-2.png](http://box.kancloud.cn/2015-07-09_559de41e05651.png)

图 4-27：创建新组织

这里填写组织名为 GotGitOrg。创建组织还要选择一个付费方案，默认会选择免费的没有私有版本库的开源方案。

接下来为新建组织设定拥有者（Owner），如图 4-28 所示。当前用户，即正在创建组织的用户，理所当然成为组织拥有者之一。还可以为组织指派更多的组织拥有者，多个组织拥有者的权限并无差别，都可以管理组织，甚至可以将其他用户从拥有者团队中删除。

![../images/new-org-3.png](http://box.kancloud.cn/2015-07-09_559de42270a77.png)

图 4-28：指派组织拥有者

完成创建后，访问用户账号设置界面中的组织面板，如图 4-29 所示，列出当前用户所属的组织（GotGitOrg）。可以重新对组织进行设定，或者退出组织。注意因为当前用户已经属于一个以上的组织，所以右侧将当前用户转换为组织的按钮被置灰。

![../images/new-org-4.png](http://box.kancloud.cn/2015-07-09_559de426dfc70.png)

图 4-29：加入组织后的组织管理界面

## 4.3.2\. 组织管理

当用户 gotgithub 成为新建组织 GotGitOrg 的一员后，就可以在用户和组织的界面之间切换。点击页面左上角“github”文字图标进入仪表板界面。

![../images/dashboard-header.png](http://box.kancloud.cn/2015-07-09_559de427d660a.png)

图 4-30：用户仪表板界面

仪表板页面左上角“github”文字图标的下面就是用户上下文列表框。点击用户上下文下拉列表，如图 4-31 所示。

![../images/dashborad-context.png](http://box.kancloud.cn/2015-07-09_559de42d51aa1.png)

图 4-31：用户上下文切换列表

在用户上下文列表中选择组织 GotGitOrg 作为用户上下文后，则仪表板中显示的菜单和个人账号仪表板菜单略有不同，如图 4-32 所示。

![../images/org-dashboard.png](http://box.kancloud.cn/2015-07-09_559de42ee27ea.png)

图 4-32：组织 GotGitOrg 的仪表板界面

组织的仪表板界面与用户仪表板的不同之处在于增加了团队管理（Team）和组织管理（Organization Settings）。选择菜单中的“Team”进入团队管理界面，可以在组织中添加任意数量的团队。添加新团队的界面如图 4-33 所示。

![../images/new-team.png](http://box.kancloud.cn/2015-07-09_559de4304c53a.png)

图 4-33：添加新团队 iOS-Team

创建一个团队需要提供四个选项（如图 4-33）：

1.  团队名称。如：iOS-Team。
2.  团队成员。如：supergirl 和 omnidroid 两个用户作为团队成员。
3.  团队权限。有三个选择：只读（Pull Only）、读写（Push & Pull）、读写并管理（Push, Pull & Administrative）。
4.  授权版本库。可以添加一个或多个版本库，只有对授权的版本库才拥有指定权限。

其中团队授权中的只读授权对于免费组织账号创建的开源项目没有实际意义，因为开源项目人人可读，只有对于付费的组织账号创建的私密版本库才体现出价值。关于付费账号和私密版本库将在后面的章节介绍。接下来介绍如何在组织账号下创建版本库。

## 4.3.3\. 版本库管理

组织拥有独立的项目托管空间，点击页面左上角的“github”文字图标进入组织账号的仪表板界面。刚刚建立的组织账号的版本库尚未创建，点击图 4-32 所示的“New Repository”按钮，创建版本库（即项目）。

新建版本库的界面如图 4-34 所示。

![../images/new-prj-for-org.png](http://box.kancloud.cn/2015-07-09_559de430f3111.png)

图 4-34：新建项目界面

在组织的托管空间创建项目与在普通用户的空间下创建稍有不同，增加了团队设置下拉框。图 4-34 显示在创建名为 MyiPad 项目时，只能为项目指派一个已定义团队，要想为项目指派更多团队可以在项目创建完毕通过项目管理界面添加。

下面来看一看如何为已建立项目指派更多的团队。进入项目管理页面，点击左侧菜单项“Team”显示项目的团队管理界面，可以通过该界面，为项目添加和移除团队，如图 4-35 所示。

![../images/org-prj-admin-teams.png](http://box.kancloud.cn/2015-07-09_559de43362bbe.png)

图 4-35：项目的团队管理

属于团队的项目（版本库）可以转移给个人，反之亦然。图 4-36 展示了如何通过项目管理界面在用户和组织之间转移项目（版本库）。

![../images/org-prj-admin-transfer.png](http://box.kancloud.cn/2015-07-09_559de434a204d.png)

图 4-36：项目转移

## 4.3.4\. 个人还是组织

若使用“Fork + Pull”的工作模式，通过个人账号还是组织账号托管版本库，几乎没有什么差别。如果一定要找出点不同，那就是在向托管版本库提交 Pull Request 时，邮件通知的用户范围有所不同。

*   对于个人账号，对其托管空间内的版本库发出 Pull Request，通知邮件会发送给该个人账号及该版本库设置的所有协作者（如果有的话）的邮箱。
*   对于组织，对其托管空间内的版本库发出 Pull Request，不会向组织的邮箱发送 Pull Request，也不会向组织的所有者（Owner 团队）发送通知邮件，而是向在版本库中拥有 Push 权限的团队（非 Owner 团队）成员发送通知邮件。

因此，如果在组织的托管空间创建版本库，一定要要为版本库指派一个拥有 Push 权限的团队，以免以“Fork + Pull”模式工作时，Pull Request 没有人响应。

若是以共享版本库方式（即集中式协同模式）工作的话，使用组织来托管版本库会比使用个人账号托管有效率得多。

*   以个人账号托管，需要逐一为版本库设置协作者（Collaborators），如果版本库较多且授权相同，配置过程繁琐且易出错。
*   以组织方式托管，将用户分组，划分为一个一个的团队（Team），以团队为单位授权则方便得多。
*   如果是以付费账号创建的私密版本库，使用组织方式管理，会有包括只读、读写等更丰富的授权类型，更符合项目管理的实际。

# 4.4\. 代码评注

针对项目的每一次 Pull Request 就相当于一次代码评审，评审以讨论的形式显示在 Pull Request 中。

在 Pull Request 中还能够看到对应的提交（一个或多个），并可以直接针对提交进行代码评注。对于采用集中式协同的项目，即使较少使用 Pull Request，也同样可以使用代码评注。代码评注会触发通知邮件给项目的开发者。

代码评注有两种形式，一种是针对整个提交的评注，另外一种是对代码进行逐行评注。

## 4.4.1\. 提交评注

查看项目的提交历史，从中选择一个提交，如图 4-37 所示。

![../images/commit-history.png](http://box.kancloud.cn/2015-07-09_559de44b713f9.png)

图 4-37：helloworld 项目提交历史

如图 4-38 是查看提交的界面。除了提交说明、提交者信息之外，还显示提交所修改的文件和改动差异。在查看提交页面的最下方显示一个提交评注对话框，可以在其中写下评注。评注可以使用 Markdown 语法。

![../images/commit-note.png](http://box.kancloud.cn/2015-07-09_559de44f87839.png)

图 4-38：添加提交评注

添加评注后，所评注的提交的作者会收到通知邮件，提醒针对自己的提交有了新的评论。通知邮件如图 4-39 所示。

![../images/commit-note-email-notify.png](http://box.kancloud.cn/2015-07-09_559de455894a5.png)

图 4-39：提交评注的通知邮件

通过 Web 界面可以看到添加在提交下方的评注，并可以撰写新的评注展开讨论。评注者本人或提交的作者还可以编辑甚至删除评注。如图 4-40 所示。

![../images/commit-note-admin.png](http://box.kancloud.cn/2015-07-09_559de45741733.png)

图 4-40：提交评注

GitHub 还支持 Git 自身提供的评注功能[[1]](http://www.kernel.org/pub/software/scm/git/docs/git-notes.html)，如图 4-41 所示的是提交[`git.io/git-notes`](http://git.io/git-notes)(即网址 [`github.com/ossxp-com/gitdemo-commit-tree/commit/e80aa74`](https://github.com/ossxp-com/gitdemo-commit-tree/commit/e80aa74))的评注，这个评注并非通过 GitHub 添加的，而是由 git-note 命令提交的评注。这种评注针对一个特定提交只能有一个，GitHub 只能显示不能编辑和删除。关于如何通过命令行查看 git-note 格式的评注，参见《Git 权威指南》第 570 页“41.5 Git 评注”。

![../images/commit-note-by-git-note.png](http://box.kancloud.cn/2015-07-09_559de45d50e02.png)

图 4-41：git-note 评注

## 4.4.2\. 逐行评注

还是以 gotgithub/helloworld 版本库中的提交为例，看一下 GitHub 支持的逐行评注功能，即针对提交中的任意一行添加评注。浏览提交，如图 4-42 所示，当鼠标置于任意一行代码时，在该行代码的左侧会显示一个添加注释的图标。

![../images/commit-line-note-btn.png](http://box.kancloud.cn/2015-07-09_559de45e0e1a7.png)

图 4-42：添加逐行评注按钮

点击该图标（用于添加逐行评注的图标），会显示如图 4-43 所示的添加逐行评注对话框。该评注对话框出现在两行代码之间，在其中写下评注。

![../images/commit-line-note-form.png](http://box.kancloud.cn/2015-07-09_559de45f090b9.png)

图 4-43：添加逐行评注

添加评注后，项目的开发人员同样会收到邮件通知。针对同一行代码的多次评论按时间顺序依次显示，图 4-44 展示了多个行间评注，其中一个评注还使用 Markdown 语法嵌入了一个图片。

![../images/commit-line-note.png](http://box.kancloud.cn/2015-07-09_559de46019001.png)

图 4-44：逐行评注和提交评注

更有意思的评注可以围观 MrMEEE/bumblebee 项目的一个 bug 修正提交（被戏称一个空格引发的惨案）。地址： [`git.io/giant-bug`](http://git.io/giant-bug)(即网址 [`github.com/MrMEEE/bumblebee/commit/a047be85247755cdbe0acce6`](https://github.com/MrMEEE/bumblebee/commit/a047be85247755cdbe0acce6))。

# 4.5\. 缺陷跟踪

缺陷跟踪（Bug Tracking）是软件研发流程中重要的一环，集项目需求管理和缺陷管理于一身，通过对研发工作流的控制帮助团队建立规范的研发体系。GitHub 提供轻量级的缺陷跟踪模块，称为 Issues。小巧、易用的 Issues 模块能与 Pull Request 紧密整合，是 Pull Request 工作流的有益补充。

一个小型、管理文档和网页的项目，使用 Pull Request 往往就足够了。试想如果贡献者能够直接修改代码（Fork and edit this file）并通过 Pull Request 贡献给项目核心开发者，那么为什么还要通过 Issues 模块报告错误并由他人来更改呢？但是对于大型项目需要做需求管理，或者参与代码开发有难度，则非常有必要通过 Issues 模块启用缺陷跟踪系统，提供更多途径让贡献者参与到项目中来。

缺陷跟踪可以通过项目的管理页面开启或关闭，如图 4-45 所示。

![../images/project-admin-features-issue.png](http://box.kancloud.cn/2015-07-09_559de47b4afe6.png)

图 4-45：开启或关闭 Issues 模块

## 4.5.1\. 标签

缺陷跟踪系统通常可用于管理多种不同类型的问题：需求、缺陷或其它，也可以通过项目不同模块、组件来为问题分类。GitHub 在问题分类的实现上非常简单，通过标签（label）来为问题建立分类。

开启 Issues 模块后，项目的菜单中多出一个“Issues”项，点击则进入问题浏览界面，如图 4-46 所示。左侧的边栏是问题过滤器，由上至下分为三个部分。最上面的过滤器根据问题的所有者对问题进行筛选，默认选择所有人的问题。中间的过滤器是根据里程碑对问题进行筛选，默认未选定任何里程碑（初始尚未创建任何里程碑），不对问题进行里程碑过滤。最下面的过滤器依据问题标签对问题进行筛选，初始标签尚未创建。

![../images/issue-no-label.png](http://box.kancloud.cn/2015-07-09_559de4872c019.png)

图 4-46：尚未定义标签

鼠标点击左侧边栏标签过滤器中的新建标签的文本框，显示如图 4-47 所示的新建标签界面。输入新的标签名，并为标签选择一个颜色，创建新的标签。

![../images/issue-new-label.png](http://box.kancloud.cn/2015-07-09_559de487eccf9.png)

图 4-47：创建新标签

创建的标签显示在过滤器中，如图 4-48 所示。点击过滤器中的标签则对问题进行筛选，取消过滤器可以点击图中标记的“Clear active milestone and label filters”链接。

![../images/issue-labels.png](http://box.kancloud.cn/2015-07-09_559de488e767e.png)

图 4-48：过滤器中的标签选项

标签过滤器下方的“Manage Labels”按钮用于管理标签，下面的输入框用于创建新的标签。

## 4.5.2\. 里程碑

里程碑（Milestones）是项目进度管理的重要工具。在传统项目管理中，里程碑对应于一个项目开发计划、一个软件版本；在敏捷项目管理中，里程碑对应于一个 Sprint（冲刺）；在软件代码的版本库中则对应于一个标签（tag）或分支（branch）。

在 Issues 模块中的“Milestones”页面用于里程碑管理。创建新的里程碑需要输入里程碑名称和里程碑的截止时间，如图 4-49 所示。

![../images/issue-new-milestone.png](http://box.kancloud.cn/2015-07-09_559de48b41cc4.png)

图 4-49：创建新里程碑

创建的里程碑以进度条形式显示在里程碑页面中，如图 4-50 所示定义了两个里程碑。这两个里程碑的时间跨度定义的太长，敏捷的项目管理从来不这么定义。

![../images/issue-milestones.png](http://box.kancloud.cn/2015-07-09_559de48d292b7.png)

图 4-50：里程碑列表

## 4.5.3\. Issue 的生命周期

GitHub 的 Issues 模块非常简单，对标签和里程碑进行简单的设置后，基本上就完成了 Issues 模块的配置工作，接下来就是如何创建和修改 Issue，完成项目的缺陷跟踪和需求管理等，这才是 Issues 模块的主要工作。

每个 Issue 都有自己的生命周期，从问题的创建，到问题的指派，再到问题的解决，直至问题的关闭。图 4-51 就是以普通贡献者身份为项目创建 Issue。

![../images/issue-new-by-non-member.png](http://box.kancloud.cn/2015-07-09_559de48f19cae.png)

图 4-51：以普通贡献者身份创建问题

录入问题标题和描述后，点击“Submit new issue”按钮，完成问题创建。图 4-52 显示了新建立的问题，可以看出新建问题尚未设置标签。

![../images/issue-created.png](http://box.kancloud.cn/2015-07-09_559de48fe4cd6.png)

图 4-52：新创建的问题尚未添加标签等

普通贡献者创建问题时只能录入问题的标题和描述，而不能设置问题的指派（谁来负责）、添加标签和设置里程碑。如果希望问题通知到特定的开发者，可以在问题描述中以“@用户名”的方式通知到该用户[[1]](https://github.com/blog/821-mention-somebody-they-re-notified)，这也是众多社交软件通行的做法。

项目成员创建问题时，拥有更大权限，也有更多的可选项。如图 4-53 所示。

![../images/issue-new-by-member.png](http://box.kancloud.cn/2015-07-09_559de49177e92.png)

图 4-53：以项目成员身份创建问题

完成上述两个问题的创建后，问题浏览界面显示新创建的两个问题，一个以项目成员身份创建的问题已经被设置了“缺陷”的标签，而另外一个问题则没有设置任何标签。如图 4-54 所示。

![../images/issue-list.png](http://box.kancloud.cn/2015-07-09_559de4929cceb.png)

图 4-54：所有问题列表

以项目成员身份登录，在问题浏览界面即可为问题重新设定标签，指派负责人，设置里程碑，以及关闭问题等。如图 4-55 所示。

![../images/issue-update.png](http://box.kancloud.cn/2015-07-09_559de4b476e87.png)

图 4-55：为问题添加指派、里程碑和标签

在问题浏览页面的过滤器中选择里程碑”Version 4.0“，可以看到两条问题都显示出来，这是因为这两条问题都属于该里程碑。里程碑的进度条显示进度为零，这是因为里程碑所包含的全部（两个）问题都处于打开状态，尚未解决。如图 4-56 所示。

![../images/issue-list-with-milestone.png](http://box.kancloud.cn/2015-07-09_559de4b61a198.png)

图 4-56：通过里程碑筛选问题

邮件通知功能是缺陷跟踪系统推动工作流的重要工具，GitHub 的 Issues 模块也具有邮件通知功能。除了像其他缺陷跟踪系统在收到邮件通知后，访问 Web 界面参与问题的讨论外，还可以直接以邮件回复的功能参与到工作流中[[2]](https://github.com/blog/811-reply-to-comments-from-email)。

GitHub 还支持版本库提交和问题建立关联，只要提交说明中出现“#xxx”（Issue 编号）字样。如果在提交说明中的问题编号前出现特定关键字，还可以关闭问题。支持的关键字有：

*   fixes #xxx
*   fixed #xxx
*   fix #xxx
*   closes #xxx
*   close #xxx
*   closed #xxx

下面就以 gotgithub/helloworld 版本库为例，关闭编号为“#1”的问题。

*   克隆版本库，若本地工作区尚不存在。

```
$ git clone git@github.com:gotgithub/helloworld.git
$ cd helloworld 
```

*   编辑文件 src/main.c，改正“问题#1”发现的文字错误。

```
$ vi src/main.c
$ git diff
diff --git a/src/main.c b/src/main.c
index 3daf9fe..f974b49 100644
--- a/src/main.c
+++ b/src/main.c
@@ -19,7 +19,7 @@ int usage(int code)
            "            say hello to the world.\n\n"
            "    hello -v, --version\n"
            "            show version.\n\n"
-           "    hello -h, -help\n"
+           "    hello -h, --help\n"
            "            this help screen.\n\n"), _VERSION);
     return code;
 } 
```

*   将修改添加至暂存区。

```
$ git add -u 
```

*   提交，并在提交说明中用 fixed #xxx 关键字关闭相关问题。

```
$ git commit -m "Fixed #1: -help should be --help." 
```

*   向 GitHub 版本库推送。

```
$ git push 
```

推送完毕后，在问题浏览界面可以看到里程碑“Version 4.0”的进度已经完成了一半，即其中一个问题（#1）已经完成并关闭。如图 4-57 所示。

![../images/issue-milestone-half-closed.png](http://box.kancloud.cn/2015-07-09_559de4b9b73ef.png)

图 4-57：关闭一个问题，里程碑完成 50%

查看已经完成的问题（#1），可以看到其中关联到一个提交，该提交正是我们刚刚创建的。如图 4-58 所示。

![../images/issue-closed-by-commit.png](http://box.kancloud.cn/2015-07-09_559de4bb55769.png)

图 4-58：已关闭问题中的提交链接

点击关联的提交，显示如图 4-59 的提交界面，出现在提交说明中的问题编号也可点击，指向缺陷追踪系统中该问题的链接。

![../images/commit-link-to-issue.png](http://box.kancloud.cn/2015-07-09_559de4bcdcddc.png)

图 4-59：提交中的问题链接

## 4.5.4\. Pull Requst 也是 Issue

Pull Request 和 Issue 一样，也是一种对项目的反馈，而且是更为主动的反馈。GitHub 的 Issues 模块将 Pull Request 也纳入到问题的管理之中，完美地将 Pull Request 整合到问题追踪的框架之中。

为了弄清二者之间的关联，首先创建一个 Pull Request。

以非项目成员（如用户 omnidroid）的账号访问 gotgithub/helloworld 项目，查看文件 src/Makefile，点击“Fork and edit this file”按钮快速创建派生项目，如图 4-60 所示。

![../images/fork-and-edit-btn-for-issue.png](http://box.kancloud.cn/2015-07-09_559de4bf16a1c.png)

图 4-60：在线编辑并创建派生项目

通过 GitHub 提供的在线编辑功能修改 src/Makefile 文件，修改完毕后撰写提交说明，点击“Propose File Change”按钮提交。如图 4-61 所示。

![../images/fork-and-edit-form-for-issue.png](http://box.kancloud.cn/2015-07-09_559de4bfd8e03.png)

图 4-61：在线编辑并提交

在提交说明中特意使用了“Fixed #2”关键字，以便该提交被上游版本库接纳后能够关闭关联的问题。

当完成提交后，GitHub 会自动开启创建新的 Pull Request 对话框，如图 4-62 所示。

![../images/new-pull-request-for-issue.png](http://box.kancloud.cn/2015-07-09_559de4c9c5d0d.png)

图 4-62：创建 Pull Request

Pull Request 创建完毕后，除了在菜单项“Pull Requests”中有显示外，在“Issues”的问题浏览页面中也会显示。如图 4-63 所示，新建立的 Pull Request 的编号不是从壹开始创建，而是接着问题的编号顺序创建，所以当 Pull Request 出现在问题列表中时，如果不注意后面的山型的分支图标，根本意识不到这不是一个普通的问题（Issue），而是一个 Pull Request。

![../images/issue-list-with-pull-request.png](http://box.kancloud.cn/2015-07-09_559de4cd0bac5.png)

图 4-63：Pull Request 也显示在 Issues 中

显示在问题浏览界面中的 Pull Request 和问题一样，可以为其设置标签、指派负责人、设置里程碑。如图 4-64 所示。

![../images/pull-request-update-as-issue.png](http://box.kancloud.cn/2015-07-09_559de4cfdc567.png)

图 4-64：可以像更新其他 Issue 那样更新 Pull Request

当 Pull Request 归类到里程碑“Version 4.0”中时，在过滤器打开里程碑“Version 4.0”，可以看到本来已经完成 50%的进度，由于新增了一个“问题”（Pull Request），导致进度降低了。如图 4-65 所示。

![../images/milestone-progress-with-pull-request.png](http://box.kancloud.cn/2015-07-09_559de4d7e40ba.png)

图 4-65：里程碑进度调整

点击编号为“#3”的问题（Pull Request），会进入到 Pull Request 页面。点击页面中的“Merge pull request”按钮实现 Pull Request 的合并。如图 4-66 所示。

![../images/merge-pull-request-for-issue.png](http://box.kancloud.cn/2015-07-09_559de4d981f4b.png)

图 4-66：在线合并 Pull Request

点击“Confirm Merge”确认合并，如图 4-67 所示。

![../images/merge-pull-request-for-issue-confirm.png](http://box.kancloud.cn/2015-07-09_559de4db11be9.png)

图 4-67：确认合并 Pull Request

完成合并后，查看该 Pull Request，可以看到该 Pull Request 已经关闭。如图 4-68 所示。

![../images/pull-request-close-for-issue.png](http://box.kancloud.cn/2015-07-09_559de4dbd57c0.png)

图 4-68：Pull Request 自动关闭

如果再回到问题浏览界面，能够猜到现在里程碑“Version 4.0”的进度是多少么？

由于关闭了编号为“#3”的 Pull Request，以及所合并的 Pull Request 中对应提交的提交说明的指令同时关闭了编号为“#2”的问题，所以现在里程碑“Version 4.0”关联的所有问题均已关闭。里程碑也显示已关闭，即里程碑完成度为 100%。

![../images/milestone-closed.png](http://box.kancloud.cn/2015-07-09_559de4df4ff34.png)

图 4-69：里程碑关闭

# 4.6\. 维基

维基是 Web 协同著作平台，可以让任何浏览网页的人都能够方便地参与网页的编辑和创建。这源自于维基如下魔力：

*   快速更改。修改网页无需复杂的后台修改和网页部署流程，浏览的网页直接提供编辑按钮，任何查看网页的用户均可在线编辑网页。
*   简洁语法。编写网页不需要学习复杂的 HTML，取而代之的是易学易用的格式化文本（维基语法），有的维基还提供图形化编辑界面。
*   版本控制。熟悉 Git 的人，可以把维基看作是 Web 的版本控制。历次修改都记录在案，历史修订可进行比较，可恢复到历史版本等。
*   维基链接。页面链接使用[[页面名称]]语法，可以非常方便地创建新页面，并实现页面间的互联。

GitHub 提供了维基模块，方便项目团队创建社区驱动和维护的项目文档。

## 4.6.1\. 维基初始化

GitHub 的维基模块可以通过项目管理页面控制开启或关闭，默认开启，如图 4-70 所示。因为 GitHub 提供了项目展示的多种途径，一些小项目如果觉得用 README 文件构建项目说明，或者用 gh-pages 分支维护项目主页就足够了，大可关闭维基模块。

![../images/project-admin-features-wiki.png](http://box.kancloud.cn/2015-07-09_559de501d2795.png)

图 4-70：开启或关闭 Wiki 模块

项目启用维基后，进入维基页面，如图 4-71 所示，会发现维基页面并没有自动创建，还需要进行初始化。

![../images/wiki-not-exist.png](http://box.kancloud.cn/2015-07-09_559de507210cd.png)

图 4-71：尚未初始化的维基界面

点击“Create Wiki Now”按钮，自动创建维基首页，如图 4-72 所示。

![../images/wiki-home-init.png](http://box.kancloud.cn/2015-07-09_559de50a8493d.png)

图 4-72：自动创建的维基首页

## 4.6.2\. 使用维基

自动创建的维基首页只有非常简单的信息，点击编辑按钮，修改维基首页。编辑界面如图 4-73 所示。

![../images/wiki-edit-init.png](http://box.kancloud.cn/2015-07-09_559de513d7959.png)

图 4-73：编辑维基首页

编辑界面有 6 个部分需要重点说明：

1.  页面名称。首页的页面名称为“Home”，不能随意更改，否则无法找到首页，或者页面之间的跳转会失效。
2.  工具条。从左至右分别是设置一级标题、二级标题、三级标题、插入链接、图片、字体加粗、斜体、代码块、列表、编号列表、引用、水平分割线等。
3.  页面语法格式。默认采用 Markdown 语法，还可以选用 AsciiDoc、Creole 等语法。注意如果改变语法格式，该维基页面的内容需要手工进行调整，而且页面的实际存储文件的文件扩展名会改变。
4.  语法帮助。当按下工具条中的帮助按钮会显示本语法帮助表格。
5.  维基内容编辑框。整个维基页面的内容都在这个编辑框中。鼠标拖动该编辑框右下角可以对编辑框大小进行缩放。
6.  可选的修改说明。修改页面时提供说明便于跟踪对页面的历史修订。

对初始创建的首页进行更改，如图 4-74 所示。

![../images/wiki-edit-format.png](http://box.kancloud.cn/2015-07-09_559de51603e78.png)

图 4-74：修改选中文本样式

图 4-74 中，在维基内容编辑框中选择两行文本，然后点击工具栏中的列表按钮，为选中内容应用列表样式。应用新样式后的效果如图 4-75 所示。然后填写提交说明，点击“Save”按钮保存更改。

![../images/wiki-edit-save.png](http://box.kancloud.cn/2015-07-09_559de516edff6.png)

图 4-75：填写提交说明保存更改

注意图 4-75 的维基内容中有[[页面名称]]样式的语法，这个语法是维基特色的页面链接语法，指向另外的维基页面。实际页面输出如图 4-76 所示。

![../images/wiki-home-simple-wiki-links.png](http://box.kancloud.cn/2015-07-09_559de5183948c.png)

图 4-76：保存更改后的维基首页

如果对维基语法[[页面名称]]生成的链接标题不满意，还可以用[[链接标题|页面名称]]格式创建维基链接。对首页重新做一次修改，修改如下：

*   将[[HowtoClone]]改为[[how to clone|HowtoClone]]。
*   将[[HowtoContribute]]改为[[how to contribute|HowtoContribute]]。

修改后的首页效果如图 4-77 所示。

![../images/wiki-home-update-wiki-links.png](http://box.kancloud.cn/2015-07-09_559de51943add.png)

图 4-77：修改维基链接标题后的首页

无论怎样更改维基页面都不怕内容丢失，因为维基记录了每一次修订历史，并可以回退任意一次修改。点击维基页面中的“Page History”按钮，查看页面修订历史，如图 4-78 所示。

![../images/wiki-page-history.png](http://box.kancloud.cn/2015-07-09_559de51a2a6ec.png)

图 4-78：页面修订历史

首页的修订历史记录着维基初始化以来所有的修改，包括修改者、修改时间、提交说明，以及一个可点击的对象 ID。点击对象 ID 查看对应版本的页面。还可以对不同版本的页面进行比较，选中两个版本点击“Compare Revisions”按钮，如图 4-79 所示。

![../images/wiki-page-compare.png](http://box.kancloud.cn/2015-07-09_559de51cebf01.png)

图 4-79：页面版本间比较

在页面版本间的比较界面中，提供回退此次修改的按钮。点击“Revert Changes”按钮（图 4-79 所示），可以回退对首页的修改。查看首页的修订历史，会看到回退记录也显示其中，如图 4-80 所示。

![../images/wiki-reverted.png](http://box.kancloud.cn/2015-07-09_559de51e652c0.png)

图 4-80：包含回退记录的页面修订历史

在维基中创建新页面有多种方法，可以点击页面中的“New Page”按钮，也可以像我们之前做的那样先在页面中用[[页面名称]]格式嵌入维基链接，然后在生成的页面中可以看到指向新页面的链接，当然这些链接所指向的页面并不存在。

![../images/wiki-newpage.png](http://box.kancloud.cn/2015-07-09_559de51f366e1.png)

图 4-81：页面中的维基链接

如图 4-81 所示，点击页面中指向不存在维基页面的链接，会自动开启创建新页面的对话框，如图 4-82 所示。

![../images/wiki-newpage-edit.png](http://box.kancloud.cn/2015-07-09_559de52677fa8.png)

图 4-82：创建新维基页面

输入维基页面的内容，然后填写提交说明，点击“Save”按钮，保存新页面。生成的新页面如图 4-83 所示。

![../images/wiki-newpage-created.png](http://box.kancloud.cn/2015-07-09_559de527d13e1.png)

图 4-83：生成的新页面

如果当前用户对页面具有写权限，则在页面左下角会看到一个删除本页面的链接。点击“Delete this page”链接并经确认后会删除页面。然后继续在维基中操作，如创建另外一个新页面 HowtoContribute。

如果对之前删除页面 Howtoclone 的操作后悔，可以通过下面方法找回。

*   访问菜单中的“Wiki History”项，显示整个维基的修订记录（不是某个页面的修订记录）。如图 4-84 所示。

    ![../images/wiki-history.png](http://box.kancloud.cn/2015-07-09_559de529dbfea.png)

    图 4-84：维基修订记录

*   从图 4-84 可见上面第二条记录就是删除 HowtoClone 页面的操作，选择该记录及前一次记录，执行版本比较，如图 4-85 所示。

    ![../images/wiki-compare-drop-page.png](http://box.kancloud.cn/2015-07-09_559de52b59fac.png)

    图 4-85：版本比较

*   点击“Revert Changes”按钮，可以取消页面删除动作。更新后的维基修订历史如图 4-86 所示。

    ![../images/wiki-page-restored.png](http://box.kancloud.cn/2015-07-09_559de53a4fb6d.png)

    图 4-86：还原修订后的维基修订记录

*   查看维基页面列表，可以看到页面 Howtoclone 已经被找回。

    ![../images/wiki-pages.png](http://box.kancloud.cn/2015-07-09_559de53c273ef.png)

    图 4-87：维基页面列表

## 4.6.3\. 维基与 Git

随着对 GitHub 维基的深入使用，可能会遇到下面的问题：如何嵌入图片？多人编辑时如何避免冲突？解决这几个问题的办法就是用 Git 操作维基。在之前查看维基修订历史，以及进行版本间比较时可能已经看出和 Git 是如何的相似，实际上 GitHub 的维基页面就是用 Git 版本库实现的。

在维基页面访问菜单中的“Git Access”项，会看到用 Git 访问维基页面的方法。如图 4-88 所示。

![../images/wiki-git-access.png](http://box.kancloud.cn/2015-07-09_559de53dd869c.png)

图 4-88：用 Git 访问维基

对于项目 gotgithub/helloworld 来说，用 Git 克隆其维基，用如下命令：

```
$ git clone git@github.com:gotgithub/helloworld.wiki.git 
```

进入到刚刚克隆的 helloworld.wiki 工作区中，查看包含的文件，会看到有三个文件。

```
$ cd helloworld.wiki
$ ls
Home.md            Howtoclone.creole  Howtocontribute.md 
```

三个文件对应于三个维基页面，文件名就是维基的页面名称，而扩展名对应于采用的维基语法。以.md 扩展名结尾的页面采用 Markdown 语法，而以.creole 结尾的文件采用 Creole 标准维基语法。

下面就通过 Git 在维基版本库中添加一个图片。添加图片的操作只通过 GitHub 维基的 Web 界面是很难实现的，而使用 Git 则易如反掌。

*   创建一个名为 images 目录。这个目录并非必须，只是为了易于管理。

```
$ mkdir images
$ cd images 
```

*   在 images 目录中添加图片。

    下面的操作从 GitHub 官方版本库中下载图片 octocat.png 并进行适当缩放。

```
$ wget https://github.com/github/media/raw/master/octocats/octocat.png
$ mogrify -resize '200' octocat.png 
```

*   将图片添加到暂存区并提交。

```
$ git add octocat.png
$ git commit -m "add sample image." 
```

*   将本地提交推送到 GitHub 远程版本库。

```
$ git push 
```

完成推送后，访问下面的网址可以看到刚刚上传的图片：

[`github.com/gotgithub/helloworld/wiki/images/octocat.png`](https://github.com/gotgithub/helloworld/wiki/images/octocat.png)

接下来在维基页面中引用图片。嵌入图片的 Markdown 语法是：![Alt text here](img/Image%C2%A0URL%C2%A0here)。当然可以通过编辑本地版本库 gotgithub/helloworld.wiki.git 中的文件，但通过 GitHub 维基编辑界面嵌入图片无需记忆复杂的语法。如图 4-89 所示。

![../images/wiki-insert-image.png](http://box.kancloud.cn/2015-07-09_559de53f12e9b.png)

图 4-89：在维基页面中嵌入图片

点击“Preview”按钮，可以在保存前查看效果。在图 4-90 所示的预览界面中可以看到修改后的效果。

![../images/wiki-insert-image-preview.png](http://box.kancloud.cn/2015-07-09_559de5406159f.png)

图 4-90：预览效果

多人同时编辑一个维基页面会引起冲突，先提交的用户会成功，其他用户的编辑界面马上会显示冲突警告，并且保存按钮也被置灰，如图 4-91 所示。

![../images/wiki-edit-conflict.png](http://box.kancloud.cn/2015-07-09_559de54195384.png)

图 4-91：编辑冲突

GitHub 的维基编辑界面没有提供冲突解决的工具，而利用 Git 本身强大的冲突解决功能可以很容易地解决这一难题。

例如用户 gotgithub 编辑维基首页 Home 遇到编辑冲突，为防止数据丢失，先将编辑框中的维基文本拷贝并粘贴到一个临时文件中，如文件中/path/to/draft.md。然后进行如下操作将 draft.md 中内容合并到维基页面中。

*   如果本地已经克隆维基版本库，则执行下面命令更新。

```
$ cd helloworld.wiki
$ git pull 
```

*   如果没有，则克隆维基版本库。

```
$ git clone git@github.com:gotgithub/helloworld.wiki.git
$ cd helloworld.wiki 
```

*   用 Git 命令查看版本库的历史，以便找出发生冲突的原始版本。

    从下面的输出可以看出我们编辑的版本是基于提交 fbb4bb4，由于用户 incredible 先于我们完成了对维基页面的修改以致发生了冲突。

```
$ git log -3 --pretty=short
commit 5ff5d998bb6cf99337813915282df94701d17ea0
Author: incredible 

    Add a note as image link broken if url without a end slash.

commit fbb4bb4f330bacf765d51736359b0a3e81ed945b
Author: gotgithub 

    Insert image in page.

commit 94182c2b57ebce1f1bf8a310f78df87ae8e8219a
Author: gotgithub 

    add sample image. 
```

*   基于提交 fbb4bb4 建立分支，如分支 mywiki。

```
$ git checkout -b mywiki fbb4bb4 
```

*   将保存的 draft.md 覆盖欲修改的文件，如 Home.md。

```
$ cp /path/to/draft.md Home.md 
```

*   提交修改。

```
$ git add -u
$ git commit -m "Use absolute image link." 
```

*   切换到 master 分支。

```
$ git checkout master 
```

*   合并我们在 mywiki 分支的修改。

```
$ git merge mywiki
Auto-merging Home.md
CONFLICT (content): Merge conflict in Home.md
Automatic merge failed; fix conflicts and then commit the result. 
```

*   调用图形工具解决冲突。

```
$ git mergetool 
```

*   提交并查看合并后的提交关系图。

```
$ git commit -m "merge with incredible's edit."
$ git log --oneline --graph -4
*   d33b55a merge with incredible's edit.
|\
| * 121c3b2 Use absolute image link.
* | 5ff5d99 Add a note as image link broken if url without a end slash.
|/
* fbb4bb4 Insert image in page. 
```

*   查看用户 incredible 的修改。

```
$ git show --oneline HEAD¹
5ff5d99 Add a note as image link broken if url without a end slash.
diff --git a/Home.md b/Home.md
index 6ada8e8..0bca3ec 100644
--- a/Home.md
+++ b/Home.md
@@ -1,5 +1,7 @@
 This is a sample project for the book "GotGitHub".

+**Note**: if can not see the following image, add a slash('/') at the end of the URL.
+
 ![GitHub Octocat](img/octocat.png)

 ## HOWTOs 
```

*   查看用户 gotgithub 的修改。

```
$ git show --oneline HEAD²
121c3b2 Use absolute image link.
diff --git a/Home.md b/Home.md
index 6ada8e8..cdb9167 100644
--- a/Home.md
+++ b/Home.md
@@ -1,6 +1,6 @@
 This is a sample project for the book "GotGitHub".

-![GitHub Octocat](img/octocat.png)
+![GitHub Octocat](https://github.com/gotgithub/helloworld/wiki/images/octocat.png)

 ## HOWTOs
 * To access the repository of this project see: [[HowtoClone]] 
```

*   将本地合并后的版本库推送到 GitHub。

```
$ git push 
```

再来看看推送后 GitHub 的维基修订历史，和本地版本库看到的历史是一致的，如图 4-92 所示。

![../images/wiki-pushed-history.png](http://box.kancloud.cn/2015-07-09_559de5432437f.png)

图 4-92：推送后的维基修订历史

GitHub 维基背后的引擎名为 Gollum，GitHub 已将其开源，项目网址：[`github.com/github/gollum`](https://github.com/github/gollum) 。安装 Gollum，在克隆的维基版本库中运行 gollum 就可以在本地启动维基服务。