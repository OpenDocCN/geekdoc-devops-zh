# 书写更优质的配置清单

## 书写更优质的配置清单

> There are only two kinds of programming languages: those people always bitch about and those nobody uses.
> 
> — Bjarne Stroustrup

在本章中，我们将学习如下内容：

*   使用资源的数组

*   使用 define 资源

*   指定资源的依赖关系

*   使用节点继承

*   使用类的继承和重载

*   给类传递参数

*   书写可重用的跨平台配置清单

*   获得系统的环境信息

*   导入动态信息

*   从 CSV 文件导入数据

*   给 Shell 命令传递参数

你的 Puppet 配置清单实际上就是你所管理的整个基础设施的活生生的文档。 保持配置清单的整洁和良好组织使其更容易维护和理解是个明智之举。 Puppet 为你方便管理代码提供了许多工具，其中包括：

*   数组（Arrays）

*   定义（Defines）

*   依赖（Dependencies）

*   继承（Inheritance）

*   类参数（Class parameters）

我们将看到如何使用上面的所有工具（甚至更多）。通过对本章的阅读，尝试使用这些例子做实验， 看看利用这些功能是否能帮你简化和改善你的 Puppet 代码。

# 使用资源的数组

### 使用资源的数组

你可以将一切均视为 Puppet 的资源，你也可以使用数组资源。 使用这种想法，可以重构你的配置清单，使其更加简洁而清晰。

#### 操作步骤

1.  在你的配置清单中，将同一类型资源的几个实例定义在一个类中，例如：packages：

    ```
    package { "sudo" : ensure =&gt; installed }
    package { "unzip" : ensure =&gt; installed }
    package { "locate" : ensure =&gt; installed }
    package { "lsof" : ensure =&gt; installed }
    package { "cron" : ensure =&gt; installed }
    package { "rubygems" : ensure =&gt; installed } 
    ```

2.  使用如下的方法用数组将它们组织在一起并用一个 package 资源代替：

    ```
    package { [ "cron",
                "locate",
                "lsof",
                "rubygems"
                "screen",
                "sudo"
                "unzip" ]:
        ensure =&gt; installed,
    } 
    ```

#### 工作原理

绝大多数的 Puppet 资源类型都可以使用数组来替换单个的资源名， 并会为每个数组的元素创建一个实例。 你为资源提供的所有参数（例如：ensure => installed）都会分配给每个新的资源实例。

#### 参见本书

*   第三章的 遍历多个项目 一节

# 使用 define 资源

### 使用 define 资源

> Girl number twenty unable to define a horse!" said Mr. Gradgrind.?—?Charles Dickens.
> 
> — Hard Times

除非你知道如何定义你想要什么，不然不会得到预期的结果。 在上一节的示例中，我们看到了如何将同类资源组合成数组从而减少冗余代码。 然而，这种技术有一个限制，那就是所有的资源必须使用相同的参数。 当你有一组资源拥有一些公用的参数而其中一些资源确有不同的参数时， 就需要使用 define 资源将它们组合在一起。

#### 操作步骤

1.  在配置清单中添加如下代码：

    ```
    define tmpfile() {
        file { "/tmp/$name":
            content =&gt; "Hello, world",
        }
    }

    tmpfile { ["a", "b", "c"]: } 
    ```

2.  运行 Puppet：

    ```
    notice: /Stage[main]//Node[cookbook]/Tmpfile[a]/File[/tmp/a]/
    ensure: defined content as '{md5}bc6e6f16b8a077ef5fbc8d59d0b931b9'
    notice: /Stage[main]//Node[cookbook]/Tmpfile[b]/File[/tmp/b]/
    ensure: defined content as '{md5}bc6e6f16b8a077ef5fbc8d59d0b931b9'
    notice: /Stage[main]//Node[cookbook]/Tmpfile[c]/File[/tmp/c]/
    ensure: defined content as '{md5}bc6e6f16b8a077ef5fbc8d59d0b931b9' 
    ```

#### 工作原理

你可以认为 **define** 就像是一个饼干切割器。它描述了一种模式，Puppet 可以用它创建许多类似的资源。任何时候你都可以在你的配置清单中声明 tmpfile 实例，Puppet 将会插入包含在 tmpfile 定义中的所有资源。

在我们的例子中，名为 tmpfile 的 define 包含了一个 file 资源， 其 content 为 "Hello, world"；其 path 为 "/tmp/$name"。 如果你用名字 foo 声明了一个 tmpfile 实例，如下所示：

```
tmpfile { "foo": } 
```

会被 Puppet 要创建的实例名字所代替。这就像是我们创建了一个新的资源类型： tmpfile，它包含一个参数（即其名字）。

与常规资源一样，我们不仅可以为其传递一个字符串的名字，我们还可以传递一个数组名， Puppet 会对每一个数组元素创建一个 tmpfile 实例，就像上面的例子那样。

#### 更多用法

在上面的例子中，我们定义的 define 仅有一个名字参数，不同实例的名字不同。 但是我们可以为其添加任何我们想要的参数，只要我们在 define 中声明这些参数即可：

```
define tmpfile( $greeting ) {
    file { "/tmp/$name":
        content => $greeting,
    }
} 
```

当我们声明一个资源的实例时，可以为其传递参数值，例如：

```
tmpfile{ "foo": greeting => "Hello, world" } 
```

你可以使用逗号间隔的列表同时声明多个参数：

```
define webapp( $domain, $path, $platform ) {
    ...
}

webapp { "mywizzoapp":
    domain   => "mywizzoapp.com",
    path     => "/var/www/apps/mywizzoapp",
    platform => "Rails",
} 
```

这是对某些常见的资源进行抽象的一项强大的技术，抽象出资源之间的共性并保存在一个地方， 你就**不必每次都做重复劳动**（**Don’t Repeat Yourself**）。 在上面的例子中，webapp 里或许有许多独立的资源：软件包（packages）、配置文件（config files）、 源码检出（source code checkouts)、虚拟主机（virtual hosts）等等， 但是除了我们为 webapp 提供的参数不同之外，所有实例要执行的工作都相同。 这些可能会在模板中引用，例如：为一个虚拟主机设置域名。

# 指定资源的依赖关系

### 指定资源的依赖关系

> Remove wrapper, open mouth, insert muffin, eat.
> 
> — Instructions on 7-11 muffin packaging

为确保事物以正确的顺序发生，你可以在 Puppet 中指定一个资源依赖另一个资源， 例如：你必须先安装软件包 X 然后再启动它提供的服务，因此应该标记这项服务依赖于软件包 X。 Puppet 会按要求的顺序排出它遇到的所有依赖。

在一些配置管理系统中，资源按照你的书写顺序被应用，换句话说，资源被应用的顺序是隐式的。 Puppet 则不是这种情况，资源或多或少以一种随机（但一致）顺序被应用， 除非你明确地使用依赖关系指出应用的顺序。 一些人更喜欢隐式的方式，因为你可以按照你需要的执行顺序书写资源定义， 并且这就是它们被执行的方式。

另一方面，许多情况下资源的顺序无关紧要。使用隐式风格的系统时， 你不能明确地告诉系统，资源 B 是否被写在了资源 A 之后，由于资源 B 依赖于 A； 或者说书写的顺序是否正确，即资源 A 写在了资源 B 之前。 这使得重构更加困难，因为移动资源有可能会打破一些隐式的依赖关系。

虽然 Puppet 会让你多做一点儿工作，就是预先指定依赖关系， 但是这样产生的代码会更清晰且更易于维护。让我们看一个例子。

#### 操作步骤

1.  使用如下的内容创建一个新文件 /etc/puppet/modules/admin/manifests/ntp.pp：

    ```
    class admin::ntp {
        package { "ntp":
            ensure =&gt; installed,
        }

        service { "ntp":
            ensure  =&gt; running,
            require =&gt; Package["ntp"],
        }

        file { "/etc/ntp.conf":
            source  =&gt; "puppet:///modules/admin/ntp.conf",
            notify  =&gt; Service["ntp"],
            require =&gt; Package["ntp"],
        }
    } 
    ```

2.  复制已经存在的 ntp.conf 文件到 Puppet 的如下目录：

    ```
    # cp /etc/ntp.conf /etc/puppet/modules/admin/files 
    ```

3.  在 nodes.pp 中将 admin::ntp 类添加到你的服务器：

    ```
    node cookbook {
        include admin::ntp
    } 
    ```

4.  现在删除系统中已存在的 ntp.conf 文件：

    ```
    # rm /etc/ntp.conf 
    ```

5.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1302960655'
    notice: /Stage[main]/Admin::Ntp/File[/etc/ntp.conf]/ensure:
    defined content as '{md5}3386aaad98dd5e0b28428966dac9e1f5'
    info: /Stage[main]/Admin::Ntp/File[/etc/ntp.conf]: Scheduling
    refresh of Service[ntp]
    notice: /Stage[main]/Admin::Ntp/Service[ntp]: Triggered 'refresh'
    from 1 events
    notice: Finished catalog run in 2.36 seconds 
    ```

#### 工作原理

在本例中演示了两种类型的依赖： require 和 notify。在第一种情况中， ntp 服务要求 ntp 包资源首先被应用：

```
service { "ntp":
    ensure  => running,
    require => Package["ntp"],
} 
```

在第二种情况中，NTP 的配置文件设置成了 notify（通知）ntp 服务；换句话说， 一旦发现配置文件有变化，Puppet 就应该使用新的配置文件重新启动 ntp 服务：

```
file { "/etc/ntp.conf":
    source  => "puppet:///modules/admin/ntp.conf",
    notify  => Service["ntp"],
    require => Package["ntp"],
} 
```

这意味着服务依赖于配置文件以及所安装的软件包，Puppet 会按照如下的正确顺序来应用这三个资源：

```
Package["ntp"] -> File["/etc/ntp.conf"] ~> Service["ntp"] 
```

事实上，这是指定相同依赖关系链的另一种方法。添加上面这行到你的配置清单中， 就会产生和上例中使用 require 和 notify 参数的同样效果 （-> 表示 require，~> 表示 notify）。然而，我更喜欢使用 require 和 notify， 因为依赖关系被定义成了资源的一部分，因此更容易看清将会发生什么。 不过，对于复杂的依赖关系链，你可能想使用 -> 符号来代替。

#### 更多用法

你也可以指定一个资源依赖于某个类：

```
require => Class["my-apt-repo"] 
```

你不仅可以指定资源和类之间的依赖关系，甚至可以指定 **collections** 之间的依赖关系：

```
Yumrepo <| |> -> Package <| provider == yum |> 
```

这是一种功能强大的表达方式，所有 provider 是 yum 的 package 资源被应用之前， 所有的 yumrepo 资源首先都应该被应用。

> ![注记](img/note.png) 历史说明:
> 
> 在 Puppet 2.7 版本之前， 所有资源编目都以非确定性的方式被应用， 这意味着每次 Puppet 运行资源的顺序都会不同。这可能会导致一些有趣的问题， 比如一个 Puppet 配置清单在一台机器上能运行成功而在另一台机器上却运行失败。 经过 Puppet Labs 的努力，现在这种情况已经不会发生。现在 Puppet 既保证可靠成功，又保证可靠失败（"either succeed reliably, or fail reliably"）。 如果你还在使用早期版本并且遇到了这种问题，请更新到新版本。

# 使用节点继承

### 使用节点继承

将所有服务器都放在一个篮子里的系统管理员是勇敢的（或者说是愚蠢的）。 比方说，你将服务器托管在三个不同的提供商：WreckSpace、GoDodgy 和 VerySlow。 他们有不同的数据中心且分布在不同的地理位置，所以你需要对服务器配置做一些小的改动， 以适应每个提供商。你有几种类型的服务器，它们随机分布在三个不同的提供商。

使用 Puppet 的一种实现方法是在节点定义中设置一个变量用于告知每个节点处于哪个提供商：

```
node webserver127 {
    $provider = "VerySlow"
    include admin::basics
    include admin::ssh
    include admin::ntp
    include puppet::client
    include backup::client
    include webserver
}

node loadbalancer5 {
    $provider = "WreckSpace"
    include admin::basics
    include admin::ssh
    include admin::ntp
    include puppet::client
    include backup::client
    include loadbalancer
} 
```

正如你看到的，结果中存在大量的重复行。而使用下面的方法会比上面的方法容易得多。例如： 首先简单地定义一种 WreckSpace 节点，然后使用从这个节点 **继承**（**inherit**） 的方法创建新节点，在新节点中只需要包含它要执行的类：比如 loadbalancer 或 webserver 即可。

#### 操作步骤

1.  为所有的节点创建一个基类，它仅包含每个节点都要执行的类，例如：

    ```
    node server {
        include admin::basics
        include admin::ssh
        include admin::ntp
        include puppet::client
        include backup::client
    } 
    ```

2.  通过这个 server 节点创建三个不同的子类，每个子类都设置了适当的 provider 变量：

    ```
    node wreckspace_server inherits server {
        $provider = "WreckSpace"
    }

    node gododgy_server inherits server {
        $provider = "GoDodgy"
    }

    node veryslow_server inherits server {
        $provider = "VerySlow"
    } 
    ```

3.  现在，为了要在 VerySlow 创建一个新的 Web 服务器，仅需要从 veryslow_server 继承即可：

    ```
    node webserver904 inherits veryslow_server {
        include webserver
    } 
    ```

#### 工作原理

当一个节点继承自另一个节点，它会应用父节点的所有配置。 然后你可以添加任何代码，从而使得这个节点成为有别于其他节点的特殊节点。

你可以配置一个节点继承自另外一个节点，而另外一个节点也可以继承自其它节点等。 但是你不能继承自多个节点（即不能多重继承），因此不能使用如下方式定义节点：

```
node movable_server inherits gododgy_server, veryslow_server,
wreckspace_server {
 # This won't work
} 
```

#### 更多用法

与定义一个普通节点一样，你可以指定一个节点列表，这些节点都继承自相同的节点定义：

```
node webserver1, webserver2, webserver3 inherits wreckspace_server {
    ...
} 
```

另外，你也可以使用正则表达式匹配多个服务器节点：

```
node /webserver\d+.veryslow.com/ inherits veryslow_server {
    ...
} 
```

#### 参见本书

*   本章的 使用类的继承和重载 一节

# 使用类的继承和重载

### 使用类的继承和重载

正如节点可以从其他节点继承一样，这可以为相似的节点复制很多代码， 同样的思想也可以用于类。

例如，假设你有一个管理 Apache Web 服务器的 apache 类， 你想要使用略有不同的配置文件设置一台新的 Apache 机器?—?也许是监听的端口不同。

你可以复制整个 apache 类，除了配置文件。另外，你可以从 apache 类中提取配置文件并创建两个新类，每个新类都包含 apache 基类并添加一个新版本的配置文件。

一个更简洁的做法是从 apache 类继承，而后仅覆盖其配置文件。

#### 准备工作

1.  为新的 apache 模块创建目录结构：

    ```
    # mkdir /etc/puppet/modules/apache
    # mkdir /etc/puppet/modules/apache/manifests
    # mkdir /etc/puppet/modules/apache/files 
    ```

2.  使用如下代码创建 /etc/puppet/modules/apache/manifests/init.pp 文件：

    ```
    class apache {
        package { "apache2-mpm-worker": ensure =&gt; installed }

        service { "apache2":
            enable  =&gt; true,
            ensure  =&gt; running,
            require =&gt; Package["apache2-mpm-worker"],
        }

        file { "/etc/apache2/ports.conf":
            source =&gt; "puppet:///modules/apache/port80.conf.apache",
            notify =&gt; Service["apache2"],
        }
    } 
    ```

3.  若 Apache 软件包还未安装，安装它，复制其包含的文件 ports.conf 到 Puppet：

    ```
    # apt-get install apache2-mpm-worker

    # cp /etc/apache2/ports.conf \
    /etc/puppet/modules/apache/files/port80.conf.apache 
    ```

4.  添加 apache 类到一个节点，例如：

    ```
    node cookbook {
        include apache
    } 
    ```

5.  运行 Puppet 验证配置清单是否正常工作。

#### 操作步骤

1.  创建 port80.conf.apache 文件的一个新版本 port8000.conf.apache，并做如下改动：

    ```
    NameVirtualHost *:8000
    Listen 8000 
    ```

2.  使用如下内容创建一个新文件 /etc/puppet/modules/apache/manifests/port8000.pp：

    ```
    class apache::port8000 inherits apache {
        File["/etc/apache2/ports.conf"] {
            source =&gt; "puppet:///modules/apache/port8000.conf.apache",
        }
    } 
    ```

3.  改变你的节点配置，使其包含 apache::port8000 类而不是 apache 类：

    ```
    node cookbook {
        include apache::port8000
    } 
    ```

4.  运行 Puppet 检查它是否会按照要求的那样发生改变：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1302970905'
    --- /etc/apache2/ports.conf 2010-11-18 14:16:23.000000000 -0700
    +++ /tmp/puppet-file20110416-6165-pzeivi-0 2011-04-16
    10:21:47.204294334 -0600
    @@ -5,8 +5,8 @@
    # Debian etch). See /usr/share/doc/apache2.2-common/NEWS.Debian.gz and
    # README.Debian.gz

    -NameVirtualHost *:80
    -Listen 80
    +NameVirtualHost *:8000
    +Listen 8000

    &lt;IfModule mod_ssl.c&gt;
    # If you add NameVirtualHost *:443 here, you will also have
    to change
    info: FileBucket adding /etc/apache2/ports.conf as {md5}38b31d2032
    6f3640a8dfbe1ff5d1c4ad
    info: /Stage[main]/Apache/File[/etc/apache2/ports.conf]:
    Filebucketed /etc/apache2/ports.conf to puppet with sum
    38b31d20326f3640a8dfbe1ff5d1c4ad
    notice: /Stage[main]/Apache/File[/etc/apache2/ports.conf]/content:
    content changed '{md5}38b31d20326f3640a8dfbe1ff5d1c4ad' to '{md5}4
    1d9d446f779c55f13c5fe5a7477d943'
    info: /Stage[main]/Apache/File[/etc/apache2/ports.conf]:
    Scheduling refresh of Service[apache2]
    notice: /Stage[main]/Apache/Service[apache2]: Triggered 'refresh'
    from 1 events
    notice: Finished catalog run in 4.85 seconds 
    ```

#### 工作原理

让我们再看看这个新类：

```
class apache::port8000 inherits apache {
    File["/etc/apache2/ports.conf"] {
        source => "puppet:///modules/apache/port8000.conf.apache",
    }
} 
```

你可以从类名后看出，此类继承（inherits）自 apache 类。这将创建一个与 apache 类完全相同的副本，除了跟随其后的变化。

如下的代码片段：

```
File["/etc/apache2/ports.conf"] { 
```

指定了我们想要改变父类中名为 /etc/apache2/ports.conf 的 file 资源 （注意 File 是首字母大写的，这意味着，我们指的是现有的资源，而不是定义一个新资源）。

如下的代码片段：

```
source => "puppet:///modules/apache/port8000.conf.apache", 
```

意味着我们将使用一个新的值覆盖父类中 source 资源的参数值。 如果我们复制整个 apache 类的定义并改变资源 source 的值，那么结果将是完全一样的：

```
class apache {
    package { "apache2-mpm-worker": ensure => installed }

    service { "apache2":
        enable  => true,
        ensure  => running,
        require => Package["apache2-mpm-worker"],
    }

    file { "/etc/apache2/ports.conf":
        source => "puppet:///modules/apache/port8000.conf.apache",
        notify => Service["apache2"],
    }
} 
```

#### 更多用法

首先覆盖被继承的类看上去有些复杂。然而一旦你掌握了这种思想，就会发现这实际上很简单。 这是一种使你的配置清单更具可读性的强大方式，因为这样消除了大量的重复代码， 使你仅专注于编写不同的代码部分。下面给出几种使用覆盖的方法。

##### 取消参数的定义

有时候你不想改变一个参数的值，只是想完全移除它的值。 为了实现这一点，可以使用 undef 值覆盖原有值。 其结果就像是此参数从未在先前定义过一样。

```
class apache::norestart inherits apache {
    File["/etc/apache2/ports.conf"] {
        notify => undef,
    }
} 
```

##### 使用 +> 操作符添加额外的值

与替换一个值类似，你可能想要在父类定义的基础上添加更多的值。 使用 **plusignment** 操作符 +> 可以实现这一功能：

```
class apache::ssl inherits apache {
    file { "/etc/ssl/certs/cookbook.pem":
        source => "puppet:///modules/apache/cookbook.pem",
    }

    Service["apache2"] {
        require +> File["/etc/ssl/certs/cookbook.pem"],
    }
} 
```

操作符 +> 在父类定义的值的基础上添加一个值（或使用方括号括起来的一个数组）。 对于上面的例子，我们最终得到的代码相当于：

```
service { "apache2":
    enable  => true,
    ensure  => running,
    require => [ Package["apache2-mpm-worker"], File["/etc/ssl/certs/
 cookbook.pem"] ],
} 
```

##### 禁用资源

继承和覆盖最常见的用途之一就是禁用服务或其他资源：

```
class apache::disabled inherits apache {
    Service["apache2"] {
        enable => false,
        ensure => stopped,
    }
} 
```

#### 参见本书

*   第四章的 使用节点继承 一节

*   第四章的 给类传递参数 一节

*   第三章的 使用标准的命名规范 一节

# 给类传递参数

### 给类传递参数

有时对一个类的某些方面进行 **参数化**（**parameterize**）是很有用的。例如， 你可能需要管理不同版本的 gem 软件包，既可以为每一种版本创建分离的单独的类， 也可以使用继承和覆盖，为一个类传递一个版本号作为参数。

#### 操作步骤

1.  声明参数作为如下类定义的一部分：

    ```
    class eventmachine( $version ) {
        package { "eventmachine":
            provider =&gt; gem,
            ensure   =&gt; $version,
        }
    } 
    ```

2.  然后在一个节点上使用如下语法包含类：

    ```
    class { "eventmachine": version =&gt; "0.12.8" } 
    ```

#### 工作原理

类定义：

```
class eventmachine( $version ) { 
```

与普通的类定义一样，不同之处在于它携带了一个参数：$version。 在此类中，我们定义了如下的 package 资源：

```
package { "eventmachine":
    provider => gem,
    ensure   => $version,
} 
```

这是一个 gem 包资源，并且要求安装 $version 版本的软件包。

当你在一个节点上包含这个类时， 替换通常的语法：

```
include eventmachine 
```

为如下的 class 语句：

```
class { "eventmachine": version => "0.12.8" } 
```

这有相同的效果，只是同时为参数 $version 指定了一个值。

#### 更多用法

你可以为一个类指定多个参数：

```
class mysql( $package, $socket, $port ) { 
```

使用同样的方法为其传递参数值：

```
class { "mysql":
    package => "percona-sql-server-5.0",
    socket  => "/var/run/mysqld/mysqld.sock",
    port    => "3306",
} 
```

你也可以为一些参数指定默认值：

```
class mysql( $package, $socket, $port = "3306" ) { 
```

或者使用如下代码片段为所有参数指定默认值：

```
class mysql(
    package = "percona-sql-server-5.0",
    socket = "/var/run/mysqld/mysqld.sock",
    port = "3306" ) { 
```

与 define 不同，一个节点上只能存在一个参数化的类实例。 所以当你需要针对一个资源创建多个不同的实例时，应该使用 define 取代类的参数化。

#### 参见本书

*   本章的 使用节点继承 一节

*   本章的 使用类的继承和重载 一节

# 书写可重用的跨平台配置清单

### 书写可重用的跨平台配置清单

每个系统管理员都梦想着使用统一的、同质的基础设施，相同的机器都运行相同的操作系统且其版本相同。 正如生活中的其他领域一样，现实往往是凌乱的、往往与理想情况不符。

你很可能会负责管理一堆使用年限不同、架构不同、运行不同发行版本的不同内核的服务器， 常常是分散在不同的数据中心和不同的互联网服务供应商（ISP）。

在这种情况下，会造成系统管理员心中对在 for 循环中执行 SSH（"SSH in a for loop"）的恐惧， 因为在每个服务器上即使执行相同的命令，也可能产生不同的、不可预知的、甚至是危险的结果。

当然，我们应该努力将旧服务器进行更新，并将一切尽可能工作在一个单一的参考平台之上， 从而使管理更简单，更廉价，更可靠。但在达到这一目标之前，我们可以使用 Puppet， 它可以使我们更容易地应对异构环境（heterogeneous environments）。

#### 操作步骤

1.  如果你有一些放置在不同数据中心的服务器，这些服务器需要略有不同的网络配置， 例如，使用节点继承技术来封装差异：

    ```
    node wreckspace_server inherits server {
            include admin::wreckspace_specific
    } 
    ```

2.  你需要应用相同的配置清单到运行着不同 OS 发行版本的服务器，其重要差别可能在于软件包名、 服务名以及配置文件的存放位置。可以通过在一个类中使用选择器（selectors） 捕获这些差异并设置全局变量的值：

    ```
    $ssh_service = $operatingsystem? {
         /Ubuntu|Debian/ =&gt; "ssh",
         default =&gt; "sshd",
    } 
    ```

    之后你就不用担心配置清单其他部分的差异了，当你要提及这些时， 可以放心的使用变量，它会根据具体环境正确的指向相应的正确值：

    ```
    service { $ssh_service:
        ensure =&gt; running,
    } 
    ```

3.  我们经常需要配合不同的架构；这可能会影响共享库的路径，也可能需要不同版本的软件包。 同样地，尝试在一个单一的 architecture 类中封装所需要的全局变量的值：

    ```
    $libdir = $architecture ? {
        x86_64 =&gt; "/usr/lib64",
        default =&gt; "/usr/lib",
    } 
    ```

    之后在需要一个架构相关的值时，你就可以引用这些全局变量的值， 无论是在配置清单中引用还是在模板中引用均可：

    ```
    ; php.ini
    [PHP]
    ; Directory in which the loadable extensions (modules) reside.
    extension_dir = &lt;%= libdir %&gt;/php/modules 
    ```

#### 工作原理

这种方法的优点（可以称为“自上而下”）是你仅需要进行一次选择。 另一种是自下而上的方法，使用这种方法，在全部配置清单中你随处可见 使用 selector 或 case 语句的设置：

```
service { $operatingsystem? {
    /Ubuntu|Debian/ => "ssh",
    default         => "sshd" }:
    ensure          => running,
} 
```

这不仅会产生许多重复代码，而且使代码难于阅读。另外，当需要管理一种新的操作系统时， 你必须在所有的配置清单中进行修改，而不是只修改一处。

#### 更多用法

如果你正在为一个公共的发布（例如 Puppet Forge）编写模块， 使其尽可能的跨平台会让模块变得更有价值。尽你所能，在不同发布、不同平台、不同架构上测试， 并且添加适当的变量，使模块尽可能的应用到各种情况。

如果你正在使用一个公共模块，并修改它适应自己的环境， 如果你认为你的修改可能会帮助到其他人，可以考虑向公共版本提交你的更新。

即使你不想发布你的模块，请铭记：一个模块可能会在生产环境中运行很长一段时间， 并且可能会对其做许多适应环境的改变。 如果从设计模块的一开始就考虑到这些，那么你（或者最终维护你的代码人）的生活将会变得更轻松。

> Always code as if the guy who ends up maintaining your code will be a violent psychopath who knows where you live.
> 
> — Dave Carhart

#### 参见本书

*   本章的 使用节点继承 一节

*   本章的 使用类的继承和重载 一节

*   第九章的 使用公共模块 一节

# 获得系统的环境信息

### 获得系统的环境信息

> In Paris they simply stared when I spoke to them in French. I never did succeed in making those idiots understand their language.
> 
> — Mark Twain

当地的知识是非常有用的。通常在 Puppet 的配置清单中，你需要知道一些所在机器的本地信息。 Puppet 的发行中包含了一个 **Facter** 工具，它提供了从环境中获取系统信息（'facts'） 的一种标准方法，这些信息包括：

*   操作系统

*   内存大小

*   体系结构

*   处理器数量

> ![注记](img/note.png)
> 
> 要查看关于你的系统中可用的完整的 facts 列表，请运行如下命令：
> 
> ```
> # facter 
> ```
> 
> 虽然它可以方便地从命令行获得信息，但 **Facter** 的真正强大之处在于， 可以在你的 Puppet 配置清单中访问这些 facts。

#### 操作步骤

1.  在你的配置清单中可以像引用其他变量一样引用一个 Facter 的值，例如：

    ```
    notify { "This is $operatingsystem version
    $operatingsystemrelease, on $architecture architecture, kernel
    version $kernelversion": } 
    ```

2.  一旦运行 Puppet，它就会为当前节点填充适当的值：

    ```
    notice: This is Ubuntu version 10.04, on i386 architecture,
    kernel version 2.6.32 
    ```

#### 工作原理

Facter 为 Puppet 提供了一个抽象层，并在配置清单中提供了一种关于环境信息的标准方法。 当你在配置清单中引用一个 fact 值时，Puppet 就会通过查询 Facter 获得当前值， 并将其插入配置清单。

#### 更多用法

你也可以在 **ERB** 模板中使用 facts。例如，你可能会在一个文件中插入一个节点的主机名， 或者基于一个节点的内存大小改变一个应用的配置设置。 当你在模板中使用 fact 的名字时，它们不需要前导的美元符号， 因为在 **ERB** 模板中使用的是 Ruby，而不是 Puppet：

```
$KLogPath <%= case kernelversion when "2.6.31" then "/var/run/rsyslog/
kmsg" else "/proc/kmsg" end %> 
```

#### 参见本书

*   第九章的 创建 Facter 的自定义 fact 一节

# 导入动态信息

### 导入动态信息

尽管有些系统管理员喜欢将他们自己与其它使用成堆的老旧打印机的办公室相隔绝， 但是我们还是需要随时与其他部门交换信息。 例如，你可能需要插入数据到你的 Puppet 配置清单，这些数据是从外部数据源派生出来的。 generate 函数在这方面相当有用。

#### 准备工作

在 Puppetmaster 上使用如下代码创建脚本 /usr/local/bin/latest-puppet.rb：

```
#!/usr/bin/ruby
require 'open-uri'
page = open("http://www.puppetlabs.com/misc/download-options/").read
print page.match(/stable version is ([\d\.]*)/)[1] 
```

#### 操作步骤

1.  在你的配置清单中添加如下的内容：

    ```
    $latestversion = generate("/usr/local/bin/latest-puppet.rb")
    notify { "The latest stable Puppet version is ${latestversion}.
    You're using ${puppetversion}.": } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    notice: The latest stable Puppet version is 2.6.5\. You're using 2.6.3. 
    ```

#### 工作原理

generate 函数在 Puppetmaster 上（不是客户端）运行指定的脚本或程序并返回结果， 在本例中，返回的结果是 Puppet 最近发布的稳定版的版本号。

我不建议你在生产环境中运行此脚本，因为 Puppet Labs 会随时重新组织其 Web 站点， 这会导致脚本中抓取的 URL 失效，但是你可以从中获得些启发和想法。 一个脚本可以做的一切，比如打印、提取或者计算（例如数据库的查询结果） 都可以使用 generate 将其带入你的配置清单。

值得记住的是：正如在模板中嵌入 Ruby 调用一样，generate 函数运行在 Puppetmaster 上，而不是运行在 Puppet 的客户端节点上。 我曾经犯过一个在模板中调用 /bin/hostname 的错误， 结果让我大吃一惊的是，所有的节点名称都变成了 puppet。

当你需要获得有关节点的特别信息时，最好使用一个自定义的 fact 来实现。

#### 更多用法

如果你在调用 generate 时需要为其传递参数去执行， 可以将这些参数作为函数调用的额外参数，例如：

```
$latestpuppet = generate("/usr/local/bin/latest-version.rb", "puppet")
$latestmc = generate("/usr/local/bin/latest-version.rb", "mcollective") 
```

你还可以使用 generate 调用 Shell 命令，Puppet 的 generate 使用限制特殊字符的方式来避免恶意的 Shell 调用，例如不能使用 Shell 的管道符（|）。 最简单而安全的办法就是将你所有的逻辑写到一个脚本中，然后使用 generate 调用此脚本。

#### 参见本书

*   第九章的 创建 Facter 的自定义 fact 一节

*   本章的 从 CSV 文件导入数据 一节

# 从 CSV 文件导入数据

### 从 CSV 文件导入数据

想要知道更多东西吗？当你需要从表中查找某些值时，可以使用冗长的 case 语句或 selectors 实现，但更整洁的方式是使用 extlookup 函数实现。 在 puppetmaster 上可以使用 extlookup 函数查询外部的 CSV 文件，并返回匹配的数据片段。

将所有数据组织到一个单一的文件并将它从 Puppet 配置清单中分离出来， 可以使维护工作变得更简单，也便于与其他人分享：例如， 一个开发团队通过部署合适的 CSV 文件作为应用程序发布的一部分， 就可以管理 Puppet 需要知道的有关其应用程序的一切。 Puppet 只需要知道在哪里可以找到这个 CSV 文件，剩下的工作交由 extlookup 去完成。

#### 准备工作

1.  使用如下内容创建 /var/www/apps/common.csv 文件：

    ```
    path,/var/www/apps/%{name}
    railsversion,3
    domain,www.%{name}.com 
    ```

2.  使用如下内容创建 /var/www/apps/myapp.csv 文件：

    ```
    railsversion,2 
    ```

#### 操作步骤

1.  在你的配置清单中添加如下代码：

    ```
    $extlookup_datadir = "/var/www/apps/"
    $extlookup_precedence = [ "%{name}", "common" ]

    class app( $name ) {
        $railsversion = extlookup("railsversion")
        $path = extlookup("path")
        $domain = extlookup("domain")
        notify { "App data: Path ${path}, Rails version
      ${railsversion}, domain ${domain}": }
    }

    class { "app": name =&gt; "myapp" } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1303129760'
    notice: App data: Path /var/www/apps/myapp, Rails version 2,
    domain www.myapp.com
    notice: /Stage[main]/App/Notify[App data: Path /var/www/apps/
    myapp, Rails version 2, domain www.myapp.com]/message: defined
    'message' as 'App data: Path /var/www/apps/myapp, Rails version 2,
    domain www.myapp.com'
    notice: Finished catalog run in 0.58 seconds 
    ```

#### 工作原理

1.  我们做的第一件事是定义一个变量 $extlookup_datadir，它告诉 extlookup 从哪个目录查找数据文件。你通常会在 site.pp 或者在你设置全局变量的文件中设置这个变量：

    ```
    $extlookup_datadir = "/var/www/apps/" 
    ```

2.  然后，我们告诉 extlookup 在哪里寻找数据文件，以及寻找的优先顺序：

    ```
    $extlookup_precedence = [ "%{name}", "common" ] 
    ```

    这可以是任意长度的数组。当我们生成一个 extlookup 查询时，Puppet 会按顺序扫描每个文件，直到找到请求的值。文件名可以包含变量。 我们期望名为 $name 的变量已经被设置，Puppet 将会使用其值作为要查找的第一个文件名。

3.  下一步，在 app 类中，调用 extlookup 获得一个值：

    ```
    $railsversion = extlookup("railsversion") 
    ```

    现在 extlookup 装置查找一个 CSV 文件并从中读取数据。这个文件保存在 $extlookup_datadir 目录（在本例中是 /var/www/apps）, 文件名为 %{name}.csv（在本例中是 myapp.csv）。 所以 extlookup 读取文件的完整路径是 /var/www/apps/myapp.csv， 此文件包含了 railsversion,2

    我们已经找到了所需要的值（2），所以 extlookup 将其返回。

4.  下一个 extlookup 调用就不那么幸运了：

    ```
    $path = extlookup("path") 
    ```

    extlookup 还是首先查找文件 myapp.csv，但是它找不到匹配 path 变量的值。 所以会查找 $extlookup_precedence 列表中的下一个文件, 即文件 common.csv：

    ```
    path,/var/www/apps/%{name}
    railsversion,3
    domain,www.%{name}.com 
    ```

    就本例而言返回值为 /var/www/apps/myapp。

你可以看到，这允许我们在 common.csv 文件中设置一个默认值的集合， 每个应用程序都可以选择其自己的 myapp.csv 文件覆盖 common.csv 文件中的默认值。 extlookup 会依次查询列在 $extlookup_precedence 中的文件，直到其找到请求的值。 由于 myapp.csv 被列在首位，所以设置在其中的任何值都优先于文件 common.csv 中的。

#### 更多用法

你也可以在 extlookup 调用中指定默认值，当在所有的 CSV 文件中都没有找到匹配的数据时就会使用这个默认值：

```
$path = extlookup("path", "/var/www/misc") 
```

你还可以指定在查找 $extlookup_precedence 所列的文件之前首先要查找的 CSV 文件：

```
$path = extlookup("path", "/var/www/misc", "paths") 
```

这首先在 paths.csv 文件中查找数据，如果没找到，将会依次查找列在 $extlookup_precedence 中的其他文件。

CSV 文件中的值也可以引用变量，正如我们曾经看到的：

```
domain,www.%{name}.com 
```

你可以使用当前范围内可用的任何变量，包括 Facter 检测出的 facts 值：

```
domain,%{fqdn} 
```

**R.I. Pienaar 的文章 “Complex data and Puppet”** 是对 extlookup 的一个很好的介绍： [`www.devco.net/archives/2009/08/31/complex_data_and_puppet.php`](http://www.devco.net/archives/2009/08/31/complex_data_and_puppet.php) 。

**Jordan Sissel 曾经写过 “about configuring your whole infrastructure using extlookup”**： [`sysadvent.blogspot.com/2010/12/day-12-scaling-operabilitywith-truth.html`](http://sysadvent.blogspot.com/2010/12/day-12-scaling-operabilitywith-truth.html) 。

#### 参见本书

*   本章的 导入动态信息 一节

*   第九章的 创建 Facter 的自定义 fact 一节

# 给 Shell 命令传递参数

### 给 Shell 命令传递参数

如果你需要在命令行下插入一个值，通常需要使用引号将其括起来，尤其是当其包含空格时更是如此。 shellquote 函数可以携带任意数量的参数（包括数组）， 它会使用引号将每个参数都括起来并返回一个可以传递到 Shell 命令行上的以空格间隔的字符串。

在下面的例子中，我们想要创建一个用于文件改名的 exec 资源， 然而源文件名和目标文件名都包含空格，因此在命令行上需要正确地使用引号将它们括起来。

#### 操作步骤

1.  在你的配置清单中添加如下的代码：

    ```
    $source = "Hello Jerry"
    $target = "Hello... Newman"
    $argstring = shellquote( $source, $target )
    $command = "/bin/mv ${argstring}"
    notify { $command: } 
    ```

2.  运行 Puppet：

    ```
    notice: /bin/mv "Hello Jerry" "Hello... Newman" 
    ```

#### 工作原理

1.  首先我们定义了 $source 和 $target 两个变量， 它们的值是将要在命令行上使用的两个文件名：

    ```
    $source = "Hello Jerry"
    $target = "Hello... Newman" 
    ```

2.  然后我们调用 shellquote 串连两个变量为一个以空格间隔的使用引号将每个变量值括起来的字符串：

    ```
    $argstring = shellquote( $source, $target ) 
    ```

3.  然后将这其组织到一起，形成最终的命令行：

    ```
    $command = "/bin/mv ${argstring}" 
    ```

    结果为：

    ```
    /bin/mv "Hello Jerry" "Hello... Newman" 
    ```

4.  现在可以使用 exec 资源执行这个命令行了。若我们没有使用 shellquote 会发生什么？

    ```
    $source = "Hello Jerry"
    $target = "Hello... Newman"
    $command = "/bin/mv ${source}  ${target}"
    notify { $command: } 
    ```

    结果为：

    ```
    notice: /bin/mv Hello Jerry Hello... Newman 
    ```

    这将无法工作，因为 mv 命令期望以空格间隔的参数， 所以 mv 将参数解释为请求移动的三个文件：Hello、Jerry 和 Hello… 以及移动的目标目录 Newman，这可不是我们想要的结果。