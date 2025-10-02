# 使用文件和软件包

## 使用文件和软件包

> If builders built buildings the way programmers wrote programs, then the first woodpecker that came along would destroy civilization.
> 
> — Gerald Weinberg

在本章中，我们将学习如下内容：

*   为配置文件添加配置行

*   使用 Augeas 自动修改配置文件

*   使用配置片段构建配置文件

*   使用 ERB 模板

*   在模板中遍历数组

*   从第三方仓库安装软件包

*   配置 APT 软件仓库

*   配置 GEM 仓库

*   从源码包自动构建软件

*   比较软件包的版本

作为 Puppet 系统管理员，你所涉及的最多的管理工作就是文件和包。 文件和包是 Puppet 中最重要的两种资源，本章将帮助你深入了解它们， 并学习一些有用的功能和模式，以帮助你更好地使用它们。

在本章中，我们将看到：如何对文件做轻微地编辑； 如何利用 **Augeas** 工具以结构化的方式实现大规模的改变； 如何串联片段构建文件；以及如何从模板生成文件。 我们还将学习，如何从额外的仓库安装包，以及如何创建这些仓库。

# 为配置文件添加配置行

### 为配置文件添加配置行

你知道 Puppet 能做微创手术吗？ 通常，我们不希望将整个配置文件交由 Puppet 管理而仅仅是在配置文件中添加某项设置?—?尤其是如果该文件是由别人管理，我们不能覆盖它的情况。 一种简单而有用的方法是，如果配置文件中不存在指定的行就添加这行配置。例如： 添加一个内核模块名到配置文件 /etc/modules 告知内核在启动时加载此模块。

你可以使用一个 exec 资源达成此任务。下面的例子将展示如何使用 exec 向一个文本文件追加一行配置。

#### 操作步骤

1.  使用如下内容创建 /etc/puppet/manifests/utils.pp 文件：

    ```
    define append_if_no_such_line($file, $line) {
        exec { "/bin/echo '$line' &gt;&gt; '$file'":
            unless =&gt; "/bin/grep -Fx '$line' '$file'",
        }
    } 
    ```

2.  添加如下行到 /etc/puppet/manifests/site.pp 文件：

    ```
    import "utils.pp" 
    ```

3.  添加如下代码到你的配置清单：

    ```
    append_if_no_such_line { "enable-ip-conntrack":
        file =&gt; "/etc/modules",
        line =&gt; "ip_conntrack",
    } 
    ```

4.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1303649606'
    notice: /Stage[main]//Node[cookbook]/Append_if_no_such_
    line[enable-ip-conntrack]/Exec[/bin/echo 'ip_conntrack' &gt;&gt; '/etc/
    modules']/returns: executed successfully
    notice: Finished catalog run in 1.22 seconds 
    ```

#### 工作原理

exec 资源将追加指定的文本 $line 到文件 $file，除非它已经存在：

```
exec { "/bin/echo '$line' >> '$file'":
    unless => "/bin/grep -Fx '$line' '$file'", 
```

现在，append_if_no_such_line 资源已经可以在你的配置清单中使用了。 在本例中，我们已经使用它确保 /etc/modules 文件（此文件指定在启动过程中需要加载哪些内核模块）包含如下的行：

```
ip_conntrack 
```

#### 更多用法

类似的，你可以使用 define 函数对文本文件执行其他细小的操作。例如， 下面的代码片段可以让你在文本文件中执行查找和替换的操作：

```
define replace_matching_line( $match, $replace ) {
    exec { "/usr/bin/ruby -i -p -e 'sub(%r{$match}, \"$replace\")'
 $name":
      onlyif    => "/bin/grep -E '$match' $name",
      logoutput => on_failure,
    }
}

replace_matching_line { "/etc/apache2/apache2.conf":
    match   => "LogLevel .*",
    replace => "LogLevel debug",
} 
```

#### 参见本书

*   本章的 使用 Augeas 自动修改配置文件 一节

# 使用 Augeas 自动修改配置文件

### 使用 Augeas 自动修改配置文件

当然，有关标准的事物是如此之多。有时每个应用程序的配置格式都略有不同， 书写正则表达式来解析和修改所有这些配置文件是一项很烦人的工作。

幸好 Augeas 在这方面可以帮助我们。Augeas 是一个旨在简化使用不同配置文件格式工作的工具， 它将不同格式的配置文件统一呈现为一个简单的包含所有配置项的树状结构。 Puppet 的 Augeas 支持允许你创建 augeas 资源，它可以智能地自动地为所需的配置做相应的改变。

#### 准备工作

在使用 Augeas 之前需要先安装它。如下的 Puppet 代码将添加 Augeas 到你的配置。

1.  使用如下内容创建 /etc/puppet/modules/admin/manifests/augeas.pp 文件：

    ```
    class admin::augeas {
        package { [ "augeas-lenses",
                    "augeas-tools",
                    "libaugeas0",
                    "libaugeas-ruby1.8" ]:
            ensure =&gt; "present"
        }
    } 
    ```

2.  在一个节点上包含此类：

    ```
    node cookbook {
        include admin::augeas
    } 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1303657095'
    notice: /Stage[main]/Admin::Augeas/Package[augeas-tools]/ensure:
    ensure changed 'purged' to 'present'
    notice: Finished catalog run in 21.96 seconds 
    ```

#### 操作步骤

1.  使用如下内容创建 /etc/puppet/modules/admin/manifests/ipforward.pp 文件：

    ```
    class admin::ipforward {
        augeas { "enable-ip-forwarding":
            context =&gt; "/files/etc/sysctl.conf",
            changes =&gt; [
                "set net.ipv4.ip_forward 1",
            ],
        }
    } 
    ```

2.  在一个节点上包含此类：

    ```
    node cookbook {
        include admin::augeas
        include admin::ipforward
    } 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1303729376'
    notice: /Stage[main]/Admin::Ipforward/Augeas[enable-ipforwarding]/
    returns: executed successfully
    notice: Finished catalog run in 3.53 seconds 
    ```

4.  使用如下命令检查配置是否生效：

    ```
    # sysctl -p |grep forward
    net.ipv4.ip_forward = 1 
    ```

#### 工作原理

下面将描述前面的代码是如何工作的：

1.  我们声明了一个名为 enable-ip-forwarding 的 augeas 资源：

    ```
    augeas { "enable-ip-forwarding": 
    ```

2.  我们指定要实施改变的文件 /etc/sysctl.conf：

    ```
     context =&gt; "/files/etc/sysctl.conf", 
    ```

3.  将我们需要改变的设置作为一个数组（本例中只有一个数组元素）传递给 changes 参数：

    ```
     changes =&gt; [
            "set net.ipv4.ip_forward 1",
        ], 
    ```

    通常使用 Augeas 改变设置需用如下的格式：

    ```
    set &lt;parameter&gt; &lt;value&gt; 
    ```

    Augeas 使用一套名为 **lenses** 的转换文件，使其能够为给定的配置文件以适当的格式改写设置。 在本例中，此设置将转换为 /etc/sysctl.conf 文件中的如下一行：

    ```
    net.ipv4.ip_forward=1 
    ```

#### 更多用法

之所以使用 /etc/sysctl.conf 文件做例子，是因为它包含了各种各样的内核设置， 你可能想要在不同的 Puppet 类中因各种不同的目的而改变这些设置。 在前面的例子中，我们为一个路由器的类启用了 IP 转发， 然而你也可以为一个负载均衡的类调整 net.core.somaxconn 的值。

这意味着简单地对 /etc/sysctl.conf 文件“Puppet 化”（"Puppetizing"） 并分发它不能胜任所有的情况，因为你要修改的设置根据不同的服务需求可能会有不同， 从而引发版本冲突。Augeas 则是这种情况的正确解决方案，因为你可以在不同的位置定义 augeas 资源去修改相同的配置文件，这样就不会引起冲突。

Augeas 是个强大的工具，使用 lenses 可以转换绝大多数标准的 Linux 配置文件， 而且如果需要管理自己的专有格式的配置文件，你也可以书写自己的专有配置格式。 更多关于使用 Puppet 和 Augeas 的信息，请访问 Puppet Labs 的 wiki 页面： [`projects.puppetlabs.com/projects/1/wiki/Puppet_Augeas`](http://projects.puppetlabs.com/projects/1/wiki/Puppet_Augeas) 。

# 使用配置片段构建配置文件

### 使用配置片段构建配置文件

你怎能一口吃头大象呢？有时你会遇到这种情况，想要从被不同类管理的各种片段构建单一的配置文件。 例如，你可能有两到三个服务需要 rsync 模块配置，因此你不能只发布单一的配置文件 rsyncd.conf。 尽管你可以使用 Augeas，但是一种更简单的方法就是使用 exec 资源将若干配置片段串接成一个文件。

#### 操作步骤

1.  使用如下代码创建 /etc/puppet/modules/admin/manifests/rsyncdconf.pp 文件：

    ```
    class admin::rsyncdconf {
        file { "/etc/rsyncd.d":
            ensure =&gt; directory,
        }

        exec { "update-rsyncd.conf":
            command =&gt; "/bin/cat /etc/rsyncd.d/*.conf &gt; /etc/rsyncd.conf",
            refreshonly =&gt; true,
        }
    } 
    ```

2.  添加如下代码到你的配置清单：

    ```
    class myapp::rsync {
        include admin::rsyncdconf

        file { "/etc/rsyncd.d/myapp.conf":
            ensure  =&gt; present,
            source  =&gt; "puppet:///modules/myapp/myapp.rsync",
            require =&gt; File["/etc/rsyncd.d"],
            notify  =&gt; Exec["update-rsyncd.conf"],
        }
    }
    include myapp::rsync 
    ```

3.  使用如下内容创建 /etc/puppet/modules/myapp/files/myapp.rsync 文件：

    ```
    [myapp]
        uid = myappuser
        gid = myappuser
        path = /opt/myapp/shared/data
        comment = Data for myapp
        list = no
        read only = no
        auth users = myappuser 
    ```

4.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1303731804'
    notice: /Stage[main]/Admin::Rsyncdconf/File[/etc/rsyncd.d]/ensure:
    created
    notice: /Stage[main]/Myapp::Rsync/File[/etc/rsyncd.d/myapp.conf]/
    ensure: defined content as '{md5}e1e57cf38bb88a7b4f2fd6eb1ea2823a'
    info: /Stage[main]/Myapp::Rsync/File[/etc/rsyncd.d/myapp.conf]:
    Scheduling refresh of Exec[update-rsyncd.conf]
    notice: /Stage[main]/Admin::Rsyncdconf/Exec[update-rsyncd.conf]:
    Triggered 'refresh' from 1 events
    notice: Finished catalog run in 1.01 seconds 
    ```

#### 工作原理

admin::rsyncdconf 类为存放 rsync 的配置片段创建了如下目录：

```
file { "/etc/rsyncd.d":
    ensure => directory,
} 
```

当你创建一个配置片段（例如本例中的 myapp::rsync），你要做的所有工作就是 require 这个目录（require => File["/etc/rsyncd.d"]）并 notify 资源 exec 更新主配置文件 （notify => Exec["update-rsyncd.conf"],）。

只要 /etc/rsyncd.d 目录中有一个配置片段被更新，exec 资源就会执行一次：

```
exec { "update-rsyncd.conf":
    command     => "/bin/cat /etc/rsyncd.d/*.conf > /etc/rsyncd.conf",
    refreshonly => true,
} 
```

这样，/etc/rsyncd.d 目录中的所有配置片段将会串接成一个 rsyncd.conf 文件。

这是非常有用的，原因在于你可以有许多不同的片段资源散布在各个不同的类或模块中， 所有这些片段最终都将被合并成单一的 rsyncd.conf 文件，而你却可以在一个地方实现配置代码的合并。

#### 更多用法

当你有一个像 rsync 这样的服务且在一个配置文件中需要包含不同的片段时，这会是一种很有用的模式。 实际上，这为你提供了类似于 Apache 的 conf.d 目录或 PHP 的 php-ini.d 目录的功能。

#### 参见本书

*   第二章的 Using tags 一节

# 使用 ERB 模板

### 使用 ERB 模板

**模板**（**template**）是一种高级别的文本文件。它可以做计算、执行 Ruby 代码或者引用你在 Puppet 配置清单中定义的变量值。能被 Puppet 部署的任何一个文本文件，你都可以使用模板来代替。 最简单的情况，一个模板可以仅是一个静态文本文件。更有用的情况是，你可以使用 **ERB** （**嵌入式 Ruby**）语法在模板中插入变量。例如：

```
 <%= name %>, this is a very large drink. 
```

如果模板使用了变量 $name，其值为 Zaphod Beeblebrox，模板将解析为：

```
 Zaphod Beeblebrox, this is a very large drink. 
```

这种简单的技术非常有用，一种情形是要生成一批文件且它们仅有一两个变量的值不同 （例如虚拟主机的配置文件）；另一种情形是要向一个脚本中插入值（例如数据库名或口令）。 在下面的例子中，我们将使用一个 ERB 模板插入口令到一个备份脚本。

#### 操作步骤

1.  使用下面的内容创建 /etc/puppet/modules/admin/templates/backup-mysql.sh 文件：

    ```
    #!/bin/sh
    /usr/bin/mysqldump -uroot -p&lt;%= mysql_password %&gt; --all-databases \
    | /bin/gzip &gt; /backup/mysql/all-databases.sql.gz 
    ```

2.  添加如下代码到你的配置清单：

    ```
    $mysql_password = "secret"
    file { "/usr/local/bin/backup-mysql":
        content =&gt; template("admin/backup-mysql.sh"),
        mode    =&gt; "755",
    } 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1308670971'
    notice: /Stage[main]//Node[cookbook]/File[/usr/local/bin/backupmysql]/
    ensure: defined content as '{md5}5853b6d4dd72420e341fa7ecb8
    91ad43'
    notice: Finished catalog run in 0.96 seconds 
    ```

4.  检查 Puppet 是否已经在模板中正确地插入了口令：

    ```
    # cat /usr/local/bin/backup-mysql 
    ```

    ```
    #!/bin/sh
    /usr/bin/mysqldump -uroot -psecret --all-databases \
    | /bin/gzip &gt; /backup/mysql/all-databases.sql.gz 
    ```

#### 工作原理

在模板中，无论在哪里引用了变量（例如 <%= mysql_password %>）， Puppet 都会使用相应的值（例如 secret）替换它。

#### 更多用法

在本例中，我们仅在模板中使用了一个变量，但是只要你需要可以引用许多变量。 它们也可以是对 facts 的引用，例如：

```
ServerName <%= fqdn %> 
```

或者使用 Ruby 表达式，例如：

```
MAILTO=<%= emails.join(',') %> 
```

或者你要使用的任何 Ruby 代码，例如：

```
ServerAdmin <%= sitedomain == 'coldcomfort.com' ? 'seth@coldcomfort.com'
 : 'flora@poste.com' %> 
```

#### 参见本书

*   本章的 在模板中遍历数组 一节

# 在模板中遍历数组

### 在模板中遍历数组

在前面的例子中，我们已经看到使用 Ruby 可以根据表达式的结果插入不同的值。 你也可以使用循环对数组中的每个元素生成内容。

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    $ipaddresses = [ '192.168.0.1',
                     '158.43.128.1',
                     '10.0.75.207' ]

    file { "/tmp/addresslist.txt":
        content =&gt; template("admin/addresslist.erb")
    } 
    ```

2.  使用如下内容创建 /etc/puppet/modules/admin/templates/addresslist.erb 文件：

    ```
    &lt;% ipaddresses.each do |ip| -%&gt;
    IP address &lt;%= ip %&gt; is present.
    &lt;% end -%&gt; 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304766335'
    notice: /Stage[main]//Node[cookbook]/File[/tmp/addresslist.txt]/
    ensure: defined content as '{md5}7ad1264ebdae101bb5ea0afef474b3ed'
    notice: Finished catalog run in 0.64 seconds 
    ```

4.  检查生成文件的内容：

    ```
    # cat /tmp/addresslist.txt
    IP address 192.168.0.1 is present.
    IP address 158.43.128.1 is present.
    IP address 10.0.75.207 is present. 
    ```

#### 工作原理

1.  在模板的第一行，我们引用了一个数组 ipaddresses，并调用了相应的 each 方法：

    ```
    &lt;% ipaddresses.each do |ip| -%&gt; 
    ```

2.  使用 Ruby 创建的这个循环将对数组中的每个元素执行一次。 每循环一次，变量 ip 将会设置成数组当前元素的值。

3.  在我们的例子中，ipaddresses 数组包含三个元素，所以下面的行将执行三次， 对每个数组元素执行一次：

    ```
    IP address &lt;%= ip %&gt; is present. 
    ```

4.  结果将产生如下三行内容：

    ```
    IP address 192.168.0.1 is present.
    IP address 158.43.128.1 is present.
    IP address 10.0.75.207 is present. 
    ```

5.  最后一行是循环的结束：

    ```
    &lt;% end -%&gt; 
    ```

6.  注意循环的第一行和最后一行都以 -%> 结束，而不是我们以前所看到的以 %> 结束。 使用 - 的作用是要抑制换行，否则会产生换行符，即在文件中产生无用的空白行。

#### 更多用法

模板也可以遍历哈希，或哈希数组，例如：

```
$interfaces = [ { name => 'eth0',
                  ip => '192.168.0.1' },
                { name => 'eth1',
                  ip => '158.43.128.1' },
                { name => 'eth2',
                  ip => '10.0.75.207' } ]

<% interfaces.each do |interface| -%>
Interface <%= interface['name'] %> has the address <%= interface['ip']%>.
<% end -%> 
```

结果为：

```
Interface eth0 has the address 192.168.0.1.
Interface eth1 has the address 158.43.128.1.
Interface eth2 has the address 10.0.75.207. 
```

#### 参见本书

*   第五章的 使用 ERB 模板 一节

# 从第三方仓库安装软件包

### 从第三方仓库安装软件包

大多数情况下，你会从发行版本的官方仓库安装软件包，所以使用简单的 package 资源就可以做到：

```
package { "exim4": ensure => installed } 
```

但是，有时你需要的软件包只能在第三方仓库（例如 **Ubuntu PPA**）中找到。 或者是第三方仓库提供的软件包版本比官方仓库中的软件包新。

对于手工管理的主机，你通常需要在安装软件包之前先添加仓库源配置到 /etc/apt/sources.list.d （如果有必要，还应该导入仓库的 GPG 公钥）。我们可以很容易地使用 Puppet 自动完成这个过程。

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    package { "python-software-properties": ensure =&gt; installed }

    exec { "/usr/bin/add-apt-repository ppa:mathiaz/puppet-backports":
        creates =&gt; "/etc/apt/sources.list.d/mathiaz-puppet-backportslucid.
     list",
        require =&gt; Package["python-software-properties"],
    } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304773240'
    notice: /Stage[main]//Node[cookbook]/Exec[/usr/bin/add-aptrepository
    ppa:mathiaz/puppet-backports]/returns: executed
    successfully
    notice: Finished catalog run in 5.97 seconds 
    ```

#### 工作原理

1.  python-software-properties 软件包提供了 add-apt-repository 命令， 使用此命令可以简化添加额外的软件仓库源配置的过程：

    ```
    package { "python-software-properties": ensure =&gt; installed } 
    ```

2.  然后我们使用 exec 资源调用这个命令添加要求的配置：

    ```
    exec { "/usr/bin/add-apt-repository ppa:mathiaz/puppet-backports": 
    ```

3.  确保 Puppet 不会在每次运行时都执行这个 exec 资源，我们指定了一个由此命令创建的文件， 如果此文件已经存在，则 Puppet 就会忽略 exec 资源的执行：

    ```
    creates =&gt; "/etc/apt/sources.list.d/mathiaz-puppet-backports-lucid.list", 
    ```

    你可能想要将这个内容与清除 /etc/apt/sources.list.d 中不需要的仓库定义相结合， 正如本书 使用文件资源递归地分发整个目录树 一节中所描述的。

#### 更多用法

这种处理仓库的方法是针对 Debian 和 Ubuntu 系统的，我们之前说过，这是本书使用的参考平台。 如果你使用的是基于 Red Hat 的系统，你可以直接使用 yumrepo 资源管理 RPM 仓库。

#### 参见本书

*   第六章的 使用文件资源递归地分发整个目录树 一节

# 配置 APT 软件仓库

### 配置 APT 软件仓库

运行自己的软件仓库有几个优点。你可以在自己的仓库中发布自己的软件包。 你可以在自己的软件仓库中放置上游软件包或第三方软件包，从而控制你使用的软件版本。 你可以将自己的软件仓库放置在其他服务器附近，从而避免网速缓慢或镜像站点无法访问的问题。

即使你不需要创建自己的软件包，也可能想要下载特定版本软件包所需的关键依赖包， 并将这些依赖包存储在自己的仓库中，从而防止因上游发生变故而产生的任何意外 （例如，你的发行版本已到达生命周期的终结或者上游仓库已经被关闭）。

这也使得通过 Puppet 自动更新软件包便得更容易。你可能偶尔需要更新一个软件包 （例如，当有一个安全更新可用时），只要在 package 资源中指定 ensure => latest 就可以方便地实现包更新。但是如果你不能控制仓库，就可能遭遇意想不到的升级风险， 使你的系统受到某种破坏。

使用自己的软件仓库是件两全其美的事情：你可以放心地使用 Puppet 从自己的软件仓库自动更新软件包，当有新版本的软件可用时，只需要将其纳入自己的软件仓库； 你可以首先测试上游的软件版本，确保其可用的情况下再纳入用于生产环境的软件仓库。

#### 准备工作

你需要第九章 管理 Apache 服务 一节中讲述的 apache 模块， 如果还没有此模块请先创建它。

在本例中，我将我的仓库命名为 packages.bitfieldconsulting.com。 你可能想要使用一个不同的仓库名，这需要替换本例中的所有的仓库名 packages.bitfieldconsulting.com 为你想要的仓库名。

#### 操作步骤

1.  创建一个新的 repo 模块：

    ```
    # mkdir /etc/puppet/modules/repo
    # mkdir /etc/puppet/modules/repo/manifests
    # mkdir /etc/puppet/modules/repo/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/repo/manifests/bitfield-server.pp 文件：

    ```
    class repo::bitfield-server {
        include apache

        package { "reprepro": ensure =&gt; installed }

        file { [ "/var/apt",
                 "/var/apt/conf" ]:
            ensure =&gt; directory,
        }

        file { "/var/apt/conf/distributions":
            source  =&gt; "puppet:///modules/repo/distributions",
            require =&gt; File["/var/apt/conf"],
        }

        file { "/etc/apache2/sites-available/apt-repo":
            source  =&gt; "puppet:///modules/repo/apt-repo.conf",
            require =&gt; Package["apache2-mpm-worker"],
        }

        file { "/etc/apache2/sites-enabled/apt-repo":
            ensure  =&gt; symlink,
            target  =&gt; "/etc/apache2/sites-available/apt-repo",
            require =&gt; File["/etc/apache2/sites-available/apt-repo"],
            notify  =&gt; Service["apache2"],
        }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/repo/files/distributions 文件：

    ```
    Origin: Bitfield Consulting
    Label: bitfield
    Suite: stable
    Codename: lucid
    Architectures: amd64 i386
    Components: main non-free contrib
    Description: Custom and cached packages for Bitfield Consulting 
    ```

4.  使用如下内容创建 /etc/puppet/modules/repo/files/apt-repo.conf 文件：

    ```
    &lt;VirtualHost *:80&gt;
        DocumentRoot /var/apt
        ServerName packages.bitfieldconsulting.com
        ErrorLog /var/log/apache2/packages.bitfieldconsulting.com.error.log

        LogLevel warn

        CustomLog /var/log/apache2/packages.bitfieldconsulting.com.access.log combined

        ServerSignature On

        # Allow directory listings so that people can browse the
        # repository from their browser too
        &lt;Directory "/var/apt"&gt;
            Options Indexes FollowSymLinks MultiViews
            DirectoryIndex index.html
            AllowOverride Options
            Order allow,deny
            allow from all
        &lt;/Directory&gt;

        # Hide the conf/ directory for all repositories
        &lt;Directory "/var/apt/conf"&gt;
            Order allow,deny
            Deny from all
            Satisfy all
        &lt;/Directory&gt;

        # Hide the db/ directory for all repositories
        &lt;Directory "/var/apt/db"&gt;
            Order allow,deny
            Deny from all
            Satisfy all
        &lt;/Directory&gt;
    &lt;/VirtualHost&gt; 
    ```

5.  在一个节点的配置清单中添加如下代码：

    ```
    include repo::bitfield-server 
    ```

6.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304775601'

    notice: /Stage[main]/Repo::Bitfield-server/File[/var/apt]/ensure:
    created

    notice: /Stage[main]/Repo::Bitfield-server/File[/var/apt/conf]/
    ensure: created

    notice: /Stage[main]/Repo::Bitfield-server/File[/var/apt/conf/
    distributions]/ensure: defined content as '{md5}65dc791b876f53318a
    35fcc42c770283'

    notice: /Stage[main]/Repo::Bitfield-server/Package[reprepro]/
    ensure: created

    notice: /Stage[main]/Repo::Bitfield-server/File[/etc/apache2/
    sites-enabled/apt-repo]/ensure: created

    notice: /Stage[main]/Repo::Bitfield-server/File[/etc/apache2/
    sites-available/apt-repo]/ensure: defined content as '{md5}2da4686
    957e5acf49220047fe6f6e6e1'

    info: /Stage[main]/Repo::Bitfield-server/File[/etc/apache2/sitesenabled/
    apt-repo]: Scheduling refresh of Service[apache2]

    notice: /Stage[main]/Apache/Service[apache2]: Triggered 'refresh'
    from 1 events

    notice: Finished catalog run in 16.32 seconds 
    ```

#### 工作原理

其实，你无需创建一个 APT 仓库。因为可以通过 HTTP 下载软件包，所以你只需要一个 Apache 虚拟主机。 你可以将实际的软件包随意放置在任何地方，只要有一个 conf/distributions 文件并在其中给出 APT 仓库的相关信息。

1.  bitfield-server 类的第一部分确保 Apache 已经被设置：

    ```
    class repo::bitfield-server {
        include apache 
    ```

2.  reprepro 是用于管理仓库本身的非常有用的工具（例如，添加一个新的软件包）：

    ```
     package { "reprepro": ensure =&gt; installed } 
    ```

3.  我们创建一个仓库的根目录 /var/apt，以及该目录下的 conf/distributions 文件：

    ```
     file { [ "/var/apt",
                 "/var/apt/conf" ]:
            ensure =&gt; directory,
        }

        file { "/var/apt/conf/distributions":
            source =&gt; "puppet:///modules/repo/distributions",
            require =&gt; File["/var/apt/conf"],
        } 
    ```

4.  这个类的其余部分部署了一个 Apache 虚拟主机的配置文件，用于响应 packages.bitfieldconsulting.com 的请求：

    ```
    file { "/etc/apache2/sites-available/apt-repo":
        source  =&gt; "puppet:///modules/repo/apt-repo.conf",
        require =&gt; Package["apache2-mpm-worker"],
    }

    file { "/etc/apache2/sites-enabled/apt-repo":
        ensure  =&gt; symlink,
        target  =&gt; "/etc/apache2/sites-available/apt-repo",
        require =&gt; File["/etc/apache2/sites-available/apt-repo"],
        notify  =&gt; Service["apache2"],
    } 
    ```

#### 更多用法

当然，一个可用的仓库里不能没有软件包。下面将介绍如何添加软件包， 以及如何配置主机并从你的仓库下载软件包。

##### 向仓库添加软件包

要添加一个软件包到你的仓库，首先下载它然后使用 reprepro 将其添加到仓库：

```
# cd /tmp
# wget http://archive.ubuntu.com/ubuntu/pool/main/n/ntp/\
ntp_4.2.4p8+dfsg-1ubuntu2.1_i386.deb
# cd /var/apt
# reprepro includedeb lucid /tmp/ntp_4.2.4p8+dfsg-1ubuntu2.1_i386.deb
Exporting indices... 
```

##### 配置节点使用仓库

1.  使用如下内容创建 /etc/puppet/modules/repo/manifests/bitfield.pp 文件 （请根据你自己的仓库服务器的 IP 地址替换如下的 IP 地址）：

    ```
    class repo::bitfield {
        host { "packages.bitfieldconsulting.com":
            ip     =&gt; "10.0.2.15",
            ensure =&gt; present,
            target =&gt; "/etc/hosts",
        }

        file { "/etc/apt/sources.list.d/bitfield.list":
            content =&gt; "deb http://packages.bitfieldconsulting.com/lucid main\n",
            require =&gt; Host["packages.bitfieldconsulting.com"],
            notify  =&gt; Exec["bitfield-update"],
        }

        exec { "bitfield-update":
            command     =&gt; "/usr/bin/apt-get update",
            require     =&gt; File["/etc/apt/sources.list.d/bitfield.list"],
            refreshonly =&gt; true,
        }
    } 
    ```

    如果你有 DNS 服务器或者你可以控制你的 DNS 区域，可以省略 host 资源的设置。

2.  应用这个类到一个节点：

    ```
    node cookbook {
        include repo::bitfield
    } 
    ```

3.  测试你仓库中的 ntp 软件包是否可用：

    ```
    # apt-cache madison ntp
      ntp | 1:4.2.4p8+dfsg-1ubuntu2.1 | http://us.archive.ubuntu.
    com/ubuntu/ lucid-updates/main Packages
      ntp | 1:4.2.4p8+dfsg-1ubuntu2.1 | http://packages.
    bitfieldconsulting.com/ lucid/main Packages
      ntp | 1:4.2.4p8+dfsg-1ubuntu2 | http://us.archive.ubuntu.
    com/ubuntu/ lucid/main Packages
      ntp | 1:4.2.4p8+dfsg-1ubuntu2 | http://us.archive.ubuntu.
    com/ubuntu/ lucid/main Sources
      ntp | 1:4.2.4p8+dfsg-1ubuntu2.1 | http://us.archive.ubuntu.
    com/ubuntu/ lucid-updates/main Sources 
    ```

##### 对软件包签名

对于生产环境，你应该对软件仓库设置 GPG 密钥并且对软件包进行签名，关于如何设置密钥和签名的信息， 请参考 Sander Marechal 撰写的关于 “设置和管理 APT 仓库” 的文章： [`www.jejik.com/articles/2006/09/setting_up_and_managing_an_apt_repository_with_reprepro/`](http://www.jejik.com/articles/2006/09/setting_up_and_managing_an_apt_repository_with_reprepro/) 。

# 配置 GEM 仓库

### 配置 GEM 仓库

使用另外的不兼容的包系统是每个系统管理员的梦想。 如果你要管理 Ruby 或 Rails 应用程序，就需要处理 Rubygems。 与维护自己的 APT 仓库一样，维护你自己的 **gem 仓库** 会有很多优点。 你可以控制包的可用性和包的版本，也可以根据需要发布你自己的 Rubygems。

#### 操作步骤

1.  使用如下内容创建 /etc/puppet/modules/repo/manifests/gem-server.pp 文件：

    ```
    class repo::gem-server {
        include apache

        file { "/etc/apache2/sites-available/gemrepo":
            source  =&gt; "puppet:///modules/repo/gemrepo.conf",
            require =&gt; Package["apache2-mpm-worker"],
            notify  =&gt; Service["apache2"],
        }

        file { "/etc/apache2/sites-enabled/gemrepo":
            ensure  =&gt; symlink,
            target  =&gt; "/etc/apache2/sites-available/gemrepo",
            require =&gt; File["/etc/apache2/sites-available/gemrepo"],
            notify  =&gt; Service["apache2"],
        }

        file { "/var/gemrepo":
            ensure =&gt; directory,
        }
    } 
    ```

2.  使用如下内容创建 /etc/puppet/modules/repo/files/gemrepo.conf 文件：

    ```
    &lt;VirtualHost *:80&gt;
        ServerAdmin john@bitfieldconsulting.com
        ServerName gems.bitfieldconsulting.com
        ErrorLog logs/gems.bitfieldconsulting.com-error_log
        CustomLog logs/gems.bitfieldconsulting.com-access_log common

        Alias / /var/gemrepo/
        &lt;Location /&gt;
            Options Indexes
        &lt;/Location&gt;
    &lt;/VirtualHost&gt; 
    ```

3.  在配置清单的节点中添加如下代码：

    ```
    node cookbook {
        include repo::gem-server
    } 
    ```

4.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304949279'

    notice: /Stage[main]/Repo::Gem-server/File[/etc/apache2/
    sites-available/gemrepo]/ensure: defined content as '{md5}
    ae1fd948098f14503de02441d02a825d'

    info: /Stage[main]/Repo::Gem-server/File[/etc/apache2/sitesavailable/
    gemrepo]: Scheduling refresh of Service[apache2]

    notice: /Stage[main]/Repo::Gem-server/File[/etc/apache2/sitesenabled/
    gemrepo]/ensure: created

    info: /Stage[main]/Repo::Gem-server/File[/etc/apache2/sitesenabled/
    gemrepo]: Scheduling refresh of Service[apache2]

    notice: /Stage[main]/Apache/Service[apache2]: Triggered 'refresh'
    from 2 events

    notice: /Stage[main]/Repo::Gem-server/File[/var/gemrepo]/ensure:
    created

    notice: Finished catalog run in 6.52 seconds 
    ```

#### 工作原理

其原理与 APT 仓库的例子是完全一样的。我们首先定义了一个 gem 仓库的存放目录， 然后定义了一个 Apache 的虚拟主机，使其能够响应 gems.bitfieldconsulting.com 的请求。

#### 更多用法

与 APT 仓库类似，仅有空仓库是无用的，下面将演示如何将 gem 包纳入仓库， 以及如何配置你的节点访问自己的 gem 仓库。

##### 添加 gem 仓库

向你的仓库添加新的 gems 包很简单。将 gem 文件存入 /var/gemrepo/gems 目录， 然后在 /var/gemrepo 目录下运行如下命令即可：

```
# gem generate_index 
```

##### 使用 gem 仓库

像使用 APT 仓库一样，首先确保你的节点能够解析主机名 gems.bitfieldconsulting.com， 既可以使用 Puppet 部署 host 资源，也可以通过配置 DNS 实现。

然后在 Puppet 中使用如下代码指定一个 gem 软件包：

```
package { "json":
    provider => "gem",
    source => "http://gems.bitfieldconsulting.com ",
} 
```

# 从源码包自动构建软件

### 从源码包自动构建软件

源码压缩包（**tarball**）会严重损害你的健康。你既可以使用 **发行版**（**distro**）或第三方软件包， 同时从源码包构建你自己的软件包也是可取的，这有时还会有许多工作要做。 创建 Debian 软件包（或者其他风格的软件包）会是一个漫长而容易出错的过程， 而且你可能没有时间或预算构建它。

如果你必须从源代码构建你的程序，至少 Puppet 可以帮你实现这一过程。 一般地构建过程是自动化的，否则你就得手工构建：

*   下载源码包

*   解压源码包

*   配置（Configure）并构建（build）程序

*   安装已构建的程序

在下面的例子中，我们将从源码构建 **OpenSSL**（虽然在生产环境中你应该使用发行版提供的软件包， 但这是一个有用的演示示例）。

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    exec { "build-openssl":
        cwd =&gt; "/root",
        command =&gt; "/usr/bin/wget ftp://ftp.openssl.org/source/
     openssl-0.9.8p.tar.gz && /bin/tar xvzf openssl-0.9.8p.tar.
     gz && cd openssl-0.9.8p && ./Configure linux-generic32 &&
     make install",
        creates =&gt; "/usr/local/ssl/bin/openssl",
        logoutput =&gt; on_failure,
        timeout =&gt; 0,
    } 
    ```

2.  运行 Puppet （这可能会执行一段时间！）：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304954159'

    notice: /Stage[main]//Node[cookbook]/Exec[build-openssl]/returns:
    executed successfully

    notice: Finished catalog run in 554.00 seconds 
    ```

#### 工作原理

exec 命令被 && 运算符分割成了五个阶段。也就是说每个阶段的命令若执行失败， 整个命令将会失败且停止运行。 当你想确保每个子命令都成功运行之后才运行下一个子命令时，这是一种有用的构造方法。

1.  第一阶段，下载源码包：

    ```
    /usr/bin/wget ftp://ftp.openssl.org/source/openssl-0.9.8p.tar.gz 
    ```

2.  第二阶段，解压源码包：

    ```
    /bin/tar xvzf openssl-0.9.8p.tar.gz 
    ```

3.  第三阶段，进入源码树的目录：

    ```
    cd openssl-0.9.8p 
    ```

4.  第四阶段，运行配置脚本（通常还会指定一些自定义选项）：

    ```
    ./Configure linux-generic32 
    ```

5.  最后阶段，创建并安装软件：

    ```
    make install 
    ```

6.  由于这是一个漫长的过程，你不想每次运行 Puppet 时都运行它，我们指定了一个条件， 即被创建的文件是否应经存在：

    ```
    creates =&gt; "/usr/local/ssl/bin/openssl", 
    ```

    无论出于任何原因，如果需要重建，你可以删除此文件。

7.  错误并非只发生在首次编译的情况。在出现问题时，我们指定了 logoutput 参数， 它将为我们显示编译或构建过程中的出错信息：

    ```
     logoutput =&gt; on_failure, 
    ```

8.  最后，因为编译过程可能会持续一段时间，所以将 timeout 参数设置成了 0 （默认情况下，Puppet 执行 exec 命令 5 分钟后就会超时）：

    ```
     timeout =&gt; 0, 
    ```

#### 更多用法

如果你有一批必须从源代码构建的软件，那么将上述的 exec 放入一个 define 函数会使这一构建过程变得更方便，因为你可以使用大致相同的代码构建每个包。

# 比较软件包的版本

### 比较软件包的版本

软件包的版本号是个奇怪的东西。它们看起来像十进制的数字，但它们不是。 例如，一个版本号通常的形式为 2.6.4。如果你需要比较两个版本号， 不能做简单的字符串比较，因为 2.6.4 会比 2.6.12 大；也不能进行数字比较， 因为它们不是有效地数字。

Puppet 的 versioncmp 函数会帮我们解决这个问题。 若你给它传递两个版本号，它会比较它们，并返回一个值，指出谁是更大的：

```
 versioncmp( A, B ) 
```

将返回如下值：

*   若 A 和 B 相同，则返回 0

*   若 A 大于 B ，则返回大于 1 的值

*   若 A 小于 B ，则返回小于 0 的值

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    $app_version = "1.2.2"
    $min_version = "1.2.10"

    if versioncmp( $app_version, $min_version ) &gt;= 0 {
        notify { "Version OK": }
    } else {
        notify { "Upgrade needed": }
    } 
    ```

2.  运行 Puppet：

    ```
    notice: Upgrade needed 
    ```

3.  现在更变 $app_version 的值：

    ```
    $app_version = "1.2.14" 
    ```

4.  在次运行 Puppet：

    ```
    notice: Version OK 
    ```

#### 工作原理

我们指定了可接受的最小版本（$min_version）是 1.2.10。 所以在上面的例子中，我们用 $app_version 为 1.2.2 的版本号与之比较。

简单的对两个字符串进行比较（例如，Ruby 中的字符串比较）将会给出错误的结果， 然而 versioncmp 却可以正确地检测出 1.2.2 小于 1.2.10 并提示我们 Upgrade needed。

当改变 $app_version 的值为 1.2.14 后，由于 versioncmp 正确地检测出 $app_version 大于 $min_version，所以提示我们 Version OK。