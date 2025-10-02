# 用户和虚拟资源

## 用户和虚拟资源

> How good the design is doesn’t matter near as much as whether the design is getting better or worse. If it is getting better, day by day, I can live with it forever. If it is getting worse, I will die.
> 
> — Kent Beck

在本章中，我们将学习如下内容：

*   使用虚拟资源

*   使用虚拟资源管理用户

*   管理用户基于密钥的 SSH 访问

*   管理用户的自定义文件

*   有效地分发 cron 任务

*   当文件更新时运行命令

*   使用主机资源

*   为文件资源指定多个源

*   使用文件资源递归地分发整个目录树

*   清理过期的旧文件

*   使用日程表资源

*   资源的审计

*   临时禁用资源

*   管理时区

维护用户是件痛苦的事，我不是说人，但毫无疑问某种情况下这确实是真的。 为了在网络中的机器上同步 UNIX 用户账号和权限文件，其中一些机器可能运行了不同的操作系统， 没有某种集中的配置管理是非常具有挑战性的。

考虑一个新的开发者要加入开发团队的情况，他在每台机器上都需要一个账号， 并为此账号的组成员分配 sudo 特权；还需要他的 SSH 密钥授权一堆不同的账号。 系统管理员若手工实现这些工作，恐怕要忙活一整天。 使用 Puppet 的系统管理员却能在几分钟内完成这些工作，提早就去吃午餐了。

在本章中，我们将会看到管理用户及其相关资源的处理模式和技术。 我们还将看到：如何在 Puppet 中安排资源的执行； 为提高效率如何将 cron 任务分散到不同的时间执行； 如何处理时区，如何处理 /etc/hosts 条目； 以及如何让 Puppet 收集审计数据，使有人搞乱了你网络中的机器时能被发现。

# 使用虚拟资源

### 使用虚拟资源

什么是虚拟资源（Virtual Resource），我们为什么需要它们？ 下面我们来看一个可能会使用虚拟资源的典型例子。 你负责管理 facesquare 和 twitstagram 两个应用程序，他们都是运行在 Apache 上的 Web 应用程序。 facesquare 的定义看起来可能像这样：

```
class app::facesquare
{
    package { "apache2-mpm-worker": ensure => installed }
} 
```

twitstagram 的定义看起来可能像这样：

```
class app::twitstagram
{
    package { "apache2-mpm-worker": ensure => installed }
} 
```

一切都很好，直到你需要将两个应用程序同时应用到单台服务器上：

```
node micawber
{
    include app::facesquare
    include app::twitstagram
} 
```

现在 Puppet 会出错，因为你试图用相同的名字 apache2-mpm-worker 定义两个 package 资源。 错误输出信息如下：

```
err: Could not retrieve catalog from remote server: Error 400 on SERVER:
Duplicate definition: Package[apache2-mpm-worker] is already defined in
file /etc/puppet/modules/app/manifests/facesquare.pp at line 2; cannot
redefine at /etc/puppet/modules/app/manifests/twitstagram.pp:2 on node
cookbook.bitfieldconsulting.com 
```

你可以从其中的一个类中移除重复的包定义，但是这样话，如果试图在另一个服务器包含 app 类时，就会因为没有准备好 Apache 而失败。

通过在自己的类中放置 Apache 的包资源并使用 include apache 包含它，你就可以解决这个问题， 因为 Puppet 不介意多次包含一个相同的类。但是这有一个缺点，即每个具有潜在冲突的资源都必须有它自己的类。 虚拟资源可以解决这个问题。虚拟资源就像是个普通的资源，特别之处在于它以 @ 字符开始，例如：

```
@package { "apache2-mpm-worker": ensure => installed } 
```

你可以把它看作是个 “FYI（仅供参考）” 资源：我只是告诉 Puppet 这个资源存在，但不希望用它做任何事情。 Puppet 将会读取并记住虚拟资源定义，但实际上不会创建这个资源，直到你明确指出要创建此资源。

要创建这个资源，使用如下的 realize 函数：

```
realize( Package["apache2-mpm-worker"] ) 
```

对于你想要的资源，可以多次调用 realize 而且不会产生冲突。 因此，虚拟资源用于：当在几个不同的类中都需要相同的资源，且它们可能会在相同的节点上共存的情况。

#### 操作步骤

1.  创建名为 app 的新模块：

    ```
    # mkdir -p /etc/puppet/modules/app/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/app/manifests/facesquare.pp 文件：

    ```
    class app::facesquare
    {
        realize( Package["apache2-mpm-worker"] )
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/app/manifests/twitstagram.pp 文件：

    ```
    class app::twitstagram
    {
        realize( Package["apache2-mpm-worker"] )
    } 
    ```

4.  使用如下内容创建 /etc/puppet/modules/admin/manifests/virtualpackages.pp 文件：

    ```
    class admin::virtual-packages
    {
        @package { "apache2-mpm-worker": ensure =&gt; installed }
    } 
    ```

5.  在一个节点上包含如下代码：

    ```
    node cookbook
    {
        include admin::virtual-packages
        include app::facesquare
        include app::twitstagram
    } 
    ```

6.  运行 Puppet。

#### 工作原理

你可以在 admin::virtual-packages 类中定义一个包的虚拟资源。 所有节点都可以包含这个类，并且你可以将所有虚拟资源都放在此类中。 这些虚拟资源都不会实际安装到节点上，直到你调用 realize：

```
class admin::virtual-packages
{
    @package { "apache2-mpm-worker": ensure => installed }
} 
```

每个需要 Apache 包的类都可以对虚拟资源调用 realize ：

```
class app::twitstagram
{
    realize( Package["apache2-mpm-worker"] )
} 
```

Puppet 知道如何处理它，因为你设置了相应的虚拟资源，你打算多次引用同一个包, 而不会意外地创建具有相同名子的两个资源。所以，这正确地实现了我们的需求。

#### 更多用法

为了实现（realize）虚拟资源，你也可以使用 **collection** 语法：

```
Package <| title = "apache2-mpm-worker" |> 
```

使用这种语法的好处是，你不仅可以指定资源名，而且可以指定 tag，例如：

```
Package <| tag = "security" |> 
```

或者，你可以指定资源类型的所有实例，在查询部分保留一个空格即可：

```
Package <| |> 
```

#### 参见本书

*   本章的 使用虚拟资源管理用户 一节

# 使用虚拟资源管理用户

### 使用虚拟资源管理用户

用户管理是使虚拟资源能派上用场的另一个很好的例子。考虑下面的设置。 你有三个用户：John、Graham 和 Steven。为了简化管理大量主机，你为两种类型的用户定义了类： developers 和 sysadmins。每个机器都需要包含 sysadmins，但只有一部分机器允许 developer 访问：

```
node server
{
    include user::sysadmins
}

node webserver inherits server
{
    include user::developers
} 
```

John 是个系统管理员，Steven 是个开发者，而 Graham 既是系统管理员又是开发者， 所以 Graham 必须是两个组中的成员。这将在 webserver 上产生冲突，因为最终会导致重复定义用户 Graham。

为了避免这种情况，常见的做法是将所有用户设置成虚拟资源，并定义在一个单独的类 user::virtual 中，每个机器都包含这个类，然后在需要时对虚拟用户执行 realize。

#### 操作步骤

1.  创建如下的用户模块：

    ```
    # mkdir -p /etc/puppet/modules/user/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/user/manifests/virtual.pp 文件：

    ```
    class user::virtual
    {
        @user { "john": }
        @user { "graham": }
        @user { "steven": }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/user/manifests/developers.pp 文件：

    ```
    class user::developers
    {
        realize( User["graham"],
                 User["steven"] )
    } 
    ```

4.  使用如下内容创建 /etc/puppet/modules/user/manifests/sysadmins.pp 文件：

    ```
    class user::sysadmins
    {
        realize( User["john"],
                 User["graham"] )
    } 
    ```

5.  在一个节点中添加如下代码：

    ```
    include user::virtual
    include user::sysadmins
    include user::developers 
    ```

6.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1305554239'
    notice: /Stage[main]/User::Virtual/User[john]/ensure: created
    notice: /Stage[main]/User::Virtual/User[steven]/ensure: created
    notice: /Stage[main]/User::Virtual/User[graham]/ensure: created
    notice: Finished catalog run in 2.36 seconds 
    ```

#### 工作原理

每个节点都应该包含 user::virtual 类，作为基本配置的一部分，所有服务器都继承这个类。 这个类将为你的组织或站点定义所有的用户。同时也应包括只是为了运行应用程序或服务的任何用户 （即非登录用户，例如 apache 或 git）。

然后你可以对你的用户进行分组（不同于 UNIX 系统中的组，只是用于区分不同的团队或任务角色）， 例如 developers 和 sysadmins。 对于一个组的类，可以 realize 组里需要的所有用户，例如：

```
class user::sysadmins
{
    realize( User["john"],
             User["graham"] )
} 
```

之后便可以在任何需要的地方包含这些组，而不用担心因为多次定义同一个用户而产生的冲突。

#### 参见本书

*   本章的 使用虚拟资源 一节

*   本章的 管理用户的自定义文件 一节

# 管理用户基于密钥的 SSH 访问

### 管理用户基于密钥的 SSH 访问

唯一安全的服务器是关闭的。尽管如此，对于服务器访问控制的一个好方法是， 使用经过私钥短语保护的 SSH 密钥的用户帐户， 而不是多个用户使用一个共享账号和一个众所周知的口令。 Puppet 使这种管理变得简单，感谢其内置的 ssh_authorized_key 类型。

将它与上一节讲述的虚拟用户相结合，你可以创建一个包括 user 和 ssh_authorized_key 的 define。 对于需要添加自定义文件和其他每用户自己的资源时，这也会是有用的。

#### 操作步骤

1.  对上一节创建的 user::virtual 类做如下修改：

    ```
    class user::virtual
    {
        define ssh_user( $key )
        {
            user { $name:
                ensure =&gt; present,
                managehome =&gt; true,
                 }

            ssh_authorized_key { "${name}_key":
                key =&gt; $key,
                type =&gt; "ssh-rsa",
                user =&gt; $name,
                                }
        }

        @ssh_user { "phil":
            key =&gt; "AAAAB3NzaC1yc2EAAAABIwAAAIEA3ATqENg+GW
     ACa2BzeqTdGnJhNoBer8x6pfWkzNzeM8Zx7/2Tf2pl7kHdbsiT
     XEUawqzXZQtZzt/j3Oya+PZjcRpWNRzprSmd2UxEEPTqDw9LqY5S2B8og/
     NyzWaIYPsKoatcgC7VgYHplcTbzEhGu8BsoEVBGYu3IRy5RkAcZik=",
        }
    } 
    ```

2.  在一个节点上包含如下代码：

    ```
    realize( User::Virtual::Ssh_user["phil"] ) 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1305561740'

    notice: /Stage[main]/User::Virtual/User::Virtual::Ssh_user[phil]/
    User[phil]/ensure: created

    notice: /Stage[main]/User::Virtual/User::Virtual::Ssh_user[phil]/
    Ssh_authorized_key[phil_key]/ensure: created

    notice: Finished catalog run in 1.04 seconds 
    ```

#### 工作原理

我们创建了一个名为 ssh_user 的 define，它包括用户自身的 user 资源以及与其相关的 ssh_authorized_key 资源，内容如下：

```
define ssh_user( $key )
{
    user { $name:
        ensure => present,
        managehome => true,
         }

    ssh_authorized_key { "${name}_key":
        key  => $key,
        type => "ssh-rsa",
        user => $name,
                        }
} 
```

然后我们对用户 phil 创建了 ssh_user 的一个虚拟资源实例：

```
 @ssh_user { "phil":
        key => "AAAAB3NzaC1yc2EAAAABIwAAAIEA3ATqENg+GW
 ACa2BzeqTdGnJhNoBer8x6pfWkzNzeM8Zx7/2Tf2pl7kHdbsiT
 XEUawqzXZQtZzt/j3Oya+PZjcRpWNRzprSmd2UxEEPTqDw9LqY5S2B8og/
 NyzWaIYPsKoatcgC7VgYHplcTbzEhGu8BsoEVBGYu3IRy5RkAcZik=",
    } 
```

回顾一下，因为资源是虚拟的，Puppet 会意识到这一点，所以它不会创建任何实际的资源， 直到你调用 realize 时才真正创建。

最后，我们在节点中添加了如下代码：

```
 realize( User::Virtual::Ssh_user["phil"] ) 
```

这会实际创建 user 资源以及包含其公钥的 authorized_keys 资源。

#### 更多用法

可以将这种思想应用到上一节提及的 “组织用户到组的类” 中，修改相应类的代码为：

```
class user::sysadmins
{
    search User::Virtual

    realize( Ssh_user["john"],
             Ssh_user["graham"] )
} 
```

使用 search User::Virtual 的目的仅仅是为了避免杂乱，这允许你直接引用 Ssh_user， 而不用每次都要使用 User::Virtual:: 前缀。

另外，你可能会遭遇如下的错误：

```
err: /Stage[main]/User::Virtual/User::Virtual::Ssh_user[graham]/Ssh_
authorized_key[graham_key]: Could not evaluate: No such file or directory
- /home/graham/.ssh 
```

这或许是因为你之前已经创建过 graham 用户，但没有使用 Puppet 管理其自家目录。 这种情况下，Puppet 不会自动创建 authorized_keys 文件所需的 .ssh 目录。 运行如下的命令：

```
# userdel graham 
```

之后再次运行 Puppet 即可解决此问题。

# 管理用户的自定义文件

### 管理用户的自定义文件

用户，与猫一样，常常觉得有必要标记他们的领地。 与猫不同，用户往往要定制自己的 shell 环境，如终端显示的颜色、别名等。 这通常是通过用户家目录下的一些以点开始的环境文件实现的，例如：.bash_profile。

通过修改上一节的 user::virtual::ssh_user 类，你可以将环境文件添加到基于 Puppet 的用户管理中。 在这个类中，你可以有选择地为客户提供 Puppet 文件仓库中的任何以点开头的文件。

#### 操作步骤

1.  对 user::virtual 类做如下修改：

    ```
    class user::virtual
    {
      define user_dotfile( $username )
      {
        file { "/home/${username}/.${name}":
          source =&gt; "puppet:///modules/user/${username}-${name}",
          owner  =&gt; $username,
          group  =&gt; $username,
        }
      }

      define ssh_user( $key, $dotfile = false )
      {
        user { $name:
          ensure     =&gt; present,
          managehome =&gt; true,
        }

        ssh_authorized_key { "${name}_key":
          key  =&gt; $key,
          type =&gt; "ssh-rsa",
          user =&gt; $name,
        }

        if $dotfile {
          user_dotfile { $dotfile:
              username =&gt; $name,
          }
        }
      }

      @ssh_user { "john":
        key =&gt; "AAAAB3NzaC1yc2EAAAABIwAAAIEA3ATqENg
     +GWACa2BzeqTdGnJhNoBer8x6pfWkzNzeM8Zx7/2Tf2pl7kHdbsi
     TXEUawqzXZQtZzt/j3Oya+PZjcRpWNRzprSmd2UxEEPTqDw9LqY5S2B8
     og/NyzWaIYPsKoatcgC7VgYHplcTbzEhGu8BsoEVBGYu3IRy5RkAcZik=",
        dotfile =&gt; [ "bashrc", "bash_profile" ],
      }
    } 
    ```

2.  使用如下内容创建 /etc/puppet/modules/user/files/john-bashrc 文件：

    ```
    export PATH=$PATH:/var/lib/gems/1.8/bin 
    ```

3.  使用如下内容创建 /etc/puppet/modules/user/files/john-bash_profile 文件：

    ```
    . ~/.bashrc 
    ```

4.  运行 Puppet。

#### 工作原理

我们添加了一个名为 user_dotfile 的 define。 对于用户要使用的每个 dotfile 都要调用这个 define 一次。 在本例中，john 用户有两个 dotfiles：.bashrc 和 .bash_profile。 其声明如下：

```
@ssh_user { "john":
    key => ...
    dotfile => [ "bashrc", "bash_profile" ],
} 
```

你既可以指定一个 dotfile，也可以像本例那样指定一个数组形式的列表。

对于每个 dotfile，user_dotfile 将在 modules/user/files 目录下查找相应源的文件。 例如，对名为 bashrc 的 dotfile，Puppet 将查找：

```
modules/user/files/john-bashrc 
```

这将被拷贝为客户节点的如下文件：

```
/home/john/.bashrc 
```

#### 参见本书

*   本章的 使用虚拟资源管理用户 一节

# 有效地分发 cron 任务

### 有效地分发 cron 任务

当你有许多服务器需要执行相同的 cron 作业时，不在同一时间运行它们通常是个好主意。 如果所有作业都要访问一个公共服务器，就会给该服务器带来大量负载， 即使这些服务器不会同时访问公共服务器，所有服务器也会在同一时间处于繁忙状态， 这可能会削减它们提供其他服务的能力。

Puppet 的 inline_template 函数允许我们使用 Ruby 的逻辑根据主机名为 cron 作业设置不同的运行时间。

#### 操作步骤

1.  在一个节点中添加如下代码：

    ```
    define cron_random( $command, $hour )
    {
      cron { $name:
        command =&gt; $command,
        minute  =&gt; inline_template("&lt;%= (hostname+name).hash.abs %60 %&gt;"),
        hour    =&gt; $hour,
        ensure  =&gt; "present",
      }
    }

    cron_random { "hello-world":
        command =&gt; "/bin/echo 'Hello world'",
        hour =&gt; 2,
    }

    cron_random { "hello-world-2":
        command =&gt; "/bin/echo 'Hello world'",
        hour =&gt; 1,
    } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1305713506'
    notice: /Stage[main]//Node[cookbook]/Cron_random[hello-world]/
    Cron[hello-world]/ensure: created
    notice: /Stage[main]//Node[cookbook]/Cron_random[hello-world-2]/
    Cron[hello-world-2]/ensure: created
    notice: Finished catalog run in 1.07 seconds 
    ```

3.  检查 crontab 查看是否成功地配置了 cron 作业：

    ```
    # crontab -l
    # HEADER: This file was autogenerated at Fri Jul 29 10:58:45 +0000
    2011 by puppet.
    # HEADER: While it can still be managed manually, it is definitely
    not recommended.
    # HEADER: Note particularly that the comments starting with
    'Puppet Name' should
    # HEADER: not be deleted, as doing so could cause duplicate cron
    jobs.
    # Puppet Name: hello-world
    25 2 * * * /bin/echo 'Hello world'
    # Puppet Name: hello-world-2
    49 1 * * * /bin/echo 'Hello world' 
    ```

#### 工作原理

我们想要为每个 cron 作业选择一个 **随机的** 执行分钟数； 而不是真正的随机 （或者说，不是每次运行 Puppet 都会改变 cron 作业的运行时间）， 但这也或多或少地保证了每个主机上的每个 cron 作业运行时间的不同。

我们可以使用 Ruby 的 hash 方法实现它，它会对任何对象（本例为一个字符串）计算出一个哈希值。 尽管看上去这个哈希值是随机的，但它每次运行时都相同，所以当再次运行 Puppet 时其值不会改变。

哈希值生成的是一个大整数，而我们想要的是一个 0 到 59 之间的整数，所以我们使用了 Ruby 的 % （模）运算符将其结果限制在这个范围内。因为只有 60 种可能的值，尽管 hash 函数被设计为尽可能产生随机的输出，还是会有些许的碰撞而且这些碰撞对于 minute 应该是均匀分布的。

因为我们希望每个哈希值在不同的主机上是不同的，所以使用主机名做 hash 处理。 然而，我们还希望同一台主机上的不同作业的哈希值也不同，所以联合使用了主机名和作业名 （例如 hello-world）做 hash 处理。

#### 更多用法

在本例中，我们仅对 cron 作业的 minute 进行了随机化，并将 hour 作为 define 定义的一部分。若你同时希望指定要在周几运行，可以在 cron_random 中添加一个附加参数来指定， 可以像下面这样为其指定默认值：

```
define cron_random( $command, $hour, $weekday = "*" ) { 
```

若你想要对 cron 作业的 hour 进行随机化（例如：要做的作业可以在一天之内的任何时间执行， 并且必须将它们均匀分布在所有的 24 个小时上），可以对 cron_random 做如下修改：

```
hour => inline_template("<%= (hostname+name).hash.abs % 24 %>"), 
```

#### 参见本书

*   第一章的 从 cron 运行 Puppet 一节

# 当文件更新时运行命令

### 当文件更新时运行命令

当某个特定的文件更新后 Puppet 就该采取一些行动，这是一个非常常见的模式。 例如，在 rsync 配置片段的例子中，一旦修改了某个片段文件，就会调用 exec 资源更新主配置文件 rsyncd.conf。

每次运行 Puppet，exec 资源都会被运行，除非指定了如下参数中的一个：

*   creates

*   onlyif

*   unless

*   refreshonly => true

refreshonly 参数的意思是：仅当从其他资源（例如一个文件资源）获得一个 notify 才执行 exec 资源。

#### 准备工作

安装 nginx 包（实际上，我们只需要配置文件，但这是获得它的最简单方式）:

```
# apt-get install nginx 
```

#### 操作步骤

1.  创建一个 nginx 模块，其目录结构如下：

    ```
    # mkdir /etc/puppet/modules/nginx
    # mkdir /etc/puppet/modules/nginx/files
    # mkdir /etc/puppet/modules/nginx/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/nginx/manifests/nginx.pp 文件：

    ```
    class nginx {
        package { "nginx": ensure =&gt; installed }

        service { "nginx":
            enable =&gt; true,
            ensure =&gt; running,
        }

        exec { "reload nginx":
            command     =&gt; "/usr/sbin/service nginx reload",
            require     =&gt; Package["nginx"],
            refreshonly =&gt; true,
        }

        file { "/etc/nginx/nginx.conf":
            source  =&gt; "puppet:///modules/nginx/nginx.conf",
            notify  =&gt; Exec["reload nginx"],
            require =&gt; Package["nginx"],
        }
    } 
    ```

3.  复制系统中的 nginx.conf 文件到模块的相应目录：

    ```
    cp /etc/nginx/nginx.conf /etc/puppet/modules/nginx/files 
    ```

4.  添加如下代码到你的配置清单：

    ```
    include nginx 
    ```

5.  对复制到 Puppet 中的 nginx.conf 文件做个小改动，以便进行后续的测试：

    ```
    # echo \# &gt;&gt;/etc/puppet/modules/nginx/files/nginx.conf 
    ```

6.  运行 Puppet：

    ```
    # puppet agent --test

    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1303745502'

    --- /etc/nginx/nginx.conf 2010-02-15 00:16:47.000000000 -0700
    +++ /tmp/puppet-file20110425-31239-158xcst-0 2011-04-25
    09:39:49.586322042 -0600
    @@ -48,3 +48,4 @@
     #         proxy       on;
     #     }
     # }
    +#

    info: FileBucket adding /etc/nginx/nginx.conf as {md5}7bf139588b5e
    cd5956f986c9c1442d44

    info: /Stage[main]/Nginx/File[/etc/nginx/nginx.conf]:
    Filebucketed /etc/nginx/nginx.conf to puppet with sum
    7bf139588b5ecd5956f986c9c1442d44

    notice: /Stage[main]/Nginx/File[/etc/nginx/nginx.conf]/content:
    content changed '{md5}7bf139588b5ecd5956f986c9c1442d44' to '{md5}
    d28d08925174c3f6917a78797c4cd3cc'

    info: /Stage[main]/Nginx/File[/etc/nginx/nginx.conf]: Scheduling
    refresh of Exec[reload nginx]

    notice: /Stage[main]/Nginx/Exec[reload nginx]: Triggered 'refresh'
    from 1 events

    notice: Finished catalog run in 1.69 seconds 
    ```

#### 工作原理

对于大多数服务来说，你应该简单地定义一个服务资源，它从配置文件获得 notify。 这会使 Puppet 重启服务，从而应用改变之后的更新。

然而，nginx 有时不能正确的重新启动，尤其是当使用 Puppet 重新启动时， 所以我为某个站点炮制了一个让 Puppet 运行 /etc/init.d/nginx reload 的补救措施， 用它替代服务的重启。下面阐述它是如何工作的。

将 exec 资源中的 refreshonly 参数设置成 true：

```
exec { "reload nginx":
    command     => "/usr/sbin/service nginx reload",
    require     => Package["nginx"],
    refreshonly => true,
} 
```

所以，仅当它获得通知才会运行。

如果配置文件发生改变，就提供所需的通知（notify）：

```
file { "/etc/nginx/nginx.conf":
    source => "puppet:///modules/nginx/nginx.conf",
    notify => Exec["reload nginx"],
} 
```

每当 Puppet 更新了这个配置文件，它就会运行 exec， 这将会调用如下命令重新加载修改后的配置文件：

```
/usr/sbin/service nginx reload 
```

如果服务支持 reload 命令参数，就会在不中断服务的情况下为 daemon 发出一个重新读取配置文件的进程信号。

实际上，对于本例，更好的方法是为 nginx 服务定义如下的 restart 命令：

```
service { "nginx":
    restart => "/etc/init.d/nginx reload",
} 
```

但是我想要和你分享一些我写的用于展示 notify -> Exec 技术的实际代码， 并且那时，我还不知道 restart 而且它也不存在。 不过作为一种通用的模式，当更新文件后需要采取某种行动时，你会发现它有用。

#### 更多用法

每当遇到资源更新就要采取某些行动的情况，你就可以使用这个类似的模式。可能的用途包括：

*   触发服务重新加载配置文件

*   运行语法检查，然后再重新启动服务

*   连接 config 片段

*   运行测试

*   链接 exec 资源

如果当一个文件（假设名为 somefile）更新后需要执行多个命令，那么在每个命令的 exec 资源声明中都使用 subscribe => File[somfile], （即一旦检测到 somefile 文件的变化就重新执行 exec 资源）会更简单， 而不是在 file 资源中使用 notify 执行命令。 效果是一样的。

> ![注记](img/note.png) 译者注
> 
> 你也可以在 service 资源定义中使用 subscribe， 例如在一个 Redhat 风格的系统中，你可以使用如下的 service 声明：
> 
> ```
>  service { "nginx":
>         enable    =&gt; true,
>         ensure    =&gt; running,
>         restart   =&gt; "/etc/init.d/nginx reload",
>         subscribe =&gt; [ File["/etc/nginx/nginx.conf"],
>                        File["/etc/sysconfig/nginx"] ]
>     } 
> ```
> 
> 在上例中，使用数组为 subscribe 指定了两个要检测的文件， 也就是说，一旦检测到两个文件中的任何一个发生变化就重启服务。

# 使用主机资源

### 使用主机资源

> I am not a number.
> 
> *"The Prisoner"* — Number Six

将机器搬来搬去是很常见的做法，尤其是在云的基础设施中搬动，所以一个特定主机的 IP 经常会改变。 正因为如此，在你的配置中使用硬编码的 IP 地址显然是个坏主意。 如果一台机器要访问另一台（例如，一个应用服务器需要访问一台数据库服务器）， 那么使用主机名而不是 IP 地址会更好。

然而，如何映射主机名到 IP 地址呢？这通常会使用 DNS，但小型组织里一般没有 DNS 服务器， 而大型组织要想无困扰地实现 DNS 更新也是相当费时的。 此外，将 DNS 信息传播到不同机器也需要不同的时间，因此确保快速而一致的 IP 地址更新的一种方法是使用由 Puppet 管理的本地主机表文件 /etc/hosts 中的条目。

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    host { "www.bitfieldconsulting.com":
        ip     =&gt; "109.74.195.241",
        target =&gt; "/etc/hosts",
        ensure =&gt; present,
    } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1305716418'

    notice: /Stage[main]//Node[cookbook]/Host[www.bitfieldconsulting.
    com]/ensure: created

    info: FileBucket adding /etc/hosts as {md5}977bf5811de978b7f041301
    9e77b4abe

    notice: Finished catalog run in 0.21 seconds 
    ```

#### 工作原理

Puppet 将检查由 target 指定的文件，若主机条目不存在，Puppet 就会添加此条目； 若主机条目存在但 IP 地址不同，Puppet 就会更新此条目。

虽然可以使用有别于 /etc/hosts 的目标文件，但这是默认值，你只需要一个目标文件 （即不能使用数组同时指定多个文件）。 我认为明确地指定 host 资源的默认值是个很好的做法， 因为依托默认的行为会有使代码变得脆弱的倾向。

#### 更多用法

将你的 host 资源组织到类中会对你有帮助。例如，你可以将所有数据库服务器的 host 资源放置在一个名为 admin::dbhosts 的类中，然后在所有的 web 服务器中包含这个类。

一些主机可能需要在多个类中同时定义（例如，一个数据库服务器也可以是一个仓库服务器）， 使用虚拟资源可以解决这个问题。例如你可以像下面这样， 在一个单独的类中将所有的主机定义成虚拟资源：

```
class admin::allhosts
{
    @host { "db1.bitfieldconsulting.com":}
} 
```

然后在不同的类中 realize 你需要的主机：

```
class admin::dbhosts
{
    realize( Host["db1.bitfieldconsulting.com"] )
}

class admin::repohosts
{
    realize( Host["db1.bitfieldconsulting.com"] )
} 
```

# 为文件资源指定多个源

### 为文件资源指定多个源

Puppet 的 file 资源有一个实用功能，那就是可以为文件指定多个源。 Puppet 会按顺序查找每一个。如果第一个不存在，就继续查找下一个，以此类推。 你可以利用这个功能指定一个默认文件源的替代品， 若一个甚至一系列替代品不存在就会使用默认的（最后一个列出的）文件源。

#### 操作步骤

1.  添加如下的类到你的配置清单：

    ```
    class mysql::app-config( $app )
    {
        file { "/etc/my.cnf":
            source =&gt; [ "puppet:///modules/admin/${app}.my.cnf",
                        "puppet:///modules/admin/generic.my.cnf", ],
        }
    } 
    ```

2.  使用如下内容创建 /etc/puppet/modules/admin/files/minutespace.my.cnf 文件：

    ```
    # MinuteSpace config file 
    ```

3.  使用如下内容创建 /etc/puppet/modules/admin/files/generic.my.cnf 文件：

    ```
    # Generic config file 
    ```

4.  在一个节点中添加如下代码：

    ```
    class { "mysql::app-config": app =&gt; "minutespace" } 
    ```

5.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1305897071'

    notice: /Stage[main]/Mysql::App-config/File[/etc/my.cnf]/ensure:
    defined content as '{md5}24f04b960f4d33c70449fbc4d9f708b6'

    notice: Finished catalog run in 0.35 seconds 
    ```

6.  检查 Puppet 是否部署了适用于指定应用程序的 config 文件：

    ```
    # cat /etc/my.cnf
    # MinuteSpace config file 
    ```

7.  现在更改节点的定义为：

    ```
    class { "mysql::app-config": app =&gt; "shreddit" } 
    ```

8.  再次运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1305897864'
    --- /etc/my.cnf 2011-05-20 13:17:56.006239489 +0000
    +++ /tmp/puppet-file20110520-15575-1icobgs-0 2011-05-20
    13:24:25.030296062 +0000

    @@ -1 +1 @@
    -# MinuteSpace config file
    +# Generic config file

    info: FileBucket adding /etc/my.cnf as {md5}24f04b960f4d33c70449fb
    c4d9f708b6

    info: /Stage[main]/Mysql::App-config/File[/etc/
    my.cnf]: Filebucketed /etc/my.cnf to puppet with sum
    24f04b960f4d33c70449fbc4d9f708b6

    notice: /Stage[main]/Mysql::App-config/File[/etc/my.cnf]/content:
    content changed '{md5}24f04b960f4d33c70449fbc4d9f708b6' to '{md5}
    b3a6e744c3ab78dfb20e46ff55f6c33c'

    notice: Finished catalog run in 0.93 seconds 
    ```

#### 工作原理

我们定义了 /etc/my.cnf 文件有如下的两个源：

```
file { "/etc/my.cnf":
    source => [ "puppet:///modules/admin/${app}.my.cnf",
                "puppet:///modules/admin/generic.my.cnf", ],
} 
```

$app 的值由任何一个使用它的类传递。在第一个例子中，我们为 app 传递了 minutespace：

```
class { "mysql::app-config": app => "minutespace" } 
```

Puppet 将首先查找 modules/admin/files/minutespace.my.cnf 文件。 由于此文件存在，所以就会使用它。到目前为止，一切正常。

然后我们把 app 的值更改为 shreddit。Puppet 现在会查找 modules/admin/files/shreddit.my.cnf 文件。 由于此文件不存在，所以 Puppet 试图查找源列表中的下一个文件： modules/admin/files/generic.my.cnf。因为此文件存在，所以会将它部署到节点。

#### 更多用法

你可以在任何一个 file 资源中使用这种手段来处理。例如， 一些节点可能需要针对特定主机的配置，而另一些节点则不需要，你可以使用类似于如下的代码实现：

```
file { "/etc/stuff.cfg":
    source => [ "puppet:///modules/stuff/${hostname}.cfg",
                "puppet:///modules/stuff/generic.cfg" ],
} 
```

然后将你的通用配置放在 generic.cfg 文件中。如果主机 cartman 需要一个特殊的配置， 将适用于此主机的配置放在 cartman.cfg 文件中。 cartman.cfg 文件优先于 generic.cfg 文件，因为它在源数组中是首先被列出的。

#### 参见本书

*   第四章的 给类传递参数 一节

# 使用文件资源递归地分发整个目录树

### 使用文件资源递归地分发整个目录树

> To understand recursion, you must first understand recursion.
> 
> — Saying

当你需要使用 Puppet 分发一批位于同一目录下的多个文件时， 就应该考虑使用文件资源的递归特性来替代逐个文件的分发。 如果你在一个目录上设置了 recurse 参数， Puppet 会复制此目录及其子目录中的所有文件到节点目录，例如：

```
file { "/usr/lib/nagios/plugins/custom":
    source  => "puppet:///modules/nagios/plugins",
    require => Package["nagios-plugins"],
    recurse => true,
} 
```

#### 操作步骤

1.  在 Puppet 仓库中的适当位置创建如下的目录树：

    ```
    # mkdir /etc/puppet/modules/admin/files/tree
    # mkdir /etc/puppet/modules/admin/files/tree/a
    # mkdir /etc/puppet/modules/admin/files/tree/b
    # mkdir /etc/puppet/modules/admin/files/tree/c
    # mkdir /etc/puppet/modules/admin/files/tree/a/1 
    ```

2.  添加如下代码到你的配置清单：

    ```
    file { "/tmp/tree":
        source  =&gt; "puppet:///modules/admin/tree",
        recurse =&gt; true,
    } 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304768523'

    notice: /Stage[main]//Node[cookbook]/File[/tmp/tree]/ensure:
    created

    notice: /File[/tmp/tree/a]/ensure: created
    notice: /File[/tmp/tree/a/1]/ensure: created
    notice: /File[/tmp/tree/b]/ensure: created
    notice: /File[/tmp/tree/c]/ensure: created
    notice: Finished catalog run in 1.25 seconds 
    ```

#### 工作原理

如果一个 file 资源上设置了 recurse 参数，而且此资源又是个目录，那么 Puppet 不仅会部署目录自身，而且还会分发其中的所有内容（包括子目录及其文件）。 对于要在节点上部署目录树及其所有的文件来说，这是一个很好的方法。 也可以用这种方法使用一个单独的资源快速创建大量路径。

#### 更多用法

有时你要部署一些文件到一个已经存在的目录，但是首先要移除该目录下的所有没被 Puppet 管理的文件。 例如，Ubuntu 的 /etc/apt/sources.list.d 目录下的文件， 你可能想要确保没有文件存在，即删除那些不是来自 Puppet 的文件。

purge 参数可以为我们实现这个功能。在 Puppet 中定义如下的目录的 file 资源：

```
file { "/etc/apt/sources.list.d":
    ensure  => directory,
    recurse => true,
    purge   => true,
} 
```

联合使用 recurse 和 purge 将会移除 /etc/apt/sources.list.d 目录及其子目录下的所有不被 Puppet 管理的文件。 然后你就可以使用独立的 file 资源将自己的文件部署到 /etc/apt/sources.list.d 目录：

```
file { "/etc/apt/sources.list.d/bitfield.list":
    content => "deb http://packages.bitfieldconsulting.com/lucid-main\n",
} 
```

如果有一个子目录，其中包含了你不想 purge 的文件， 可以将此子目录作为 Puppet 的一个 file 资源单独进行定义， 这样这个子目录中的内容就会保留：

```
file { "/etc/exim4/conf.d/acl":
    ensure => directory,
} 
```

> ![注记](img/note.png)
> 
> 要知道，至少在 Puppet 的当前实现中，采用递归方式复制文件相当慢而且会占用大量服务器的内存。 如果要部署的文件不经常改变，使用 tarball 进行部署比使用这种递归方式会更好。

# 清理过期的旧文件

### 清理过期的旧文件

我们每隔一段时间就会打扫一次房间。 Puppet 的 tidy 资源可以帮助你清理已过期的旧文件，从而减少不必要的磁盘占用。 例如，如果你像 生成报告 一节描述的那样启用了 Puppet 的报告， 就会希望定期清除旧的报告文件。

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    tidy { "/var/lib/puppet/reports":
        age     =&gt; "1w",
        recurse =&gt; true,
    } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent -–test
    info: Retrieving plugin info: Caching catalog for cookbook.
    bitfieldconsulting.com

    notice: /Stage[main]//Node[cookbook]/Tidy[/var/lib/puppet/
    reports]: Tidying File[/var/lib/puppet/reports/cookbook.
    bitfieldconsulting.com/201102241546.yaml]

    notice: /Stage[main]//Node[cookbook]/Tidy[/var/lib/puppet/
    reports]: Tidying File[/var/lib/puppet/reports/cookbook.
    bitfieldconsulting.com/20110214727.yaml]

    ...

    info: Applying configuration version '1306149187'

    notice: /File[/var/lib/puppet/reports/cookbook.bitfieldconsulting.
    com/201102241546.yaml]/ensure: removed

    notice: /File[/var/lib/puppet/reports/cookbook.bitfieldconsulting.
    com/201102141727.yaml]/ensure: removed …

    notice: Finished catalog run in 1.48 seconds 
    ```

#### 工作原理

Puppet 会搜索指定路径下所有匹配 age 参数的文件：本例中的存活时间为 1w（一周）。 也可以搜索子目录（recurse => true）。

任何与你指定的标准匹配的文件都会被删除。

#### 更多用法

你可以指定文件存活时间的单位，单位可以是：秒（s）、小时（h）、天（d）和周（w），下面是几个例子：

*   60s

*   180m

*   24h

*   30d

*   4w

你也可以指定文件的大小，当文件的尺寸大于你指定的尺寸后就会删除它，例如：

```
 size => "100m", 
```

这会删除所有大于 100 megabytes（m） 的文件。单位也可以是：kilobytes（k）或 bytes（b）。

> ![注记](img/note.png)
> 
> 值得注意的是，如果你同时指定了 age 和 size 参数，它们将被视为独立的标准 （即参数之间是 “或” 逻辑而不是 “与” 逻辑）。 例如，你对一个目录的文件指定了如下的参数，那么 Puppet 会删除所有超过一天的文件， 同时也会删除所有尺寸大于 512KB 的文件。
> 
> ```
>  age =&gt; "1d",
>   size =&gt; "512k", 
> ```

# 使用日程表资源

### 使用日程表资源

使用启用了 schedule 参数的资源，你可以控制当前资源何时被应用。 例如，你希望如下的 exec 资源每天应用一次，将资源参数 schedule 设置成了内置的值 daily：

```
exec { "/usr/bin/apt-get update":
    schedule => daily,
} 
```

遗憾的是，给 schedule 参数指定 daily 并不能保证该资源每天都能应用一次。 内置的 daily 仅能限制 exec 资源在一天之内不能被应用多余一次， 但是资源是否被应用以及何时被应用完全取决于 Puppet 是否运行以及何时运行。

正因为如此，使用 schedule 资源才是安排执行其他资源的最佳选择。例如， 你可能想要确保 apt-get update 一小时内运行不超过一次； 或者确保一项维护工作在白天生产时间期间不被执行。

为此，你需要创建自己的 schedule 资源。

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    schedule { "not-in-office-hours":
        period =&gt; daily,
        range  =&gt; [ "17:00-23:59", "00:00-09:00" ],
        repeat =&gt; 1,
    }

    exec { "/bin/echo Doing maintenance!":
        schedule =&gt; "not-in-office-hours",
    } 
    ```

2.  Run Puppet。

#### 工作原理

我们创建了名为 not-in-office-hours 的 schedule，它指定了重复周期为 daily, 且指定了允许的时间范围为下午 5 点到次日早上 9 点：

```
 period  => daily,
    range   => [ "17:00-23:59", "00:00-09:00" ], 
```

我们同时设置了在每个时间周期内资源被应用的最大次数为 1：

```
 repeat => 1, 
```

然后，我们在如下的 exec 资源中使用这个自定义的 schedule：

```
 exec { "/bin/echo Doing maintenance!":
        schedule => "not-in-office-hours",
    } 
```

若 exec 资源不使用 schedule 参数，每次运行 Puppet，这个 exec 资源就会被应用一次。 现在，Puppet 将依照 not-in-office-hours 资源的设置做如下测试：

*   当前时间是否在允许的范围内

*   在一个重复周期内，资源是否已经应用到了允许的最大次数

例如，让我考察在如下连续的几个小时内如果 Puppet 每小时执行一次将会发生什么：

*   4 p.m.：超出了允许的时间范围，所以 Puppet 不会做任何事情。

*   5 p.m.：在允许的时间范围内，且在此重复周期内还没有运行过，所以 Puppet 会应用此资源。

*   6 p.m.：在允许的时间范围内，且在此重复周期内已经运行过一次，由于已经到达了 repeat 设置的最大值，所以 Puppet 不会做任何事情。

依此类推，直到第二天再重复这一过程。

#### 更多用法

如果需要，你可以增大 repeat 参数的值，例如：在每小时内运行一个作业不超过 6 次：

```
 period => hourly,
    repeat => 6, 
```

要记住这不能保证每个小时此作业都会运行 6 次。这只是设置了一个上限。 无论 Puppet 经常运行还是会发生其他情况，如果一个小时内已经运行了 6 次就不会再次运行。 如果 Puppet 每天仅执行一次，那么这个作业也仅执行一次。 因此，要确保某事件在特定的时间 **不会** 发生（或者 **不** 超过给定的频率），使用 schedule 是最有用的。

# 资源的审计

### 资源的审计

不是每个问题都有技术性的答案。我曾经诊断过一台服务器，它对 ping、SSH 或控制台连接均无响应。 我不能确定这到底是硬件故障还是软件故障。

当我电话询问主机所在位置的站点客服时，奥秘最终被揭开了。他们告诉我： 先期抵达的两名不明身份的男子，进入大厦直奔服务器机房，拔下了机器，并轻易地将机器带出了大厦。 后来我们发现，在机房所在的地区（Missouri，M.O.）曾发生过一连串的电脑失窃案。

从这个案例获得的重要信息是：最终知道了谁对你的服务器做了些什么，这还是不错的。

模拟运行模式（使用 --noop 开关）审计由 Puppet 控制的任何机器的变化是一种简单的方法。 然而，Puppet 还有一个专门的审计功能，它可以报告资源或特定属性的变化。

#### 操作步骤

使用 audit 元参数定义一个资源：

```
file { "/etc/passwd":
    audit => [ owner, mode ],
} 
```

#### 工作原理

**元参数**（**metaparameter**）是一种可以应用到任何资源的参数，不仅用于指定类型。 audit 元参数告诉 Puppet，你想要记录和监视关于这个资源的某些事件。 其值可以是你要审计的一个参数列表。

在本例中，一旦 Puppet 运行，它就开始记录 /etc/passwd 文件的属主和权限模式。 无论是属主改变还是权限改变，例如：如果你运行了如下命令：

```
# chmod 666 /etc/passwd 
```

当下一次运行 Puppet 时，它将会察觉到这个变化并将其记入日志：

```
notice: /Stage[main]//Node[cookbook]/File[/etc/passwd]/mode: audit
change: previously recorded value 644 has been changed to 666 
```

#### 更多用法

对于需要审记大型网络中的机器所做的任何改变（无论是恶意的还是意外的）的情况， 使用 audit 特性是非常有用的。 你还可以使用 tagmail 报告特性通过 e-mail 自动发送审计通知信息。

对于要时刻关注不被 Puppet 管理的任何事物（例如生产服务器上的应用程序代码）的情况， 使用 audit 特性也是非常方便的。 你可以从 [`www.puppetlabs.com/blog/allabout-auditing-with-puppet/`](http://www.puppetlabs.com/blog/allabout-auditing-with-puppet/) 获取更多关于 Puppet 审计能力的信息。

如果你希望审计一个资源的一切，可以使用如下的代码：

```
file { "/etc/passwd":
    audit => all,
} 
```

#### 参见本书

*   第二章的 测试你的 Puppet 配置清单 一节

*   第二章的 通过 Email 发送包含特定标签的日志信息 一节

# 临时禁用资源

### 临时禁用资源

有时候，你希望在不会干扰其他工作的情况下临时禁用一个资源。例如， 你可能想要调整服务器上的一个配置文件，经过 Puppet 的不断测试，直到获得你想要的确切设置。 在此期间你不想让 Puppet 覆盖旧版本的配置，为此你可以在资源上设置如下的 noop 元参数：

```
 noop => true, 
```

#### 操作步骤

1.  添加如下代码到你的配置清单：

    ```
    file { "/tmp/test.cfg":
        content =&gt; "Hello, world!\n",
        noop =&gt; true,
    } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1306159566'

    notice: /Stage[main]//Node[cookbook]/File[/tmp/test.cfg]/ensure:
    is absent, should be file (noop)

    notice: Finished catalog run in 0.53 seconds 
    ```

3.  测试之后，移除 noop 参数：

    ```
    file { "/tmp/test.cfg":
        content =&gt; "Hello, world!\n",
    } 
    ```

4.  再次运行 Puppet：

    ```
    # puppet agent --test

    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1306159705'

    notice: /Stage[main]//Node[cookbook]/File[/tmp/test.cfg]/ensure:
    defined content as '{md5}746308829575e17c3331bbcb00c0898b'

    notice: Finished catalog run in 0.52 seconds 
    ```

#### 工作原理

首次运行 Puppet 时，由于 noop 元参数被设置为 true， 因此对于这一特定资源， 与你使用 --noop 标志运行 Puppet 的效果是一样的。 Puppet 会指出此资源将被应用， 除此之外什么也没做。

再次运行 Puppet 时，由于 noop 元参数被移除，所以此资源会被正常地应用到节点。

# 管理时区

### 管理时区

> I try to take one day at a time, but sometimes several days attack at once.
> 
> — Ashleigh Brilliant

你迟早会遭遇由于服务器的时区不同而带来的各种奇怪问题。 为了避免此类问题的发生，确保所有服务器都使用相同的时区是明智之举。 无论这些服务器所处的地理位置在何处，都应该使用相同的时区（GMT 是合乎逻辑的选择）。

除非服务器是由太阳能供电的，否则我认为没有任何理由不关心服务器的时区设置。

#### 操作步骤

1.  使用如下内容创建 /etc/puppet/modules/admin/manifests/gmt.pp 文件：

    ```
    class admin::gmt
    {
        file { "/etc/localtime":
            ensure =&gt; link,
            target =&gt; "/usr/share/zoneinfo/GMT",
        }
    } 
    ```

2.  在所有的节点中添加如下代码：

    ```
    include admin::gmt 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1304955158'

    info: FileBucket adding /etc/localtime as {md5}02b73b0cf0d96e2f75c
    ae56b178bf58e

    info: /Stage[main]/Admin::Gmt/File[/etc/localtime]: Filebucketed /
    etc/localtime to puppet with sum 02b73b0cf0d96e2f75cae56b178bf58e

    notice: /Stage[main]/Admin::Gmt/File[/etc/localtime]/ensure:
    ensure changed 'file' to 'link'

    notice: Finished catalog run in 1.94 seconds 
    ```

#### 更多用法

如果你想要使用其他的时区，请在 /usr/share/zoneinfo 中选择相应的文件，例如：US/Eastern。