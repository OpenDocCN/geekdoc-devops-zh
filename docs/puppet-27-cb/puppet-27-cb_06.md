# Puppet 基础设施

## Puppet 基础设施

> Computers in the future may have as few as 1,000 vacuum tubes and weigh only 1.5 tons.
> 
> — Popular Mechanics (1949)

在本章中，我们将学习如下内容：

*   使用版本控制

*   使用提交钩子

*   使用 Rake 部署变更

*   配置 Puppet 的文件服务器

*   从 cron 运行 Puppet

*   使用自动签名

*   预签名证书

*   从 Puppet 的 filebucket 检索文件

*   使用 Passenger 扩展 Puppet 的部署规模

*   创建去中心化的分布式 Puppet 架构

本书中的一些处方体现了 Puppet 社区约定的最佳实践。 其他的一些处方是一些技巧和窍门，这些技巧使你使用 Puppet 工作变得更容易， 或向你介绍一些先前还不知道的功能。 其中的一些处方仅仅是轻量级的捷径方法，我不建议你将其作为生产环境的标准操作过程， 但在紧急情况下可能还是有用的。 最后，这里还提供了一些实验性的处方，你可能想尝试一下， 但这些处方仅适用于大型基础设施或其他不寻常的场合。

我希望你通过阅读和学习这里提供的处方，能获得有关 Puppet 如何工作以及你该如何使用 Puppet 的更深入、更广泛的理解， 从而帮助你建立更完善的基础设施。 只有你自己才能决定一个特定的处方对你或你的组织是否适用， 但我希望这里提供的处方能激励你进行实验，找出更多更重要的处方?—?从中获得使用 Puppet 的乐趣！

> ![提示](img/tip.png)
> 
> 纵观这本书的所有例子，你会看到大多数的命令以 root 用户运行。 如果你更愿意使用一个普通用户账号和 sudo 管理系统， 请使用这种方式替代 root 用户的操作。

因为不同 Linux 发行版（例如：Ubuntu、Red Hat、CentOS）在具体细节上会有不同，比如： 软件包名、配置文件的路径，等等。为了节省篇幅和提高清晰度的原因， 我决定为本书挑选一种发行版（Ubuntu 10.04 Lucid）并坚持使用它。 然而，Puppet 几乎可以在所有 Linux 发行版上运行，所以在你偏爱的操作系统和发行版上应该很少遇到麻烦， 仅需要对处方做适当调整即可。

在这本书写作时，Puppet 2.7 是最新的稳定版本，因此我选择了它作为本书使用的 Puppet 的参考版本。 然而，Puppet 命令的语法每隔一段时间可能就会改变， 所以要注意，旧版本的 Puppet 仍然是非常有用的， 旧版本的 Puppet 可能不支持这本书中所描述的所有功能和语法。

# 使用版本控制

### 使用版本控制

> Unix was not designed to stop you from doing stupid things, because that would also stop you from doing clever things.
> 
> — Doug Gwyn

你曾经遇到过误删除了某些文件而又希望恢复的情形吧？ 本书中提及的最重要的技巧就是将 Puppet 的 配置清单（manifests）纳入像 Git 或 Subversion 这样的 **版本控制系统（Version Control System，VCS）**。 直接在 **Puppetmaster** 上编辑配置清单并非明智之举，因为在你还没确信应用这些更改之前可能已经被应用。 Puppet 会自动检测配置清单的变化，因此可能会将配置清单的半成品应用到客户端。 这可是个令人讨厌的结果。

取而代之的正确做法是：使用 VCS （我推荐 Git）并从一个版本仓库导出（checkout） Puppetmaster 上所需的 /etc/puppet 目录内容。这样做有如下好处：

*   你不用担心 Puppet 会应用未完成的配置清单

*   你可以撤消更改，并将配置清单恢复到以前的任何版本

*   你可以使用分支（branch）来尝试使用新功能，而不会影响到生产线上使用的主版本（master version）

*   如果多个人需要修改配置清单，他们可以彼此独立的工作，修改他们自己的工作副本，之后合并（merge）他们的改动

*   你可以使用日志来查看何时，何人改动了什么

#### 准备工作

你需要一个运行 Puppetmaster 的主机并且配置清单文件集存放在 /etc/puppet 目录。 如果你还没有准备好这些，可以参考 Puppet 文档（[`docs.puppetlabs.com/`](http://docs.puppetlabs.com/)）： 如何安装 Puppet（[`docs.puppetlabs.com/guides/installation.html`](http://docs.puppetlabs.com/guides/installation.html)）以及如何建立自己的第一个配置清单 （[`docs.puppetlabs.com/guides/setting_up.html`](http://docs.puppetlabs.com/guides/setting_up.html)）。

然后将你的配置清单纳入版本控制，可以从 Pupppetmaster 上的 /etc/puppet 目录导入到版本控制系统中, 并使用它作为工作副本。在本例中，我们将使用一个 **GitHub** 账号存储 Puppet 的所有配置。

你需要一个 GitHub 账号（可以免费注册）和并创建一个仓库。跟随 [www.github.com](http://www.github.com) 的指示去创建一个吧。

> ![注记](img/note.png) 译者注
> 
> 可以参考 [蒋鑫](http://www.worldhello.net/) 撰写的 [GotGitHub](http://www.worldhello.net/gotgithub/) 学习 GitHub 的使用。
> 
> ![提示](img/tip.png)
> 
> 你可以使用你的账号从 [`www.PacktPub.com`](http://www.PacktPub.com) 下载你购买的 Packt 出版的所有书籍的案例代码文件。 如果你从其他地方购买了本书，你可以访问 [`www.PacktPub.com/support`](http://www.PacktPub.com/support) ， 注册并直接从 e-mail 获得案例代码文件。

#### 操作步骤

1.  将 Puppetmaster 上的 /etc/puppet 目录纳入一个 Git 仓库，执行如下命令:

    ```
    root@cookbook:/etc/puppet# git init
    Initialized empty Git repository in /etc/puppet/.git/
    root@cookbook:/etc/puppet# git add manifests/ modules/
    root@cookbook:/etc/puppet# git commit -m "initial commit"
    [master (root-commit) c7a24cf] initial commit
     59 files changed, 1790 insertions(+), 0 deletions(-)
     create mode 100644 manifests/site.pp
     create mode 100644 manifests/utils.pp
    ... 
    ```

2.  关联你的 GitHub 仓库并执行推送（push）:

    ```
    # git push -u origin master
    Counting objects: 91, done.
    Compressing objects: 100% (69/69), done.
    Writing objects: 100% (91/91), 21.07 KiB, done.
    Total 91 (delta 4), reused 0 (delta 0)
    To git@github.com:bitfield/puppet-demo.git
     * [new branch] master -&gt; master 
    ```

    分支 master 设置为从 origin 跟踪远程分支 masters。

#### 工作原理

你已经在 GitHub 上创建了一个 “master” **仓库（repository）**（通常简写为 **repo**）， 它包含了你的 Puppet 配置清单。 你可以在不同的地方导出多个副本，提交变更之前在这些副本上工作。 例如, 如果你有一个系统管理员的团队，他们每个人都可以在他们自己的本地仓库副本上工作。

Puppetmaster 上的 /etc/puppet 目录仅是从属于 GitHub 仓库的另外一个工作副本。 当你决定要在 /etc/puppet 的工作副本上应用 GitHub 仓库上的变更时， 你可以更新这个本地副本，从 GitHub 仓库上获取（pull）最近的变更。

#### 更多用法

既然已经配置了版本控制，你就可以使用如下的工作流程编辑你的 Puppet 配置清单了：

1.  使用自己偏爱的文本编辑器在你自己的工作副本上修改 Puppet 配置清单。

    例如：在我的笔记本电脑上自己的工作副本上添加新的配置清单文件（或做一些编辑工作）：

    ```
    john@laptop:~$ cd puppet-work
    john@laptop:~/puppet-work$ mkdir manifests
    john@laptop:~/puppet-work$ touch manifests/nodes.pp
    john@laptop:~/puppet-work$ git add manifests/nodes.pp 
    ```

2.  提交（commit）变更并推送（push）变更至 GitHub 仓库。

    ```
    john@laptop:~/puppet-work$ git commit -m "adding nodes.pp"
    [master 5c7b94c] adding nodes.pp
     0 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 manifests/nodes.pp
    john@laptop:~/puppet-work$ git push
    Counting objects: 7, done.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 409 bytes, done.
    Total 4 (delta 1), reused 0 (delta 0)
    To git@github.com:bitfield/puppet-demo.git
        c7a24cf..b74d452 master -&gt; master 
    ```

3.  使用 git pull 从 Github 仓库更新 Puppetmaster 上的工作副本。

    ```
    root@cookbook:/etc/puppet# git pull
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 4 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From git@github.com:bitfield/puppet-demo.git
        26d668c..5c7b94c master -&gt; origin/master
    Updating 26d668c..5c7b94c
    Fast-forward
    0 files changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 manifests/nodes.pp 
    ```

你可以使用像 **Rake** 这样的工具实现这一过程的自动化。

#### 参见本书

*   本章的 使用 Rake 部署变更 一节

*   本章的 创建去中心化的分布式 Puppet 架构 一节

*   本章的 使用提交钩子 一节

# 使用提交钩子

### 使用提交钩子

如果我们在提交配置清单之前能够发现其中的语法错误将会是个好消息。 检查 Puppet 配置清单的语法可以使用 puppet parser validate 命令：

```
# puppet parser validate /etc/puppet/manifests/site.pp
err: Could not parse for environment production: Syntax error at end of
file at /etc/puppet/manifests/site.pp:3 
```

这种语法检查是特别有用的，因为配置清单中的任何一个错误将使运行 Puppet 的节点停止工作， 甚至不会应用配置清单中错误代码的节点也会停止工作。

因此，导入（check in）一个有错误的配置清单，将导致 Puppet 应用这些更新到生产环境使其停止运行， 直到问题被发现，这可能会产生严重的后果。

避免错误发生的最好方法是在你的版本控制仓库中使用 **pre-commit hook** 自动执行语法检查。

#### 操作步骤

如果你使用的是 Git 版本控制系统，可以添加一个脚本 .git/hooks/pre-commit 用于为你提交的所有配置清单文件执行语法检查。 下面的脚本示例来自 Puppet Labs wiki: [`projects.puppetlabs.com/projects/puppet/wiki/`](http://projects.puppetlabs.com/projects/puppet/wiki/) 。

```
#!/bin/sh

syntax_errors=0
error_msg=$(mktemp /tmp/error_msg.XXXXXX)

if git rev-parse --quiet --verify HEAD > /dev/null
then
  against=HEAD
else
    # Initial commit: diff against an empty tree object
    against=4b825dc642cb6eb9a060e54bf8d69288fbee4904
fi

# Get list of new/modified manifest and template files to check (in git index)
for indexfile in `git diff-index --diff-filter=AM --name-only --cached \
  $against | egrep '\.(pp|erb)'`
do
    # Don't check empty files
    if [ `git cat-file -s :0:$indexfile` -gt 0 ]
    then
 case $indexfile in
            *.pp )
                # Check puppet manifest syntax
                #git cat-file blob :0:$indexfile | puppet \
                # --color=false --parseonly --ignoreimport > $error_msg
                # Updated for 2.7.x
                puppet parser validate $indexfile > $error_msg
                ;;
            *.erb )
                # Check ERB template syntax
                # -P : ignore lines which start with "%"
                git cat-file blob :0:$indexfile | erb -P -x -T - |\
                  ruby -c 2> $error_msg > /dev/null
                ;;
        esac
 if [ "$?" -ne 0 ]
        then
  echo -n "$indexfile: "
            cat $error_msg
            syntax_errors=`expr $syntax_errors + 1`
        fi
 fi
done

rm -f $error_msg

if [ "$syntax_errors" -ne 0 ]
then
  echo "Error: $syntax_errors syntax errors found, aborting commit."
    exit 1
fi 
```

#### 工作原理

这个提交钩子脚本 .git/hooks/pre-commit 会防止你提交任何有语法错误的配置清单文件：

```
# git commit -m "spot the deliberate mistake" manifests/site.pp
err: Could not parse for environment production: Syntax error at end of
file; expected '}' at /etc/puppet/manifests/site.pp:3
manifests/site.pp: Error: 1 syntax errors found, aborting commit. 
```

#### 更多用法

你可以在 Puppet Labs wiki: [`projects.puppetlabs.com/projects/1/wiki/Puppet_Version_Control`](http://projects.puppetlabs.com/projects/1/wiki/Puppet_Version_Control) 上找到有关这个脚本的更多详细信息。

你也可以使用类似的 update 钩子防止有语法错误的配置清单推送到 Puppetmaster： 查看同样的 wiki 页面也可以获得使用 update 钩子的详细信息。

#### 参见本书

*   本章的 使用版本控制 一节

# 使用 Rake 部署变更

### 使用 Rake 部署变更

每个人的生活都离不开键盘，但我讨厌不必要的敲打。 如果你按照 使用版本控制 一节所描述的工作流程工作， 可以添加一些自动化任务使这个处理过程更加容易。 有许多工具可以帮助我们在远程机器上执行命令，包括 Capistrano（[`github.com/capistrano/capistrano`](https://github.com/capistrano/capistrano)）和 Fabric（[`github.com/fabric/fabric`](https://github.com/fabric/fabric)），但在本例中，我们将使用 Rake（[`rake.rubyforge.org/`](http://rake.rubyforge.org/)）。

#### 准备工作

若你还没有安装 Rake， 运行如下的命令：

```
apt-get install rake 
```

执行这个命令，你需要一个 Internet 连接。

#### 操作步骤

1.  在你的 Puppet 配置清单工作副本的顶级目录下创建一个名为 Rakefile 的文件，例如：

    ```
    john@laptop:~/puppet-work$ vi Rakefile 
    ```

    文件内容看上去像这样：

    ```
    PUPPETMASTER = 'cookbook'
    SSH = 'ssh -t -A'

    task :deploy do
        sh "git push"
        sh "#{SSH} #{PUPPETMASTER} 'cd /etc/puppet && sudo git pull'"
    end 
    ```

2.  当你在本地副本上应用 Puppet 配置清单的改动时，可以简单地运行命令：

    ```
    $ rake deploy 
    ```

3.  Rake 会更新远程 Git 仓库并刷新 Puppetmaster 的工作目录副本：

    ```
    $ git push
    Counting objects: 4, done.
    Delta compression using 2 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 452 bytes, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To ssh:/ /git@cookbook.bitfieldconsulting.com/var/git/cookbook
        561e5a6..a8b8c76 master -&gt; master
    ssh -A -l root cookbook 'cd /etc/puppet && git pull'
    From ssh://cookbook.bitfieldconsulting.com/var/git/cookbook
        561e5a6..a8b8c76 master -&gt; origin/master
    Updating 561e5a6..a8b8c76
    Fast-forward
     Rakefile | 6 ++++++
     1 files changed, 6 insertions(+), 0 deletions(-)
     create mode 100644 Rakefile 
    ```

4.  你还可以添加一个 Rake 任务用于在客户端上运行 Puppet：

    ```
    task :apply =&gt; [:deploy] do
        client = ENV['CLIENT']
        sh "#{SSH} #{client} 'sudo puppet agent --test'" do |ok,
        status|
            puts case status.exitstatus
                when 0 then "Client is up to date."
                when 1 then "Puppet couldn't compile the manifest."
                when 2 then "Puppet made changes."
                when 4 then "Puppet found errors."
            end
        end
    end 
    ```

5.  当你要在指定客户端上测试你所做的变更时，可以运行如下的命令：

    ```
    rake CLIENT=cookbook apply 
    ```

    替换 cookbook 为你客户端的名字，或设置 CLIENT 环境变量，使 Rake 知道你要在哪个客户机上运行 Puppet。

    ```
    info: Caching catalog for cookbook
    info: Applying configuration version '1292865016'
    info: Creating state file /var/lib/puppet/state/state.yaml
    notice: Finished catalog run in 0.03 seconds 
    ```

6.  如果你只想查看 Puppet 将会做些什么，而不是实际应用这些变更， 你可以使用 --noop 标志：

    ```
    task :noop =&gt; [:deploy] do
        client = ENV['CLIENT']
        sh "#{SSH} #{client} 'sudo puppet agent --test --noop'"
    end 
    ```

7.  现在你可以运行：

    ```
    $ rake noop 
    ```

    这将会显示一个变更预览。

#### 工作原理

一个 Rakefile 文件由若干任务组成，任务由关键字 task 来标识。 任务定义了一系列的操作步骤，在本例中，使用一系列的 shell 命令推送你的配置清单到主版本仓库， 然后更新 Puppetmaster 上的工作副本。

任务可以互相引用，因为一个任务可能会依赖其它任务。例如，在我们的 Rakefile 中， apply 任务引用了 deploy 任务，每当你运行 rake apply ，Rake 会先确保完成 deploy 任务，然后再执行 apply 任务。

#### 更多用法

你可以扩展这个 Rakefile 实现更多的自动化任务，包括在更新 Puppet 配置清单之前运行语法检查, 甚至可以在引导启动过程中使用 Puppet 来初始化你的新机器。 Rake 是一个功能强大的工具，在使用 Puppet 管理大型网络时会为我们提供很大的帮助。

#### 参见本书

*   本章的 使用版本控制 一节

*   本章的 创建去中心化的分布式 Puppet 架构 一节

*   本章的 使用提交钩子 一节

# 配置 Puppet 的文件服务器

### 配置 Puppet 的文件服务器

部署配置文件是 Puppet 最常见的用途之一。许多服务都需要一些配置文件， 你可以让 Puppet 使用 file 资源将这些配置文件推送到客户端，如下面的代码所示：

```
file { "/opt/nginx/conf.d/app_production.conf":
    source => "puppet:///modules/app/app_production.conf",
} 
```

source 参数是这样约定的： puppet:/// 之后的第一部分假定是一个 **挂装点（mount point）** 名称，其余部份被视为一个文件路径，如下所示：

```
puppet:///<mount point>/<path> 
```

通常 <mount point> 的值是一个模块名称，如上例所示。在这个例子中， Puppet 将在如下的位置查找文件：

```
manifests/modules/app/files/app_production.conf 
```

modules 是 Puppet 予以特别对待的一个挂装点：它期望接下来的路径组成是一个模块名， 并在模块的 files 目录下针对路径的其余部分寻找文件。

然而 Puppet 也允许你创建自定义的挂装点，你可以为自定义的挂装点设置个别的访问控制， 并将其映射到 Puppetmaster 的不同文件系统位置。 在本节中，我们将展示如何创建和配置这些自定义的挂载点。

#### 操作步骤

1.  在 PuppetMaster 的 fileserver.conf 中添加新的一节，将挂装点的名称用方括号括起， path 的值就是 Puppet 将会寻找数据的目录路径，如下所示：

    ```
    [san]
        path /mnt/san/mydata/puppet 
    ```

2.  在你的配置清单里，使用 source 指定你的挂载点名称，如下所示：

    ```
    source =&gt; "puppet:///san/admin/users.htpasswd", 
    ```

    Puppet 会将其转换为如下的路径：

    ```
    /mnt/san/mydata/puppet/admin/users.htpasswd 
    ```

    像这样创建一个自定义挂载点的主要原因就是提高安全性。 例如，你有个秘密的口令文件只需部署到 web 服务器，而其它机器则不需要。 如果有人能够在任何机器上运行 Puppet，并且有合法的证书访问 Puppetmaster， 那么没有人能阻止他像这样执行下面的配置清单：

    ```
    file { "/home/cracker/goodstuff/passwords.txt":
        source =&gt; "puppet:///web/passwords.txt",
    } 
    ```

    他们可以轻而易举地获取秘密数据。事实上，可以导出 Puppet 仓库的任何人以及 在 Puppetmaster 上有账户的任何人都可以访问此文件。 为了避免这种情况发生的方法之一就是将秘密数据放在自定义挂载点并启用访问控制。

3.  在 fileserver.conf 中添加 allow 和 deny 参数来定义你的挂载点，如下所示：

    ```
    [secret]
        /data/secret
        allow web.example.com
        deny * 
    ```

#### 工作原理

在本例中, 仅允许 web.example.com 访问此文件。 默认的策略是拒绝所有的访问， 因此 deny * 这行是可选的，但它确实是个好的习惯，因为看上去更清晰。 之后 web 服务器就可以使用 file 资源了，如下所示：

```
file { "/etc/passwords.txt":
    source => "puppet:///secret/passwords.txt",
} 
```

如果此配置清单是在 web.example.com 上执行，将会正常工作； 若在其它客户端上执行则执行失败。

#### 更多用法

你也可以将指定的 IP 地址替换为主机名，也可使用 **无类型域间路由(CIDR)** 或使用通配符（wildcard）来表示一组地址，如下所示：

```
allow 10.0.55.0/24
allow 192.168.0.* 
```

#### 参见本书

*   第三章的 使用模块 一节

*   第六章的 使用文件资源递归地分发整个目录树 一节

*   第六章的 为文件资源指定多个源 一节

# 从 cron 运行 Puppet

### 从 cron 运行 Puppet

你的 Puppet 工作在休眠状态吗？ 默认情况下，当你在客户端上运行 Puppet agent 时， 它会以守护进程（后台进程）的方式执行，每隔 30 分钟唤醒一次并检查配置清单是否有更新并应用这些变更 （也可以在 puppet.conf 中将 splay 选项的值设为 true 来指定一个随机的时间间隔）。 如果想要更灵活的控制 Puppet 的运行，你可以安排 cron 任务来替代守护进程的触发执行方式。

例如，如果你有很多的 Puppet 客户端，可能需要刻意地错开 Puppet 的运行时间以减轻 Puppetmaster 的负载压力。一个简单的方法是：将客户端主机名的哈希（散列）值作为 cron 任务的分钟或小时的时间参数。

#### 操作步骤

使用 Puppet 的 inline_template 函数，它允许你执行 Ruby 代码：

```
cron { "run-puppet":
    command => "/usr/sbin/puppet agent --test",
    minute  => inline_template("<%= hostname.hash.abs % 60 %>"),
}
service { "puppet":
    ensure => stopped,
    enable => false,
} 
```

#### 工作原理

因为每个主机名会生成一个唯一的哈希值，所以每个客户端将会在一小时的不同分钟数运行 Puppet。 这种散列技术对于使 cron 任务随机运行是相当有用的，因为分散了运行时间，改善了互相干扰， 从而减轻了 Puppetmaster 的负载压力。

#### 更多用法

你可能会发现，随着时间的推移以守护进程方式运行的 Puppet 会占用更多的内存， 或者偶尔 Puppet 与 Puppetmaster 的通信处于停滞状态（stuck state）。 以 cron 方式运行 Puppet 会解决这些问题。

参考 Puppet Labs 的 Wiki 页面 [`projects.puppetlabs.com/projects/puppet/wiki/Cron_Patterns`](http://projects.puppetlabs.com/projects/puppet/wiki/Cron_Patterns) 获得更多信息。

触发 Puppet 的运行还有其他方式，包括 **MCollective** 工具， 我们将在这本书的后续章节中做详细介绍。

#### 参见本书

*   第六章的 有效地分发 cron 任务 一节

*   第三章的 使用嵌入式 Ruby 代码 一节

*   第九章的 使用 MCollective 一节

# 使用自动签名

### 使用自动签名

在密码学中，跟生活一样，在你签名时必须小心慎重。一般地，当你要为 Puppetmaster 介绍一个新的客户端加入时，需要先在客户端上生成一个证书请求（certificate request）， 然后到 Puppetmaster 上签署这个证书请求。 然而，你可以使用 **自动签名（autosigning）** 跳过这一步骤。

#### 操作步骤

在 Puppetmaster 上创建文件 /etc/puppet/autosign.conf ，包含如下内容：

```
*.example.com 
```

#### 工作原理

Puppet 会检查所有的证书请求是否匹配 autosign.conf 中任何一行。 任何能匹配客户端主机名 *.example.com 的证书请求，Puppetmaster 都会为其自动签名。

> ![重要](img/important.png)
> 
> 这里存在一个潜在的安全问题，因为这相当于 Puppetmaster 信任了任何可以连接到它的客户端 （只要主机名匹配）。基于这种原因，不推荐你使用自动签名。 如果你确信要使用自动签名，请确保 Puppetmaster 被防火墙保护，且只允许信任的客户端或者 IP 地址段连接 Puppetmaster。更安全的做法是使用 **预签名（pre-signing）**。

#### 参见本书

*   本章的 预签名证书 一节

# 预签名证书

### 预签名证书

因为存在安全隐患，如果你能辅助签名过程，最好避免使用自动签名。 在一般情况下，如果你想自动加入大量的客户端，最好在 Puppetmaster 上预先生成证书，然后将其作为构建过程的一部分推送给客户端。 你可以使用 puppet cert --generate <hostname> 命令生成预签名证书（pre-signed certificate）。

#### 操作步骤

1.  使用如下命令为 client1.example.com 生成预签名证书：

    ```
    puppet cert --generate client1.example.com 
    ```

    Puppet 现在将为客户端 client1.example.com 生成并签署客户端证书。

2.  传输三个所需的文件到新的客户端：包括客户端私钥、客户端证书和 CA 证书。 这三个文件位于：

    ```
    /etc/puppet/ssl/private_keys/client1.example.com.pem
    /etc/puppet/ssl/certs/client1.example.com.pem
    /etc/puppet/ssl/certs/ca.pem 
    ```

    复制上述三个文件到客户端相应的目录下，Puppet 会自动进行身份验证从而省略证书请求这一步骤。 值得注意的是 Puppet 的 SSL 证书的位置依赖于 puppet.conf 中的 ssldir 设置。

#### 参见本书

*   本章的 使用自动签名 一节

# 从 Puppet 的 filebucket 检索文件

### 从 Puppet 的 filebucket 检索文件

> A Freudian slip is when you say one thing, but mean your mother.
> 
> — Anon

我们每个人都会犯错误，这就是为什么通常铅笔上会配有橡皮擦的原因。 每当 Puppet 客户端在改变一个文件时，就会将改变前的版本做个备份。 如果在 Puppet 客户端上对一个已经存在的文件做修改，不管多小的改变， 我们都可以看到这一过程：

```
# puppet agent --test
info: Caching catalog for cookbook
info: Applying configuration version '1293459139'
--- /etc/sudoers 2010-12-27 07:12:20.421896753 -0700
+++ /tmp/puppet-file20101227-1927-13hjvy6-0 2010-12-27 07:13:21.645702932
-0700
@@ -12,7 +12,7 @@

 # User alias specification
-User_Alias SYSOPS = john
+User_Alias SYSOPS = john,bob

info: FileBucket adding /etc/sudoers as {md5}
c07d0aa2d43d58ea7b5c5307f532a0b1
info: /Stage[main]/Admin::Sudoers/File[/etc/sudoers]: Filebucketed /etc/
sudoers to puppet with sum c07d0aa2d43d58ea7b5c5307f532a0b1

notice: /Stage[main]/Admin::Sudoers/File[/etc/sudoers]/content: content
changed '{md5}c07d0aa2d43d58ea7b5c5307f532a0b1' to '{md5}0d218c16bd31206e
312c885884fa947d'

notice: Finished catalog run in 0.45 seconds 
```

我们感兴趣的是下面这行：

```
info: /Stage[main]/Admin::Sudoers/File[/etc/sudoers]: Filebucketed /etc/
sudoers to puppet with sum c07d0aa2d43d58ea7b5c5307f532a0b1 
```

Puppet 会根据文件内容创建一个 MD5 哈希，并使用它来创建一个 **filebucket** 路径， **filebucket** 的值是基于哈希的前几个字符的。 filebucket 用来保存 Puppet 替换下来的任何文件副本，它存放的默认位置是 /var/lib/puppet/clientbucket：

```
# ls /var/lib/puppet/clientbucket/c/0/7/d/0/a/a/2/
c07d0aa2d43d58ea7b5c5307f532a0b1
contents  paths 
```

正如你看到的，ls 命令列出了文件名。 你在 bucket 的存放位置会看到两个文件： contents 和 paths 。 contens 文件的内容即为原始文件，paths 文件的内容即为原始文件的路径。

如果你知道文件内容的哈希值（像你看到的上面的例子），可以很容易地找到该文件； 如果你不知道，那么通过对整个 filebucket 创建一个索引文件的表将会非常有用。

#### 操作步骤

1.  使用如下命令创建索引文件：

    ```
    # find /var/lib/puppet/clientbucket -name paths -execdir cat {}\; \
       -execdir pwd \; -execdir date -r {} +"%F %T" \; -exec echo \; \
       &gt; bucket.txt 
    ```

2.  在索引文件中查找你要寻找的文件：

    ```
    # cat bucket.txt
    /etc/sudoers
    /var/lib/puppet/clientbucket/c/0/7/d/0/a/a/2/
    c07d0aa2d43d58ea7b5c5307f532a0b1
    2010-12-27 07:13:21

    /etc/sudoers
    /var/lib/puppet/clientbucket/1/0/9/0/e/2/8/a/1090e28a70ebaae872c2e
    c78894f49eb
    2010-12-27 07:12:20 
    ```

3.  一旦你要恢复一个已知 bucket 路径的文件，只要复制该文件到原始文件名即可：

    ```
    # cp /var/lib/puppet/clientbucket/1/0/9/0/e/2/8/a/1090e28a70ebaae8
    72c2ec78894f49eb/contents /etc/sudoers 
    ```

#### 工作原理

上面的 find 命令会创建一份完整的 filebucket 文件列表清单， 显示原始文件的名称，bucket 的路径，以及修改日期 （在上例中你学习到了如何恢复文件到以前版本）， 一旦你知道 bucket 的路径，那么就可以复制文件到正确的位置。

#### 更多用法

你可以让 Puppet 在原始目录下创建备份文件，而不是在 filebucket。 为了做到这一点，只需要在配置清单中指定 backup 参数的值：

```
file { "/etc/sudoers":
    mode => "440",
    source => "puppet:///modules/admin/sudoers",
    backup => ".bak",
} 
```

现在，如果 Puppet 替换了旧文件，就会在原始路径下面创建一个扩展名为 .bak 的备份文件。 若希望 Puppet 对所有的文件执行这样的默认备份策略，可以在配置清单中使用如下代码：

```
File {
    backup => ".bak",
} 
```

要完全禁用备份，使用下面的代码：

```
 backup => false, 
```

# 使用 Passenger 扩展 Puppet 的部署规模

### 使用 Passenger 扩展 Puppet 的部署规模

如果你的 Puppet 基础设施开始出现依依呀呀的裂缝，罪魁祸首很可能出现在 Puppetmaster 的 Web 服务器上。 Puppet 携带了一个名为 **Webrick** 的简单 Web 服务器来处理客户端与 Puppetmaster 的连接。Webrick 确实不适合在生产环境下运行 Puppet； 超过几个客户请求处理之后就会使 Puppetmaster 一蹶不振，严重影响其性能。

有时会建议使用 **Mongrel** 替换 Webrick，因为 **Mongrel** 比 Webrick 有少许的性能提升，但不太明显。为了扩展 Puppetmaster 能服务于数百台服务器， 首选的方法是切换到高性能的 Web 服务器，如使用包含 **Passenger**（mod_rails） 扩展模块的 Apache。

Puppet 在 Passenger 下运行需要些必要的配置，你需要安装 Apache 和 Passenger，并添加一个合适的虚拟主机。下面的示例是基于 Ubuntu 10.4 的。 你可以在 Puppet Labs 的站点 [`projects.puppetlabs.com/projects/1/wiki/Using_Passenger`](http://projects.puppetlabs.com/projects/1/wiki/Using_Passenger) 上找到 Red Hat Linux、CentOS 以及其它发行版本的对应操作指示。

#### 准备工作

为了方便配置，需要有你要运行的 Puppet 的源代码包（**tarball**）， 因为它提供了用于配置 Passenger 的一些模板文件和配置片段。 例如，假如你要运行 Puppet 2.7.1，就要下载这个文件： [`puppetlabs.com/downloads/puppet/puppet-2.7.1.tar.gz`](http://puppetlabs.com/downloads/puppet/puppet-2.7.1.tar.gz) 。

若你使用不同的版本，请到 [`puppetlabs.com`](http://puppetlabs.com) 下载合适的版本。 下载之后使用如下命令解开源码包：

```
tar xzf puppet-2.7.1.tar.gz 
```

#### 操作步骤

1.  安装 Apache 和 Passenger, 及其所依赖的软件包：

    ```
    # apt-get install apache2 libapache2-mod-passenger rails
    librack-ruby libmysql-ruby
    # gem install rack 
    ```

2.  为 Passenger 查找 Puppet 的配置创建必要的目录：

    ```
    # mkdir -p /etc/puppet/rack
    # mkdir -p /etc/puppet/rack/public 
    ```

    这两个目录的属主为 root 且权限为 0755。

    ```
    # chown -R root:root /etc/puppet/rack
    # chmod -R 0755 /etc/puppet/rack 
    ```

3.  创建文件 config.ru ，此文件告知 Passenger 如何启动 Puppet 应用程序。 你可以使用 Puppet 发布中提供的示例文件：

    ```
    # cp /tmp/puppet-2.7.1/ext/rack/files/config.ru /etc/puppet/rack/
    # chown puppet /etc/puppet/rack/config.ru 
    ```

    对于 Puppet 2.7.1, 应该包含如下内容：

    ```
    # a config.ru, for use with every rack-compatible webserver.
    # SSL needs to be handled outside this, though.

    # if puppet is not in your RUBYLIB:
    # $:.unshift('/opt/puppet/lib')

    $0 = "master"

    # if you want debugging:
    # ARGV &lt;&lt; "--debug"

    ARGV &lt;&lt; "--rack"
    require 'puppet/application/master'
    # we're usually running inside a Rack::Builder.new {} block,
    # therefore we need to call run *here*.
    run Puppet::Application[:master].run 
    ```

4.  你现在需要在 Apache 上创建一个虚拟主机，使其监听正确的端口并向 Puppet 应用程序发送请求。同样地，你可以使用 Puppet 发布中提供的示例文件：

    ```
    # cp /tmp/puppet-2.7.1/ext/rack/files/apache2.conf \
      /etc/apache2/sites-available/puppetmasterd
    # a2ensite puppetmasterd 
    ```

    文件的内容如下所示：

    ```
    # you probably want to tune these settings
    PassengerHighPerformance on
    PassengerMaxPoolSize 12
    PassengerPoolIdleTime 1500
    # PassengerMaxRequests 1000
    PassengerStatThrottleRate 120
    RackAutoDetect Off
    RailsAutoDetect Off

    Listen 8140

    &lt;VirtualHost *:8140&gt;
      SSLEngine on
      SSLProtocol -ALL +SSLv3 +TLSv1
      SSLCipherSuite ALL:!ADH:RC4+RSA:+HIGH:+MEDIUM:-LOW:-SSLv2:-EXP

      SSLCertificateFile /etc/puppet/ssl/certs/cookbook.
      bitfieldconsulting.com.pem
      SSLCertificateKeyFile /etc/puppet/ssl/private_keys/cookbook.
      bitfieldconsulting.com.pem
      SSLCertificateChainFile /etc/puppet/ssl/ca/ca_crt.pem
      SSLCACertificateFile /etc/puppet/ssl/ca/ca_crt.pem
      # If Apache complains about invalid signatures on the CRL, you
      # can try disabling
      # CRL checking by commenting the next line, but this is not
      # recommended.
      SSLCARevocationFile /etc/puppet/ssl/ca/ca_crl.pem
      SSLVerifyClient optional
      SSLVerifyDepth 1
      SSLOptions +StdEnvVars

      DocumentRoot /etc/puppet/rack/public/
      RackBaseURI /
      &lt;Directory /etc/puppet/rack/&gt;
        Options None
        AllowOverride None
        Order allow,deny
        allow from all
      &lt;/Directory&gt;
    &lt;/VirtualHost&gt; 
    ```

5.  编辑这个文件，将 SSLCertificateFile 和 SSLCertificateKeyFile 的值修改为你自己的证书（最简单地生成证书的方法是你已经运行 Puppet 至少一次）。

6.  你还要在 Apache 中启用 Passenger 和 mod_ssl 模块：

    ```
    # a2enmod passenger ssl 
    ```

7.  添加如下的行到你的 /etc/puppet/puppet.conf 文件：

    ```
    ssl_client_header = SSL_CLIENT_S_DN
    ssl_client_verify_header = SSL_CLIENT_VERIFY 
    ```

8.  停止正在运行的 Puppetmaster。

9.  启动 Apache：

    ```
    # /etc/init.d/apache2 restart 
    ```

10.  如果一切工作正常，你可以像往常一样运行 Puppet：

    ```
    # puppet agent --test
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1294145142'
    notice: Finished catalog run in 0.25 seconds 
    ```

#### 工作原理

Puppet 内置的 web 服务器处理速度相当慢，每次只能处理一个请求，使用 Apache 替换它之后，现在你就可以使用这个高性能多线程的 web 服务器了。 在这种情况下，Puppet 是一个使用 Rack 框架（Rack framework） 的嵌入式应用程序， 这大大提高了运行效率。你应该会发现，使用 "Apache + Passenger" 的配置能处理更多的客户端和更频繁的 Puppet 请求，并且改善了服务器内存的占用， 使用的内存比标准的 Puppetmaster 守护进程占用的内存更少。

#### 更多用法

下面是一个 Puppet 配置清单的示例，用于为你实现上面的处理步骤（基于 Ubuntu 系统）：

```
class puppet::passenger {
    package { [ "apache2-mpm-worker",
                "libapache2-mod-passenger",
                "librack-ruby",
                "libmysql-ruby" ]:
        ensure => installed,
    }

    service { "apache2":
        enable => true,
        ensure => running,
        require => Package["apache2-mpm-worker"],
    }

    package { "rack":
        provider => gem,
        ensure => installed,
    }

    file { [ "/etc/puppet/rack",
             "/etc/puppet/rack/public" ]:
        ensure => directory,
        mode => "755",
    }

    file { "/etc/puppet/rack/config.ru":
        source => "puppet:///modules/puppet/config.ru",
        owner => "puppet",
    }

    file { "/etc/apache2/sites-available/puppetmasterd":
        source => "puppet:///modules/puppet/puppetmasterd.conf",
    }

    file { "/etc/apache2/sites-enabled/puppetmasterd":
        ensure => symlink,
        target => "/etc/apache2/sites-available/puppetmasterd",
    }

    exec { "/usr/sbin/a2enmod ssl":
        creates => "/etc/apache2/mods-enabled/ssl.load",
    }
} 
```

一旦你以 Passenger 方式运行，就可以使用如下命令重新启动 Puppetmaster 应用程序：

```
# service apache2 restart 
```

为了监视 Passenger 正在运行，可以检查名为 ApplicationPoolServerExecutable 的进程。

你也可以用配置常规 web 应用的方法为 Passenger 实例配置负载均衡。

更多详细信息，或者如果你遇到问题，可以参考 **Puppet-on-Passenger** 文档： [`projects.puppetlabs.com/projects/1/wiki/Using_Passenger`](http://projects.puppetlabs.com/projects/1/wiki/Using_Passenger) 。

#### 参见本书

*   本章的 创建去中心化的分布式 Puppet 架构 一节

# 创建去中心化的分布式 Puppet 架构

### 创建去中心化的分布式 Puppet 架构

> I have the world’s largest collection of seashells. I keep it scattered around the beaches of the world… perhaps you’ve seen it.
> 
> — Steven Wright

某些系统（尤其像 Mafia）在去中心化的分布式架构环境下运行良好。 使用 Puppet 的一个最常见的方法是运行一个 Puppetmaster 服务器， Puppet 客户端连接这个服务器并从它获取配置清单。 无论如何，你仍旧可以使用下面的方法直接针对配置清单运行 puppet apply 命令。 （通常会使用 -v 参数开启冗余输出模式， 这样就可以看到详细的执行过程）：

```
# puppet apply -v manifest.pp
info: Applying configuration version '1294313350' 
```

你甚至可以直接在命令行上运行配置清单的代码片断：

```
# puppet apply -e "file { '/tmp/test': ensure => present }"
notice: /Stage[main]//File[/tmp/test]/ensure: created 
```

换言之，如果能安排合适的配置清单并分发到客户端，你就可以在 Puppet 客户端上直接执行它，而不必使用中心化的 Puppetmaster 服务器。 这将消除单台 Puppetmaster 服务器的性能瓶颈，也消除了单点故障。 与此同时，也避免了新增客户端时 SSL 证书签署以及证书交换的步骤。

将配置清单文件推送到客户端有多种实现方法， 显然 Git （或者其它版本控制系统，如 Mercurial 或 Subversion）能为你做绝大部分的工作。 你可以在本地编辑 Puppet 配置清单的本地副本，提交到 Git 并将更新推送到中心仓库， 然后从 Git 中心仓库自动将配置清单分发到客户机。

#### 准备工作

如果你的 Puppet 配置清单还没有纳入 Git，请参考本章 使用版本控制 一节的操作步骤。

#### 操作步骤

1.  在客户端使用如下命令对 Puppet 仓库克隆一个裸仓库（裸仓库没有工作区，只包含 .git 目录中的所有内容）：

    ```
    # git clone --bare ssh://git@repo.example.com/var/git/puppet 
    ```

2.  使用如下命令检出裸仓库内容到 /etc/puppet/ 目录：

    ```
    # git archive --format=tar HEAD | (cd /etc/puppet && tar xf -) 
    ```

3.  使用 Puppet 命令执行 site.pp 文件

    ```
    # puppet apply -v /etc/puppet/manifests/site.pp
    info: Applying configuration version '1294313353' 
    ```

    一旦完成上面的工作，下一步就是配置仓库的自动推送将变动推送到客户端。 使用 Git 的 remote 命令可以实现此功能，即配置本地仓库的远程仓库别名。例如：

    ```
    # git remote add web ssh://git@web1.example.com/etc/puppet 
    ```

    如果你有多个客户端， 可以为同一个远程别名添加多个 URL：

    ```
    # git remote set-url --add webs ssh://git@web2.example.com/etc/puppet
    # git remote set-url --add webs ssh://git@web3.example.com/etc/puppet
    ... 
    ```

    或者像这样，简单编辑 Git 配置文件 (.git/config) ：

    ```
    [remote "web"]
        url = ssh://git@web1.example.com/etc/puppet
        url = ssh://git@web2.example.com/etc/puppet
        url = ssh://git@web3.example.com/etc/puppet
        ... 
    ```

4.  现在你可以使用如下命令从 Git 中心仓库推送更新到任意一台（或一组）客户端：

    ```
    # git push web 
    ```

5.  最后一步，每当客户端接收到从 Git 中心仓库的推送就应该更新它自己的 /etc/puppet 目录。你可以使用 Git 的 postreceive 钩子来实现此功能。 在你的仓库中，创建一个名为 hooks/post-receive 的文件并为其设置可执行权限（0755）， 文件内容为：

    ```
    #!/bin/sh
    git archive --format=tar HEAD | (cd /etc/puppet && tar xf -) 
    ```

#### 工作原理

与连接 Puppetmaster 获取已经编译好的客户端配置清单不同， 使用这种方式每个客户端都要从本地副本编译自己的配置清单。 每当 Git 中心服务器推送一次更新（或者从 Git 仓库 checkout 检出）就会重新编译一次。 这大大高了网络带宽的利用率，因为客户端不必每次运行时都连接 Puppetmaster。 同时也消除了单点故障，因为客户端可以从任何分布式的 Git 仓库 （这些仓库的内容是由 Git 中心仓库自动推送的）获得更新。

使用基于 Git 的去中心化的分布式 Puppet 架构为你提供了一个非常灵活的处理方式。 你可以使用 SSH 密钥来配置访问控制，按需要允许每一个客户端或一组客户端的访问。 例如：用于数据库服务器的配置清单仅允许数据库组的机器访问获取。

当然还需要一些额外的配置工作，但对于大多数小型组织是没有必要的， 这种去中心化的 Puppet 部署方式提供了额外的灵活性，同时也适用于更为苛刻的权限控制环境。

#### 更多用法

如果你希望每次中心仓库推送更新之后 Puppet 立即执行并应用更新， 可以编辑 post-receive 脚本来完成，或者执行你需要的其它的动作。 另外，你也可以手动运行 Puppet，还可以用 从 cron 运行 Puppet 一节描述的方法使用 cron 调度运行，但要记得是运行 puppet apply ， 而不是运行 puppet agent 。

使用基于 Git 的架构也存在一些缺点：不能使用 Puppet 提供的高级特性， 例如外部节点分类器（external node classifier）以及存储配置（stored configuration）等。 不管怎样，当你需要将 Puppet 的部署扩展到大量节点时，这是一种最简单的实现方式。

你可以在 Stephen Nelson-Smith 的这篇文章里找到更多更详细的关于这种架构的讨论，文章地址是： [`bitfieldconsulting.com/scaling-puppet-with-distributedversion-control`](http://bitfieldconsulting.com/scaling-puppet-with-distributedversion-control) 。

#### 参见本书

*   本章的 使用 Passenger 扩展 Puppet 的部署规模 一节

*   本章的 使用版本控制 一节