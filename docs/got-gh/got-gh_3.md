# 3\. 项目托管

本章介绍如何在 GitHub 上创建一个新项目，包括创建版本库及为项目设计主页等。

# 3.1\. 创建新项目

## 3.1.1\. 新版本库即是新项目

在 GitHub，一个项目对应唯一的 Git 版本库，创建一个新的版本库就是创建一个新的项目。访问仪表板（Dashboard）页面，如图 3-1，可以看到关注的版本库中已经有一个，但自己的版本库为零。在显示为零的版本库列表面板中有一个按钮“New Repository”，点击该按钮开始创建新版本库。

![../images/new-repo-btn.png](http://box.kancloud.cn/2015-07-09_559de28541b71.png)

图 3-1：版本库列表面板

新建版本库的界面如图 3-2 所示。

![../images/new-project.png](http://box.kancloud.cn/2015-07-09_559de28606de3.png)

图 3-2：创建新项目

我们为新建立的版本库命名为“helloworld”，相应的项目名亦为“helloworld”，创建完毕后访问项目页，提示版本库尚未初始化，并给出如何初始化版本库的帮助，如图 3-3 所示。

![../images/project-uninitial.png](http://box.kancloud.cn/2015-07-09_559de287b84c9.png)

图 3-3：项目尚未初始化

在图 3-3 中可以看到访问协议增加了一个支持读写的 SSH 协议，访问地址为：git@github.com:gotgithub/helloworld.git。注意任何 GitHub 用户均可使用该 URL 访问此公开版本库，但只有版本库建立者 gotgithub 具有读写权限，其他人只有只读权限。在初始化版本库之前，最好先确认是否是用正确的公钥进行认证，如下：

```
$ ssh -T git@github.com
Hi gotgithub! You've successfully authenticated, but GitHub does not provide shell access. 
```

## 3.1.2\. 版本库初始化[]

如果是从头创建版本库，可以采用先克隆，建立提交数据，最后再通过推送完成 GitHub 版本库的初始化。步骤如下：

*   克隆版本库。

    克隆过程会显示警告，不过这个警告可以忽略，因为 GitHub 创建的版本库本来就是一个空白的版本库。

```
$ git clone git@github.com:gotgithub/helloworld.git
Cloning into 'helloworld'...
warning: You appear to have cloned an empty repository. 
```

*   创建文件 README.md（注：以扩展名.md，.mkd，.mkdn，.mdown，.markdown 等为结尾的文件，均以 Markdown 标记语言语法进行解析并显示。)。

    下面是一段示例文字，把这段文字保存为文件 README.md，该文件的内容将会直接显示在项目首页中（显示效果参见后面的图 3-5）。

```
# 我的第一个 GitHub 项目

这是项目 [helloworld](https://github.com/gotgithub/helloworld) ，
欢迎访问。

这个项目的版本库是 **Git 格式** ，在 Windows、Linux、Mac OS X
平台都有客户端工具可以访问。虽然版本库只提供 Git 一种格式，
但是你还是可以用其他用其他工具访问，如 ``svn`` 和 ``hg`` 。

## 版本库地址

支持三种访问协议：

* HTTP 协议: `https://github.com/gotgithub/helloworld.git` 。
* Git 协议: `git://github.com/gotgithub/helloworld.git` 。
* SSH 协议: `ssh://git@github.com/gotgithub/helloworld.git` 。

## 克隆版本库

操作示例：

$ git clone git://github.com/gotgithub/helloworld.git 
```

上面这段文字采用 Markdown 格式，您也可以使用其他支持的格式，只要确保 README 文件使用正确的扩展名。本书附录部分介绍了 Markdown 及其他 GitHub 支持的标记语言。关于 Markdown，目前我们只需知道这一个易于识别和理解的纯文本格式，可以方便的转换为 HTML。Markdown 语法非常像我们在写邮件（纯文本）时用空行来分隔段落、用星号开启列表、用缩进表示引用内容等等。

*   添加 README.md 文件并提交。

```
$ git add README.md
$ git commit -m "README for this project." 
```

*   向 GitHub 推送，完成版本库初始化。

```
$ git push origin master 
```

然后查看 GitHub 上新建项目的首页。项目首页的上半部分可见版本库包含了一个新的提交，以及版本库目录树中包含的文件，如图 3-4 所示。

![../images/project-pushed-head.png](http://box.kancloud.cn/2015-07-09_559de28bddef2.png)

图 3-4：完成推送后的项目首页上半部分

在项目首页的下半部分，会看到 README.md 文件被转换为 HTML 显示，如图 3-5 所示。

![../images/project-pushed-tail.png](http://box.kancloud.cn/2015-07-09_559de28e5cf29.png)

图 3-5：完成推送后的项目首页下半部分

## 3.1.3\. 从已有版本库创建

如果在 GitHub 项目初始化之前，数据已经存在于本地版本库中，显然像上面那样先克隆、再提交、后推送的方法就不适宜了。应该采用下面的方法。

为试验新的版本库初始化方法，先把刚刚新建的测试项目“helloworld”删除，同时也将本地工作区中克隆的“helloworld”删除。警告：删除项目的操作非常危险，不可恢复，慎用。

*   点击项目首页中项目名称旁边的“Admin”按钮进入项目管理页，再点击页面最下方的删除版本按钮，如图 3-6 所示。

    ![../images/project-delete.png](http://box.kancloud.cn/2015-07-09_559de28fab801.png)

    图 3-6：删除项目

*   然后再重建版本库“helloworld”，如本章一开始图 3-2 所示。

接下来使用下面的步骤完成“helloworld”版本库的初始化。

*   本地建立一个 Git 版本库。

```
$ mkdir helloworld
$ cd helloworld
$ git init 
```

*   然后在版本库中添加示例文件，如 README.md 文件，内容同前。

```
$ git add README.md
$ git commit -m "README for this project." 
```

*   为版本库添加名为 origin 的远程版本库。

```
$ git remote add origin git@github.com:gotgithub/helloworld.git 
```

*   执行推送命令，完成 GitHub 版本库的初始化。注意命令行中的-u 参数，在推送成功后自动建立本地分支与远程版本库分支的追踪。

```
$ git push -u origin master 
```

# 3.2\. 操作版本库

## 3.2.1\. 强制推送

细心的读者可能从图 3-4 已经看出，显示的提交者并非 gotgithub 用户，而是一个名为 ossxp-com 的用户，这是因为 GitHub 是通过提交中的邮件地址来对应到 GitHub 用户的。看看提交说明：

```
$ git log --pretty=fuller
commit 92dee9b8125afc9a606394ed463f9f264f2d3d58
Author:     Jiang Xin 
AuthorDate: Wed Dec 14 14:52:40 2011 +0800
Commit:     Jiang Xin 
CommitDate: Wed Dec 14 14:52:40 2011 +0800

    README for this project. 
```

原来提交用户设置的邮件地址并非 gotgithub 用户设置的邮件地址。补救办法就是对此提交进行修改，然后强制推送到 GitHub。

*   重新设置 user.name 和 user.email 配置变量。

    因为 gotgithub 是一个仅在本书使用的示例账号，我可不想影响本地其他项目的提交，因此下面的设置命令没有使用--global 参数，只对本地 helloworld 版本库进行设置。

```
$ git config user.name "Jiang Xin"
$ git config user.email "gotgithub@gmail.com" 
```

*   执行 Git 修补提交命令。

    注意使用参数--reset-author 会将提交信息中的属性 Author 连同 AuthorDate 一并修改，否则只修改 Commit 和 CommitDate。参数-C HEAD 维持提交说明不变。

```
$ git commit --amend --reset-author -C HEAD 
```

*   查看提交日志，发现提交者信息和作者信息都已经更改。

```
$ git log --pretty=fuller
commit e1e52d99fa71fd6f606903efa9da04fd0055fca9
Author:     Jiang Xin 
AuthorDate: Wed Dec 14 15:05:47 2011 +0800
Commit:     Jiang Xin 
CommitDate: Wed Dec 14 15:05:47 2011 +0800

    README for this project. 
```

*   直接推送会报错。

    错误信息中出现 non-fast-forword（非快进式推送），含义为要推送的提交并非继远程版本库最新提交之后的提交，推送会造成覆盖导致服务器端有数据（提交）会丢失。

```
$ git push
To git@github.com:gotgithub/helloworld.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:gotgithub/helloworld.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details. 
```

*   使用强制推送。

    对于此例，考虑到还没有其他人关注 helloworld 这个刚刚建立的示例项目，显然不需要向上面命令的错误信息所提示的那样先执行 git pull 合并上游版本库再推送，而是选择强制推送，以新的修补提交覆盖包含错误提交者 ID 的提交。

```
$ git push -f
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 629 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:gotgithub/helloworld.git
 + 92dee9b...e1e52d9 master -> master (forced update) 
```

完成强制推送后，再查看 GitHub 项目页面，会发现提交者已经显示为 gotgithub 用户。如图 3-7 所示。

![../images/force-push.png](http://box.kancloud.cn/2015-07-09_559de2a491e91.png)

图 3-7：强制更新后，提交者已更改

## 3.2.2\. 新建分支

Git 的分支就是保存在`.git/refs/heads/`命名空间下的引用。引用文件的内容是该分支对应的提交 ID。当前版本库中的默认分支 master 就对应于文件`.git/refs/heads/master`。

若在 GitHub 版本库中创建分支，首先要在本地版本库中创建新的分支（即引用），然后用推送命令将本地创建的新的引用连同所指向的提交推送到 GitHub 版本库中完成 GitHub 上分支的创建。操作如下：

*   本地版本库中建立新分支 mybranch1。

    创建分支有多种方法，如使用 git branch 命令，但最为便捷的就是 git checkout-b 命令，同时完成新分支的创建和分支切换。

```
$ git checkout -b mybranch1
Switched to a new branch 'mybranch1' 
```

*   为了易于识别，添加一个新文件 hello1，并提交。

```
$ touch hello1
$ git add hello1
$ git commit -m "add hello1 for mark."
[mybranch1 f46a284] add hello1 for mark.
 0 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello1 
```

*   通过推送命令，将本地分支 mybranch1 推送到 GitHub 远程版本库，完成在 GitHub 上的新分支创建。

```
$ git push -u origin mybranch1
Counting objects: 4, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 281 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To git@github.com:gotgithub/helloworld.git
 * [new branch]      mybranch1 -> mybranch1
Branch mybranch1 set up to track remote branch mybranch1 from origin. 
```

在 GitHub 上查看版本库，会看到新增了一个分支 mybranch1，不过默认分支仍为 master，如图 3-8 所示。

![../images/new-branch.png](http://box.kancloud.cn/2015-07-09_559de2ab6292a.png)

图 3-8：版本库新增了一个分支

## 3.2.3\. 设置默认分支

可以改变 GitHub 上版本库显示的默认分支，如果版本库包含多个分支的话。例如修改版本库的默认分支为 mybranch1，点击项目名称旁边的“Admin”按钮，修改项目的默认分支。如图 3-9 所示。

![../images/set-default-branch.png](http://box.kancloud.cn/2015-07-09_559de2ae7d6f5.png)

图 3-9：设置缺省分支

设置了 GitHub 默认分支后，如果再从 GitHub 克隆版本库，本地克隆后版本库的默认分支也将改变。

```
$ git clone git@github.com:gotgithub/helloworld.git helloworld-nb
Cloning into 'helloworld-nb'...
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 0), reused 6 (delta 0)
Receiving objects: 100% (6/6), done.
$ cd helloworld-nb
$ git branch
* mybranch1 
```

实际上修改 GitHub 上版本库的默认分支，就是将 GitHub 版本库的头指针 HEAD 指向了其他分支，如 mybranch1 分支。这可以从下面命令看出。

```
$ git branch -r
  origin/HEAD -> origin/mybranch1
  origin/master
  origin/mybranch1 
```

也可以从 git ls-remote 命令看出头指针 HEAD 和引用 refs/heads/mybranch1 指向同一个对象的哈希值。

```
$ git ls-remote
From git@github.com:gotgithub/helloworld.git
f46a28484adb6c1b4830eb4df582325c740e9d6c        HEAD
e1e52d99fa71fd6f606903efa9da04fd0055fca9        refs/heads/master
f46a28484adb6c1b4830eb4df582325c740e9d6c        refs/heads/mybranch1 
```

## 3.2.4\. 删除分支

删除当前工作分支会报错。例如下面的命令试图分支 mybranch1，但没有成功：

```
$ git branch -d mybranch1
error: Cannot delete the branch 'mybranch1' which you are currently on. 
```

错误信息显示不能删除当前工作分支。因此先切换到其他分支，例如从 GitHub 版本库中取出 master 分支并切换。

```
$ git checkout master 
```

可以看出新的工作分支为 master 分支。

```
$ git branch
* master
  mybranch1 
```

现在可以删除 mybanch1 分支。下面的命令之所以使用-D 参数，而非-d 参数，是因为 Git 在删除分支时为避免数据丢失，默认禁止删除尚未合并的分支。参数-D 则可强制删除尚未合并的分支。

```
$ git branch -D mybranch1
Deleted branch mybranch1 (was f46a284). 
```

现在只是本地分支被删除了，远程 GitHub 服务器上的 mybranch1 分支尚在。删除远程 GitHub 版本库中的分支就不能使用 git branch 命令，而是要使用 git push 命令，不过在使用推送分支命令时要使用一个特殊的引用表达式（冒号前为空）。如下：

```
$ git push origin :mybranch1
remote: error: refusing to delete the current branch: refs/heads/mybranch1
To git@github.com:gotgithub/helloworld.git
 ! [remote rejected] mybranch1 (deletion of the current branch prohibited)
error: failed to push some refs to 'git@github.com:gotgithub/helloworld.git' 
```

为什么删除远程分支出错了呢？是因为没有使用强制推送么？

实际上即使使用强制推送也会遇到上面的错误。GitHub 发现要删除的 mybranch1 分支是远程版本库的缺省分支，因而禁止删除。重新访问 GitHub 的项目管理页面，将缺省分支设置回 master 分支，参照图 3-9。然后再次执行如下命令，即可成功删除分支。

```
$ git push origin :mybranch1
To git@github.com:gotgithub/helloworld.git
 - [deleted]         mybranch1 
```

执行 git ls-remote 命令可以看到 GitHub 远程版本库已经不存在分支 mybranch1。

```
$ git ls-remote git@github.com:gotgithub/helloworld.git
From git@github.com:gotgithub/helloworld.git
e1e52d99fa71fd6f606903efa9da04fd0055fca9        HEAD
e1e52d99fa71fd6f606903efa9da04fd0055fca9        refs/heads/master 
```

## 3.2.5\. 里程碑管理

里程碑即 tag，其管理和分支管理非常类似。里程碑和分支一样也是以引用的形式存在的，保存在.git/refs/tags/路径下。引用可能指向一个提交，但也可能是其他类型（Tag 对象）。

*   轻量级里程碑：用 git tag [] 命令创建，引用直接指向一个提交对象。
*   带说明的里程碑：用 git tag -a [] 命令创建，并且在创建时需要提供创建里程碑的说明。Git 会创建一个 tag 对象保存里程碑说明、里程碑的指向、创建里程碑的用户等信息，里程碑引用指向该 Tag 对象。
*   带签名的里程碑：用 git tag -s [] 命令创建。是在带说明的里程碑的基础上引入了 PGP 签名，保证了所创建的里程碑的完整性和不可拒绝性。

下面演示一下里程碑的创建和管理。

*   先在本地创建一个新提交。

```
$ touch hello1
$ git add hello1
$ git commit -m "add hello1 for mark." 
```

*   本地创建里程碑 mytag1、mytag2 和 mytag3。

```
$ git tag -m "Tag on initial commit" mytag1 HEAD^
$ git tag -m "Tag on new commit"     mytag2
$ git tag mytag3 
```

*   查看新建立的里程碑。

```
$ git tag -l -n1
mytag1          Tag on initial commit
mytag2          Tag on new commit
mytag3          add hello1 for mark. 
```

*   将本地里程碑推送到 GitHub 远程版本库。

```
$ git push origin refs/tags/*
Counting objects: 6, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 548 bytes, done.
Total 5 (delta 0), reused 0 (delta 0)
To git@github.com:gotgithub/helloworld.git
 * [new tag]         mytag1 -> mytag1
 * [new tag]         mytag2 -> mytag2
 * [new tag]         mytag3 -> mytag3 
```

*   删除本地里程碑。

```
$ git tag -d mytag3
Deleted tag 'mytag3' (was c71231c) 
```

*   删除 GitHub 远程版本库中的里程碑。

```
$ git push origin :mytag3
To git@github.com:gotgithub/helloworld.git
  [deleted]         mytag3 
```

此时查看 GitHub 上的项目页，会看到已有两个里程碑，如图 3-10 所示。

![../images/tags-list.png](http://box.kancloud.cn/2015-07-09_559de2afeabd7.png)

图 3-10：里程碑列表

# 3.3\. 公钥认证管理

开发者向 GitHub 版本库写入最常用到的协议是 SSH 协议，因为 SSH 协议使用公钥认证，可以实现无口令访问，而若使用 HTTPS 协议每次身份认证时都需要提供口令(可以通过在文件`~/.netrc`中写入明文口令实现使用 HTTPS 协议时也能自动完成认证。具体格式参见 ftp 命令的 MAN 手册中相关介绍)。使用 SSH 公钥认证，就涉及到公钥的管理。

## 3.3.1\. 用户级公钥管理

开发者可能会从不止一台电脑访问 GitHub 中的版本库（用 SSH 协议），因不同的电脑有不同的公钥/私钥对，这就需要为 GitHub 账号添加多个公钥。点击账号设置中的“SSH Public Keys”进入 SSH 公钥管理界面，如图 3-11 所示。

![../images/ssh-public-keys.png](http://box.kancloud.cn/2015-07-09_559de2c5492f9.png)

图 3-11：SSH 多公钥管理

如图 3-11，在创建 gotgithub 账号一开始，就手工添加了名为“My Mac OS X”的公钥，显然这是为苹果电脑准备的。图中正在添加的名为“Key on Windows”是为 Windows 环境下使用 SSH 协议访问 GitHub 准备的公钥。

当添加了新的公钥后，无论是从哪台电脑（苹果或 PC）用 SSH 协议访问版本库时都拥有相同授权，即都是以 gotgithub 账号身份来访问。例如用户级公钥访问 GitHub 的 SSH 服务，在提示信息中会显示用户 ID，如下：

```
$ ssh -T git@github.com
Hi gotgithub! You've successfully authenticated, but GitHub does not provide shell access. 
```

## 3.3.2\. 项目级公钥管理

多增加一个用户级别的公钥，就意味着可以从另外一台电脑访问该用户所有版本库。但有时只希望从某台电脑上向某一个版本库“写入”，其他版本库则不可写，这可以通过设置版本库级别的公钥认证实现。

以项目管理者（创建者）身份登录 GitHub，例如以 gotgithub 用户身份访问 gotgithub/helloworld 版本库，进入到项目的管理页面，选择菜单中的“Deploy Keys”，即可设置项目级别公钥。如图 3-12 所示。

![../images/deploy-keys.png](http://box.kancloud.cn/2015-07-09_559de2c94a9b3.png)

图 3-12：项目级公钥管理

就像一个用户可以设置多个用户级公钥一样，也可以为一个项目设置多个项目级公钥。无论是项目级公钥还是用户级公钥都有同样的限制：一个公钥只能使用一次。

当使用项目级公钥访问 GitHub 的 SSH 服务，会在提示信息中显示版本库 ID 而非用户 ID。如下的命令输出中显示了版本库 IDgotgithub/helloworld。

```
$ ssh -i ~/.ssh/deploy-key -T git@github.com
Hi gotgithub/helloworld! You've successfully authenticated, but GitHub does not provide shell access. 
```

# 3.4\. 版本库钩子扩展

通过钩子扩展，GitHub 托管的版本库可以和外部应用实现整合。整合的接口完全开放，开发者可以访问 GitHub 的开源项目 [github/github-services](https://github.com/github/github-services) 开发新的应用整合脚本。目前 GitHub 已经支持超过 50 个外部应用的整合，在这里恕不一一列举，仅以 helloworld 项目为例，介绍几个常见应用的整合。

## 3.4.1\. 邮件通知功能

配置邮件通知，可以实现新提交推送至版本库时，发送通知邮件。在版本库的管理界面，选择“Service Hooks”中的 Email 进入邮件通知配置界面，如图 3-15 所示。配置界面很简单，写上邮件地址，选择激活即可。为了便于整个团队都能收到通知邮件，可以将收件地址设置为一个邮件列表。如果选择“Send From Author”，邮件的发件者显示为提交者的邮件地址，否则发件者为 noreply@github.com。

![../images/hooks-email.png](http://box.kancloud.cn/2015-07-09_559de2e849ad3.png)

图 3-15：邮件通知功能配置

邮件通知配置生效后，当有新提交推送到版本库时，会发出通知邮件，如图 3-16 所示。

![../images/mail-in-gg.png](http://box.kancloud.cn/2015-07-09_559de2ed4b765.png)

图 3-16：提交触发邮件通知

## 3.4.2\. 和 Redmine 整合

Redmine 是一个开源的项目管理平台，用于项目的需求管理和缺陷跟踪。Redmine 可以和多种版本库（包括 Git）整合，可以直接通过 Web 界面浏览 Git 提交，还实现了提交和问题的关联。

Redmine 需要周期性地扫描版本库，以便更新内置数据库及建立提交和问题的关联。通常是以计划任务（crontab）的方式实现版本库的周期性扫描，这导致 Redmine 中版本库更新会存在一定的延迟。GitHub 提供的 Redmine 整合的钩子脚本能够在 GitHub 版本库更新后，通过 WebService 触发 Redmine 主动扫描 Git 版本库获取更新。

GitHub 提供的 Redmine 整合的配置界面如图 3-17 所示。

![../images/hooks-redmine.png](http://box.kancloud.cn/2015-07-09_559de2f228e87.png)

图 3-17：与 Redmine 整合

图 3-17 中的地址是 Redmine 部署的 URL 地址，项目 ID 是 Redmine 中的相关项目（如果不填写则更新所有项目），而“Api Key”并非 GitHub 项目中配置的 Api Key，而是 Redmine 中为版本库更新配置的全局 Api Key。相应的 Redmine 配置界面如图 3-18 所示。

![../images/redmine-api-key.png](http://box.kancloud.cn/2015-07-09_559de2f391765.png)

图 3-18：Redmine 中的 API Key 配置

# 3.5\. 建立主页

很多开源项目托管平台都支持为托管的项目建立主页，但主页的维护方式都没有 GitHub 这么酷。大多数托管平台无非是开放一个 FTP 或类似服务，用户把制作好的网页或脚本上传了事，而在 GitHub 用户通过创建特殊名称的 Git 版本库或在 Git 库中建立特别的分支实现对主页的维护。

## 3.5.1\. 创建个人主页

GitHub 为每一个用户分配了一个二级域名.github.io，用户为自己的二级域名创建主页很容易，只要在托管空间下创建一个名为.github.io 的版本库，向其 master 分支提交网站静态页面即可，其中网站首页为 index.html。下面以 gotgithub 用户为例介绍如何创建个人主页。

*   用户 gotgithub 创建一个名为 gotgithub.github.io 的 Git 版本库。

    在 GitHub 上创建版本库的操作，参见“第 3.1 节 [*创建新项目*](http://www.worldhello.net/gotgithub/03-project-hosting/010-new-project.html#new-project)”。

*   在本地克隆新建立的版本库。

```
$ git clone git@github.com:gotgithub/gotgithub.github.io.git
$ cd gotgithub.github.io/ 
```

*   在版本库根目录中创建文件 index.html 作为首页。

```
$ printf "<h1>GotGitHub's HomePage</h1>It works.\n" > index.html 
```

*   建立提交。

```
$ git add index.html
$ git commit -m "Homepage test version." 
```

*   推送到 GitHub，完成远程版本库创建。

```
$ git push origin master 
```

*   访问网址： [`gotgithub.github.io/`](http://gotgithub.github.io/) 。

    最多等待 10 分钟，GitHub 就可以完成新网站的部署。网站完成部署后版本库的所有者会收到邮件通知。

    还有要注意访问用户二级域名的主页要使用 HTTP 协议非 HTTPS 协议。

## 3.5.2\. 创建项目主页

如前所述，GitHub 会为每个账号分配一个二级域名.github.io 作为用户的首页地址。实际上还可以为每个项目设置主页，项目主页也通过此二级域名进行访问。

例如 gotgithub 用户创建的 helloworld 项目如果启用了项目主页，则可通过网址[`gotgithub.github.io/helloworld/访问。`](http://gotgithub.github.io/helloworld/访问。)

为项目启用项目主页很简单，只需要在项目版本库中创建一个名为 gh-pages 的分支，并向其中添加静态网页即可。也就是说如果项目的 Git 版本库中包含了名为 gh-pages 分支的话，则表明该项目提供静态网页构成的主页，可以通过网址[`.github.io/访问到。`](http://.github.io/访问到。)

下面以用户 gotgithub 的项目 helloworld 为例，介绍如何维护项目主页。

如果本地尚未从 GitHub 克隆 helloworld 版本库，执行如下命令。

```
$ git clone git@github.com:gotgithub/helloworld.git
$ cd helloworld 
```

当前版本库只有一个名为 master 的分支，如果直接从 master 分支创建 gh-pages 分支操作非常简单，但是作为保存网页的 gh-pages 分支中的内容和 master 分支中的可能完全不同。如果不希望 gh-pages 分支继承 master 分支的历史和文件，即想要创建一个干净的 gh-pages 分支，需要一点小技巧。

若使用命令行创建干净的 gh-pages 分支，可以从下面三个方法任选一种。

第一种方法用到两个 Git 底层命令：git write-tree 和 git commit-tree。步骤如下：

*   基于 master 分支建立分支 gh-pages。

```
$ git checkout -b gh-pages 
```

*   删除暂存区文件，即相当于清空暂存区。

```
$ rm .git/index 
```

*   创建项目首页 index.html。

```
$ printf "hello world.\n" > index.html 
```

*   添加文件 index.html 到暂存区。

```
$ git add index.html 
```

*   用 Git 底层命令创建新的根提交，并将分支 gh-pages 重置。

```
$ git reset --hard $(echo "branch gh-pages init." | git commit-tree $(git write-tree)) 
```

*   执行推送命令，在 GitHub 远程版本库创建分支 gh-pages。

```
$ git push -u origin gh-pages 
```

第二种方法用到 Git 底层命令：git symbolic-ref。步骤如下：

*   用 git symbolic-ref 命令将当前工作分支由 master 切换到一个尚不存在的分支 gh-pages。

```
$ git symbolic-ref HEAD refs/heads/gh-pages 
```

*   删除暂存区文件，即相当于清空暂存区。

```
$ rm .git/index 
```

*   创建项目首页 index.html。

```
$ printf "hello world.\n" > index.html 
```

*   添加文件 index.html 到暂存区。

```
$ git add index.html 
```

*   执行提交。提交完毕分支 gh-pages 完成创建。

```
$ git commit -m "branch gh-pages init." 
```

*   执行推送命令，在 GitHub 远程版本库创建分支 gh-pages。

```
$ git push -u origin gh-pages 
```

第三种方法没有使用任何 Git 底层命令，是从另外的版本库获取提交建立分支。操作如下：

*   在 helloworld 版本库之外创建另外一个版本库，例如 helloworld-web。

```
$ git init ../helloworld-web
$ cd ../helloworld-web 
```

*   在 helloworld-web 版本库中创建主页文件 index.html。

```
$ printf "hello world.\n" > index.html 
```

*   添加文件 index.html 到暂存区。

```
$ git add index.html 
```

*   执行提交。

    实际提交到 master 分支，虽然提交说明中出现的是 gh-pages 。

```
$ git commit -m "branch gh-pages init." 
```

*   切换到 helloworld 版本库目录。

```
$ cd ../helloworld 
```

*   从 helloworld-web 版本库获取提交，并据此创建 gh-pages 分支。

```
$ git fetch ../helloworld-web
$ git checkout -b gh-pages FETCH_HEAD 
```

*   执行推送命令，在 GitHub 远程版本库创建分支 gh-pages。

```
$ git push -u origin gh-pages 
```

无论哪种方法，一旦在 GitHub 远程版本库中创建分支 gh-pages，项目的主页就已经建立。稍后（不超过 10 分钟），用浏览器访问下面的地址即可看到刚刚提交的项目首页： [`gotgithub.github.io/helloworld/`](http://gotgithub.github.io/helloworld/) 。

除了以上通过命令行创建 gh-pages 分支为项目设定主页之外，GitHub 还提供了图形操作界面。如图 3-19 所示。

![../images/github-pages.png](http://box.kancloud.cn/2015-07-09_559de30aaa25b.png)

图 3-19：项目管理页面中的 GitHub Pages 选项

当在项目管理页面中勾选“GitHub Pages”选项，并按照提示操作，会自动在项目版本库中创建 gh-pages 分支。然后执行下面命令从版本库检出 gh-pages 分支，对项目主页进行相应定制。

```
$ git fetch
$ git checkout gh-pages 
```

## 3.5.3\. 使用专有域名

无论是用户主页还是项目主页，除了使用 github.com 下的二级域名访问之外，还可以使用专有域名。实现起来也非常简单，只要在 master 分支（用户主页所在版本库）或 gh-pages 分支（项目版本库）的根目录下检入一个名为 CNAME 的文件，内容为相应的专有域名。当然还要更改专有域名的域名解析，使得该专有域名的 IP 地址指向相应的 GitHub 二级域名的 IP 地址。

例如 worldhello.net(“Hello, world”最为程序员所熟知，2002 年申请不到 helloworld 相关域名便退而求其次，申请了 worldhello.net。)是我的个人网站，若计划将网站改为由 GitHub 托管，并由账号 gotgit 通过个人主页提供服务，可做如下操作。

首先按照前面章节介绍的步骤，为账号 gotgit 设置账户主页。

1.  在账户 gotgit 下创建版本库 gotgit.github.io 以维护该账号主页。

    地址： [`github.com/gotgit/gotgit.github.io/`](https://github.com/gotgit/gotgit.github.io/)

2.  将网站内容提交并推送到该版本库 master 分支中。

    即在 gotgit.github.io 版本库的根目录下至少包含一个首页文件，如 index.html。还可以使用下节将要介绍到的 Jekyll 技术，让网页有统一的显示风格，此时首页文件可能并非一个完整的 HTML 文档，而是套用了页面模版。

3.  至此当访问网址[`gotgit.github.io`](http://gotgit.github.io)时，会将账号 gotgit 的版本库 gotgit.github.io 中的内容作为网站内容显示出来。

接下来进行如下操作，使得该网站能够使用专有域名 www.worldhello.net 提供服务。

1.  在账号 gotgit 的版本库 gotgit.github.io 根目录下添加文件 CNAME，文件内容为：www.worldhello.net。

    参见： [`github.com/gotgit/gotgit.github.io/blob/master/CNAME`](https://github.com/gotgit/gotgit.github.io/blob/master/CNAME)

2.  然后更改域名 www.worldhello.net 的 IP 地址，指向域名 gotgit.github.io 对应的 IP 地址（注意不是 github.com 的 IP 地址）。

    完成域名的 DNS 指向后，可试着用 ping 或 dig 命令确认域名 www.worldhello.net 和 gotgit.github.io 指向同一 IP 地址。

```
$ dig @8.8.8.8 -t a www.worldhello.net
...
; ANSWER SECTION:
www.worldhello.net.     81078   IN      A       204.232.175.78

$ dig @8.8.8.8 -t a gotgit.github.io
...
; ANSWER SECTION:
gotgit.github.io.      43200   IN      A       204.232.175.78 
```

设置完成后用浏览器访问 [`www.worldhello.net/`](http://www.worldhello.net/) 即可看到由账号 gotgit 的版本库 gotgit.github.io 维护的页面。若将域名 worldhello.net（不带 www 前缀）也指向 IP 地址 204.232.175.78，则访问网址[`worldhello.net/`](http://worldhello.net/)会发现 GitHub 体贴地将该网址重定向到正确的地址[`www.worldhello.net/`](http://www.worldhello.net/)。

在账号 gotgit 下的其他版本库，若包含了 gh-pages 分支，亦可由域名 www.worldhello.net 访问到。

*   网址 [`www.worldhello.net/doc`](http://www.worldhello.net/doc) 实际对应于版本库 [gotgit/doc](https://github.com/gotgit/doc) 。
*   网址 [`www.worldhello.net/gotgit`](http://www.worldhello.net/gotgit) 实际对应于版本库 [gotgit/gotgit](https://github.com/gotgit/gotgit) 。
*   网址 [`www.worldhello.net/gotgithub`](http://www.worldhello.net/gotgithub) 实际对应于版本库[gotgit/gotgithub](https://github.com/gotgit/gotgithub) 。

## 3.5.4\. 使用 Jekyll 维护网站

Jekyll 是一个支持 Textile、Markdown 等标记语言的静态网站生成软件，还支持博客和网页模版，由 Tom Preston-Werner（GitHub 创始人之一）开发。Jekyll 用 Ruby 语言实现，项目在 GitHub 的托管地址： [`github.com/mojombo/jekyll/`](http://github.com/mojombo/jekyll/) ，专有的 URL 地址为：[`jekyllrb.com/`](http://jekyllrb.com/)。

GitHub 为用户账号或项目提供主页服务，会从相应版本库的 master 分支或 gh-pages 分支检出网页文件，然后执行 Jekyll 相应的命令对网页进行编译。因此在设计 GitHub 的用户主页和项目主页时都可以利用 Jekyll，实现用 Markdown 等标记语言撰写网页及博客，并用页面模版实现网页风格的统一。

安装 Jekyll 最简单的方法是通过 RubyGems 安装，会自动将 Jekyll 依赖的 directory_watcher、liquid、open4、maruku 和 classifier 等 Gem 包一并安装。

```
$ gem install jekyll 
```

如果安装过程因编译扩展模组遇到错误，可能是因为尚未安装所需的头文件，需要进行如下操作：

*   对于 Debian Linux、Ubuntu 等可以用如下方法安装所需软件包：

```
$ sudo apt-get install ruby1.8-dev 
```

*   如果是 Red Hat、CentOS 或 Fedora 等系统，使用如下命令安装：

```
$ sudo yum install ruby-devel 
```

*   对于 Mac OSX，可能需要更新 RubyGems，如下：

```
$ sudo gem update --system 
```

Jekyll 安装完毕，执行下面的命令显示软件版本：

```
$ jekyll -v
Jekyll 0.11.0 
```

要学习如何用 Jekyll 设计网站，可以先看一下作者 Tom Preston-Werner 在 GitHub 上的个人网站是如何用 Jekyll 制作出来的。

克隆版本库：

```
$ git clone git://github.com/mojombo/mojombo.github.com.git 
```

版本库包含的文件如下：

```
$ cd mojombo.github.com
$ ls -F
CNAME           _config.yml     _posts/         css/            index.html
README.textile  _layouts/       atom.xml        images/         random/ 
```

版本库根目录下的 index.html 文件不是一个普通的 HTML 文件，而是使用 Liquid 模版语言[[2]](http://liquidmarkup.org/)定义的页面。

```
1 ---
 2 layout: default
 3 title: Tom Preston-Werner
 4 ---
 5
 6 <div id="home">
 7   <h1>Blog Posts</h1>
 8   <ul class="posts">
 9     {% for post in site.posts %}
10       <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
11     {% endfor %}
12   </ul>
   ...
63 </div> 
```

为方便描述为内容添加了行号，说明如下：

*   第 1-4 行是 YAML 格式的文件头，设定了该文件所使用的模版文件及模版中要用到的变量。

    凡是设置有 YAML 文件头的文件（目录 _layouts 除外）无论文件扩展名是什么，都会在 Jekyll 编译时进行转换。若源文件由 Markdown 等标记语言撰写（扩展名为.md、.textile 等），Jekyll 还会将编译后的文件还将以扩展名.html 来保存。

*   其中第 2 行含义为使用 default 模版。

    对应的模版文件为 _layouts/default.html。

*   第 3 行设定本页面的标题。

    在模版文件 _layouts/default.html 中用{{ page.title }}语法嵌入所设置的标题。下面是模版文件中部分内容：

```
<head>
   <meta http-equiv="content-type" content="text/html; charset=utf-8" />
   <title>{{ page.title }}</title> 
```

*   第 6 行开始的内容绝大多数是标准的 HTML 语法，其中夹杂少量 Liquid 模版特有的语法。

*   第 9 行和第 11 行，对于有着 Liquid 或其他模版编程经验的用户，不难理解其中出现的由“{%”和“%}”标识的指令是一个循环指令（for 循环），用于逐条对博客进行相关操作。

*   第 10 行中由“{{”和“}}”标识的表达式则用于显示博文的日期、链接和标题。

非下划线（`_`）开头的文件（包括子目录中文件），如果包含 YAML 文件头，就会使用 Jekyll 进行编译，并将编译后的文件复制到目标文件夹（默认为`_site`目录）下。对于包含 YAML 文件头并用标记语言 Markdown 等撰写的文件，还会将编译后的文件以`.html`扩展名保存。而以下划线开头的文件和目录有的直接忽略不予处理（如`_layouts`、`_site`目录等），有的则需要特殊处理（如`_post`目录）。

目录 _post 用于保存博客条目，每个博客条目都以`&lt;YYYY&gt;-&lt;MM&gt;-&lt;DD&gt;-&lt;blog-tiltle&gt;`格式的文件名命名。扩展名为.md 的为 Markdown 格式，扩展名为.textile 的为 Textile 格式。这些文件都包含类似的文件头：

```
---
layout: post
title: How I Turned Down $300,000 from Microsoft to go Full-Time on GitHub
--- 
```

即博客使用文件`_layouts/post.html`作为页面模版，而不是 index.html 等文件所使用的`_layouts/default.html`模版。这些模版文件都采用 Liquid 模版语法。保存于 _post 目录下的博客文件编译后会以`&lt;YYYY&gt;/&lt;MM&gt;/&lt;DD&gt;/&lt;blog-title&gt;.html`文件名保存在输出目录中。

在根目录下还有一个配置文件 _config.yml 用于覆盖 Jekyll 的默认设置，例如本版本库中的设置。

```
markdown: rdiscount
pygments: true 
```

第 1 行设置使用 rdiscount 软件包作为 Markdown 的解析引擎，而非默认的 Maruku。第 2 行开启 pygments 支持。对于中文用户强烈建议通过配置文件 _config.yml 重设 markdown 解析引擎，默认的 Maruku 对中文支持不好，而使用 rdiscount 或 kramdown 均可。关于该配置文件的更多参数详见 Jekyll 项目维基[[3]](https://github.com/mojombo/jekyll/wiki/configuration) 。

编译 Jekyll 编辑网站只需在根目录执行 jekyll 命令，下面的命令是 GitHub 更新网站所使用的默认指令。

```
$ jekyll --pygments --safe 
```

现在执行这条命令，就会将整个网站创建在目录 _site 下。

如果没有安装 Apache 等 Web 服务器，还可以使用 Jekyll 的内置 Web 服务器。

```
$ jekyll --server 
```

默认在端口 4000 开启 Web 服务器。

网址 [`gitready.com/`](http://gitready.com/) 是一个提供 Git 使用小窍门的网站，如图 3-20 所示。

![../images/gitready.png](http://box.kancloud.cn/2015-07-09_559de30ce3c24.png)

图 3-20：Git Ready 网站

你相信这是一个用 Jekyll 制作的网站么？看看该网站对应的 IP，会发现其指向的正是 GitHub。研究 GitHub 上 [gitready](https://github.com/gitready) 用户托管的版本库，会发现 en 版本库的 gh-pages 分支负责生成 gitready.com 网站，de 版本库的 gh-pages 分支负责生成德文网站 de.gitready.com，等等。而 gitready 版本库则是各种语种网站的汇总。

我的个人网站也使用 Jekyll 构建并托管在 GitHub 上，网址：[`www.worldhello.net/`](http://www.worldhello.net/)。