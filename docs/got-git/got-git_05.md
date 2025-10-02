# 5\. 搭建 Git 服务器

团队协作就涉及到搭建 Git 服务器。

搭建 Git 服务器可以非常简单，例如直接将 Git 裸版本库“扔到”Web 服务器中作为一个共享目录，或者运行`git daemon`命令，甚至只需要轻点一下鼠标[[1]](#id10)就可以迅速将自己的版本库设置为只读共享。利用这个技术可以在团队中创建一个基于拉拽（pull）操作的 Git 工作流。

如果需要一个支持“写”操作的 Git 服务器，常用的方案包括使用 Git 本身提供的`TODO CGI`实现的智能 HTTP 服务，或者使用`Gitolite`提供基于 SSH 协议的支持精细读写授权的 Git 服务器。

安卓（Android）项目以 Git 做版本控制，但其工作模式非常特殊，提交操作产生的“补丁”先要在一个 Web 平台上做代码审核，审核通过才合并到 Git 版本库中。谷歌开源了这个代码审核平台，称为 Gerrit。在第 TODO 章将会介绍 Gerrit 服务器搭建和工作流程。

不过您可能不必去亲手搭建 Git 服务器，因为有 GitHub[[2]](#id11)。GitHub 是开源软件的大本营，为开源软件提供免费的版本库托管和社交编程服务，并且还提供 Git 版本库的商业托管服务。类似 GitHub 的 Git 版本库托管服务提供商还有很多（如 Bitbucket[[3]](#id12)、国内的 GitCafe[[4]](#id13)、GitShell[[5]](#id14)、CSDN-Code[[6]](#id15)、开源中国[[7]](#id16)等），您可以根据需要进行选择。

想在本地搭建一个 GitHub 克隆？至少有两个开源软件 GitLab[[8]](#id17) 和 Gitorious[[9]](#id18) 可供选择，它们都提供了 GitHub 相仿的功能。在第 TODO 章介绍用 GitLab 在本地搭建专有的 GitHub 服务。

目录:

*   5.1\. 使用 HTTP 协议
    *   5.1.1\. 哑传输协议
    *   5.1.2\. 智能 HTTP 协议
    *   5.1.3\. Gitweb 服务器
        *   5.1.3.1\. Gitweb 安装
        *   5.1.3.2\. Gitweb 配置
        *   5.1.3.3\. 版本库的 Gitweb 相关设置
*   5.2\. 使用 Git 协议
    *   5.2.1\. Git 协议语法格式
    *   5.2.2\. Git 服务软件
    *   5.2.3\. 以 inetd 方式配置运行
    *   5.2.4\. 以 runit 方式配置运行
*   5.3\. 使用 SSH 协议
    *   5.3.1\. SSH 协议语法格式
    *   5.3.2\. 服务架设方式比较
    *   5.3.3\. 关于 SSH 公钥认证
    *   5.3.4\. 关于 SSH 主机别名
*   5.4\. Gitolite 服务架设
    *   5.4.1\. 安装 Gitolite
        *   5.4.1.1\. 安装之前
        *   5.4.1.2\. 以发布包形式安装
        *   5.4.1.3\. 从源代码开始安装
    *   5.4.2\. 管理 Gitolite
        *   5.4.2.1\. 管理员克隆 gitolite-admin 管理库
        *   5.4.2.2\. 增加新用户
        *   5.4.2.3\. 更改授权
    *   5.4.3\. Gitolite 授权详解
        *   5.4.3.1\. 授权文件的基本语法
        *   5.4.3.2\. 定义用户组和版本库组
        *   5.4.3.3\. 版本库 ACL
        *   5.4.3.4\. Gitolite 授权机制
    *   5.4.4\. 版本库授权案例
        *   5.4.4.1\. 常规版本库授权
        *   5.4.4.2\. 通配符版本库授权
        *   5.4.4.3\. 每个人创建自己的版本库
        *   5.4.4.4\. 传统模式的引用授权
        *   5.4.4.5\. 扩展模式的引用授权
        *   5.4.4.6\. 禁用规则的使用
        *   5.4.4.7\. 用户分支
        *   5.4.4.8\. 对路径的写授权
    *   5.4.5\. 创建和导入版本库
        *   5.4.5.1\. 在配置文件中出现的版本库，即时生成
        *   5.4.5.2\. 通配符版本库，管理员通过推送创建
        *   5.4.5.3\. 向 Gitolite 中导入版本库
    *   5.4.6\. 对 Gitolite 的改进
    *   5.4.7\. Gitolite 功能拓展
        *   5.4.7.1\. 版本库镜像
        *   5.4.7.2\. Gitweb 和 Git daemon 支持
        *   5.4.7.3\. 其他功能拓展和参考
*   5.5\. Gitosis 服务架设
    *   5.5.1\. 安装 Gitosis
        *   5.5.1.1\. Gitosis 的安装
        *   5.5.1.2\. 服务器端创建专用帐号
        *   5.5.1.3\. Gitosis 服务初始化
    *   5.5.2\. 管理 Gitosis
        *   5.5.2.1\. 管理员克隆`gitolit-admin`管理库
        *   5.5.2.2\. 增加新用户
        *   5.5.2.3\. 更改授权
    *   5.5.3\. Gitosis 授权详解
        *   5.5.3.1\. Gitosis 缺省设置
        *   5.5.3.2\. 管理版本库`gitosis-admin`
        *   5.5.3.3\. 定义用户组和授权
        *   5.5.3.4\. Gitweb 整合
    *   5.5.4\. 创建新版本库
    *   5.5.5\. 轻量级管理的 Git 服务
*   5.6\. Gerrit 代码审核服务器
    *   5.6.1\. Gerrit 的实现原理
        *   5.6.1.1\. SSH 协议的 Git 服务器
        *   5.6.1.2\. 特殊引用`refs/for/<branch-name>`和`refs/changes/nn/<review-id>/m`
        *   5.6.1.3\. Git 库的钩子脚本`hooks/commit-msg`
        *   5.6.1.4\. 其余一切交给 Web
    *   5.6.2\. 架设 Gerrit 的服务器
    *   5.6.3\. Gerrit 的配置文件
    *   5.6.4\. Gerrit 的数据库访问
    *   5.6.5\. 立即注册为 Gerrit 管理员
    *   5.6.6\. 管理员访问 SSH 的管理接口
    *   5.6.7\. 创建新项目
    *   5.6.8\. 从已有 Git 库创建项目
    *   5.6.9\. 定义评审工作流
    *   5.6.10\. Gerrit 评审工作流实战
        *   5.6.10.1\. 开发者在本地版本库中工作
        *   5.6.10.2\. 开发者向审核服务器提交
        *   5.6.10.3\. 审核评审任务
        *   5.6.10.4\. 评审任务没有通过测试
        *   5.6.10.5\. 重新提交新的补丁集
        *   5.6.10.6\. 新修订集通过评审
        *   5.6.10.7\. 从远程版本库更新
    *   5.6.11\. 更多 Gerrit 参考
*   5.7\. Git 版本库托管
    *   5.7.1\. Github
    *   5.7.2\. Gitorious

| [[1]](#id1) | 在 TortoiseGit 中只需要点击右键菜单中的“Git Daemon”。 |

| [[2]](#id2) | [`github.com`](https://github.com) |

| [[3]](#id3) | [`bitbucket.org`](https://bitbucket.org) |

| [[4]](#id4) | [`gitcafe.com`](https://gitcafe.com) |

| [[5]](#id5) | [`gitshell.com`](https://gitshell.com) |

| [[6]](#id6) | [`code.csdn.net`](https://code.csdn.net) |

| [[7]](#id7) | [`git.oschina.net`](http://git.oschina.net) |

| [[8]](#id8) | [`gitlab.org`](http://gitlab.org) |

| [[9]](#id9) | [`gitorious.org`](https://gitorious.org) |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.1\. 使用 HTTP 协议

HTTP 协议是版本控制工具普遍采用的协议，具有安全（HTTPS），方便（跨越防火墙）等优点。Git 在 1.6.6 版本之前对 HTTP 协议支持有限，是哑协议，访问效率低，但是在 1.6.6 之后，通过一个 CGI 实现了智能的 HTTP 协议支持。

## 5.1.1\. 哑传输协议

所谓的哑传输协议（dumb protocol）就是在 Git 服务器和 Git 客户端的会话过程中只使用了相关协议提供的基本传输功能，而未针对 Git 的传输特点进行相关优化设计。采用哑协议，Git 客户端和服务器间的通讯缺乏效率，用户使用时最直观的体验之一就是在操作过程没有进度提示，应用会一直停在那里直到整个通讯过程处理完毕。

但是哑传输协议配置起来却非常的简单。通过哑 HTTP 协议提供 Git 服务，基本上就是把包含 Git 版本库的目录通过 HTTP 服务器共享出来。下面的 Apache 配置片段就将整个目录`/path/to/repos`共享，然后就可以通过地址`http://<服务器名称>/git/<版本库名称>`访问该共享目录下所有的 Git 版本库。

```
Alias /git /path/to/repos

<Directory /path/to/repos>
    Options FollowSymLinks
    AllowOverride None
    Order Allow,Deny
    Allow from all
</Directory>

```

如果以为直接把现存的 Git 版本库移动到该目录下就可以直接用 HTTP 协议访问，可就大错特错了。因为哑协议下的 Git 版本库需要包含两个特殊的文件，并且这两个文件要随 Git 版本库更新。例如将一个包含提交数据的裸版本库复制到路径`/path/to/repos/myrepos.git`中，然后使用下面命令克隆该版本库（服务器名称为`server`），可能会看到如下错误：

```
$ git clone http://server/git/myrepos.git
正克隆到 'myrepos'...
fatal: repository 'http://server/git/myrepos.git/' not found

```

这时，您仅需在 Git 版本库目录下执行`git update-server-info`命令即可在 Git 版本库中创建哑协议需要的相关文件。

```
$ git update-server-info

```

然后该 Git 版本库即可正常访问了。如下：

```
$ git clone http://server/git/myrepos.git
正克隆到 'myrepos'...
检查连接... 完成。

```

从上面的介绍中可以看出在使用哑 HTTP 协议时，服务器端运行`git update-server-info`的重要性。运行该命令会产生或更新两个文件：

*   文件`.git/info/refs`：该文件中包含了版本库中所有的引用列表，每一个引用都指向正确的 SHA1 哈希值。
*   文件`.git/objects/info/packs`：该文件记录 Git 对象库中打包文件列表。

正是通过这两个文件，Git 客户端才检索到版本库的引用列表和对象库的包列表，从而实现对版本库的读取操作。

为支持哑 HTTP 协议，必须在版本库更新后及时更新上述两个文件。幸好 Git 版本库的钩子脚本`post-update`可以帮助完成这个无聊的工作。在版本库的`hooks`目录下创建可执行脚本文件`post-update`，内容如下：

```
#!/bin/sh
#
# An example hook script to prepare a packed repository for use over
# dumb transports.
#
# To enable this hook, rename this file to "post-update".

exec git update-server-info

```

哑 HTTP 协议也可以对版本库写操作提供支持，即允许客户端向服务器推送。这需要在 Apache 中为版本库设置 WebDAV，并配置口令认证。例如下面的 Apache 配置片段：

```
Alias /git /path/to/repos

<Directory /path/to/repos>
    Options FollowSymLinks
    AllowOverride None
    Order Allow,Deny
    Allow from all

    # 启用 WebDAV
    DAV on

    # 简单口令认证
    AuthType Basic
    AuthName "Git Repository"
    AuthBasicProvider file
    # 该口令文件用 htpasswd 命令进行管理
    AuthUserFile /path/to/git-passwd
    Require valid-user

    # 基于主机 IP 的认证和基于口令的认证必须同时满足
    Satisfy All
</Directory>

```

配置了口令认证后，最好使用 HTTPS 协议访问服务器，以避免因为口令在网络中明文传输造成口令泄露。还可以在 URL 地址中加上用户名，以免在连接过程中的重复输入。下面的示例中以特定用户（如：jiangxin）身份访问版本库：

*   如果版本库尚未克隆，使用如下命令克隆：

    ```
    $ git clone https://jiangxin@server/git/myrepo.git

    ```

*   如果已经克隆了版本库，可以执行下面命令修改远程`origin`版本库的 URL 地址：

    ```
    $ cd myrepos
    $ git remote set-url origin https://jiangxin@server/git/myrepo.git
    $ git pull

    ```

第一次连接服务器，会提示输入口令。正确输入口令后，完成克隆或版本库的更新。试着在版本库中添加新的提交，然后执行`git push`推送到 HTTP 服务器。

如果推送失败，可能是 WebDAV 配置的问题，或者是版本库的文件、目录的权限不正确（需要能够被执行 Apache 进程的用户可以读写）。一个诊断 Apache 的小窍门是查看和跟踪 Apache 的配置文件[[1]](#id7)。如下：

```
$ tail -f /var/www/error.log

```

## 5.1.2\. 智能 HTTP 协议

Git 1.6.6 之后的版本，提供了针对 HTTP 协议的 CGI 程序`git-http-backend`，实现了智能的 HTTP 协议支持。同时也要求 Git 客户端的版本也不低于 1.6.6。

查看文件`git-http-backend`的安装位置，可以用如下命令。

```
$ ls $(git --exec-path)/git-http-backend
/usr/lib/git-core/git-http-backend

```

在 Apache2 中为 Git 配置智能 HTTP 协议如下。

```
SetEnv GIT_PROJECT_ROOT /var/www/git
SetEnv GIT_HTTP_EXPORT_ALL
ScriptAlias /git/ /usr/lib/git-core/git-http-backend/

```

说明：

*   第一行设置版本库的根目录为`/var/www/git`。

*   第二行设置所有版本库均可访问，无论是否在版本库中存在`git-daemon-export-ok`文件。

    缺省只有在版本库目录中存在文件`git-daemon-export-ok`，该版本库才可以访问。这个文件是 git-daemon 服务的一个特性。

*   第三行，就是使用`git-http-backend`CGI 脚本来相应客户端的请求。

    当用地址`http://server/git/myrepo.git`访问时，即由此 CGI 提供服务。

**写操作授权**

上面的配置只能提供版本库的读取服务，若想提供基于 HTTP 协议的写操作，必须添加认证的配置指令。当用户通过认证后，才能对版本库进行写操作。

下面的 Apache 配置，在前面配置的基础上，为 Git 写操作提供授权：

```
<LocationMatch "^/git/.*/git-receive-pack$">
  AuthType Basic
  AuthName "Git Access"
  AuthType Basic
  AuthBasicProvider file
  AuthUserFile /path/to/passwd/file
  ...
</LocationMatch>

```

**读和写均需授权**

如果需要对读操作也进行授权，那就更简单了，一个`Location`语句就够了。

```
<Location /git/private>
  AuthType Basic
  AuthName "Git Access"
  AuthType Basic
  AuthBasicProvider file
  AuthUserFile /path/to/passwd/file
  ...
</Location>

```

**对静态文件的直接访问**

如果对静态文件的访问不经过 CGI 程序，直接由 Apache 提供服务，会提高访问性能。

下面的设置对 Git 版本库中的`objects`目录下文件的访问，不经过 CGI。

```
SetEnv GIT_PROJECT_ROOT /var/www/git

AliasMatch ^/git/(.*/objects/[0-9a-f]{2}/[0-9a-f]{38})$          /var/www/git/$1
AliasMatch ^/git/(.*/objects/pack/pack-[0-9a-f]{40}.(pack|idx))$ /var/www/git/$1
ScriptAlias /git/ /usr/libexec/git-core/git-http-backend/

```

Git 的智能 HTTP 服务彻底打破了以前哑传输协议给 HTTP 协议带来的恶劣印象，让 HTTP 协议成为 Git 服务的一个重要选项。但是在授权的管理上，智能 HTTP 服务仅仅依赖 Apache 自身的授权模型，相比后面要介绍的 Gitosis 和 Gitolite，可管理性要弱的多。

*   创建版本库只能在服务器端进行，不能通过远程客户端进行。
*   配置认证和授权，也只能在服务器端进行，不能在客户端远程配置。
*   版本库的写操作授权只能进行非零即壹的授权，不能针对分支甚至路径进行授权。

需要企业级的版本库管理，还需要考虑后面介绍的基于 SSH 协议的 Gitolite 或 Gitosis。

## 5.1.3\. Gitweb 服务器

前面介绍的 HTTP 哑协议和智能 HTTP 协议服务架设，都可以用于提供 Git 版本库的读写服务，而本节介绍的 Gitweb 作为一个 Web 应用，只提供版本库的图形化浏览功能，而不能提供版本库本身的读写。

Gitweb 是用 Perl 语言开发的 CGI 脚本，架设比较方便。Gitweb 支持多个版本库，可以对版本库进行目录浏览（包括历史版本），可以查看文件内容，查看提交历史，提供搜索以及 RSS feed 支持。也可以提供目录文件的打包下载等。图 27-1 就是 kernel.org 上的 Gitweb 示例。

![../_images/gitweb-kernel-org_full.png](img/gitweb-kernel-org_full.png)

图 27-1：Gitweb 界面(kernel.org)

### 5.1.3.1\. Gitweb 安装

各个 Linux 平台都会提供 Gitweb 软件包。如在 Debian/Ubuntu 上安装 Gitweb：

```
$ sudo aptitude install gitweb

```

安装文件列表：

*   配置文件：`/etc/gitweb.conf`。
*   Apache 配置文件：`/etc/apache2/conf.d/gitweb`。默认设置用 URL 地址`/gitweb`来访问 Gitweb 服务。
*   CGI 脚本：`/usr/share/gitweb/index.cgi`。
*   其他附属文件：`/usr/share/gitweb/*`，如：图片和 css 等。

### 5.1.3.2\. Gitweb 配置

编辑`/etc/gitweb.conf`，更改 Gitweb 的默认设置。

*   版本库根目录的设置。

    ```
    $projectroot = "/var/cache/git";

    ```

*   访问版本库多种协议的地址设置。

    Gitweb 可以为每个版本库显示访问的协议地址。可以在列表中填入多个地址。

    ```
    @git_base_url_list = ("git://bj.ossxp.com/git", "ssh://git\@bj.ossxp.com", "http://bj.ossxp.com/git");

    ```

*   增加 actions 菜单

    ```
    $feature{'actions'}{'default'} = [('git', 'git://bj.ossxp.com/git/%n', 'tree')];

    ```

*   在首页上显示自定义信息

    设定自定义 HTML 的文件名。

    ```
    $home_text = "indextext.html";

    ```

    在 CGI 脚本所在的目录下，创建`indextext.html`文件。下面是我们公司（北京群英汇信息技术有限公司）内部 gitweb 自定义首页的内容。

    ```
    <html>
    <head>
    </head>
    <body>
    <h2>群英汇 - git 代码库</h2>
    <ul>
      <li>点击版本库，进入相应的版本库页面，有 URL 指向一个 git://... 的检出链接</li>
      <li>使用命令 git clone git://... 来克隆一个版本库</li>
      <li>对于名称中含有 <i>-gitsvn</i> 字样的代码库, 是用 git-svn 从 svn 代码库镜像而来的。对于它们的镜像，需要做进一步的工作。
        <ul>
          <li>要将 git 库的远程分支(.git/ref/remotes/*) 也同步到本地！
            <pre>
            $ git config --add remote.origin.fetch '+refs/remotes/*:refs/remotes/*'
            $ git fetch
            </pre>
          </li>
          <li>如果需要克隆库和 Subversion 同步。用 git-svn 初始化代码库，并使得相关配置和源保持一致 </li>
        </ul>
      </li>
    </ul>
    </body>
    </html>

    ```

*   版本库列表。

    缺省扫描版本库根目录，查找版本库。如果版本库非常多，这个查找过程可能很耗时，可以提供一个文本文件包含版本库的列表，会加速 Gitweb 显示初始化。

    ```
    # $projects_list = $projectroot;
    $projects_list = "/home/git/gitosis/projects.list";

    ```

    后面介绍的 Gitosis 和 Gitolite 都可以自动生成这么一个版本库列表，供 Gitweb 使用。

*   Gitweb 菜单定制。

    *   在 tree view 文件的旁边显示追溯（blame）链接。

        ```
  $feature{'blame'}{'default'} = [1];
        $feature{'blame'}{'override'} = 1;

        ```

    *   可以通过版本库的配置文件`config`对版本库进行单独设置。

        下面的设置覆盖 Gitweb 的全局设置，不对该项目显示 blame 菜单。

        ```
  [gitweb]
        blame = 0

        ```

    *   为每个 tree 添加快照（snapshot）下载链接。

        ```
  $feature{'pickaxe'}{'default'} = [1];
        $feature{'pickaxe'}{'override'} = 1;
        $feature{'snapshot'}{'default'} = ['zip', 'tgz'];
        $feature{'snapshot'}{'override'} = 1;

        ```

### 5.1.3.3\. 版本库的 Gitweb 相关设置

可以通过 Git 版本库下的配置文件，定制版本库在 Gitweb 下的显示。

*   文件`description`。

    提供一行简短的 git 库描述。显示在版本库列表中。

    也可以通过`config`配置文件中的`gitweb.description`进行设置。但是文件优先。

*   文件`README.html`。

    提供更详细的项目描述，显示在 Gitweb 项目页面中。

*   文件`cloneurl`。

    版本库访问的 URL 地址，一个一行。

*   文件`config`。

    通过`[gitweb]`小节的配置，覆盖 Gitweb 全局设置。

    *   `gitweb.owner`用于显示版本库的创建者。
    *   `gitweb.description`显示项目的简短描述，也可以通过`description`文件来提供。（文件优先）
    *   `gitweb.url`显示项目的 URL 列表，也可以通过`cloneurl`文件来提供。（文件优先）

| [[1]](#id2) | Apache 日志文件的位置参见 Apache 配置文件中`ErrorLog`指令的设定。 |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.2\. 使用 Git 协议

Git 协议是提供 Git 版本库只读服务的最为常用的协议，也是非常易用和易于配置的协议。该协议的缺点就是不能提供身份认证，而且一般也不提供写入服务。

## 5.2.1\. Git 协议语法格式

Git 协议的语法格式如下。

```
语法： git://<server>[:<port>]/path/to/repos.git/

```

说明：

*   端口为可选项，默认端口为 9418。
*   版本库路径`/path/to/repos.git`的根目录并不一定是系统的根目录，可以在**git-daemon**启动时用参数`--base-path`指定根目录。如果**git-daemon**没有设置根目录，则对应于系统的根目录。

## 5.2.2\. Git 服务软件

Git 服务由名为 git-daemon 的服务软件提供。虽然 git-daemon 也可以支持写操作，但因为 git-daemon 没有提供认证支持，因此没有人胆敢配置 git-daemon 提供匿名的写服务。使用 git-daemon 提供的 Git 版本库的只读服务，效率很高，而且是一种智能协议，在操作过程有进度显示，远比 HTTP 哑通讯协议方便（Git 1.6.6 之后的版本已经支持智能 HTTP 通讯协议）。因此 git-daemon 很久一来，一直是 Git 版本库只读服务的首选。

Git 软件包本身提供了 git-daemon，因此只要安装了 Git，一般就已经安装了 git-daemon。默认 git-daemon 并没有运行，需要对其进行配置以服务方式运行。下面介绍两种不同的配置运行方式。

## 5.2.3\. 以 inetd 方式配置运行

最简单的方式，是以 inetd 服务方式运行 git-daemon。在配置文件`/etc/inetd.conf`中添加设置如下:

```
git stream tcp nowait nobody  /usr/bin/git
    git daemon --inetd --verbose --export-all
    /gitroot/foo /gitroot/bar

```

说明：

*   以 nobody 用户身份执行**git daemon**服务。

*   缺省**git daemon**只对包含文件`git-daemon-export-ok`的版本库提供服务。使用参数`--export-all`后，无论版本库是否存在标识文件`git-daemon-export-ok`，都对版本库提供 Git 协议支持。

*   后面的两个参数是版本库。

    也可以通过`--base-path=<path>`参数，设置版本库的根，对于这个目录下的所有版本库提供访问服务。例如下面的 inetd 配置：

    ```
    git stream tcp nowait nobody  /usr/bin/git
        git daemon --inetd --verbose --export-all
        --base-path=/var/cache /var/cache/git

    ```

## 5.2.4\. 以 runit 方式配置运行

runit 是类似于 sysvinit 的服务管理进程，但是更简单。在 Debian/Ubuntu 上的软件包**git-daemon-run**就是基于 runit 启动 git-daemon 服务。

*   安装**git-daemon-run**：

    ```
    $ sudo aptitude install git-daemon-run

    ```

*   配置**git-daemon-run**：

    缺省的服务配置文件：`/etc/sv/git-daemon/run`。和之前的 inetd 运行方式相比，以独立的服务进程启动，相应速度更快。

    ```
    #!/bin/sh
    exec 2>&1
    echo 'git-daemon starting.'
    exec chpst -ugitdaemon \
      "$(git --exec-path)"/git-daemon --verbose --export-all --base-path=/var/cache /var/cache/git

    ```

缺省版本库中需要存在文件`git-daemon-export-ok`，git-daemon 才对此版本库提供服务。不过可以通过启动 git-daemon 时提供的参数`--export-all`，无论版本库是否存在标识文件`git-daemon-export-ok`，都对版本库提供 Git 协议支持。

通过 git-daemon 提供的 Git 访问协议存在着局限性：

*   不支持认证。管理员大概可以做的只是配置防火墙，限制某个网段用户的使用。
*   只能提供匿名的版本库读取服务。因为写操作没有授权控制，因此一般不用来提供写操作。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.3\. 使用 SSH 协议

SSH 协议用于为 Git 提供远程读写操作，是远程写操作的标准服务，在智能 HTTP 协议出现之前，甚至是写操作的唯一标准服务。

## 5.3.1\. SSH 协议语法格式

对于拥有 shell 登录权限的用户帐号，可以用下面的语法访问 Git 版本库：

```
语法 1： ssh://[<username>@]<server>[:<port>]/path/to/repos/myrepo.git
语法 2： [<username>@]<server>:/path/to/repos/myrepo.git

```

说明：

*   SSH 协议地址格式可以使用两种不同的写法，第一种是使用`ssh://`开头的 SSH 协议标准 URL 写法，另外一种是 SCP 格式的写法。

    两种写法均可，SSH 协议标准 URL 写法稍嫌复杂，但是对于非标准 SSH 端口（非 22 端口），可以通过 URL 给出端口号。

*   <username>是服务器<server>上的用户帐号。

    如果省略用户名，则缺省使用当前登录用户名（配置和使用了服务器别名除外）。

*   <port>为 SSH 协议端口缺省为 22。

    端口只有在 SSH 协议标准 URL 写法可以给出，如果省略则使用缺省值 22（配置和使用了服务器别名除外）。

*   `/path/to/repos/myrepo.git`是服务器中版本库的绝对路径。若用相对路径则相对于 username 用户的主目录而言。

*   如果采用口令认证，不能像 HTTPS 协议那样可以在 URL 中同时给出登录名和口令，必须每次连接时输入。

*   如果采用公钥认证，则无须输入口令。

## 5.3.2\. 服务架设方式比较

SSH 协议来实现 Git 服务，有如下方式：

*   其一是用标准的 ssh 帐号访问版本库。即用户帐号可以直接登录到服务器，获得 shell。

    对于这种使用标准 SSH 帐号方式，直接使用标准的 SSH 服务就可以了，无须赘述。

*   另外的方式是，所有用户都使用同一个专用的 SSH 帐号访问版本库。各个用户通过公钥认证的方式用此专用 SSH 帐号访问版本库。而用户在连接时使用的不同的公钥可以用于区分不同的用户身份。

    Gitosis 和 Gitolite 就是实现该方式的两个服务器软件。

标准 SSH 帐号和专用 SSH 帐号的区别见表 29-1。

> 表 29-1：不同 SSH 服务架设 Git 的对照
> 
> |   | 标准 SSH | Gitosis/Gitolite |
> | --- | --- | --- |
> | 帐号 | 每个用户一个帐号 | 所有用户共用同一个帐号 |
> | 认证方式 | 口令或公钥认证 | 公钥认证 |
> | 用户是否能直接登录 shell | 是 | 否 |
> | 安全性 | 差 | 好 |
> | 管理员是否需要 shell | 是 | 否 |
> | 版本库路径 | 相对路径或绝对路径 | 相对路径 |
> | 授权方式 | 操作系统中用户组和目录权限 | 通过配置文件授权 |
> | 对分支进行写授权 | 否 | Gitolite |
> | 对路径进行写授权 | 否 | Gitolite |
> | 架设难易度 | 简单 | 复杂 |

实际上，标准 SSH，也可以用公钥认证的方式实现所有用户共用同一个帐号。不过这类似于把一个公共帐号的登录口令同时告诉给多个人。

*   在服务器端(server)创建一个公共帐号，例如：anonymous。

*   管理员收集需要访问 git 服务的用户公钥。如:`user1.pub`、`user2.pub`。

*   使用**ssh-copy-id**命令远程将各个 git 用户的公钥加入服务器（server）的公钥认证列表中。

    ```
    $ ssh-copy-id -i user1.pub anonymous@server
    $ ssh-copy-id -i user2.pub anonymous@server

    ```

    如果直接在服务器上操作，则直接将文件追加到`authorized_keys`文件中。

    ```
    $ cat /path/to/user1.pub >> ~anonymous/.ssh/authorized_keys
    $ cat /path/to/user2.pub >> ~anonymous/.ssh/authorized_keys

    ```

*   在服务器端的`anonymous`用户主目录下建立 git 库，就可以实现多个用户利用同一个系统帐号（git）访问 Git 服务了。

这样做除了免除了逐一设置帐号，以及用户无需口令认证之外，标准 SSH 部署 Git 服务的缺点一个也不少，而且因为用户之间无法区分，更无法进行针对用户授权。

下面重点介绍一下 SSH 公钥认证，因为它们是后面介绍的 Gitosis 和 Gitolite 服务器软件的基础。

## 5.3.3\. 关于 SSH 公钥认证

关于公钥认证的原理，维基百科上的这个条目是一个很好的起点：[`en.wikipedia.org/wiki/Public-key_cryptography`](http://en.wikipedia.org/wiki/Public-key_cryptography)。

如果用户的主目录下不存在`.ssh`目录，说明 SSH 公钥/私钥对尚未创建。可以用这个命令创建：

> $ ssh-keygen

该命令会在用户主目录下创建`.ssh`目录，并在其中创建两个文件：

*   id_rsa

    私钥文件。是基于 RSA 算法创建。该私钥文件要妥善保管，不要泄漏。

*   id_rsa.pub

    公钥文件。和`id_rsa`文件是一对儿，该文件作为公钥文件，可以公开。

创建了自己的公钥/私钥对后，就可以使用下面的命令，实现无口令登录远程服务器，即用公钥认证取代口令认证。

```
$ ssh-copy-id -i .ssh/id_rsa.pub <user>@<server>

```

说明:

*   该命令会提示输入用户 user 在 server 上的 SSH 登录口令。
*   当此命令执行成功后，再以 user 用户登录 server 远程主机时，不必输入口令直接登录。
*   该命令实际上将`.ssh/id_rsa.pub`公钥文件追加到远程主机 server 的 user 主目录下的`.ssh/authorized_keys`文件中。

检查公钥认证是否生效，运行 SSH 到远程主机，正常的话应该直接登录成功。如果要求输入口令则表明公钥认证配置存在问题。如果 SSH 登录存在问题，可以通过查看服务器端的`/var/log/auth.log`日志文件进行诊断。

## 5.3.4\. 关于 SSH 主机别名

在实际应用中，有时需要使用多套公钥/私钥对，例如：

*   使用默认的公钥访问 git 帐号，获取 shell，进行管理员维护工作。
*   使用单独创建的公钥访问 git 帐号，执行 git 命令。
*   访问 GitHub（免费的 Git 服务托管商）采用其他公钥。

首先要能够创建不同名称的公钥/私钥对。还是用**ssh-keygen**命令，如下：

```
$ ssh-keygen -f ~/.ssh/<filename>

```

注：

*   将`<filename>`替换为有意义的名称。
*   会在`~/.ssh`目录下创建指定的公钥/私钥对。 文件`<filename>`是私钥，文件`<filename>.pub`是公钥。

将新生成的公钥添加到远程主机的`.ssh/authorized_keys`文件中，建立新的公钥认证。例如:

```
$ ssh-copy-id -i .ssh/<filename>.pub <user>@<server>

```

这样，就有两个公钥用于登录主机 server，那么当执行下面的 ssh 登录指令，用到的是那个公钥呢？

```
$ ssh <user>@<server>

```

当然是默认公钥`~/.ssh/id_rsa.pub`。那么如何用新建的公钥连接 server 呢？

SSH 的客户端配置文件`~/.ssh/config`可以通过创建主机别名，在连接主机时，使用特定的公钥。例如`~/.ssh/config`文件中的下列配置：

```
host bj
  user git
  hostname bj.ossxp.com
  port 22
  identityfile ~/.ssh/jiangxin

```

当执行

```
$ ssh bj

```

或者执行

```
$ git clone bj:path/to/repos/myrepo.git

```

含义为：

*   登录的 SSH 主机为`bj.ossxp.com`。
*   登录时使用的用户名为 git。
*   认证时使用的公钥文件为`~/.ssh/jiangxin.pub`。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.4\. Gitolite 服务架设

Gitolite 是一款 Perl 语言开发的 Git 服务管理工具，通过公钥对用户进行认证，并能够通过配置文件对写操作进行基于分支和路径的精细授权。Gitolite 采用的是 SSH 协议并且使用 SSH 公钥认证，因此无论是管理员还是普通用户，都需要对 SSH 非常熟悉。在开始之前，请确认您已经通读过第二十九章“使用 SSH 协议”。

Gitolite 的官方网址是：[`github.com/sitaramc/gitolite`](http://github.com/sitaramc/gitolite)。从提交日志里可以看出作者是 Sitaram Chamarty，最早的提交开始于 2009 年 8 月。作者是受到了 Gitosis 的启发，开发了这款功能更为强大和易于安装的软件。Gitolite 的命名，作者的原意是 Gitosis 和 lite 的组合，不过因为 Gitolite 的功能越来越强大，已经超越了 Gitosis，因此作者笑称 Gitolite 可以看作是 Github-lite——轻量级的 Github。

我是在 2010 年 8 月才发现 Gitolite 这个项目的，并尝试将公司基于 Gitosis 的管理系统迁移至 Gitolite。在迁移和使用过程中，增加和改进了一些实现，如：通配符版本库的创建过程，对创建者的授权，版本库名称映射等。本文关于 Gitolite 的介绍也是基于我改进的版本[[1]](#id37)。

*   原作者的版本库地址：

    [`github.com/sitaramc/gitolite`](http://github.com/sitaramc/gitolite)

*   笔者改进后的 Gitolite 分支：

    [`github.com/ossxp-com/gitolite`](http://github.com/ossxp-com/gitolite)

Gitolite 的实现机制和使用特点概述如下：

*   Gitolite 安装在服务器（server）某个帐号之下，例如`git`帐号。

*   管理员通过**git**命令检出名为`gitolite-admin`的版本库。

    ```
    $ git clone git@server:gitolite-admin.git

    ```

*   管理员将所有 Git 用户的公钥保存在 gitolite-admin 库的`keydir`目录下，并编辑`conf/gitolite.conf`文件为用户授权。

*   当管理员提交对 gitolite-admin 库的修改并推送到服务器之后，服务器上`gitolite-admin`版本库的钩子脚本将执行相应的设置工作。

    *   新用户的公钥自动追加到服务器端安装帐号主目录下的`.ssh/authorized_keys`文件中，并设置该用户的 shell 为 gitolite 的一条命令**gl-auth-command**。在`.ssh/authorized_keys`文件中增加的内容示例如下： [[2]](#id38)

        ```
  command="/home/git/bin/gl-auth-command jiangxin",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa AAAAB3NzaC1yc2...(公钥内容来自于 jiangxin.pub)...

        ```

    *   更新服务器端的授权文件`~/.gitolite/conf/gitolite.conf`。

    *   编译授权文件为`~/.gitolite/conf/gitolite.conf-compiled.pm`。

*   若用 ssh 命令登录服务器（以 git 用户登录）时，因为公钥认证的相关设置（使用**gl-auth-command**作为 shell），不能进入 shell 环境，而是打印服务器端 git 库授权信息后马上退出。即用户不会通过 git 用户进入服务器的 shell，也不会对系统的安全造成威胁。

    ```
    $ ssh git@bj
    hello jiangxin, the gitolite version here is v1.5.5-9-g4c11bd8
    the gitolite config gives you the following access:
         R          gistore-bj.ossxp.com/.*$
      C  R  W       ossxp/.*$
     @C @R  W       users/jiangxin/.+$
    Connection to bj closed.

    ```

*   用户可以用 git 命令访问授权的版本库。

*   若管理员授权，用户可以远程在服务器上创建新版本库。

下面介绍 Gitolite 的部署和使用。

## 5.4.1\. 安装 Gitolite

安装 Gitolite（2.1 版本）对服务器的要求是：

*   Git 版本为 1.6.6 或以上。
*   Unix 或类 Unix（Linux、MacOS 等）操作系统。
*   服务器开启 SSH 服务。

和其他 Unix 上软件包一样 Gitolite 既可通过操作系统本身提供的二进制发布包方式安装，也可通过克隆 Gitolite 源码库从源代码安装 Gitolite。

Note

老版本的 Gitolite 提供了一种从客户端发起安装的模式，但该安装模式需要管理员维护两套不同公钥/私钥对（一个公钥用于无口令登录服务器以安装和更新软件，另外一个公钥用于克隆和推送 gitolite-admin 版本库），稍嫌复杂，在 2.1 之后的 Gitolite 取消了这种安装模式。

### 5.4.1.1\. 安装之前

Gitolite 搭建的 Git 服务器是以 SSH 公钥认证为基础的，无论是普通 Git 用户还是 Gitolite 的管理员都通过公钥认证访问 Gitolite 服务器。在 Gitolite 的安装过程中需要提供管理员公钥，以便在 Gitolite 安装完毕后管理员能够远程克隆`gitolite-admin`版本库（仅对管理员授权），对 Gitolite 服务器进行管理——添加新用户和为用户添加授权。

为此在安装 Gitolite 之前，管理员需要在客户端（用于远程管理 Gitolite 服务器的客户端）创建用于连接 Gitolite 服务器的 SSH 公钥（如果尚不存在的话），并把公钥文件拷贝到服务器上。

1.  在客户端创建 SSH 公钥/私钥对。

    如果管理员在客户端尚未创建公钥/私钥对，使用下面的命令会在用户主目录下创建名为`~/.ssh/id_rsa`的 SSH 私钥和名为`~/.ssh/id_rsa.pub`的公钥文件：

    ```
    $ ssh-keygen

    ```

2.  将公钥文件从客户端复制到服务器端，以便安装 Gitolite 时备用。

    可以使用**ftp**或 U 盘拷贝等方式从客户端向服务器端传送文件，不过用**scp**命令是非常方便的，例如服务器地址为`server`，相应的拷贝命令为：

    ```
    $ scp ~/.ssh/id_rsa.pub server:/tmp/admin.pub

    ```

### 5.4.1.2\. 以发布包形式安装

常见的 Linux 发行版都包含了 Gitolite 软件包，安装 Gitolite 使用如下命令：

*   Debian/Ubuntu：

    ```
    $ sudo aptitude install gitolite

    ```

*   RedHat：

    ```
    $ sudo yum install gitolite

    ```

安装完毕后会自动创建一个专用系统账号如`gitolite`。在 Debian 平台上创建的`gitolite`账号使用`/var/lib/gitolite`作为用户主目录，而非`/home/gitolite`。

```
$ getent passwd gitolite
gitolite:x:114:121:git repository hosting,,,:/var/lib/gitolite:/bin/bash

```

安装完毕，运行如下命令完成对 Gitolite 的配置：

1.  切换至新创建的`gitolite`用户账号。

    ```
    $ sudo su - gitolite

    ```

2.  运行`gl-setup`命令，并以客户端复制过来的公钥文件路径作为参数。

    ```
    $ gl-setup /tmp/admin.pub

    ```

Debian 等平台会在安装过程中（或运行**sudo dpkg-reconfigure gitolite**命令时），开启配置界面要求用户输入 Gitolite 专用账号、Git 版本库根目录、管理员公钥文件名，然后自动执行**gl-setup**完成设置。

### 5.4.1.3\. 从源代码开始安装

如果想在系统中部署多个 Gitolite 实例，希望部署最新的 Gitolite 版本，或者希望安装自己或他人对 Gitolite 的定制版本，就要采用从源代码进行 Gitolite 部署。

1.  创建专用系统账号。

    首先需要在服务器上创建 Gitolite 专用帐号。因为所有用户都要通过此帐号访问 Git 版本库，为方便易记一般选择更为简练的`git`作为专用帐号名称。

    ```
    $ sudo adduser --system --group --shell /bin/bash git

    ```

    注意添加的用户要能够远程登录，若系统只允许特定用户组（如`ssh`用户组）的用户才可以通过 SSH 协议登录，就需要将新建的`git`用户添加到该特定的用户组中。执行下面的命令可以将`git`用户添加到`ssh`用户组。

    ```
    $ sudo adduser git ssh

    ```

    取消`git`用户的口令，以便只能通过公钥对`git`账号进行认证，增加系统安全性。

    ```
    $ sudo passwd --delete git

    ```

2.  切换到新创建的用户账号，后续的安装都以该用户身份执行。

    ```
    $ sudo su - git

    ```

3.  在服务器端下载 Gitolite 源码。一个更加“Git”的方式就是克隆 Gitolite 的版本库。

    *   克隆官方的 Gitolite 版本库如下：

        ```
  $ git clone git://github.com/sitaramc/gitolite.git

        ```

    *   也可以克隆定制后的 Gitolite 版本库，如我在 GitHub 上基于 Gitolite 官方版本库建立的分支版本：

        ```
  $ git clone git://github.com/ossxp-com/gitolite.git

        ```

4.  安装 Gitolite。

    运行源码目录中的**src/gl-system-install**执行安装。

    ```
    $ cd gitolite
    $ src/gl-system-install

    ```

    如果像上面那样不带参数的执行安装程序，会将 Gitolite 相关命令安装到`~/bin`目录中，相当于执行：

    ```
    $ src/gl-system-install $HOME/bin $HOME/share/gitolite/conf $HOME/share/gitolite/hooks

    ```

5.  运行**gl-setup**完成设置。

    若 Gitolite 安装到`~/bin`目录下（即没有安装到系统目录下），需要设置`PATH`环境变量以便**gl-setup**能够正常运行。

    ```
    $ export PATH=~/bin:$PATH

    ```

    然后运行**gl-setup**命令，并以客户端复制过来的公钥文件路径作为参数。

    ```
    $ ~/bin/gl-setup /tmp/admin.pub

    ```

## 5.4.2\. 管理 Gitolite

### 5.4.2.1\. 管理员克隆 gitolite-admin 管理库

当 Gitolite 安装完成后，就会在服务器端版本库根目录下创建一个用于管理 Gitolite 的版本库。若以`git`用户安装，则该 Git 版本库的路径为：`~git/repositories/gitolite-admin.git`。

在客户端用**ssh**命令连接服务器`server`的`git`用户，如果公钥认证验证正确的话，Gitolite 将此 SSH 会话的用户认证为`admin`用户，显示`admin`用户的权限。如下：

```
$ ssh -T git@server
hello admin, this is gitolite v2.1-7-ge5c49b7 running on git 1.7.7.1
the gitolite config gives you the following access:
     R   W      gitolite-admin
    @R_ @W_     testing

```

从上面命令的倒数第二行输出可以看出用户`admin`对版本库`gitolite-admin`拥有读写权限。

为了对 Gitolite 服务器进行管理，需要在客户端克隆`gitolite-admin`版本库，使用如下命令：

```
$ git clone git@server:gitolite-admin.git
$ cd gitolite-admin/

```

在客户端克隆的`gitolite-admin`目录下有两个子目录`conf/`和`keydir/`，包含如下文件：

*   文件：`keydir/admin.pub`。

    目录`keydir`下初始时只有一个用户公钥，即管理员`admin`的公钥。

*   文件：`conf/gitolite.conf`。

    该文件为授权文件。初始内容为：

    ```
    repo    gitolite-admin
            RW+     =   admin

    repo    testing
            RW+     =   @all

    ```

    默认授权文件中只设置了两个版本库的授权：

    *   gitolite-admin

        即本版本库。此版本库用于 Gitolite 管理，只有`admin`用户有读写和强制更新的权限。

    *   testing

        默认设置的测试版本库。设置为任何人都可以读写及强制更新。

### 5.4.2.2\. 增加新用户

增加新用户，就是允许新用户能够通过其公钥访问 Git 服务。只要将新用户的公钥添加到`gitolite-admin`版本库的`keydir`目录下，即完成新用户的添加，具体操作过程如下。

1.  管理员从用户获取公钥，并将公钥按照`username.pub`格式进行重命名。

    *   用户可以通过邮件或其他方式将公钥传递给管理员，切记不要将私钥误传给管理员。如果发生私钥泄漏，马上重新生成新的公钥/私钥对，并将新的公钥传递给管理员，并申请将旧的公钥作废。
    *   用户从不同的客户端主机访问有着不同的公钥，如果希望使用同一个用户名进行授权，可以按照`username@host.pub`的方式命名公钥文件，和名为`username.pub`的公钥指向同一个用户`username`。
    *   Gitolite 也支持邮件地址格式的公钥，即形如`username@gmail.com.pub`的公钥。Gitolite 能够很智能地区分是以邮件地址命名的公钥还是相同用户在不同主机上的公钥。如果是邮件地址命名的公钥，将以整个邮件地址作为用户名。
    *   还可以在`keydir`目录下创建子目录来管理用户公钥，同一用户的不同公钥可以用同一名称保存在不同子目录中。
2.  管理员进入`gitolite-admin`本地克隆版本库中，复制新用户公钥到`keydir`目录。

    ```
    $ cp /path/to/dev1.pub keydir/
    $ cp /path/to/dev2.pub keydir/
    $ cp /path/to/jiangxin.pub keydir/

    ```

3.  执行**git add**命令，将公钥添加到版本库。

    ```
    $ git add keydir

    ```

4.  执行**git commit**，完成提交。

    ```
    $ git commit -m "add user: jiangxin, dev1, dev2"

    ```

5.  执行**git push**，同步到服务器，才真正完成新用户的添加。

    ```
    $ git push
    Counting objects: 8, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (6/6), done.
    Writing objects: 100% (6/6), 1.38 KiB, done.
    Total 6 (delta 0), reused 0 (delta 0)
    remote: Already on 'master'
    remote:
    remote:                 ***** WARNING *****
    remote:         the following users (pubkey files in parens) do not appear in the config file:
    remote: dev1(dev1.pub),dev2(dev2.pub),jiangxin(jiangxin.pub)

    ```

    在**git push**的输出中，以`remote`标识的输出是服务器端执行`post-update`钩子脚本的错误输出，用于提示新增的三个用户（公钥）在授权文件中没有被引用。接下来会介绍如何修改授权文件，以及如何为用户添加授权。

服务器端的`git`主目录下的`.ssh/authorized_keys`文件会随着新增用户公钥而更新，即添加三条新的记录。如下：

```
$ cat ~git/.ssh/authorized_keys
# gitolite start
command="/home/git/bin/gl-auth-command admin",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty    <用户 admin 的公钥...>
command="/home/git/bin/gl-auth-command dev1",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty     <用户 dev1 的公钥...>
command="/home/git/bin/gl-auth-command dev2",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty     <用户 dev2 的公钥...>
command="/home/git/bin/gl-auth-command jiangxin",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty <用户 jiangxin 的公钥...>
# gitolite end

```

### 5.4.2.3\. 更改授权

新用户添加完毕，接下来需要为新用户添加授权，这个过程也比较简单，只需修改`conf/gitolite.conf`配置文件，提交并推送。具体操作过程如下：

1.  管理员进入`gitolite-admin`本地克隆版本库中，编辑`conf/gitolite.conf`。

    ```
    $ vi conf/gitolite.conf

    ```

2.  授权指令比较复杂，先通过建立新用户组尝试一下更改授权文件。

    考虑到之前增加了三个用户公钥，服务器端发出了用户尚未在授权文件中出现的警告。现在就在这个示例中解决这个问题。

    *   可以在其中加入用户组`@team1`，将新添加的用户 jiangxin、dev1、dev2 都归属到这个组中。

        只需要在`conf/gitolite.conf`文件的文件头加入如下指令即可。用户名之间用空格分隔。

        ```
  @team1 = dev1 dev2 jiangxin

        ```

    *   编辑完毕退出。可以用**git diff**命令查看改动：

        还修改了版本库`testing`的授权，将`@all`用户组改为新建立的`@team1`用户组。

        ```
  $ git diff
        diff --git a/conf/gitolite.conf b/conf/gitolite.conf
        index 6c5fdf8..f983a84 100644
        --- a/conf/gitolite.conf
        +++ b/conf/gitolite.conf
        @@ -1,5 +1,7 @@
        +@team1 = dev1 dev2 jiangxin
        +
         repo    gitolite-admin
                 RW+     =   admin

         repo    testing
        -        RW+     =   @all
        +        RW+     =   @team1

        ```

3.  编辑结束，提交改动。

    ```
    $ git add conf/gitolite.conf
    $ git commit -q -m "new team @team1 auth for repo testing."

    ```

4.  执行**git push**，同步到服务器，授权文件的更改才真正生效。

    可以注意到，推送后的输出中没有了警告。

    ```
    $ git push
    Counting objects: 7, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 398 bytes, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: Already on 'master'
    To git@server:gitolite-admin.git
       bd81884..79b29e4  master -> master

    ```

## 5.4.3\. Gitolite 授权详解

### 5.4.3.1\. 授权文件的基本语法

下面看一个不那么简单的授权文件。为方便描述添加了行号。

```
 1  @manager = jiangxin wangsheng
 2  @dev   = dev1 dev2 dev3
 3
 4  repo    gitolite-admin
 5          RW+                         = jiangxin
 6
 7  repo    ossxp/[a-z].+
 8          C                           = @manager
 9          RW+                         = CREATOR
10          RW                          = WRITERS
11          R                           = READERS @dev
12
13  repo    testing
14          RW+                         =   @manager
15          RW      master              =   @dev
16          RW      refs/tags/v[0-9]    =   dev1
17          -       refs/tags/          =   @all

```

在上面的示例中，演示了很多授权指令：

*   第 1 行，定义了用户组`@manager`，包含两个用户`jiangxin`和`wangsheng`。
*   第 2 行，定义了用户组`@dev`，包含三个用户`dev1`、`dev2`和`dev3`。
*   第 4-5 行，定义了版本库`gitolite-admin`。指定只有超级用户`jiangxin`才能够访问，并拥有读（R）写（W）和强制更新（+）的权限。
*   第 7 行，通过正则表达式为一组版本库进行批量授权。即针对`ossxp`目录下以小写字母开头的所有版本库进行授权。
*   第 8 行，用户组`@manager`中的用户可以创建版本库。即可以在`ossxp`目录下创建以小写字母开头的版本库。
*   第 9 行，版本库的创建者拥有对所创建版本库的完全权限。版本库的创建者是通过**git push**命令创建版本库的那一个人。
*   第 10-11 行，出现了两个特殊角色`WRITERS`和`READERS`，这两个角色不在本配置文件中定义，而是由版本库创建者使用 Gitolite 支持的`setperms`命令进行设置。
*   第 11 行，还设置了`@dev`用户组的用户对`ossxp`目录下的版本库具有读取权限。
*   第 13 行开始，对`testing`版本库进行授权。其中使用了对引用授权的语法。
*   第 14 行，用户组`@manager`对所有引用包括分支拥有读写、重置、添加和删除的授权，但里程碑除外，因为第 17 行定义了一条禁用规则。
*   第 15 行，用户组`@dev`可以读写`master`分支。（还包括名字以`master`开头的其他分支，如果有的话。）
*   第 16 行，用户`dev1`可以创建里程碑（即以`refs/tags/v[0-9]`开始的引用）。
*   第 17 行，禁止所有人（`@all`）对以`refs/tags/`开头的引用进行写操作。实际上由于之前第 14 行和第 16 行建立的授权，用户组`@manager`的用户和用户`dev1`能够创建里程碑，而且用户组`@manager`还能删除里程碑。

下面针对授权指令进行详细的讲解。

### 5.4.3.2\. 定义用户组和版本库组

在`conf/gitolite.conf`授权文件中，可以定义用户组或版本库组。组名称以`@`字符开头，可以包含一个或多个成员。成员之间用空格分开。

*   例如定义管理员组：

    ```
    @admin = jiangxin wangsheng

    ```

*   组可以嵌套：

    ```
    @staff = @admin @engineers tester1

    ```

除了作为用户组外，同样的语法也适用于版本库组。版本库组和用户组的定义没有任何区别，只是在版本库授权指令中处于不同的位置。即位于授权指令中的版本库位置代表版本库组，位于授权指令中的用户位置代表用户组。

### 5.4.3.3\. 版本库 ACL

一个版本库可以包含多条授权指令，这些授权指令组成了一个版本库的权限控制列表（ACL）。例如：

```
repo testing
    RW+                 = jiangxin @admin
    RW                  = @dev @test
    R                   = @all

```

#### 5.4.3.3.1\. 版本库

每一个版本库授权都以一条`repo`指令开始。指令`repo`后面是版本库列表，版本之间用空格分开，还可以包括版本库组。示例如下：

```
repo sandbox/test1 sandbox/test2 @test_repos

```

注意版本库名称不要添加`.git`后缀，在版本库创建或权限匹配过程中会自动添加`.git`后缀。用`repo`指令定义的版本库会自动在服务器上创建，但使用正则表达式定义的通配符版本库除外。

通配符版本库就是在`repo`指令定义的版本库名称中使用了正则表达式。通配符版本库针对的不是某一个版本库，而是匹配一组版本库，这些版本库可能已经存在或尚未创建。例如下面的`repo`指令定义了一组通配符版本库。

```
repo redmine/[a-zA-Z].+

```

通配符版本库匹配时会自动在版本库名称前面加上前缀`^`，在后面添加后缀`$`。即通配符版本库对版本库名称进行完整匹配而非部分匹配，这一点和后面将要介绍的正则引用（refex）大不一样。

有时`repo`指令定义普通版本库和通配符版本库的界限并不是那么清晰，像下面这条`repo`指令：

```
repo ossxp/.+

```

因为点号（`.`）和加号（`+`）也可以作为普通字符出现在版本库名称中，这条指令会导致 Gitolite 创建`ossxp`目录，并在目录下创建名为`.+.git`的版本库。因此在定义通配符版本库时要尽量写得“复杂点”以免造成误判。

Tip

我对 Gitolite 进行了一点改进，能够减少对诸如`ossxp/.+`通配符版本库误判的可能。并提供在定义通配符版本库时使用`^`前缀和`$`后缀，以减少误判。如使用如下方式定义通配符版本库：`repo ^myrepo`。

#### 5.4.3.3.2\. 授权指令

在`repo`指令之后是缩进的一条或多条授权指令。授权指令的语法如下：

```
<权限>  [零个或多个正则表达式匹配的引用] = <user> [<user> ...]

```

每条指令必须指定一个权限，称为授权关键字。包括传统的授权关键字：`C`、`R`、`RW`和`RW+`，以及将分支创建和分支删除分离出来的扩展授权关键字：`RWC`、`RW+C`、`RWD`、`RW+D`、`RWCD`、`RW+CD`。

传统的授权关键字包括：

*   `C`

    `C`代表创建版本库，仅在对通配符版本库进行授权时方可使用。用于设定谁可以创建名称与通配符匹配的版本库。

*   `R`

    `R`代表只读权限。

*   `RW`

    `RW`代表读写权限。如果在同一组（针对同一版本库）授权指令中没有出现代表创建分支的扩展授权关键字，则`RW`还包括创建分支的权限，而不仅是在分支中的读写。

*   `RW+`

    `RW+`除了具有读写权限外，还可以强制推送（执行非快进式推送）。如果在同一组授权指令中没有出现代表分支删除的扩展授权关键字，则`RW+`还同时包含了创建分支和删除分支的授权。

*   `-`

> `-`含义为禁用。因为禁用规则只在第二阶段授权生效[[3]](#id39)，所以一般只用于撤销特定用户对特定分支或整个版本库的写操作授权。

扩展的授权关键字将创建分支和删除分支的权限从传统授权关键字中分离出来，从而新增了六个授权关键字。在一个版本库的授权指令中一旦发现创建分支和/或删除分支的授权使用了下列新的扩展授权关键字后，原有的`RW`和`RW+`不再行使对创建分支和/或删除分支的授权。

*   `RWC`

    `RWC`代表读写授权、创建新引用（分支、里程碑等）的授权。

*   `RW+C`

    `RW+C`代表读写授权、强制推送和创建新引用的授权。

*   `RWD`

    `RWD`代表读写授权、删除引用的授权。

*   `RW+D`

    `RW+D`代表读写授权、强制推送和删除引用的授权。

*   `RWCD`

    `RWCD`代表读写授权、创建新引用和删除引用的授权。

*   `RW+CD`

    `RW+CD`代表读写授权、强制推送、创建新引用和删除引用的授权。

授权关键字后面（等号前面）是一个可选的正则引用（refex）或正则引用列表（用空格分隔）。

*   正则表达式格式的引用，简称正则引用（refex），在授权检查时对 Git 版本库的引用进行匹配。
*   如果在授权指令中省略正则引用，则意味着该授权指令对全部的引用都有效。
*   正则引用如果不以`refs/`开头，会自动添加`refs/heads/`作为前缀。
*   正则引用默认采用部分匹配策略，即如果不以`$`结尾，则后面可以匹配任意字符，相当于添加`.*$`作为后缀。

授权关键字后面（等号前面）也可以包含一个以`NAME/`为前缀的表达式，但这个表达式并非引用，而是路径。支持基于路径的写操作授权。

授权指令以等号（=）为标记分为前后两段，等号后面的是用户列表。用户之间用空格分隔，并且可以使用用户组。

### 5.4.3.4\. Gitolite 授权机制

Gitolite 的授权实际分为两个阶段。第一个阶段称为前 Git 阶段，即在 Git 命令执行前，由 SSH 连接触发的**gl-auth-command**命令执行的授权检查。包括：

*   版本库的读。

    如果用户拥有版本库或版本库的任意分支具有下列权限之一：`R`、`RW`、`RW+`（或其他扩展关键字），则整个版本库（包含所有分支）对用户均可读，否则版本库不可读取。

    最让人迷惑的就是只为某用户分配了对某个分支的读授权（`R`），而该用户实际上能够读取版本库的任意分支。之所以 Gitolite 对读授权不能细化到分支甚至目录，只能针对版本库进行粗放的非零即壹的读操作授权，是因为读授权只在版本库授权的第一个阶段进行检查，而在此阶段还获取不到版本库的分支。

*   版本库的写。

    版本库的写授权实际上要在两个阶段分别进行检查。本阶段，即第一阶段仅检查用户是否拥有下列权限之一：`RW`、`RW+`或`C`授权，具有这些授权则通过第一阶段的写权限检查。第二个阶段的授权检查由 Git 版本库的钩子脚本触发，能够实现基于分支和路径的写操作授权，以及对分支创建、删除和是否可强制更新进行授权检查，具体见第二阶段授权过程描述。

*   版本库的创建。

    仅对正则表达式定义的通配符版本库有效。即拥有`C`授权的用户可以创建和相应的正则表达式匹配的版本库。创建版本库（尤其是通过执行**git push**命令创建版本库）不免要涉及到执行新创建的版本库的钩子脚本，所以需要为版本库设置一条创建者可读写的授权。如：

    ```
    RW = CREATOR

    ```

Gitolite 对授权的第二个阶段的检查，实际上是通过`update`钩子脚本进行的。因为版本库的读操作不执行`update`钩子，所以读操作只在授权的第一个阶段（前 Git 阶段）就完成了检查，授权的第二个阶段仅对写操作进行更为精细的授权检查。

*   钩子脚本`update`针对推送操作的各个分支进行逐一检查，因此第二个阶段可以进行针对分支写操作的精细授权。
*   在这个阶段可以获取到要更新的新、老引用的 SHA1 哈希值，因此可以判断出是否发生了非快进式推送、是否有新分支创建，以及是否发生了分支的删除，因此可以针对这些操作进行精细的授权。
*   基于路径的写授权也是在这个阶段进行的。

## 5.4.4\. 版本库授权案例

Gitolite 的授权非常强大也很复杂，因此从版本库授权的实际案例来学习是非常行之有效的方式。

### 5.4.4.1\. 常规版本库授权

授权文件如下：

```
1  @admin = jiangxin
2  @dev   = dev1 dev2 badboy jiangxin
3  @test  = test1 test2
4
5  repo testing
6      RW+ = @admin
7      R = @test
8      - = badboy
9      RW = @dev test1

```

关于授权的说明：

*   用户`jiangxin`对版本库具有写的权限，并能够强制推送。

    由于用户`jiangxin`属于用户组`@admin`，通过第 6 行授权指令而具有读写权限，以及强制推送、创建和删除引用的权限。

*   用户`test1`对版本库具有写的权限。

    第 7 行定义了`test1`所属的用户组`@test`具有只读权限。第 9 行定义了`test1`用户具有读写权限。Gitolite 的实现是对读权限和写权限分别进行判断并汇总（并集），从而`test1`用户具有读写权限。

*   用户`badboy`对版本库只具有读操作的权限，没有写操作权限。

    第 8 行的指令以减号（-）开始，是一条禁用指令。禁用指令只在授权的第二阶段起作用，即只对写操作起作用，不会对`badboy`用户的读权限施加影响。在第 9 行的指令中，`badboy`所在的`@dev`组拥有读写权限。但禁用规则会对写操作起作用，导致`badboy`只有读操作权限，而没有写操作。

上面在 Gitolite 配置文件中对`testing`版本库进行的授权，当通过推送更新至 Gitolite 服务器上时，如果服务器端尚不存在一个名为`testing`的版本库，Gitolite 会自动初始化一个空白的`testing`版本库。

### 5.4.4.2\. 通配符版本库授权

授权文件如下：

```
 1   @administrators = jiangxin admin
 2   @dev            = dev1 dev2 badboy
 3   @test           = test1 test2
 4
 5   repo    sandbox/[a-z].+
 6           C       = @administrators
 7           RW+     = CREATOR
 8           R       = @test
 9           -       = badboy
10           RW      = @dev test1

```

这个授权文件的版本库名称中使用了正则表达式，匹配在`sandbox`目录下的任意以小写字母开头的版本库。因为通配符版本库并非指代一个具体版本库，因而不会在服务器端自动创建，而是需要管理员手动创建。

创建和通配符匹配的版本库，Gitolite 的原始实现是克隆即创建。例如管理员`jiangxin`创建名为`sandbox/repos1.git`版本库，执行下面命令：

```
jiangxin$ git clone git@server:sandbox/repos1.git

```

这种克隆即创建的方式很容易因为录入错误而导致意外创建错误的版本库。我改进的 Gitolite 需要通过推送来创建版本库。下面的示例通过推送操作（以`jiangxin`用户身份），远程创建版本库`sandbox/repos1.git`。

```
jiangxin$ git remote add origin git@server:sandbox/repos1.git
jiangxin$ git push origin master

```

对创建完成的`sandbox/repo1.git`版本库进行授权检查，会发现：

*   用户`jiangxin`对版本库具有读写权限，而用户`admin`则不能读取`sandbox/repo1.git`版本库。

    第 6 行的授权指令同时为用户`jiangxin`和`admin`赋予了创建与通配符相符的版本库的权限。但因为版本库`sandbox/repo1.git`是由`jiangxin`而非`admin`创建的，所以第 7 条的授权指令只为版本库的创建者`jiangxin`赋予了读写权限。

    Gitolite 通过在服务器端该版本库目录下创建一个名为`gl-creater`的文件记录了版本库的创建者。

*   和之前的例子相同的是：

    *   用户`test1`对版本库具有写的权限。
    *   禁用指令让用户`badboy`对版本库仅具有只读权限。

如果采用接下来的示例中的版本库权限设置，版本库`sandbox/repo1.git`的创建者`jiangxin`还可以使用**setperms**命令为版本库添加授权。具体用法参见下面的示例。

### 5.4.4.3\. 每个人创建自己的版本库

授权文件如下：

```
1  @administrators = jiangxin admin
2
3  repo    users/CREATOR/[a-zA-Z].*
4          C   =  @all
5          RW+ =  CREATOR
6          RW  =  WRITERS
7          R   =  READERS @administrators

```

关于授权的说明：

*   第 4 条指令，设置用户可以在自己的名字空间（`/usrs/<userid>/`）下，自己创建版本库。例如下面就是用户`dev1`执行**git push**命令在 Gitolite 服务器上自己的名字空间下创建版本库。

    ```
    dev1$ git push git@server:users/dev1/repos1.git master

    ```

*   第 5 条指令，设置版本库创建者对版本库具有完全权限。

    即用户`dev1`拥有对其自建的`users/dev1/repos1.git`拥有最高权限。

*   第 7 条指令，让管理员组`@administrators`的用户对于`users/`下用户自建的版本库拥有读取权限。

那么第 6、7 条授权指令中出现的`WRITERS`和`READERS`是如何定义的呢？实际上这两个变量可以看做是两个用户组，不过这两个用户组不是在 Gitolite 授权文件中设置，而是由版本库创建者执行**ssh**命令创建的。

版本库`users/dev1/repos1.git`的创建者`dev1`可以通过**ssh**命令连接服务器，使用**setperms**命令为自己的版本库设置角色。命令`setperms`的唯一一个参数就是版本库名称。当执行命令时，会自动进入一个编辑界面，手动输入角色定义后，按下`^D`（Ctrl+D）结束编辑。如下所示：

```
dev1$ ssh git@server setperms users/dev1/repos1.git
READERS dev2 dev3
WRITERS jiangxin
^D

```

即在输入`setperms`指令后，进入一个编辑界面，输入`^D`（Ctrl+D）结束编辑。也可以将角色定义文件保存到文件中，用**setperms**指令加载。如下：

```
dev1$ cat > perms << EOF
READERS dev2 dev3
WRITERS jiangxin
EOF

dev1$ ssh git@server setperms users/dev1/repos1.git < perms
New perms are:
READERS dev2 dev3
WRITERS jiangxin

```

当版本库创建者`dev1`对版本库`users/dev1/repos1.git`进行了如上设置后，Gitolite 在进行授权检查时会将`setperms`设置的角色定义应用到授权文件中。故此版本库`users/dev1/repos1.git`中又补充了新的授权：

*   用户`dev2`和`dev3`具有读取权限。
*   用户`jiangxin`具有读写权限。

版本库`users/dev1/repos1.git`的建立者`dev1`可以使用**getperms**查看自己版本库的角色设置。如下：

```
dev1$ ssh git@server getperms users/dev1/repos1.git
READERS dev2 dev3
WRITERS jiangxin

```

如果在用户自定义授权中需要使用`READERS`和`WRITERS`之外的角色，管理员可以通过修改`gitolite.rc`文件中的变量`$GL_WILDREPOS_PERM_CATS`实现。该变量的默认设置如下：

```
$GL_WILDREPOS_PERM_CATS = "READERS WRITERS";

```

### 5.4.4.4\. 传统模式的引用授权

传统模式的引用授权指的是在授权指令中只采用`R`、`RW`和`RW+`的传统授权关键字，而不包括后面介绍的扩展授权指令。传统的授权指令没有把分支的创建和分支删除权限细分，而是和写操作及强制推送操作混杂在一起。

*   非快进式推送必须拥有上述关键字中的`+`方可授权。
*   创建引用必须拥有上述关键字中的`W`方可授权。
*   删除引用必须拥有上述关键字中的`+`方可授权。
*   如果没有在授权指令中提供引用相关的参数，相当于提供`refs/.*`作为引用的参数，意味着对所有引用均有效。

授权文件：

```
1  @administrators = jiangxin admin
2  @dev            = dev1 dev2 badboy
3  @test           = test1 test2
4
5  repo    test/repo1
6          RW+                           = @administrators
7          RW master refs/heads/feature/ = @dev
8          R                             = @test

```

关于授权的说明：

*   第 6 行，对于版本库`test/repo1`，管理员组用户`jiangxin`和`admin`可以读写任意分支、强制推送，以及创建和删除引用。
*   第 7 行，用户组`@dev`除了对`master`和`refs/heads/feature/`开头的引用具有读写权限外，实际上可以读取所有引用。这是因为读取操作授权阶段无法获知引用。
*   第 8 行，用户组`@test`对版本库拥有只读授权。

### 5.4.4.5\. 扩展模式的引用授权

扩展模式的引用授权，指的是该版本库的授权指令出现了下列授权关键字中的一个或多个：`RWC`、`RWD`、`RWCD`、`RW+C`、`RW+D`、`RW+CD`，将分支的创建权限和删除权限从读写权限中分离出来，从而可对分支进行更为精细的权限控制。

*   非快进式推送必须拥有上述关键字中的`+`方可授权。
*   创建引用必须拥有上述关键字中的`C`方可授权。
*   删除引用必须拥有上述关键字中的`D`方可授权。

即引用的创建和删除使用了单独的授权关键字，和写权限和强制推送权限分开。

下面是一个采用扩展授权关键字的授权文件：

```
1   repo    test/repo2
2           RW+C = @administrators
3           RW+  = @dev
4           RW   = @test
5
6   repo    test/repo3
7           RW+CD = @administrators
8           RW+C  = @dev
9           RW    = @test

```

通过上面的配置文件，对于版本库`test/repo2.git`具有如下的授权：

*   第 2 行，用户组`@administrators`中的用户，具有创建和删除引用的权限，并且能强制推送。

    其中创建引用来自授权关键字中的`C`，删除引用来自授权关键中的`+`，因为该版本库授权指令中没有出现`D`，因而删除应用授权沿用传统授权关键字。

*   第 3 行，用户组`@dev`中的用户，不能创建引用，但可以删除引用，并且可以强制推送。

    因为第 2 行授权关键字中字符`C`的出现，使得创建引用采用扩展授权关键字，因而用户组`@dev`不具有创建引用的权限。

*   第 4 行，用户组`@test`中的用户，拥有读写权限，但是不能创建引用，不能删除引用，也不能强制推送。

通过上面的配置文件，对于版本库`test/repo3.git`具有如下的授权：

*   第 7 行，用户组`@administrators`中的用户，具有创建和删除引用的权限，并且能强制推送。

    其中创建引用来自授权关键字中的`C`，删除引用来自授权关键中的`D`。

*   第 8 行，用户组`@dev`中的用户，可以创建引用，并能够强制推送，但不能删除引用。

    因为第 7 行授权关键字中字符`C`和`D`的出现，使得创建和删除引用都采用扩展授权关键字，因而用户组`@dev`不具有删除引用的权限。

*   第 9 行，用户组`@test`中的用户，可以推送到任何引用，但是不能创建引用，不能删除引用，也不能强制推送。

### 5.4.4.6\. 禁用规则的使用

授权文件片段：

```
1     RW      refs/tags/v[0-9]        =   jiangxin
2     -       refs/tags/v[0-9]        =   @dev
3     RW      refs/tags/              =   @dev

```

关于授权的说明：

*   用户`jiangxin`可以创建任何里程碑，包括以`v`加上数字开头的版本里程碑。
*   用户组`@dev`，只能创建除了版本里程碑（以`v`加上数字开头）之外的其他里程碑。
*   其中以`-`开头的授权指令建立禁用规则。禁用规则只在授权的第二阶段有效，因此不能限制用户的读取权限。

### 5.4.4.7\. 用户分支

前面我们介绍过通过`CREATOR`特殊关键字实现用户自建版本库的功能。与之类似，Gitolite 还支持在一个版本库中用户自建分支的功能。

用户在版本库中自建分支用到的关键字是`USER`而非`CREATOR`。即当授权指令的引用表达式中出现的`USER`关键字时，在授权检查时会动态替换为用户 ID。例如授权文件片段：

```
1   repo    test/repo4
2           RW+CD                      = @administrators
3           RW+CD refs/heads/u/USER/   = @all
4           RW+   master               = @dev

```

关于授权的说明：

*   第 2 行，用户组`@administrators`中的用户，对所有引用具有读写、创建和删除的权限，并且能强制推送。
*   第 3 行，所有用户都可以创建以`u/<userid>/`（含自己用户 ID）开头的分支。对自己名字空间下的引用具有完全权限。对于他人名字空间的引用只有读取权限，不能修改。
*   第 4 行，用户组`@dev`对`master`分支具有读写和强制更新的权限，但是不能删除。

### 5.4.4.8\. 对路径的写授权

Gitolite 也实现了对路径的写操作的精细授权，并且非常巧妙的是实现此功能所增加的代码可以忽略不计。这是因为 Gitolite 把路径当作是特殊格式的引用的授权。

在授权文件中，如果一个版本库的授权指令中的正则引用字段出现了以`NAME/`开头的引用，则表明该授权指令是针对路径进行的写授权，并且该版本库要进行基于路径的写授权判断。

示例：

```
1  repo foo
2      RW                  =   @junior_devs @senior_devs
3
4      RW  NAME/           =   @senior_devs
5      -   NAME/Makefile   =   @junior_devs
6      RW  NAME/           =   @junior_devs

```

关于授权的说明：

*   第 2 行，初级程序员`@junior_devs`和高级程序员`@senior_devs`可以对版本库`foo`进行读写操作。
*   第 4 行，设定高级程序员`@senior_devs`对所有文件（`NAME/`）进行写操作。
*   第 5 行和第 6 行，设定初级程序员`@junior_devs`对除了根目录的`Makefile`文件外的其他文件具有写权限。

## 5.4.5\. 创建和导入版本库

Gitolite 维护的版本库默认位于安装用户主目录下的`repositories`目录中 ，即如果安装用户为`git`，则版本库都创建在`/home/git/repositories`目录之下。可以通过配置文件`.gitolite.rc`修改默认的版本库的根路径。

```
$REPO_BASE="repositories";

```

有多种创建版本库的方式。一种是在授权文件中用`repo`指令设置版本库（未使用正则表达式的版本库）的授权，当对`gitolite-admin`版本库执行**git push**操作时，自动在服务端创建新的版本库。另外一种方式是在授权文件中用正则表达式定义的通配符版本库，不会即时创建（也不可能被创建），而是被授权的用户在远程创建后推送到服务器上完成创建。

### 5.4.5.1\. 在配置文件中出现的版本库，即时生成

尝试在授权文件`conf/gitolite.conf`中加入一段新的版本库授权指令，而这个版本库尚不存在。新添加到授权文件中的内容为：

```
repo testing2
    RW+                 = @all

```

然后将授权文件的修改提交并推送到服务器，会看到授权文件中添加新授权的版本库`testing2`被自动创建。

```
$ git push
Counting objects: 7, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 375 bytes, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Already on 'master'
remote: creating testing2...
remote: Initialized empty Git repository in /home/git/repositories/testing2.git/
To gitadmin.bj:gitolite-admin.git
   278e54b..b6f05c1  master -> master

```

注意其中带`remote`标识的输出，可以看到版本库`testing2.git`被自动初始化了。

此外使用版本库组的语法（即用`@`创建的组，用作版本库），也会被自动创建。例如下面的授权文件片段设定了一个包含两个版本库的组`@testing`，当将新配置文件推送到服务器上时，会自动创建`testing3.git`和`testing4.git`。

```
@testing = testing3 testing4

repo @testing
    RW+                 = @all

```

### 5.4.5.2\. 通配符版本库，管理员通过推送创建

通配符版本库是用正则表达式语法定义的版本库，所指的并非某一个版本库而是和正则表达式相匹配的一组版本库。要想使用通配符版本库，需要在服务器端 Gitolite 的安装用户（如`git`）主目录下，修改配置文件`.gitolite.rc`，使其包含如下配置：

```
$GL_WILDREPOS = 1;

```

使用通配符版本库，可以对一组版本库进行授权，非常有效。但是版本库的创建则不像前面介绍的那样，不会在授权文件推送到服务器时创建，而是由拥有版本库创建授权（C）的用户手工进行创建。

对于用通配符设置的版本库，用`C`指令指定能够创建此版本库的管理员（拥有创建版本库的授权）。例如：

```
repo ossxp/[a-z].+
    C                   = jiangxin
    RW                  = dev1 dev2

```

用户`jinagxin`可以创建路径符合正则表达式`ossxp/[a-z].+`的版本库，用户`dev1`和`dev2`对版本库具有读写（但是没有强制更新）权限。

*   本地建库。

    ```
    $ mkdir somerepo
    $ cd somerepo
    $ git init
    $ git commit --allow-empty

    ```

*   使用**git remote**指令设置远程版本库。

    ```
    jiangxin$ git remote add origin git@server:ossxp/somerepo.git

    ```

*   运行**git push**完成在服务器端版本库的创建。

    ```
    jiangxin$ git push origin master

    ```

使用该方法创建版本库后，创建者`jiangxin`的用户 ID 将被记录在版本库目录下的`gl-creater`文件中。该帐号具有对该版本库最高的权限。该通配符版本库的授权指令中如果出现关键字`CREATOR`将会用创建者的用户 ID 替换。

实际上 Gitolite 的原始实现是通过克隆即可创建版本库。即当克隆一个不存在的、名称匹配通配符版本库的、且拥有创建权限（`C`），Gitolite 会自动在服务器端创建该版本库。但是我认为这不是一个好的实践，会经常因为在克隆时把 URL 写错，从而导致在服务器端创建垃圾版本库。因此我重新改造了 Gitolite 通配符版本库创建的实现方法，使用推送操作实现版本库的创建，而克隆一个不存在的版本库会报错、退出。

### 5.4.5.3\. 向 Gitolite 中导入版本库

在 Gitolite 搭建时，已经存在并使用的版本库需要导入到 Gitolite 中。如果只是简单地把这些裸版本库（以`.git`为后缀不带工作区的版本库）复制到 Gitolite 的版本库根目录下，针对这些版本库的授权可能不能正常工作。这是因为 Gitolite 管理的版本库都配置了特定的钩子脚本，以实现基于分支和/或路径的授权，直接拷贝到 Gitolite 中的版本库没有正确地设置钩子脚本。而且 Gitolite 还利用版本库中的`gl-creater`记录版本库创建者，用`gl-perms`记录版本库的自定义授权，而这些也是拷贝过来的版本库不具备的。

对于少量的版本库，直接修修改`gitolite-admin`的授权文件、添加同名的版本库授权、提交并推送，就会在 Gitolite 服务器端完成同名版本库的初始化。然后在客户端进入到相应版本库的工作区，执行**git push**命令将原有版本库的各个分支和里程碑导入到 Gitolite 新建的版本库中。

```
$ git remote add origin git@server:<repo-name>.git
$ git push --all  origin
$ git push --tags origin

```

如果要导入的版本库较多，逐一在客户端执行**git push**操作很繁琐。可以采用下面的方法。

*   确认要导入所有版本库都以裸版本库形式存在（以`.git`为后缀，无工作区）。
*   将要导入的裸版本库复制到 Gitolite 服务器的版本库根目录中。
*   在客户端修改`gitolite-admin`授权文件，为每个导入的版本库添加授权。
*   推送对`gitolite-admin`版本库的修改，相应版本库的钩子脚本会自动进行设置。

如果版本库非常多，就连在`gitolite-admin`的授权文件中添加版本库授权也是难事，还可以采用下面的办法：

*   确认要导入所有版本库都以裸版本库形式存在（以`.git`为后缀，无工作区）。
*   将要导入的裸版本库复制到 Gitolite 服务器的版本库根目录中。
*   在服务器端，为每个导入的裸版本库下添加文件`gl-creater`，内容为版本库创建者 ID。
*   在服务器端运行**gl-setup**程序（无需提供公钥参数），参见 Gitolite 安装相应章节。
*   在客户端修改`gitolite-admin`授权文件，以通配符版本库形式为导入的版本库进行授权。

## 5.4.6\. 对 Gitolite 的改进

Gitolite 托管在 GitHub 上，任何人都可以基于原作者 Sitaramc 的工作进行定制。我对 Gitolite 的定制版本在[`github.com/ossxp-com/gitolite`](http://github.com/ossxp-com/gitolite)， 包含的扩展和改进有：

*   通配符版本库的创建方式和授权。

    原来的实现是克隆即创建（克隆者需要被授予`C`的权限）。同时还要通过另外的授权语句为用户设置`RW`权限，否则创建者没有读和写权限。

    新的实现是通过推送创建版本库（推送者需要被授予`C`权限）。不必再为创建者赋予`RW`等权限，创建者自动具有对版本库最高的授权。

*   避免通配符版本库的误判。

    若将通配符版本库误判为普通版本库名称，会导致在服务器端创建错误的版本库。新的设计可以在通配符版本库的正则表达式之前添加`^`或之后添加`$`字符避免误判。

*   改变默认配置。

    默认安装即支持通配符版本库。

*   版本库重定向。

    Gitosis 的一个很重要的功能——版本库名称重定向，没有在 Gitolite 中实现。我为 Gitolite 增加了这个功能。

    在 Git 服务器架设的初期，版本库的命名可能非常随意，例如 redmine 的版本库直接放在根下：`redmine-0.9.x.git`、`redmine-1.0.x.git`，...随着 redmine 项目越来越复杂，可能就需要将其放在子目录下进行管理，例如放到`ossxp/redmine/`目录下。只需要在 Gitolite 的授权文件中添加下面一行`map`语句，就可以实现版本库名称的重定向。使用旧地址的用户不必重新检出，可以继续使用。

    ```
    map (redmine.*) = ossxp/redmine/$1

    ```

## 5.4.7\. Gitolite 功能拓展

### 5.4.7.1\. 版本库镜像

Git 版本库控制系统往往并不需要设计特别的容灾备份，因为每一个 Git 用户就是一个备份。但是下面的情况，就很有必要考虑容灾了。

*   Git 版本库的使用者很少（每个库可能只有一个用户）。
*   版本库克隆只限制在办公区并且服务器也在办公区内（所有鸡蛋都在一个篮子里）。
*   Git 版本库采用集中式的应用模型，需要建立双机热备（以便在故障出现时，实现快速的服务器切换）。

可以在两台或多台安装了 Gitolite 服务的服务器之间实现版本库的镜像。数据镜像的最小单位为版本库，对于任意一个 Git 版本库可以选择在其中一个服务器上建立主版本库（只能有一个主版本库），在其他服务器上建立的为镜像库。镜像库只接受来自主版本库的数据同步而不接受来自用户的推送。

#### 5.4.7.1.1\. Gitolite 服务器命名

首先要为每一台服务器架设 Gitolite 服务，并建议所有的服务器上 Gitolite 服务都架设在同一用户（如`git`）之下。如果 Gitolite 服务安装到不同的用户账号下，就必需通过文件`~/.ssh/config`建立 SSH 别名，以便能够使用正确的用户名连接服务器。

接下来为每个服务器设置一个名称，服务器之间数据镜像时就使用各自的名称进行连接。假设我们要配置的两个 Gitolite 服务器的其中一个名为`server1`，另一个名为`server2`。

打开`server1`上 Gitolite 的配置文件`~/.gitolite.rc`，进行如下设置：

```
$GL_HOSTNAME = 'serer1';
$GL_GITCONFIG_KEYS = "gitolite.mirror.*";

```

*   设置`$GL_HOSTNAME`为本服务器的别名，如`serer1`。

*   设量`$GL_GITCONFIG_KEYS`以便允许在 Gitolite 授权文件中为版本库动态设置配置变量。

    例如本例设置了`GL_GITCONFIG_KEYS`为`gitolite.mirror.*`后，允许在`gitolite-admin`管理库的`conf/gitolite.conf`中用`config`指令对版本库添加配置变量。

    ```
    repo testing
          config gitolite.mirror.master       =   "server1"
          config gitolite.mirror.slaves       =   "server2 server3"

    ```

同样对`server2`进行设置，只不过将`$GL_HOSTNAME`设置为`serer2`。

#### 5.4.7.1.2\. 服务器之间的公钥认证

接下来每一个服务器为 Gitolite 的安装用户创建公钥/私钥对。

```
$ sudo su - git
$ ssh-keygen

```

然后把公钥拷贝到其他服务器上，并以本服务器名称命名。例如：

*   `server1`上创建的公钥复制到`server2`上，命名为`server1.pub`备用。
*   `server2`上创建的公钥复制到`server1`上，命名为`server2.pub`备用。

再运行**gl-tool**设置其他服务器到本服务器上的公钥认证。例如在`server1`上执行命令：

```
$ gl-tool add-mirroring-peer server2.pub

```

当完成上述设置后，就可以从一个服务器发起到另外服务器的 SSH 连接，连接过程无需口令认证并显示相关信息。例如从`server1`发起到`server2`的连接如下：

```
$ ssh git@server2 info
Hello server1, I am server2

```

#### 5.4.7.1.3\. 配置版本库镜像

做了前面的准备工作后，就可以开始启用版本库镜像了。下面通过一个示例介绍如何建立版本库镜像，将服务器`server1`上的版本库`testing`要镜像到服务器`server2`上。

首先要修改`server1`和`server2`的 Gitolite 管理库`gitolite-admin`，为`testing`版本库添加配置变量，如下：

```
repo    testing
        config gitolite.mirror.master = "server1"
        config gitolite.mirror.slaves = "server2"

```

两个服务器`server1`和`server2`都要做出同样的修改，提交改动并推送到服务器上。当推送完成，两个服务器上的`testing`版本库的`config`就会被更新，包含类似如下的设置：

```
[gitolite "mirror"]
        master = server1
        slaves = server2

```

当向服务器`server1`的`testing`版本库推送新的提交时，就会自动同步到`server2`上。

```
$ git push git@server1:testing.git master
[master c0b097a] test
Counting objects: 1, done.
Writing objects: 100% (1/1), 185 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
remote: (29781&) server1 ==== (testing) ===> server2
To git@server1:testing.git
   d222699..c0b097a  master -> master

```

如果需要将服务器`server1`上所有版本库，包括`gitolite-admin`版本库都同步到`server2`上，不必对版本库逐一设置，可以采用下面的简便方法。

修改`server1`和`server2`的 Gitolite 管理版本库`gitolite-admin`，在配置文件`conf/gitolite.conf`最开始插入如下设置。

```
repo   @all
    config gitolite.mirror.master = "server1"
    config gitolite.mirror.slaves = "server2"

```

然后分别提交并推送。要说明的是`gitolite-admin`版本库此时尚未建立同步，直到服务器`server1`的`gitolite-admin`版本库推送新的提交，才开始`gitolite-admin`版本库的同步。

也可以在`server1`服务器端执行命令开始同步。例如：

```
$ gl-mirror-shell request-push gitolite-admin

```

Gitolite 官方版本在版本库同步时有个局限，要求在镜像服务器上必需事先存在目标版本库并正确设置了`gitolite.mirror.*`参数，才能同步成功。例如允许用户自行创建的通配符版本库，必需在主服务器上和镜像服务器上分别创建，之后版本库同步才能正常执行。我在 GitHub 上的 Gitolite 分支项目提交了一个补丁解决了这个问题。

关于 Gitolite 版本库镜像的更详悉资料，参见[`sitaramc.github.com/git`](http://sitaramc.github.com/git) olite/doc/mirroring.html。

### 5.4.7.2\. Gitweb 和 Git daemon 支持

Gitolite 和 git-daemon 的整合很简单，就是由 Gitolite 创建的版本库会在版本库目录中创建一个空文件`git-daemon-export-ok`。

Gitolite 和 Gitweb 的整合则提供了两个方面的内容。一个是可以设置版本库的描述信息，用于在 Gitweb 的项目列表页面中显示。另外一个是自动生成项目的列表文件供 Gitweb 参考，避免 Gitweb 使用低效率的目录递归搜索查找 Git 版本库列表。

可以在授权文件中设定版本库的描述信息，并在 gitolite-admin 管理库更新时创建到版本库的`description`文件中。

```
reponame = "one line of description"
reponame "owner name" = "one line of description"

```

*   第 1 行，为名为`reponame`的版本库设定描述。
*   第 2 行，同时设定版本库的属主名称，以及一行版本库描述。

对于通配符版本库，使用这种方法则很不现实。Gitolite 提供了 SSH 子命令供版本库的创建者使用。

```
$ ssh git@server setdesc path/to/repos.git
$ ssh git@server getdesc path/to/repos.git

```

*   第一条指令用于设置版本库的描述信息。
*   第二条指令显示版本库的描述信息。

至于生成 Gitweb 所用的项目列表文件，默认创建在用户主目录下的`projects.list`文件中。对于所有启用 Gitweb 的`[repo]`小节所设定的版本库，以及通过版本库描述隐式声明的版本库都会加入到版本库列表中。

### 5.4.7.3\. 其他功能拓展和参考

Gitolite 源码的`doc`目录包含用 markdown 标记语言编写的手册，可以直接在 Github 上查看。也可以使用 markdown 的文档编辑工具将`.mkd`文档转换为 html 文档。转换工具很多，有**rdiscount**、**Bluefeather**、**Maruku**、**BlueCloth2**，等等。

在这些参考文档中，用户可以发现 Gitolite 包含的更多的小功能或秘籍，包括：

*   版本库设置。

    授权文件通过**git config**指令为版本库进行附加的设置。例如：

    ```
    repo gitolite
        config hooks.mailinglist = gitolite-commits@example.tld
        config hooks.emailprefix = "[gitolite] "
        config foo.bar = ""
        config foo.baz =

    ```

*   多级管理员授权。

    可以为不同的版本库设定管理员，操作`gitolite-admin`库的部分授权文件。具体参考：`doc/5-delegation.mkd`。

*   自定义钩子脚本。

    因为 Gitolite 占用了几个钩子脚本，如果需要对同名钩子进行扩展，Gitolite 提供了级联的钩子脚本，将定制放在级联的钩子脚本里。

    例如：通过自定义`gitolite-admin`的`post-update.secondary`脚本，以实现无须登录服务器即可更改`.gitolite.rc`文件。具体参考：`doc/shell-games.mkd`。

    关于钩子脚本的创建和维护，具体参考：`doc/hook-propagation.mkd`。

*   管理员自定义命令。

    通过设置配置文件中的`$GL_ADC_PATH`变量，在远程执行该目录下的可执行脚本，如：**rmrepo**。

    具体参考：`doc/admin-defined-commands.mkd`。

*   创建匿名的 SSH 认证。

    允许匿名用户访问 Gitolite 提供的 Git 服务。即建立一个和 Gitolite 服务器端帐号同 ID 同主目录的用户，设置其的特定 shell，并且允许口令为空。

    具体参考：`doc/mob-branches.mkd`。

*   可以通过名为`@all`的版本库进行全局的授权。

    但是不能在`@all`版本库中对`@all`用户组进行授权。

*   版本库或用户非常之多（几千个）的时候，需要使用**大配置文件**模式。

    因为 Gitolite 的授权文件要先编译才能生效，而编译文件的大小是和用户及版本库数量的乘积成正比的。选择大配置文件模式则不对用户组和版本库组进行扩展。

    具体参考：`doc/big-config.mkd`。

*   授权文件支持包含语句，可以将授权文件分成多个独立的单元。

*   执行外部命令，如**rsync**。

*   Subversion 版本库支持。

    如果在同一个服务器上以`svn+ssh`方式运行 Subversion 服务器，可以使用同一套公钥，同时为用户提供 Git 和 Subversion 服务。

*   HTTP 口令文件维护。通过名为`htpasswd`的 SSH 子命令实现。

* * *

| [[1]](#id1) | 对 Gitolite 的各项改动采用了 Topgit 特性分支进行维护，以便和上游最新代码同步更新。还要注意如果在 Gitolite 使用中发现问题，要区分是由上游软件引发的还是我的改动引起的，而不要把我的错误算在 Sitaram 头上。 |

| [[2]](#id2) | 公钥的内容为一整行，因排版需要做了换行处理。 |

| [[3]](#id15) | 可以为版本库设置配置变量`gitolite-options.deny-repo`在第一个授权阶段启用禁用规则检查。 |

| [4] | 参见第 8 部分 41.2.2“Git 模板”相关内容。 |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.5\. Gitosis 服务架设

Gitosis 是 Gitolite 的鼻祖，同样也是一款基于 SSH 公钥认证的 Git 服务管理工具，但是功能要比之前介绍的 Gitolite 要弱的多。Gitosis 由 Python 语言开发，对于偏爱 Python 不喜欢 Perl 的开发者（我就是其中之一），可以对 Gitosis 加以关注。

Gitosis 的出现远早于 Gitolite，作者 Tommi Virtanen 从 2007 年 5 月就开始了 gitosis 的开发，最后一次提交是在 2009 年 9 月，已经停止更新了。但是 Gitosis 依然有其生命力。

*   配置简洁，可以直接在服务器端编辑，成为为某些服务定制的内置的无需管理的 Git 服务。

    Gitosis 的配置文件非常简单，直接保存于服务安装用户（如`git`）的主目录下`.gitosis.conf`文件中，可以直接在服务器端创建和编辑。

    Gitolite 的授权文件需要复杂的编译，因此一般需要管理员克隆`gitolite-admin`库，远程编辑并推送至服务器。因此用 Gitolite 实现一个无需管理的 Git 服务难度要大很多。

*   支持版本库重定向。

    版本库重定向一方面在版本库路径变更后保持旧的 URL 仍可工作，另一方面用在客户端用简洁的地址屏蔽服务器端复杂的地址。

    例如我开发的一款备份工具（Gistore），版本库位于`/etc/gistore/tasks/system/repo.git`（符号链接），客户端使用`system.git`即映射到复杂的服务器端地址。

    这个功能我已经在定制的 Gitolite 中实现。

*   Python 语言开发，对于喜欢 Python，不喜欢 Perl 的用户，可以选择 Gitosis。

*   在 Github 上有很多 Gitosis 的克隆，我对 gitosis 的改动放在了 github 上：

    [`github.com/ossxp-com/gitosis`](http://github.com/ossxp-com/gitosis)

Gitosis 因为是 Gitolite 的鼻祖，因此下面的 Gitosis 实现机理，似曾相识：

*   Gitosis 安装在服务器（`server.name`）某个帐号之下，例如`git`帐号。

*   管理员通过 Git 命令检出名为`gitosis-admin`的版本库。

    ```
    $ git clone git@server.name:gitosis-admin.git

    ```

*   管理员将 git 用户的公钥保存在`gitosis-admin`库的`keydir`目录下，并编辑`gitosis.conf`文件为用户授权。

*   当管理员对`gitosis-admin`库的修改提交并 PUSH 到服务器之后，服务器上`gitosis-admin`版本库的钩子脚本将执行相应的设置工作。

    *   新用户公钥自动追加到服务器端安装帐号的`.ssh/authorized_keys`中，并设置该用户的 shell 为 gitosis 的一条命令**gitosis-serve**。

        ```
  command="gitosis-serve jiangxin",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa <公钥内容来自于 jiangxin.pub ...>

        ```

    *   更新服务器端的授权文件`~/.gitosis.conf`。

*   用户可以用 Git 命令访问授权的版本库。

*   当管理员授权，用户可以远程在服务器上创建新版本库。

下面介绍 Gitosis 的部署和使用。在下面的示例中，约定：服务器的名称为`server`，Gitolite 的安装帐号为`git`。

## 5.5.1\. 安装 Gitosis

Gitosis 的部署和使用可以直接参考源代码中的`README.rst`。可以直接访问 Github 上我的 gitosis 克隆，因为 Github 能够直接将 rst 文件显示为网页。

参考:

```
http://github.com/ossxp-com/gitosis/blob/master/README.rst

```

### 5.5.1.1\. Gitosis 的安装

Gitosis 安装需要在服务器端执行。下面介绍直接从源代码进行安装，以便获得最新的改进。

Gitosis 的官方 Git 库位于`git://eagain.net/gitosis.git`。我在 Github 上创建了一个分支：

> [`github.com/ossxp-com/gitosis`](http://github.com/ossxp-com/gitosis)

*   使用 git 下载 Gitosis 的源代码。

    ```
    $ git clone git://github.com/ossxp-com/gitosis.git

    ```

*   进入 `gitosis`目录，执行安装。

    ```
    $ cd gitosis
    $ sudo python setup.py install

    ```

*   可执行脚本安装在`/usr/local/bin`目录下。

    ```
    $ ls /usr/local/bin/gitosis-*
    /usr/local/bin/gitosis-init  /usr/local/bin/gitosis-run-hook  /usr/local/bin/gitosis-serve

    ```

### 5.5.1.2\. 服务器端创建专用帐号

安装 Gitosis，还需要在服务器端创建专用帐号，所有用户都通过此帐号访问 Git 库。一般为方便易记，选择 git 作为专用帐号名称。

```
$ sudo adduser --system --shell /bin/bash --disabled-password --group it

```

创建用户`git`，并设置用户的 shell 为可登录的 shell，如`/bin/bash`，同时添加同名的用户组。

有的系统，只允许特定的用户组（如`ssh`用户组）的用户才可以通过 SSH 协议登录，这就需要将新建的`git`用户添加到`ssh`用户组中。

```
$ sudo adduser git ssh

```

### 5.5.1.3\. Gitosis 服务初始化

Gitosis 服务初始化，就是初始化一个`gitosis-admin`库，并为管理员分配权限，还要将 Gitosis 管理员的公钥添加到专用帐号的`~/.ssh/authorized_keys`文件中。

*   如果管理员在客户端没有公钥，使用下面命令创建。

    ```
    $ ssh-keygen

    ```

*   管理员上传公钥到服务器。

    ```
    $ scp ~/.ssh/id_rsa.pub server:/tmp

    ```

*   服务器端进行 Gitosis 服务初始化。

    以 git 用户身份执行`gitosis-init`命令，并向其提供管理员公钥。

    ```
    $ sudo su - git
    $ gitosis-init < /tmp/id_rsa.pub

    ```

*   确保`gitosis-admin`版本库的钩子脚本可执行。

    ```
    $ sudo chmod a+x ~git/repositories/gitosis-admin.git/hooks/post-update

    ```

## 5.5.2\. 管理 Gitosis

### 5.5.2.1\. 管理员克隆`gitolit-admin`管理库

当 Gitosis 安装完成后，在服务器端自动创建了一个用于 Gitosis 自身管理的 Git 库：`gitosis-admin.git`。

管理员在客户端克隆`gitosis-admin.git`库，注意要确保认证中使用正确的公钥：

```
$ git clone git@server:gitosis-admin.git
$ cd gitosis-admin/

$ ls -F
gitosis.conf  keydir/

$ ls keydir/
jiangxin.pub

```

可以看出`gitosis-admin`目录下有一个陪孩子文件和一个目录`keydir`。

*   `keydir/jiangxin.pub`文件

    `keydir`目录下初始时只有一个用户公钥，即管理员的公钥。管理员的用户名来自公钥文件末尾的用户名。

*   `gitosis.conf`文件

    该文件为授权文件。初始内容为:

    ```
    1  [gitosis]
    2
    3  [group gitosis-admin]
    4  writable = gitosis-admin
    5  members = jiangxin

    ```

    可以看到授权文件的语法完全不同于之前介绍的 Gitolite 的授权文件。整个授权文件是以用户组为核心，而非版本库为核心。

    *   定义了一个用户组`gitosis-admin`。

        第 3 行开始定义了一个用户组`gitosis-admin`。

    *   第 5 行设定了该用户组包含的用户列表。

        初始时只有一个用户，即管理员公钥所属的用户。

    *   第 4 行设定了该用户组对那些版本库具有写操作。

        这里配置对`gitosis-admin`版本库具有写操作。写操作自动包含了读操作。

### 5.5.2.2\. 增加新用户

增加新用户，就是允许新用户能够通过其公钥访问 Git 服务。只要将新用户的公钥添加到`gitosis-admin`版本库的`keydir`目录下，即完成新用户的添加。

*   管理员从用户获取公钥，并将公钥按照`username.pub`格式进行重命名。

    用户可以通过邮件或者其他方式将公钥传递给管理员，切记不要将私钥误传给管 理员。如果发生私钥泄漏，马上重新生成新的公钥/私钥对，并将新的公钥传递给管理员，并申请将旧的公钥作废。

    关于公钥名称，我引入了类似 Gitolite 的实现：

    *   用户从不同的客户端主机访问有着不同的公钥，如果希望使用同一个用户名进行授权，可以按照`username@host.pub`方式命名公钥文件，和名为`username@pub`的公钥指向同一个用户`username`。
    *   也支持邮件地址格式的公钥，即形如`username@gmail.com.pub`的公钥。Gitosis 能够很智能的区分是以邮件地址命名的公钥还是相同用户在不同主机上的公钥。如果是邮件地址命名的公钥，将以整个邮件地址作为用户名。
*   管理员进入`gitosis-admin`本地克隆版本库中，复制新用户公钥到`keydir`目录。

    ```
    $ cp /path/to/dev1.pub keydir/
    $ cp /path/to/dev2.pub keydir/

    ```

*   执行**git add**命令，将公钥添加入版本库。

    ```
    $ git add keydir
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   keydir/dev1.pub
    #       new file:   keydir/dev2.pub
    #

    ```

*   执行**git commit**，完成提交。

    ```
    $ git commit -m "add user: dev1, dev2"
    [master d7952a5] add user: dev1, dev2
     2 files changed, 2 insertions(+), 0 deletions(-)
     create mode 100644 keydir/dev1.pub
     create mode 100644 keydir/dev2.pub

    ```

*   执行**git push**，同步到服务器，才真正完成新用户的添加。

    ```
    $ git push
    Counting objects: 7, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (5/5), 1.03 KiB, done.
    Total 5 (delta 0), reused 0 (delta 0)
    To git@server:gitosis-admin.git
       2482e1b..d7952a5  master -> master

    ```

如果这时查看服务器端`~git/.ssh/authorized_keys`文件，会发现新增的用户公钥也附加其中：

```
### autogenerated by gitosis, DO NOT EDIT
command="gitosis-serve jiangxin",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty     <用户 jiangxin 的公钥...>
command="gitosis-serve dev1",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa <用户 dev1 的公钥...>
command="gitosis-serve dev2",no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty ssh-rsa <用户 dev1 的公钥...>

```

### 5.5.2.3\. 更改授权

新用户添加完毕，可能需要重新进行授权。更改授权的方法也非常简单，即修改`gitosis.conf`配置文件，提交并推送。

首先管理员进入`gitosis-admin`本地克隆版本库中，编辑`gitosis.conf`。

```
$ vi gitosis.conf

```

授权指令比较复杂，先通过建立一个新用户组并授权新版本库`testing`尝试一下更改授权文件。例如在`gitosis.conf`中添加如下授权内容：

```
1   [group testing-admin]
2   members = jiangxin @gitosis-admin
3   admin = testing
4
5   [group testing-devloper]
6   members = dev1 dev2
7   writable = testing
8
9   [group testing-reader]
10  members = @all
11  readonly = testing

```

*   上面的授权文件为版本库`testing`赋予了三个角色。分别是`@testing-admin`用户组，`@testing-developer`用户组和`@testing-reader`用户组。

*   第 1 行开始的`testing-admin`小节，定义了用户组`@testing-admin`。

*   第 2 行设定该用户组包含的用户有`jiangxin`，以及前面定义的`@gitosis-admin`用户组用户。

*   第 3 行用`admin`指令，设定该用户组用户可以创建版本库`testing`。

    `admin`指令是笔者新增的授权指令，请确认安装的 Gitosis 包含笔者的改进。

*   第 7 行用`writable`授权指令，设定该`@testing-developer`用户组用户可以读写版本库`testing`。

    笔者改进后的 Gitosis 也可以使用`write`作为`writable`指令的同义词指令。

*   第 11 行用`readonly`授权指令，设定该`@testing-reader`用户组用户（所有用户）可以只读访问版本库`testing`。

    笔者改进后的 Gitosis 也可以使用`read`作为`readonly`指令的同义词指令。

编辑结束，提交改动。

```
$ git add gitosis.conf
$ git commit -q -m "auth for repo testing."

```

执行**git push**，同步到服务器，才真正完成授权文件的编辑。

```
$ git push

```

## 5.5.3\. Gitosis 授权详解

### 5.5.3.1\. Gitosis 缺省设置

在`[gitosis]`小节中定义 Gitosis 的缺省设置。如下：

```
1  [gitosis]
2  repositories = /gitroot
3  #loglevel=DEBUG
4  gitweb = yes
5  daemon = yes
6  generate-files-in = /home/git/gitosis

```

其中：

*   第 2 行，设置版本库缺省的根目录是`/gitroot`目录。

    否则缺省路径是安装用户主目录下的`repositories`目录。

*   第 3 行，如果打开注释，则版本库操作时显示 Gitosis 调试信息。

*   第 4 行，启用 gitweb 的整合。

    可以通过`[repo name]`小节为版本库设置描述字段，用户显示在 Gitweb 中。

*   第 5 行，启用 git-daemon 的整合。

    即新创建的版本库中，创建文件`git-daemon-export-ok`。

*   第 6 行，设置创建的项目列表文件（供 gitweb 使用）所在的目录。

    缺省即为安装用户的主目录下的`gitosis`目录。

### 5.5.3.2\. 管理版本库`gitosis-admin`

```
1  [group gitosis-admin]
2  write = gitosis-admin
3  members = jiangxin
4  repositories = /home/git

```

除了第 4 行，其他内容在前面都已经介绍过了，是 Gitosis 自身管理版本库的用户组设置。

第 4 行，重新设置了版本库的缺省根路经，覆盖缺省的`[gitosis]`小节中的缺省根路径。实际的`gitosis-admin`版本库的路径为`/home/git/gitosis-admin.git`。

### 5.5.3.3\. 定义用户组和授权

下面的两个示例小节定义了两个用户组，并且用到了路径变换的指令。

```
1   [group ossxp-admin]
2   members = @gitosis-admin jiangxin
3   admin = ossxp/**
4   read = gistore/*
5   map admin redmine-* = ossxp/redmine/\1
6   map admin ossxp/redmine-* = ossxp/(redmine-.*):ossxp/redmine/\1
7   map admin ossxp/testlink-* = ossxp/(testlink-.*):ossxp/testlink/\1
8   map admin ossxp/docbones* = ossxp/(docbones.*):ossxp/docutils/\1
9
10  [group all]
11  read = ossxp/**
12  map read redmine-* = ossxp/redmine/\1
13  map read testlink-* = ossxp/testlink/\1
14  map read pysvnmanager-gitsvn = mirrors/pysvnmanager-gitsvn
15  map read ossxp/redmine-* = ossxp/(redmine-.*):ossxp/redmine/\1
16  map read ossxp/testlink-* = ossxp/(testlink-.*):ossxp/testlink/\1
17  map read ossxp/docbones* = ossxp/(docbones.*):ossxp/docutils/\1
18  repositories = /gitroot

```

在上面的示例中，演示了授权指令以及 Gitosis 特色的`map`指令。

*   第 1 行，定义了用户组`@ossxp-admin`。

*   第 2 行，设定该用户组包含用户`jiangxin`以及用户组`@gitosis-admin`的所有用户。

*   第 3 行，设定该用户组具有创建及读写与通配符`ossxp/**`匹配的版本库。

    两个星号匹配任意字符包括路径分隔符（/）。此功能属于笔者扩展的功能。

*   第 4 行，设定该用户组可以只读访问`gistore/*`匹配的版本库。

    一个星号匹配任意字符包括路径分隔符（/）。 此功能也属于笔者扩展的功能。

*   第 5 行，是 Gitosis 特有的版本库名称重定位功能。

    即对`redmine-*`匹配的版本库，先经过名称重定位，在名称前面加上`ossxp/remdine`。其中`\\1`代表匹配的整个版本库名称。

    用户组`@ossxp-admin`的用户对于重定位后的版本库，具有`admin`（创建和读写）权限。

*   第 6 行，是我扩展的版本库名称重定位功能，支持正则表达式。

    等号左边的名称进行通配符匹配，匹配后，再经过右侧的一对正则表达式进行转换（冒号前的用于匹配，冒号后的用于替换）。

*   第 10 行，使用了内置的`@all`用户组，因此不需要通过`members`设定用户，因为所有用户均属于该用户组。

*   第 11 行，设定所有用户均可以只读访问`ossxp/**`匹配的版本库。

*   第 12-17 行，对特定路径进行映射，并分配只读权限。

*   第 18 行，设置版本库的根路径为`/gitroot`，而非缺省的版本库根路径。

### 5.5.3.4\. Gitweb 整合

Gitosis 和 Gitweb 的整合，提供了两个方面的内容。一个是可以设置版本库的描述信息，用于在 gitweb 的项目列表页面显示。另外一个是自动生成项目的列表文件供 Gitweb 参考，避免 Gitweb 使用效率低的目录递归搜索查找 Git 版本库列表。

例如在`gitosis.conf`中下面的配置用于对`redmine-1.0.x`版本库的 Gitweb 整合进行设置。

```
1  [repo ossxp/redmine/redmine-1.0.x]
2  gitweb = yes
3  owner = Jiang Xin
4  description = Redmine 1.0.x 群英汇定制开发

```

*   第 1 行，`repo`小节用于设置版本库的 Gitweb 整合。

    版本库的实际路径是用版本库缺省的根（即在`[gitosis]`小节中定义的或者缺省的）加上此小节中的版本库路径组合而成的。

*   第 2 行，启用 Gitweb 整合。如果省略，使用全局`[gitosis]`小节中 gitweb 的设置。

*   第 3 行，用于设置版本库的属主。

*   第 4 行，用于设置版本库的描述信息，显示在 Gitweb 的版本库列表中。

每一个`repo`小节所指向的版本库，如果启用了 Gitweb 选项，则版本库名称汇总到一个项目列表文件中。该项目列表文件缺省保存在`~/gitosis/projects.list`中。

## 5.5.4\. 创建新版本库

Gitosis 维护的版本库位于安装用户主目录下的`repositories`目录中，即如果安装用户为`git`，则版本库都创建在`/home/git/repositories`目录之下。可以通过配置文件`gitosis.conf`修改缺省的版本库的根路径。

可以直接在服务器端创建，或者在客户端远程创建版本库。

**克隆即创建，还是 PUSH 即创建？**

在客户端远程创建版本库时，Gitosis 的原始实现是对版本库具有`writable`（读写）权限的用户，对不存在的版本库执行克隆操作时，自动创建。但是我认为这不是一个好的实践，会经常因为克隆的 URL 写错，导致在服务器端创建垃圾版本库。笔者改进的实现如下：

*   增加了名为`admin`（或`init`）的授权指令，只有具有此授权的用户，才能够创建版本库。
*   只具有`writable`（或`write`）权限的用户，不能在服务器上创建版本库。
*   不通过克隆创建版本库，而是在对版本库进行 PUSH 的时候进行创建。当克隆一个不存在的版本库，会报错退出。

远程在服务器上创建版本库的方法如下：

*   首先，本地建库。

    ```
    $ mkdir somerepo
    $ cd somerepo
    $ git init
    $ git commit --allow-empty

    ```

*   使用**git remote**指令添加远程的源。

    ```
    $ git remote add origin git@server:ossxp/somerepo.git

    ```

*   运行**git push**完成在服务器端版本库的创建

    ```
    $ git push origin master

    ```

## 5.5.5\. 轻量级管理的 Git 服务

轻量级管理的含义是不采用缺省的稍显复杂的管理模式（远程克隆`gitosis-admin`库，修改并 PUSH 的管理模式），而是直接在服务器端通过预先定制的配置文件提供 Git 服务。这种轻量级管理模式，对于为某些应用建立快速的 Git 库服务提供了便利。

例如在使用备份工具 Gistore 进行文件备份时，可以用 Gitosis 架设轻量级的 Git 服务，可以在远程使用 Git 命令进行双机甚至是异地备份。

首先创建一个专用帐号，并设置该用户只能执行**gitosis-serve**命令。例如创建帐号`gistore`，通过修改**/etc/ssh/sshd_config**配置文件，实现限制该帐号登录的可执行命令。

```
Match user gistore
    ForceCommand gitosis-serve gistore
    X11Forwarding no
    AllowTcpForwarding no
    AllowAgentForwarding no
    PubkeyAuthentication yes
    #PasswordAuthentication no

```

上述配置信息告诉 SSH 服务器，凡是以`gistore`用户登录的帐号，强制执行 Gitosis 的命令。

然后，在该用户的主目录下创建一个配置文件`.gitosis.conf`（注意文件名前面的点号），如下：

```
[gitosis]
repositories = /etc/gistore/tasks
gitweb = yes
daemon = no

[group gistore]
members = gistore
map readonly * = (.*):\1/repo

```

上述配置的含义是：

*   用户`gistore`才能够访问`/etc/gistore/tasks`下的 Git 库。
*   版本库的名称要经过变换，例如`system`库会变换为实际路径`/etc/gistore/tasks/system/repo.git`。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.6\. Gerrit 代码审核服务器

谷歌 Android 开源项目在 Git 的使用上有两个重要的创新，一个是为多版本库协同而引入的 repo，在前面第二十五章已经详细讨论过。另外一个重要的创新就是 Gerrit——代码审核服务器。Gerrit 为 Git 引入的代码审核是强制性的，就是说除非特别的授权设置，向 Git 版本库的推送（Push）必须要经过 Gerrit 服务器，修订必须经过代码审核的一套工作流之后，才可能经批准并纳入正式代码库中。

首先贡献者的代码通过 Git 命令（或 repo 封装）推送到 Gerrit 管理下的 Git 版本库，推送的提交转化为一个一个的代码审核任务，审核任务可以通过`refs/changes/`下的引用访问到。代码审核者可以通过 Web 界面查看审核任务、代码变更，通过 Web 界面做出通过代码审核或者打回等决定。测试者也可以通过`refs/changes/`之下的引用获取（fetch）修订对其进行测试，如果测试通过就可以将该评审任务设置为校验通过（verified）。最后经过了审核和校验的修订可以通过 Gerrit 界面中提交动作合并到版本库对应的分支中。

Android 项目网站上有一个代码贡献流程图，详细的介绍了 Gerrit 代码审核服务器的工作流程。翻译后的工作流程图见图 32-1。

![../_images/gerrit-workflow.png](img/gerrit-workflow.png)

图 32-1：Gerrit 代码审核工作流

## 5.6.1\. Gerrit 的实现原理

Gerrit 更准确的说应该称为 Gerrit2。因为 Android 项目最早使用的评审服务器 Gerrit 不是今天这个样子，最早版本的 Gerrit 是用 Python 开发运行于 Google App Engine 上，是从 Python 之父 Guido van Rossum 开发的 Rietveld 分支而来。在这里要讨论的 Gerrit 实为 Gerrit2，是用 Java 语言实现的。从这里（[`code.google.com/p/gerrit/wiki/Background`](http://code.google.com/p/gerrit/wiki/Background)）可以看到 Gerrit 更为详尽的发展历史。

### 5.6.1.1\. SSH 协议的 Git 服务器

Gerrit 本身基于 SSH 协议实现了一套 Git 服务器，这样就可以对 Git 数据推送进行更为精确的控制，为强制审核的实现建立了基础。

Gerrit 提供的 Git 服务的端口并非标准的 22 端口，缺省是 29418 端口。可以访问 Gerrit 的 Web 界面得到这个端口。对 Android 项目的代码审核服务器，访问[`review.source.android.com/ssh_info`](https://review.source.android.com/ssh_info)就可以查看到 Git 服务的服务器域名和开放的端口。下面用**curl**命令查看网页的输出。

```
$ curl -L -k http://review.source.android.com/ssh_info
review.source.android.com 29418

```

### 5.6.1.2\. 特殊引用`refs/for/<branch-name>`和`refs/changes/nn/<review-id>/m`

Gerrit 的 Git 服务器，禁止用户向`refs/heads`命名空间下的引用执行推送（除非特别的授权），即不允许用户直接向分支进行提交。为了让开发者能够向 Git 服务器提交修订，Gerrit 的 Git 服务器只允许用户向特殊的引用`refs/for/<branch-name>`下执行推送，其中`<branch-name>`即为开发者的工作分支。向`refs/for/<branch-name>`命名空间下推送并不会在其中创建引用，而是为新的提交分配一个 ID，称为 review-id ，并为该 review-id 的访问建立如下格式的引用`refs/changes/nn/<review-id>/m`，其中：

*   review-id 为 Gerrit 为评审任务顺序分配的全局唯一的号码。
*   `nn`为 review-id 的后两位数，位数不足用零补齐。即`nn`为 review-id 除以 100 的余数。
*   `m`为修订号，该 review-id 的首次提交修订号为 1，如果该修订被打回，重新提交修订号会自增。

### 5.6.1.3\. Git 库的钩子脚本`hooks/commit-msg`

为了保证已经提交审核的修订通过审核入库后，被别的分支拣选（cherry-pick）后再推送至服务器时不会产生新的重复的评审任务，Gerrit 设计了一套方法，即要求每个提交包含唯一的 Change-Id，这个 Change-Id 因为出现在日志中，当执行拣选时也会保持，Gerrit 一旦发现新的提交包含了已经处理过的`Change-Id`，就不再为该修订创建新的评审任务和 review-id，而直接将提交入库。

为了实现 Git 提交中包含唯一的 Change-Id，Gerrit 提供了一个钩子脚本，放在开发者本地 Git 库中（hooks/commit-msg）。这个钩子脚本在用户提交时自动在提交说明中创建以“Change-Id:”及包含**git hash-object**命令产生的哈希值的唯一标识。当 Gerrit 获取到用户向`refs/for/<branch-name>`推送的提交中包含“Change-Id: I...”的变更 ID，如果该 Change-Id 之前没有见过，会创建一个新的评审任务并分配新的 review-id，并在 Gerrit 的数据库中保存 Change-Id 和 review-id 的关联。

如果当用户的提交因为某种原因被要求打回重做，开发者修改之后重新推送到 Gerrit 时就要注意在提交说明中使用相同的“Change-Id”（使用`--amend`提交即可保持提交说明），以免创建新的评审任务，还要在推送时将当前分支推送到`refs/changes/nn/review-id/m`中。其中`nn`和`review-id`和之前提交的评审任务的修订相同，`m`则要人工选择一个新的修订号。

以上说起来很复杂，但是在实际操作中只要使用 repo 这一工具，就相对容易多了。

### 5.6.1.4\. 其余一切交给 Web

Gerrit 另外一个重要的组件就是 Web 服务器，通过 Web 服务器实现对整个评审工作流的控制。关于 Gerrit 工作流，参见在本章开头出现的 Gerrit 工作流程图。

感受一下 Gerrit 的魅力？直接访问 Android 项目的 Gerrit 网站：[`review.source.android.com/`](https://review.source.android.com/)。会看到如图 32-2 的界面。

> ![../_images/android-gerrit-merged.png](img/android-gerrit-merged.png)
> 
> 图 32-2：Android 项目代码审核网站

Android 项目的评审网站，匿名即可访问。点击菜单中的“Merged”显示了已经通过评审合并到代码库中的审核任务。图 32-3 中显示的就是 Andorid 一个已经合并到代码库中的历史评审任务。

> ![../_images/android-gerrit-16993.png](img/android-gerrit-16993.png)
> 
> 图 32-3：Android 项目的 16993 号评审

从图 32-3 可以看出：

*   URL 中显示的评审任务编号为 16993。
*   该评审任务的 Change-Id 以字母`I`开头，包含了一个唯一的 40 位 SHA1 哈希。
*   整个评审任务有三个人参与，一个人进行了检查（verify），两个人进行了代码审核。
*   该评审任务的状态为已合并：“merged”。
*   该评审任务总共包含两个补丁集：Patch set 1 和 Patch set 2。
*   补丁集的下载方法是:**repo download platform/sdk 16993/2**。

如果使用`repo`命令获取补丁集是非常方便的，因为封装后的`repo`屏蔽掉了 Gerrit 的一些实现细节，例如补丁集在 Git 库中的存在位置。如前所述，补丁集实际保存在`refs/changes`命名空间下。使用**git ls-remote**命令，从 Gerrit 维护的代码库中可以看到补丁集对应的引用名称。

```
$ git ls-remote \
      ssh://review.source.android.com:29418/platform/sdk \
      refs/changes/93/16993*
5fb1e79b01166f5192f11c5f509cf51f06ab023d        refs/changes/93/16993/1
d342ef5b41f07c0202bc26e2bfff745b7c86d5a7        refs/changes/93/16993/2

```

接下来就来介绍一下 Gerrit 服务器的部署和使用方法。

## 5.6.2\. 架设 Gerrit 的服务器

**下载 war 包**

Gerrit 是由 Java 开发的，封装为一个 war 包：`gerrit.war`，安装非常简洁。如果需要从源码编译出 war 包，可以参照文档：[`gerrit.googlecode.com/svn/documentation/2.1.5/dev-readme.html`](http://gerrit.googlecode.com/svn/documentation/2.1.5/dev-readme.html)。不过最简单的就是从 Google Code 上直接下载编译好的 war 包。

从下面的地址下载 Gerrit 的 war 包：[`code.google.com/p/gerrit/downloads/list`](http://code.google.com/p/gerrit/downloads/list)。在下载页面会有一个文件名类似`Gerrit-x.x.x.war`的 war 包，这个文件就是 Gerrit 的全部。示例中使用的是 2.1.5.1 版本，把下载的`Gerrit-2.1.5.1.war`包重命名为 Gerrit.war。下面的介绍就是基于这个版本。

**数据库选择**

Gerrit 需要数据库来维护账户信息、跟踪评审任务等。目前支持的数据库类型有 PostgreSQL、MySQL 以及嵌入式的 H2 数据库。

选择使用默认的 H2 内置数据库是最简单的，因为这样无须任何设置。如果想使用更为熟悉的 PostgreSQL 或者 MySQL，则预先建立数据库。

对于 PostgreSQL，在数据库中创建一个用户`gerrit`，并创建一个数据库 reviewdb。

```
createuser -A -D -P -E gerrit
createdb -E UTF-8 -O gerrit reviewdb

```

对于 MySQL，在数据库中创建一个用户 gerrit 并为其设置口令（不要真如下面的将口令置为`secret`），并创建一个数据库 reviewdb。

```
$ mysql -u root -p

mysql> CREATE USER 'gerrit'@'localhost' IDENTIFIED BY 'secret';
mysql> CREATE DATABASE reviewdb;
mysql> ALTER DATABASE reviewdb charset=latin1;
mysql> GRANT ALL ON reviewdb.* TO 'gerrit'@'localhost';
mysql> FLUSH PRIVILEGES;

```

**以一个专用用户帐号执行安装**

在系统中创建一个专用的用户帐号如：gerrit。以该用户身份执行安装，将 Gerrit 的配置文件、内置数据库、war 包等都自动安装在该用户主目录下的特定目录中。

```
$ sudo adduser gerrit
$ sudo su gerrit
$ cd ~gerrit
$ java -jar gerrit.war init -d review_site

```

在安装过程中会提问一系列问题。

*   创建相关目录。

    缺省 Grerit 在安装用户主目录下创建目录`review_site`并把相关文件安装在这个目录之下。Git 版本库的根路径缺省位于此目录之下的`git`目录中。

    ```
    *** Gerrit Code Review 2.1.5.1
    ***

    Create '/home/gerrit/review_site' [Y/n]?

    *** Git Repositories
    ***

    Location of Git repositories   [git]:

    ```

*   选择数据库类型。

    选择 H2 数据库是简单的选择，无须额外的配置。

    ```
    *** SQL Database
    ***

    Database server type           [H2/?]:

    ```

*   设置 Gerrit Web 界面认证的类型。

    缺省为 openid，即使用任何支持 OpenID 的认证源（如 Google、Yahoo）进行身份认证。此模式支持用户自建帐号，当用户通过 OpenID 认证源的认证后，Gerrit 会自动从认证源获取相关属性如用户全名和邮件地址等信息创建帐号。Android 项目的 Gerrit 服务器即采用此认证模式。

    如果有可用的 LDAP 服务器，那么 ldap 或者 ldap_bind 也是非常好的认证方式，可以直接使用 LDAP 中的已有帐号进行认证，不过此认证方式下 Gerrit 的自建帐号功能关闭。此安装示例选择的就是 LDAP 认证方式。

    http 认证也是可选的认证方式，此认证方式需要配置 Apache 的反向代理并在 Apache 中配置 Web 站点的口令认证，通过口令认证后 Gerrit 在创建帐号的过程中会询问用户的邮件地址并发送确认邮件。

    ```
    *** User Authentication
    ***

    Authentication method          [OPENID/?]: ?
           Supported options are:
             openid
             http
             http_ldap
             ldap
             ldap_bind
             development_become_any_account
    Authentication method          [OPENID/?]: ldap
    LDAP server                    [ldap://localhost]:
    LDAP username                  :
    Account BaseDN                 : dc=foo,dc=bar
    Group BaseDN                   [dc=foo,dc=bar]:

    ```

*   发送邮件设置。

    缺省使用本机的 SMTP 发送邮件。

    ```
    *** Email Delivery
    ***

    SMTP server hostname           [localhost]:
    SMTP server port               [(default)]:
    SMTP encryption                [NONE/?]:
    SMTP username                  :

    ```

*   Java 相关设置。

    使用 OpenJava 和 Sun Java 均可。Gerrit 的 war 包要复制到`review_site/bin`目录中。

    ```
    *** Container Process
    ***

    Run as                         [gerrit]:
    Java runtime                   [/usr/lib/jvm/java-6-sun-1.6.0.21/jre]:
    Copy gerrit.war to /home/gerrit/review_site/bin/gerrit.war [Y/n]?
    Copying gerrit.war to /home/gerrit/review_site/bin/gerrit.war

    ```

*   SSH 服务相关设置。

    Gerrit 的基于 SSH 协议的 Git 服务非常重要，缺省的端口为 29418。换做其他端口也无妨，因为 repo 可以自动探测到该端口。

    ```
    *** SSH Daemon
    ***

    Listen on address              [*]:
    Listen on port                 [29418]:

    Gerrit Code Review is not shipped with Bouncy Castle Crypto v144
      If available, Gerrit can take advantage of features
      in the library, but will also function without it.
    Download and install it now [Y/n]?
    Downloading http://www.bouncycastle.org/download/bcprov-jdk16-144.jar ...  OK
    Checksum bcprov-jdk16-144.jar OK
    Generating SSH host key ... rsa... dsa... done

    ```

*   HTTP 服务相关设置。

    缺省启用内置的 HTTP 服务器，端口为 8080，如果该端口被占用（如 Tomcat），则需要更换为其他端口，否则服务启动失败。如下例就换做了 8888 端口。

    ```
    *** HTTP Daemon
    ***

    Behind reverse proxy           [y/N]? y
    Proxy uses SSL (https://)      [y/N]? y
    Subdirectory on proxy server   [/]: /gerrit
    Listen on address              [*]:
    Listen on port                 [8081]:
    Canonical URL                  [https://localhost/gerrit]:

    Initialized /home/gerrit/review_site

    ```

**启动 Gerrit 服务**

Gerrit 服务正确安装后，运行 Gerrit 启动脚本启动 Gerrit 服务。

> ```
> $ /home/gerrit/review_site/bin/gerrit.sh start
> Starting Gerrit Code Review: OK
> 
> ```

服务正确启动之后，会看到 Gerrit 服务打开两个端口，这两个端口是在 Gerrit 安装时指定的。您的输出和下面的示例可能略有不同。

```
$ sudo netstat -ltnp | grep -i gerrit
tcp        0      0 0.0.0.0:8081            0.0.0.0:*               LISTEN      26383/GerritCodeRev
tcp        0      0 0.0.0.0:29418           0.0.0.0:*               LISTEN      26383/GerritCodeRev

```

**设置 Gerrit 服务开机自动启动**

Gerrit 服务的启动脚本支持 start、stop、restart 参数，可以作为 init 脚本开机自动执行。

```
$ sudo ln -snf \
       /home/gerrit/review_site/bin/gerrit.sh \
       /etc/init.d/gerrit.sh
$ sudo ln -snf ../init.d/gerrit.sh /etc/rc2.d/S90gerrit
$ sudo ln -snf ../init.d/gerrit.sh /etc/rc3.d/S90gerrit

```

服务自动启动脚本`/etc/init.d/gerrit.sh`需要通过`/etc/default/gerritcodereview`提供一些缺省配置。以下面内容创建该文件。

```
GERRIT_SITE=/home/gerrit/review_site
NO_START=0

```

**Gerrit 认证方式的选择**

如果是开放服务的 Gerrit 服务，使用 OpenId 认证是最好的方法，就像谷歌 Android 项目的代码审核服务器配置的那样。任何人只要在具有 OpenId provider 的网站上（如 Google、Yahoo 等）具有帐号，就可以直接通过 OpenId 注册，Gerrit 会在用户登录 OpenId provider 网站成功后，自动获取（经过用户的确认）用户在 OpenId provider 站点上的部分注册信息（如用户全名或者邮件地址）在 Gerrit 上自动为用户创建帐号。

如果架设有 LDAP 服务器，并且用户帐号都在 LDAP 中进行管理，那么采用 LDAP 认证也是非常好的方法。登录时提供的用户名和口令通过 LDAP 服务器验证之后，Gerrit 会自动从 LDAP 服务器中获取相应的字段属性，为用户创建帐号。创建的帐号的用户全名和邮件地址因为来自于 LDAP，因此不能在 Gerrit 更改，但是用户可以注册新的邮件地址。我在配置 LDAP 认证时遇到了一个问题就是创建帐号的用户全名是空白，这是因为在 LDAP 相关的字段没有填写的原因。如果 LDAP 服务器使用的是 OpenLDAP，Gerrit 会从 displayName 字段获取用户全名，如果使用 Active Directory 则用 givenName 和 sn 字段的值拼接形成用户全名。

Gerrit 还支持使用 HTTP 认证，这种认证方式需要架设 Apache 反向代理，在 Apache 中配置 HTTP 认证。当用户访问 Gerrit 网站首先需要通过 Apache 配置的 HTTP Basic Auth 认证，当 Gerrit 发现用户已经登录后，会要求用户确认邮件地址。当用户邮件地址确认后，再填写其他必须的字段完成帐号注册。HTTP 认证方式的缺点除了在口令文件管理上需要管理员手工维护比较麻烦之外，还有一个缺点就是用户一旦登录成功后，想退出登录或者更换其他用户帐号登录变得非常麻烦，除非关闭浏览器。关于切换用户有一个小窍门：例如 Gerrit 登录 URL 为[`server/gerrit/login/`](https://server/gerrit/login/)，则用浏览器访问[`nobody:wrongpass@server/gerrit/login/`](https://nobody:wrongpass@server/gerrit/login/)，即用错误的用户名和口令覆盖掉浏览器缓存的认证用户名和口令，这样就可以重新认证了。

在后面的 Gerrit 演示和介绍中，为了设置帐号的方便，使用了 HTTP 认证，因此下面再介绍一下 HTTP 认证的配置方法。

**配置 Apache 代理访问 Gerrit**

缺省 Gerrit 的 Web 服务端口为 8080 或者 8081，通过 Apache 的反向代理就可以使用标准的 80（http）或者 443（https）来访问 Gerrit 的 Web 界面。

```
ProxyRequests Off
ProxyVia Off
ProxyPreserveHost On

<Proxy *>
      Order deny,allow
      Allow from all
</Proxy>

ProxyPass /gerrit/ http://127.0.0.1:8081/gerrit/

```

如果要配置 Gerrit 的 http 认证，则还需要在上面的配置中插入 Http Basic 认证的设置。

```
<Location /gerrit/login/>
  AuthType Basic
  AuthName "Gerrit Code Review"
  Require valid-user
  AuthUserFile /home/gerrit/review_site/etc/gerrit.passwd
</Location>

```

在上面的配置中，指定了口令文件的位置：`/home/gerrit/review_site/etc/gerrit.passwd`。可以用**htpasswd**命令维护该口令文件。

```
$ touch /home/gerrit/review_site/etc/gerrit.passwd

$ htpasswd -m /home/gerrit/review_site/etc/gerrit.passwd jiangxin
New password:
Re-type new password:
Adding password for user jiangxin

```

至此为止，Gerrit 服务安装完成。在正式使用 Gerrit 之前，先来研究一下 Gerrit 的配置文件，以免安装过程中遗漏或错误的设置影响使用。

## 5.6.3\. Gerrit 的配置文件

Gerrit 的配置文件保存在部署目录下的`etc/gerrit.conf`文件中。如果对安装时的配置不满意，可以手工修改配置文件，重启 Gerrit 服务即可。

全部采用缺省配置时的配置文件：

```
[gerrit]
        basePath = git
        canonicalWebUrl = http://localhost:8080/
[database]
        type = H2
        database = db/ReviewDB
[auth]
        type = OPENID
[sendemail]
        smtpServer = localhost
[container]
        user = gerrit
        javaHome = /usr/lib/jvm/java-6-openjdk/jre
[sshd]
        listenAddress = *:29418
[httpd]
        listenUrl = http://*:8080/
[cache]
        directory = cache

```

如果采用 LDAP 认证，下面的配置文件片断配置了一个支持匿名绑定的 LDAP 服务器配置。

```
[auth]
  type = LDAP
[ldap]
  server = ldap://localhost
  accountBase = dc=foo,dc=bar
  groupBase = dc=foo,dc=bar

```

如果采用 MySQL 而非缺省的 H2 数据库，下面的配置文件显示了相关配置。

```
[database]
        type = MYSQL
        hostname = localhost
        database = reviewdb
        username = gerrit

```

LDAP 绑定或者数据库连接的用户口令保存在:file:` etc/secure.config`文件中。

```
[database]
  password = secret

```

下面的配置将 Web 服务架设在 Apache 反向代理的后面。

```
[httpd]
        listenUrl = proxy-https://*:8081/gerrit

```

## 5.6.4\. Gerrit 的数据库访问

之所以要对数据库访问多说几句，是因为一些对 Gerrit 的设置往往在 Web 界面无法配置，需要直接修改数据库，而大部分用户在安装 Gerrit 时都会选用内置的 H2 数据库，如何操作 H2 数据库可能大部分用户并不了解。

实际上无论选择何种数据库，Gerrit 都提供了两种数据库操作的命令行接口。第一种方法是在服务器端调用`gerrit.war`包中的命令入口，另外一种方法是远程 SSH 调用接口。

对于第一种方法，需要在服务器端执行，而且如果使用的是 H2 内置数据库还需要先将 Gerrit 服务停止。先以安装用户身份进入 Gerrit 部署目录下，在执行命令调用`gerrit.war`包，如下：

```
$ java -jar bin/gerrit.war gsql
Welcome to Gerrit Code Review 2.1.5.1
(H2 1.2.134 (2010-04-23))

Type '\h' for help.  Type '\r' to clear the buffer.

gerrit>

```

当出现“`gerrit>`”提示符时，就可以输入 SQL 语句操作数据库了。

第一种方式需要登录到服务器上，而且操作 H2 数据库时还要预先停止服务，显然很不方便。但是这种方法也有存在的必要，就是不需要认证，尤其是在管理员帐号尚未建立之前就可以查看和更改数据库。

当在 Gerrit 上注册了第一个帐号，即拥有了管理员帐号，正确为该帐号配置公钥之后，就可以访问 Gerrit 提供的 SSH 登录服务。Gerrit 的 SSH 协议提供第二个访问数据库的接口。下面的命令就是用管理员公钥登录 Gerrit 的 SSH 服务器，操作数据库。虽然演示用的是本机地址（localhost），但是操作远程服务器也是可以的，只要拥有管理员授权。

```
$ ssh -p 29418 localhost gerrit gsql
Welcome to Gerrit Code Review 2.1.5.1
(H2 1.2.134 (2010-04-23))

Type '\h' for help.  Type '\r' to clear the buffer.

gerrit>

```

即连接 Gerrit 的 SSH 服务，运行命令**gerrit gsql**。当连接上数据库管理接口后，便出现“`gerrit>`”提示符，在该提示符下可以输入 SQL 命令。下面的示例中使用的数据库后端为 H2 内置数据库。

可以输入`show tables`命令显示数据库列表。

```
gerrit> show tables;
 TABLE_NAME                  | TABLE_SCHEMA
 ----------------------------+-------------
 ACCOUNTS                    | PUBLIC
 ACCOUNT_AGREEMENTS          | PUBLIC
 ACCOUNT_DIFF_PREFERENCES    | PUBLIC
 ACCOUNT_EXTERNAL_IDS        | PUBLIC
 ACCOUNT_GROUPS              | PUBLIC
 ACCOUNT_GROUP_AGREEMENTS    | PUBLIC
 ACCOUNT_GROUP_MEMBERS       | PUBLIC
 ACCOUNT_GROUP_MEMBERS_AUDIT | PUBLIC
 ACCOUNT_GROUP_NAMES         | PUBLIC
 ACCOUNT_PATCH_REVIEWS       | PUBLIC
 ACCOUNT_PROJECT_WATCHES     | PUBLIC
 ACCOUNT_SSH_KEYS            | PUBLIC
 APPROVAL_CATEGORIES         | PUBLIC
 APPROVAL_CATEGORY_VALUES    | PUBLIC
 CHANGES                     | PUBLIC
 CHANGE_MESSAGES             | PUBLIC
 CONTRIBUTOR_AGREEMENTS      | PUBLIC
 PATCH_COMMENTS              | PUBLIC
 PATCH_SETS                  | PUBLIC
 PATCH_SET_ANCESTORS         | PUBLIC
 PATCH_SET_APPROVALS         | PUBLIC
 PROJECTS                    | PUBLIC
 REF_RIGHTS                  | PUBLIC
 SCHEMA_VERSION              | PUBLIC
 STARRED_CHANGES             | PUBLIC
 SYSTEM_CONFIG               | PUBLIC
 TRACKING_IDS                | PUBLIC
(27 rows; 65 ms)

```

输入`show columns`命令显示数据库的表结构。

```
gerrit> show columns from system_config;
 FIELD                      | TYPE         | NULL | KEY | DEFAULT
 ---------------------------+--------------+------+-----+--------
 REGISTER_EMAIL_PRIVATE_KEY | VARCHAR(36)  | NO   |     | ''
 SITE_PATH                  | VARCHAR(255) | YES  |     | NULL
 ADMIN_GROUP_ID             | INTEGER(10)  | NO   |     | 0
 ANONYMOUS_GROUP_ID         | INTEGER(10)  | NO   |     | 0
 REGISTERED_GROUP_ID        | INTEGER(10)  | NO   |     | 0
 WILD_PROJECT_NAME          | VARCHAR(255) | NO   |     | ''
 BATCH_USERS_GROUP_ID       | INTEGER(10)  | NO   |     | 0
 SINGLETON                  | VARCHAR(1)   | NO   | PRI | ''
(8 rows; 52 ms)

```

关于 H2 数据库更多的 SQL 语法，参考：[`www.h2database.com/html/grammar.html`](http://www.h2database.com/html/grammar.html)。

下面开始介绍 Gerrit 的使用。

## 5.6.5\. 立即注册为 Gerrit 管理员

第一个 Gerrit 账户自动成为权限最高的管理员，因此 Gerrit 安装完毕后的第一件事情就是立即注册或者登录，以便初始化管理员帐号。下面的示例是在本机（localhost）以 HTTP 认证方式架设的 Gerrit 审核服务器。当第一次访问的时候，会弹出非常眼熟的 HTTP Basic Auth 认证界面，如图 32-4。

![../_images/gerrit-account-http-auth.png](img/gerrit-account-http-auth.png)

图 32-4：Http Basic Auth 认证界面

输入正确的用户名和口令登录后，系统自动创建 ID 为 1000000 的帐号，该帐号是第一个注册的帐号，会自动该被赋予管理员身份。因为使用的是 HTTP 认证，用户的邮件地址等个人信息尚未确定，因此登录后首先进入到个人信息设置界面。如图 32-5。

![../_images/gerrit-account-init-1.png](img/gerrit-account-init-1.png)

图 32-5：Gerrit 第一次登录后的个人信息设置界面

在图 32-5 中可以看到在菜单中有“Admin”菜单项，说明当前登录的用户被赋予了管理员权限。在图 32-5 的联系方式确认对话框中有一个注册新邮件地址的按钮，点击该按钮弹出邮件地址录入对话框，如图 32-6。

![../_images/gerrit-account-init-2.png](img/gerrit-account-init-2.png)

图 32-6：输入个人的邮件地址

必须输入一个有效的邮件地址以便能够收到确认邮件。这个邮件地址非常重要，因为 Git 代码提交时在提交说明中出现的邮件地址需要和这个地址一致。当填写了邮件地址后，会收到一封确认邮件，点击邮件中的确认链接会重新进入到 Gerrit 帐号设置界面，如图 32-7。

![../_images/gerrit-account-init-4-settings-username.png](img/gerrit-account-init-4-settings-username.png)

图 32-7：邮件地址确认后进入 Gerrit 界面

在`Full Name`字段输入用户名，点击保存更改后，右上角显示的“Anonymous Coward” 就会显示为登录用户的姓名和邮件地址。

接下来需要做的最重要的一件事就是配置公钥（如图 32-8）。通过该公钥，注册用户可以通过 SSH 协议向 Gerrit 的 Git 服务器提交，如果具有管理员权限还能够远程管理 Gerrit 服务器。

![../_images/gerrit-account-init-5-settings-ssh-pubkey.png](img/gerrit-account-init-5-settings-ssh-pubkey.png)

图 32-8：Gerrit 的 SSH 公钥设置界面

在文本框中粘贴公钥。关于如何生成和管理公钥，参见第二十九章“使用 SSH 协议”相关内容。

点击“Add”按钮，完成公钥的添加。添加的公钥就会显示在列表中（如图 32-9）。一个用户可以添加多个公钥。

![../_images/gerrit-account-init-6-settings-ssh-pubkey-added.png](img/gerrit-account-init-6-settings-ssh-pubkey-added.png)

图 32-9：用户的公钥列表

点击左侧的“Groups”（用户组）菜单项，可以看到当前用户所属的分组，如图 32-10。

![../_images/gerrit-account-init-7-settings-groups.png](img/gerrit-account-init-7-settings-groups.png)

图 32-10：Gerrit 用户所属的用户组

第一个注册的用户同时属于三个用户组，一个是管理员用户组（Administrators），另外两个分别是 Anonymous Users（任何用户）和 Registered Users（注册用户）。

## 5.6.6\. 管理员访问 SSH 的管理接口

当在 Gerrit 个人配置界面中设置了公钥之后，就可以连接 Gerrit 的 SSH 服务器执行命令，示例使用的是本机（localhost），其实远程 IP 地址一样可以。只是对于远程主机需要确认端口不要被防火墙拦截，Gerrit 的 SSH 服务器使用特殊的端口，缺省是 29418。

任何用户都可以通过 SSH 连接执行**gerrit ls-projects**命令查看项目列表。下面的命令没有输出，是因为项目尚未建立。

```
$ ssh -p 29418 localhost gerrit ls-projects

```

可以执行**scp**命令从 Gerrit 的 SSH 服务器中拷贝文件。

```
$ scp -P 29418 -p -r localhost:/ gerrit-files

$ find gerrit-files -type f
gerrit-files/bin/gerrit-cherry-pick
gerrit-files/hooks/commit-msg

```

可以看出 Gerrit 服务器提供了两个文件可以通过**scp**下载，其中`commit-msg`脚本文件应该放在用户本地 Git 库的钩子目录中以便在生成的提交中包含唯一的 Change-Id。在之前的 Gerrit 原理中介绍过。

除了普通用户可以执行的命令外，管理员还可以通过 SSH 连接执行 Gerrit 相关的管理命令。例如之前介绍的管理数据库：

```
$ ssh -p 29418 localhost gerrit gsql
Welcome to Gerrit Code Review 2.1.5.1
(H2 1.2.134 (2010-04-23))

Type '\h' for help.  Type '\r' to clear the buffer.

gerrit>

```

此外管理员还可以通过 SSH 连接执行帐号创建，项目创建等管理操作，可以执行下面的命令查看帮助信息。

```
$ ssh -p 29418 localhost gerrit --help
gerrit COMMAND [ARG ...] [--] [--help (-h)]

 --          : end of options
 --help (-h) : display this help text

Available commands of gerrit are:

   approve
   create-account
   create-group
   create-project
   flush-caches
   gsql
   ls-projects
   query
   receive-pack
   replicate
   review
   set-project-parent
   show-caches
   show-connections
   show-queue
   stream-events

See 'gerrit COMMAND --help' for more information.

```

更多的帮助信息，还可以参考 Gerrit 版本库中的帮助文件：`Documentation/cmd-index.html`。

## 5.6.7\. 创建新项目

一个 Gerrit 项目对应于一个同名的 Git 库，同时拥有一套可定制的评审流程。创建一个新的 Gerrit 项目就会在对应的版本库根目录下创建 Git 库。管理员可以使用命令行创建新项目。

```
$ ssh -p 29418 localhost gerrit create-project --name new/project

```

当执行**gerrit ls-projects**命令，可以看到新项目创建已经成功创建。

```
$ ssh -p 29418 localhost gerrit ls-projects
new/project

```

在 Gerrit 的 Web 管理界面，也可以看到新项目已经建立，如图 32-11。

![../_images/gerrit-project-1-list.png](img/gerrit-project-1-list.png)

图 32-11：Gerrit 中项目列表

在项目列表中可以看到除了新建的`new/project`项目之外还有一个名为“– All Projects –”的项目，其实它并非一个真实存在的项目，只是为了项目授权管理的方便。即在“– All Projects –” 中建立的项目授权能够被其他项目共享。

在服务器端也可以看到 Gerrit 部署中版本库根目录下已经有同名的 Git 版本库被创建。

```
$ ls -d /home/gerrit/review_site/git/new/project.git
/home/gerrit/review_site/git/new/project.git

```

这个新的版本库刚刚初始化，尚未包括任何数据。是否可以通过**git push**向该版本库推送一些初始数据呢？下面用 Gerrit 的 SSH 协议克隆该版本库，并尝试向其推送数据。

```
$ git clone ssh://localhost:29418/new/project.git myproject
Cloning into myproject...
warning: You appear to have cloned an empty repository.

$ cd myproject/

$ echo hello > readme.txt

$ git add readme.txt

$ git commit -m "initialized."
[master (root-commit) 15a549b] initialized.
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 readme.txt
09:58:54 jiangxin@hp:~/tmp/myproject$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 222 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/new/project.git
 ! [remote rejected] master -> master (prohibited by Gerrit)
error: failed to push some refs to 'ssh://localhost:29418/new/project.git'

```

向 Gerrit 的 Git 版本库推送失败，远程 Git 服务器返回错误信息：“prohibited by Gerrit”。这是因为 Gerrit 缺省不允许直接向分支推送，而是需要向`refs/for/<branch-name>`的特殊引用进行推送以便将提交转换为评审任务。

但是如果希望将版本库的历史提交不经审核直接推送到 Gerrit 维护的 Git 版本库中可以么？是的，只要通过 Gerrit 的管理界面为该项目授权：允许某个用户组（如 Administrators 组）的用户可以向分支推送。（注意该授权在推送完毕后尽快撤销，以免被滥用）

Gerrit 的界面对用户非常友好（如图 32-12）。例如在添加授权的界面中，只要在用户组的输入框中输入前几个字母，就会弹出用户组列表供选择。

![../_images/gerrit-project-3-acl-create-branch.png](img/gerrit-project-3-acl-create-branch.png)

图 32-12：添加授权的界面

添加授权完毕后，项目“new/project”的授权列表就会出现新增的为 Administrators 管理员添加的“+2: Create Branch”授权，如图 32-13。

![../_images/gerrit-project-4-acl-created-branch.png](img/gerrit-project-4-acl-created-branch.png)

图 32-13：添加授权后的授权列表

因为已经为管理员分配了直接向`refs/heads/*`引用推送的授权，这样就能够向 Git 版本库推送数据了。再执行一次推送任务，看看能否成功。

```
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 222 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/new/project.git
 ! [remote rejected] master -> master (you are not committer jiangxin@ossxp.com)
error: failed to push some refs to 'ssh://localhost:29418/new/project.git'

```

推送又失败了，但是服务器端返回的错误信息不同。上一次出错返回的是“prohibited by Gerrit”，而这一次返回的错误信息是“you are not committer”。

这是为什么呢？看看提交日志：

```
$ git log --pretty=full
commit 15a549bac6bd03ad36e643984fed554406480b2c
Author: Jiang Xin <jiangxin@ossxp.com>
Commit: Jiang Xin <jiangxin@ossxp.com>

    initialized.

```

提交者（Commit）为“Jiang Xin <jiangxin@ossxp.com>”，而 Gerrit 中注册的用户的邮件地址是“jiangxin@moon.ossxp.com”，两者之间的不一致，导致 Gerrit 再一次拒绝了提交。如果再到 Gerrit 看一下`new/project`的权限设置，会看到这样一条授权：

```
Category        Group Name        Reference Name  Permitted Range
========        ==========        ==============  ===============
Forge Identity  Registered Users  refs/*          +1: Forge Author Identity

```

这条授权的含义是提交中的 Author 字段不进行邮件地址是否注册的检查，但是要对 Commit 字段进行邮件地址检查。如果增加一个更高级别的“Forge Identity”授权，也可以忽略对 Committer 的邮件地址检查，但是尽量不要对授权进行非必须的改动，因为在提交的时候使用注册的邮件地址是一个非常好的实践。

下面就通过**git config**命令修改提交时所用的邮件地址，和 Gerrit 注册时用的地址保持一致。然后用`--amend`参数重新执行提交以便让修改后的提交者邮件地址在提交中生效。

```
$ git config user.email jiangxin@moon.ossxp.com

$ git commit --amend -m initialized
[master 82c8fc3] initialized
 Author: Jiang Xin <jiangxin@ossxp.com>
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 readme.txt

$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 233 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/new/project.git
 * [new branch]      master -> master

```

看这次提交成功了！之所以成功，是因为提交者的邮件地址更改了。看看重新提交的日志，可以发现作者（Author）和提交者（Commit）的邮件地址的不同，`Commit`字段的邮件地址和注册时使用的邮件地址相同。

```
$ git log --pretty=full
commit 82c8fc3805d57cc0d17d58e1452e21428910fd2d
Author: Jiang Xin <jiangxin@ossxp.com>
Commit: Jiang Xin <jiangxin@moon.ossxp.com>

    initialized

```

注意，版本库初始化完成之后，应尽快把为项目新增的“Push Branch”类型的授权删除，对新的提交强制使用 Gerrit 的评审流程。

## 5.6.8\. 从已有 Git 库创建项目

如果已经拥有很多版本库，希望从这些版本库创建 Gerrit 项目，如果像上面介绍的那样一个一个的创建项目，再执行**git push**命令推送已经包含历史数据的版本库，将是十分麻烦的事情。那么有没有什么简单的办法呢？可以通过下面的步骤，实现多项目的快速创建。

首先将已有版本库创建到 Gerrit 的版本库根目录下。注意版本库名称将会成为项目名（除去`.git`后缀），而且创建（或克隆）的版本库应为裸版本库，即使用`--bare`参数创建。

例如在 Gerrit 的 Git 版本库根目录下创建名为`hello.git`的版本库。下面的示例中我偷了一下懒，直接从`new/project`克隆到`hello.git`。:)

```
$ git clone --mirror \
      /home/gerrit/review_site/git/new/project.git \
      /home/gerrit/review_site/git/hello.git

```

这时查看版本库列表，却看不到新建立的名为`hello.git`的 Git 库出现在项目列表中。

```
$ ssh -p 29418 localhost gerrit ls-projects
new/project

```

可以通过修改 Gerrit 数据库来注册新项目，即连接到 Gerrit 数据库，输入 SQL 插入语句。

```
$ ssh -p 29418 localhost gerrit gsql
Welcome to Gerrit Code Review 2.1.5.1
(H2 1.2.134 (2010-04-23))

Type '\h' for help.  Type '\r' to clear the buffer.

gerrit> INSERT INTO projects
     -> (use_contributor_agreements ,submit_type ,name)
     -> VALUES
     -> ('N' ,'M' ,'hello');
UPDATE 1; 1 ms
gerrit>

```

注意 SQL 语句中的项目名称是版本库名称除去`.git`后缀的部分。在数据库插入数据后，再来查看项目列表就可以看到新注册的项目了。

```
$ ssh -p 29418 localhost gerrit ls-projects
hello
new/project

```

可以登录到 Gerrit 项目对新建立的项目进行相关设置。例如修改项目的说明，项目的提交策略，是否要求提交说明中必须包含“Signed-off-by”信息等，如图 32-14。

![../_images/gerrit-project-5-newproject-settings.png](img/gerrit-project-5-newproject-settings.png)

图 32-14：项目基本设置

这种通过修改数据库从已有版本库创建项目的方法适合大批量的项目创建。下面就对新建立的 hello 项目进行一次完整的 Gerrit 评审流程。

## 5.6.9\. 定义评审工作流

刚刚安装好的 Gerrit 的评审工作流并不完整，还不能正常的开展评审工作，需要对项目授权进行设置以定制适合的评审工作流。

缺省安装的 Gerrit 中只内置了四个用户组，如表 32-1 所示。

> 表 32-1：Gerrit 内置用户组
> 
> | 用户组 | 说明 |
> | --- | --- |
> | Administrators | Gerrit 管理员 |
> | Anonymous Users | 任何用户，登录或未登录 |
> | Non-Interactive Users | Gerrit 中执行批处理的用户 |
> | Registered Users | 任何登录用户 |

未登录的用户只属于 Anonymous Users，登录用户则同时拥有 Anonymous Users 和 Registered Users 的权限。对于管理员则还拥有 Administrators 用户组权限。

查看全局（伪项目“– All Projects –”）的初始权限设置。会看到如表 32-2 一样的授权表格。

> 表 32-2：Gerrit 授权表格
> 
> | 编号 | 类别 | 用户组名称 | 引用名称 | 权限范围 |
> | --- | --- | --- | --- | --- |
> | 1 | Code Review | Registered Users | refs/heads/* | -1: I would prefer that you didn’t submit this |
> | +1: Looks good to me, but someone else must approve |
> | 2 | Forge Identity | Registered Users | refs/* | +1: Forge Author Identity |
> | 3 | Read Access | Administrators | refs/* | +1: Read access |
> | 4 | Read Access | Anonymous Users | refs/* | +1: Read access |
> | 5 | Read Access | Registered Users | refs/* | +2: Upload permission |

对此表格中的授权解读如下：

*   对于匿名用户：根据第 4 条授权策略，匿名用户能够读取任意版本库。

*   对于注册用户：根据第 5 条授权策略，注册用户具有比第四条授权高一个等级的权限，即注册用户除了具有读取版本库权限外，还可以向版本库的`refs/for/<branch-name>`引用推送，产生评审任务的权限。

    之所以这种可写的权限也放在“Read Access”类别中，是因为 Git 的写操作必须建立在拥有读权限之上，因此 Gerrit 将其与读取都放在“Read Access”归类之下，只不过更高一个级别。

*   对于注册用户：根据第 2 条授权策略，在向服务器推送提交的时候，忽略对提交中 Author 字段的邮件地址检查。这个在之前已经讨论过。

*   对于注册用户：根据第 1 条授权策略，注册用户具有代码审核的一般权限，即能够将评审任务设置为“+1”级别（看起来不错，但需要通过他人认可），或者将评审任务标记为“-1”，即评审任务没有通过不能提交。

*   对于管理员：根据第 3 条策略，管理员能够读取任意版本库。

上面的授权策略仅仅对评审流程进行了部分设置。如：提交能够进入评审流程，因为登录用户（注册用户）可以将提交以评审任务方式上传；注册用户可以将评审任务标记为“+1: 看起来不错，但需其他人认可”。但是没有人有权限可以将评审任务提交——合并到正式版本库中，即没人能够对评审任务做最终的确认及提交，因此评审流程是不完整的。

要想实现对评审最终确认的授权，有两种方法可以实现，一种是赋予特定用户 Verified 类别中的“+1: Verified”的授权，另外一个方法是赋予特定用户 Code Review 类别中更高级别的授权：“+2: Looks good to me, approved”。要想实现对经过确认的评审任务提交，还需要赋予特定用户 Submit 类别中的“+1: Submit”授权。

下面的示例中，创建两个新的用户组 Reviewer 和 Verifier，并为其赋予相应的授权。

创建用户组，可以通过 Web 界面或者命令行。如果通过 Web 界面添加用户组，选择“Admin”菜单下的“Groups”子菜单，如图 32-15。

![../_images/gerrit-addgroup-1.png](img/gerrit-addgroup-1.png)

图 32-15：Gerrit 用户组创建

输入用户组名称后，点击“Create Group”按钮。进入创建用户组后的设置页，如图 32-16。

![../_images/gerrit-addgroup-2.png](img/gerrit-addgroup-2.png)

图 32-16：Gerrit 用户组设置页

注意到在用户设置页面中有一个 Owners 字段名称和用户组名称相同，实际上这是 Gerrit 关于用户组的一个特别的功能。一个用户组可以设置另外一个用户组为本用户组的 Owners，属于 Owners 用户组的用户实际上相当于本用户组的管理者，可以添加用户、修改用户组名称等。不过一般最常用的设置是使用同名的用户组作为 Owners。

在用户组设置页面的最下面，是用户组用户分配对话框，可以将用户分配到用户组中。注意 Gerrit 的用户组不能包含，即只能将用户分配到用户组中。

图 32-17 是添加了两个新用户组后的用户组列表：

![../_images/gerrit-addgroup-3-list.png](img/gerrit-addgroup-3-list.png)

图 32-17：Gerrit 用户组列表

接下来要为新的用户组授权，需要访问“Admin”菜单下的“Projects”子菜单，点击对应的项目进入权限编辑界面。为了简便起见，选择“– All Projects –”，对其授权的更改可以被所有其他的项目共享。图 32-18 是为 Reviewer 用户组建立授权过程的页面。

![../_images/gerrit-acl-1-reviewer.png](img/gerrit-acl-1-reviewer.png)

图 32-18：为 Reviewer 用户组建立授权

分别为两个新建立的用户组分配授权，如表 32-3 所示。编号从 6 开始，是因为这里补充的授权是建立在前面的缺省授权列表的基础上的。

> 表 32-3：新用户组权限分配表
> 
> | 编号 | 类别 | 用户组名称 | 引用名称 | 权限范围 |
> | --- | --- | --- | --- | --- |
> | 6 | Code Review | Reviewer | refs/* | -2: Do not submit |
> | +2: Looks good to me, approved |
> | 7 | Verified | Verifier | refs/* | -1: Fails |
> | +1: Verified |
> | 8 | Submit | Verifier | refs/* | +1: Submit |

这样，就为 Gerrit 所有的项目设定了可用的评审工作流。

## 5.6.10\. Gerrit 评审工作流实战

分别再注册两个用户帐号`dev1@moon.ossxp.com`和`dev2@moon.ossxp.com`，两个用户分别属于 Reviewer 用户组和 Verifier 用户组。这样 Gerrit 部署中就拥有了三个用户帐号，用帐号`jiangxin`进行代码提交，用`dev1`帐号对任务进行代码审核，用`dev2`用户对审核任务进行最终的确认。

### 5.6.10.1\. 开发者在本地版本库中工作

Repo 是 Gerrit 的最佳伴侣，凡是需要和 Gerrit 版本库交互的工作都封装在 repo 命令中。关于 repo 的用法在上一部分的 repo 多版本库协同的章节中已经详细介绍了。这里只介绍开发者如何只使用 Git 命令来和 Gerrit 服务器交互。这样也可以更深入的理解 repo 和 gerrit 整合的机制。

首先克隆 Gerrit 管理的版本库，使用 Gerrit 提供的运行于 29418 端口的 SSH 协议。

```
$ git clone ssh://localhost:29418/hello.git
Cloning into hello...
remote: Counting objects: 3, done
remote: Compressing objects: 100% (3/3)
Receiving objects: 100% (3/3), done.

```

然后拷贝 Gerrit 服务器提供的`commit-msg`钩子脚本。

```
$ cd hello
$ scp -P 29418 -p localhost:/hooks/commit-msg .git/hooks/

```

别忘了修改 Git 配置中提交者的邮件地址，以便和 Gerrit 中注册的地址保持一致。不使用`--global`参数调用**git config**可以只对本版本库的提交设定提交者邮件。

```
$ git config user.email jiangxin@moon.ossxp.com

```

然后修改`readme.txt`文件，并提交。注意提交的时候使用了`-s`参数，目的是在提交说明中加入“Signed-off-by:”标记，这在 Gerrit 提交中可能是必须的。

```
$ echo "gerrit review test" >> readme.txt
$ git commit -a -s -m "readme.txt hacked."
[master c65ab49] readme.txt hacked.
 1 files changed, 1 insertions(+), 0 deletions(-)

```

查看一下提交日志，会看到其中有特殊的标签。

```
$ git log --pretty=full -1
commit c65ab490f6d3dc36429b8f1363b6191357202f2e
Author: Jiang Xin <jiangxin@moon.ossxp.com>
Date:   Mon Nov 15 17:50:08 2010 +0800

    readme.txt hacked.

    Change-Id: Id7c9d88ebf5dac2d19a7e0896289de1ae6fb6a90
    Signed-off-by: Jiang Xin <jiangxin@moon.ossxp.com>

```

提交说明中出现了“Change-Id:”标签，这个标签是由钩子脚本`commit-msg`自动生成的。至于这个标签的含义，在前面 Gerrit 的实现原理中介绍过。

好了，准备把这个提交 PUSH 到服务器上吧。

### 5.6.10.2\. 开发者向审核服务器提交

由 Gerrit 控制的 Git 版本库不能直接提交，因为正确设置的 Gerrit 服务器，会拒绝用户直接向`refs/heads/*`推送。

```
$ git status
# On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#
nothing to commit (working directory clean)

$ git push
Counting objects: 5, done.
Writing objects: 100% (3/3), 332 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/hello.git
 ! [remote rejected] master -> master (prohibited by Gerrit)
error: failed to push some refs to 'ssh://localhost:29418/hello.git'

```

直接推送就会出现遇到“prohibited by Gerrit”的错误。

正确的做法是向特殊的引用推送，这样 Gerrit 会自动将新提交转换为评审任务。

```
$ git push origin HEAD:refs/for/master
Counting objects: 5, done.
Writing objects: 100% (3/3), 332 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/hello.git
 * [new branch]      HEAD -> refs/for/master

```

看到了么，向`refs/for/master`推送成功。

### 5.6.10.3\. 审核评审任务

以`dev1`用户登录 Gerrit 网站，点击“All”菜单下的“Open”标签，可以新提交到 Gerrit 状态为 Open 的评审任务，如图 32-19。

![../_images/gerrit-review-1-tasklist.png](img/gerrit-review-1-tasklist.png)

图 32-19：Gerrit 评审任务列表

点击该评审任务，显示关于此评审任务的详细信息，如图 32-20。

![../_images/gerrit-review-2-changeid_full.png](img/gerrit-review-2-changeid_full.png)

图 32-20：Gerrit 评审任务概述

从 URL 地址栏可以看到该评审任务的评审编号为 1。目前该评审任务有一个补丁集（Patch Set 1），可以点击“Diff All Side-by-Side”查看变更集，以决定该提交是否应该被接受。作为测试，先让此次提交通过代码审核，于是以`dev1`用户身份点击“Review”按钮。

点击“Review”按钮后，弹出代码评审对话框，如图 32-21。

![../_images/gerrit-review-3-review-approved.png](img/gerrit-review-3-review-approved.png)

图 32-21：Gerrit 任务评审对话框

选择“+2: Looks good to me, approved.”，点击按钮“Publish Comments”以通过评审。注意因为没有给`dev1`用户（Reviewer 用户组）授予 Submit 权限，因此此时`dev1`还不能将此审核任务提交。

当`dev1`用户做出通过评审的决定后，代码提交者`jiangxin`会收到一封邮件，如图 32-22。

![../_images/gerrit-review-4-review-mail-notify.png](img/gerrit-review-4-review-mail-notify.png)

图 32-22：Gerrit 通知邮件

### 5.6.10.4\. 评审任务没有通过测试

下面以`dev2`帐号登录 Gerrit，查看处于打开状态的评审任务，如图 32-23。会看到评审任务 1 的代码评审已经通过，但是尚未进行测试检查（Verify）。于是`dev2`可以下载该补丁集，在本机进行测试。

![../_images/gerrit-review-5-review-verify-view.png](img/gerrit-review-5-review-verify-view.png)

图 32-23：Gerrit 评审任务显示

假设测试没有通过，`dev2`用户点击该评审任务的“Review”按钮，重置该任务的评审状态，如图 32-24。

![../_images/gerrit-review-6-review-verify-failed.png](img/gerrit-review-6-review-verify-failed.png)

图 32-24：Gerrit 评审任务未通过

注意到图 32-24 中`dev2`用户的评审对话框有三个按钮，多出的“Publish and Submit”按钮是因为`dev2`拥有 Submit 授权。`dev2`用户在上面的对话框中，选择了“-1: Fails”，当点击“Publish Comments”按钮，该评审任务的评审记录被重置，同时提交者和其他评审参与者会收到通知邮件，如图 32-25。

![../_images/gerrit-review-7-review-mail-notify-failed.png](img/gerrit-review-7-review-mail-notify-failed.png)

图 32-25：Gerrit 通知邮件：评审未通过

### 5.6.10.5\. 重新提交新的补丁集

提交者收到代码被打回的邮件，一定很难过。不过这恰恰说明了这个软件过程已经相当的完善，现在发现问题总比在集成测试时甚至被客户发现要好的多吧。

根据评审者和检验者的提示，开发者对代码进行重新修改。下面的 bugfix 过程仅仅是一个简单的示例，bugfix 没有这么简单的，对么？;-)

```
$ echo "fixed" >> readme.txt

```

重新修改后，需要使用`--amend`参数进行提交，即使用前次提交的日志重新提交，这一点非常重要。因为这样就会对原提交说明中的“Change-Id:”标签予以原样保留，当再将新提交推送到服务器时，Gerrit 不会为新提交生成新的评审任务编号而是会重用原有的任务编号，将新提交转化为老的评审任务的新的补丁集。

在执行**git commit --amend**时，可以修改提交说明，但是注意不要删除 Change-Id 标签，更不能修改它。

```
$ git add -u
$ git commit --amend

readme.txt hacked with bugfix.

Change-Id: Id7c9d88ebf5dac2d19a7e0896289de1ae6fb6a90
Signed-off-by: Jiang Xin <jiangxin@moon.ossxp.com>

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#
# Changes to be committed:
#   (use "git reset HEAD¹ <file>..." to unstage)
#
# modified:   readme.txt
#

```

提交成功后，执行**git ls-remote**命令会看到 Gerrit 维护的 Git 库中只有一个评审任务（编号 1），且该评审任务只有一个补丁集（Patch Set 1）。

```
$ git ls-remote origin
82c8fc3805d57cc0d17d58e1452e21428910fd2d        HEAD
c65ab490f6d3dc36429b8f1363b6191357202f2e        refs/changes/01/1/1
82c8fc3805d57cc0d17d58e1452e21428910fd2d        refs/heads/master

```

把修改后的提交推送到 Gerrit 管理下的 Git 版本库中。注意依旧推送到`refs/for/master`引用中。

```
$ git push origin HEAD:refs/for/master
Counting objects: 5, done.
Writing objects: 100% (3/3), 353 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/hello.git
 * [new branch]      HEAD -> refs/for/master

```

推送成功后，再执行**git ls-remote**命令，会看到唯一的评审任务（编号 1）有了两个补丁集。

```
$ git ls-remote origin
82c8fc3805d57cc0d17d58e1452e21428910fd2d        HEAD
c65ab490f6d3dc36429b8f1363b6191357202f2e        refs/changes/01/1/1
1df9e8e05fcf97a46588488918a476abd1df8121        refs/changes/01/1/2
82c8fc3805d57cc0d17d58e1452e21428910fd2d        refs/heads/master

```

### 5.6.10.6\. 新修订集通过评审

当提交者重新针对评审任务进行提交时，原评审任务的审核者会收到通知邮件，提醒有新的补丁集等待评审，如图 32-26。

![../_images/gerrit-review-8-2-review-new-patchset-mail-notify.png](img/gerrit-review-8-2-review-new-patchset-mail-notify.png)

图 32-26：Gerrit 通知邮件：新补丁集

登录 Gerrit 的 Web 界面，可以看到评审任务 1 有了新的补丁集，如图 32-27。

![../_images/gerrit-review-8-review-new-patchset.png](img/gerrit-review-8-review-new-patchset.png)

图 32-27：Gerrit 新补丁集显示

再经过代码审核和测试，这次`dev2`用户决定让评审通过，点击了“Publish and Submit”按钮。Submit（提交）动作会将评审任务（refs/changes/01/1/2）合并到对应分支（master）。图 32-28 显示的是通过评审完成合并的评审任务 1。

![../_images/gerrit-review-9-review-patchset-merged.png](img/gerrit-review-9-review-patchset-merged.png)

图 32-28：Gerrit 合并后的评审任务

### 5.6.10.7\. 从远程版本库更新

当`dev1`和`dev2`用户完成代码评审，提交者会收到多封通知邮件。这其中最让人激动的就是代码被接受并合并到开发主线（master）中（如图 32-29），这是多么另开发者感到荣耀的啊。

![../_images/gerrit-review-10-review-merged-mail-notify.png](img/gerrit-review-10-review-merged-mail-notify.png)

图 32-29：Gerrit 通知邮件：修订已合并

代码提交者执行**git pull**，和 Gerrit 管理的版本库同步。

```
$ git ls-remote origin
1df9e8e05fcf97a46588488918a476abd1df8121        HEAD
c65ab490f6d3dc36429b8f1363b6191357202f2e        refs/changes/01/1/1
1df9e8e05fcf97a46588488918a476abd1df8121        refs/changes/01/1/2
1df9e8e05fcf97a46588488918a476abd1df8121        refs/heads/master

$ git pull
From ssh://localhost:29418/hello
   82c8fc3..1df9e8e  master     -> origin/master
Already up-to-date.

```

## 5.6.11\. 更多 Gerrit 参考

Gerrit 涉及到的内容非常庞杂，还有诸如和 Gitweb、git-daemon 整合，Gerrit 界面定制等功能，恕不在此一一列举。可以直接参考 Gerrit 网站上的帮助。

参见：[`gerrit.googlecode.com/svn/documentation/`](http://gerrit.googlecode.com/svn/documentation/)。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 5.7\. Git 版本库托管

想不想在互联网上为自己的 Git 版本库建立一个克隆？这样再也不必为数据的安全担忧（异地备份），还可以和他人数据共享、协同工作？但是这样做会不会很贵呢？比如要购买域名、虚拟主机、搭建 Git 服务器什么的？

实际上这种服务（Git 版本库托管服务）可以免费获得！GitHub、Gitorious、Bitbucket 等都可以免费提供这些服务。

## 5.7.1\. Github

如果读者是按部就班式的阅读本书，那么可能早就注意到本书的很多示例版本库都是放在 GitHub 上的。GitHub 提供了 Git 版本库托管服务，即包括收费商业支持，也提供免费的服务，很多开源项目把版本库直接放在了 GitHub 上，如：jQuery、curl、Ruby on Rails 等。

注册一个 GitHub 帐号非常简单，访问 GitHub 网站：`https://github.com/`，点击菜单中的“Pricing and Signup”就可以看到 GitHub 的服务列表（如图 33-1）。会看到其中有一个免费的服务：“Free for open source” ，并且版本库托管的数量不受限制。当然免费的午餐是不管饱的，托管的空间只有 300MB，而且不能创建私有版本库。

![../_images/github-plans.png](img/github-plans.png)

图 33-1：GitHub 服务价目表

点击按钮“Create a free account”，就可以创建一个免费的帐号。GitHub 的用法和前面介绍的 Gerrit Web 界面的用法很类似，一旦帐号创建，应该马上为新建立的帐号设置公钥，以便能够用 SSH 协议读写自己帐号下创建的版本库，如图 33-2。

![../_images/github-whoru.png](img/github-whoru.png)

图 33-2：GitHub 上配置公钥

创建仓库的操作非常简单，首先点击菜单上的“主页”（Dashboard），再点击右侧边栏上的“新建仓库”按钮就可以创建新的版本库了，如图 33-3。

![../_images/github-create-repo.png](img/github-create-repo.png)

图 33-3：GitHub 页面上的新建版本库按钮

新建版本库会浪费本来就不多的托管空间，从 GitHub 上已有版本库派生（fork）一个克隆是一个好办法。首先通过 GitHub 搜索版本库名称，找到后点击“派生”按钮，就可以在自己的托管空间内建立相应版本库的克隆，如图 33-4。

![../_images/github-fork-repo.png](img/github-fork-repo.png)

图 33-4：自 GitHub 上的版本库派生

版本库建立后就可以用 Git 命令访问 GitHub 上托管的版本库了。GitHub 提供三种协议可供访问，如图 33-5。其中 SSH 协议和 HTTP 协议支持读写，Git-daemon 提供只读访问。对于自己的版本库当然选择支持读写的服务方式了，其中 SSH 协议是首选。

![../_images/github-repo-url.png](img/github-repo-url.png)

图 33-5：版本库访问 URL

## 5.7.2\. Gitorious

Gitorious 是另外一个 Git 版本库托管提供商，网址为`http://gitorious.org/`。最酷的是 Gitorious 本身的架站软件也是开源的，可以通过 Gitorious 上的 Gitorious 项目访问。如果读者熟悉 Ruby on Rails，可以架设一个本地的 Gitorious 服务。

![../_images/gitorious_full.png](img/gitorious_full.png)

图 33-6：Gitorious 上的 Gitorious 项目

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.