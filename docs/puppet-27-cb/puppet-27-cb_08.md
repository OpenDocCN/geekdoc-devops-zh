# Puppet 语言及其写作风格

## Puppet 语言及其写作风格

> Computer language design is just like a stroll in the park. Jurassic Park, that is.
> 
> — Larry Wall

在本章中，我们将学习如下内容：

*   使用 Puppet 社区规范

*   使用模块

*   使用标准的命名规范

*   使用嵌入式 Ruby 代码

*   使用纯 Ruby 代码书写配置清单

*   遍历多个项目

*   书写强大的条件语句

*   在 if 语句中使用正则表达式

*   使用选择器和 case 语句

*   检测字符串中是否包含指定的值

*   使用正则表达式替换

> Elegance is not a dispensable luxury, but a factor that decides between success and failure.
> 
> — Edsger W. Dijkstra

在本章，你将学习如何书写优雅的 Puppet 配置清单。“优雅”的含义体现在几个方面： 可读性、高效性、书写符合社区规范的代码。

我们将会看到，如何遵循社区规范将你的代码组织并构造成模块，以便他人能很容易地阅读和维护你的代码。 我还会向你展示一些 Puppet 语言的强大功能，使你写出更简洁而传神的配置清单。

# 使用 Puppet 社区规范

### 使用 Puppet 社区规范

> A society made up of individuals who were all capable of original thought would probably be unendurable.
> 
> — H. L. Mencken

“随大溜” 有时会是个好主意。如果其他人需要阅读或管理你的配置清单， 或者如果你想要分享代码到社区， 那么尽可能遵循现有的样式约定会是个好主意。

#### 操作步骤

1.  将你的资源名用双引号括起来；例如使用 package { "exim4": 而不是 package { exim4:

    当使用一些像连字符和空格这样的字符时很容易引起 Puppet 解析器的混乱， 明智而又安全的做法是始终使用双引号将所有的资源名括起来。

2.  总是将非 Puppet 保留字的参数值用双引号括起来，例如：

    ```
    name  =&gt; "Nucky Thompson",
    mode  =&gt; "0700",
    owner =&gt; "deploy", 
    ```

    但保留字不用使用引号：

    ```
    ensure =&gt; installed,
    enable =&gt; true,
    ensure =&gt; running, 
    ```

    当在字符串中引用变量的值时，始终使用大括号将变量名括起来。例如：

    ```
    source =&gt; "puppet:///modules/webserver/${brand}.conf", 
    ```

    否则 Puppet 的解析器不得不猜测哪些字符是变量名的一部分。使用大括号将使其变得清晰。

3.  总是以逗号结束参数声明的行，即使它是最后一个参数：

    ```
    service { "memcached":
        ensure =&gt; running,
        enable =&gt; true,
    } 
    ```

    很多时候，当你编辑文件时会追加一个额外的参数， 而忘记在原来最后一行的末尾添加必要的逗号！

4.  当声明仅有一个参数的资源时，将声明放在一行上且无需使用结尾的逗号：

    ```
    package { "puppet": ensure =&gt; installed } 
    ```

5.  当声明有多个参数的资源时，每个参数占一行：

    ```
    package { "rake":
        ensure   =&gt; installed,
        provider =&gt; gem,
        require  =&gt; Package["rubygems"],
    } 
    ```

6.  当声明 **符号链接**（**symlinks**）时，使用 ensure => link：

    ```
    file { "/etc/php5/cli/php.ini":
        ensure =&gt; link,
        target =&gt; "/etc/php.ini",
    } 
    ```

7.  为了使代码便于阅读，所有的箭头以最长一个参数对齐，例如：

    ```
    file { "/var/www/${app}/shared/config/rvmrc":
        owner   =&gt; "deploy",
        group   =&gt; "deploy",
        content =&gt; template("rails/rvmrc"),
        require =&gt; File["/var/www/${app}/shared/config"],
    } 
    ```

    每个资源中的箭头（=>）都要对齐，而不是每个文件中的箭头都对齐； 否则会在不同文件之间复制代码时带来麻烦。

#### 更多用法

Puppet Labs 网站上维护着 Puppet 社区规范指南文档： [`docs.puppetlabs.com/guides/style_guide.html`](http://docs.puppetlabs.com/guides/style_guide.html) 。

Tim Sharpe 写了一个 puppet-lint 工具， 你可以用它检查你的配置清单是否符合规范。 运行 gem install puppet-lint 安装它，详细的信息请参考 [`github.com/rodjek/puppet-lint`](https://github.com/rodjek/puppet-lint) 。

# 使用模块

### 使用模块

你会对自己编写的 Puppet 代码感到羞愧吗？其他人看你的代码时会有恐惧感吗？ 为了使你的 Puppet 配置清单更清晰且易于维护，一件最重要的事情就是将这些配置清单组织成 **模块（modules）**。

模块是对相关事物进行分组的一种简单方式；例如，一个 webserver 模块可能包含作为一个 Web 服务器所需的一切，包括 Apache 配置文件，虚拟主机配置模板以及部署这些所需的 Puppet 代码。

将配置清单拆分成模块易于代码的重用和共享；也是一种组织配置清单的最合乎逻辑的方式。 在本节中，我们将会创建一个管理 memcached 的模块，memcached 是一个 Web 应用程序常用的内存缓存系统。

#### 操作步骤

1.  在 puppet.conf 中查找你的模块路径，其默认值是 /etc/puppet/modules。 如果你像我前面建议的那样将 Puppet 配置清单纳入了版本控制， 那么就使用你的工作副本中的 modules 目录，之后再由他部署到 /etc/puppet/modules。

    ```
    # puppet --genconfig |grep modulepath
    modulepath = /etc/puppet/modules:/usr/share/puppet/modules 
    ```

    在模块路径下创建一个名为 memcached 的子目录：

    ```
    # cd /etc/puppet/modules
    # mkdir memcached 
    ```

    在此目录中，创建 manifests 和 files 两个子目录：

    ```
    # cd memcached
    # mkdir manifests files 
    ```

    在 manifests 目录下，以如下内容创建 init.pp 文件：

    ```
    class memcached {
        package { "memcached":
            ensure =&gt; installed,
        }

        file { "/etc/memcached.conf":
            source =&gt; "puppet:///modules/memcached/memcached.conf",
        }

        service { "memcached":
            ensure  =&gt; running,
            enable  =&gt; true,
            require =&gt; [ Package["memcached"],
                         File["/etc/memcached.conf"] ],
        }
    } 
    ```

    切换到 files 目录，以如下内容创建 memcached.conf 文件：

    ```
    -m 64
    -p 11211
    -u nobody
    -l 127.0.0.1 
    ```

2.  为了使用你的新模块，你的节点定义中添加如下的代码：

    ```
    node cookbook {
        include memcached
    } 
    ```

3.  运行 Puppet 检测新配置：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1300361964'
    notice: /Stage[main]/Memcached/Package[memcached]/ensure: ensure
    changed 'purged' to 'present'
    ...
    info: /Stage[main]/Memcached/File[/etc/memcached.conf]:
    Filebucketed /etc/memcached.conf to puppet with sum a977521922a151
    c959ac953712840803
    notice: /Stage[main]/Memcached/File[/etc/memcached.conf]/content:
    content changed '{md5}a977521922a151c959ac953712840803' to '{md5}
    f5c0bb01a24a5b3b86926c7b067ea6ba'
    notice: Finished catalog run in 20.68 seconds 
    ```

4.  检查新配置的服务是否正在运行：

    ```
    # service memcached status
      * memcached is running 
    ```

#### 工作原理

模块有特定的目录结构。并非所有子目录都要存在，但如果存在就该以如下的布局方式组织：

```
MODULEPATH/
 `-- MODULE_NAME
         |-- files/
         |-- templates/
         |-- manifests/
         |--   ...
         `-- README 
```

在 memcached.pp 文件中定义了 memcached 类， 它可以被 Puppet 自动导入。现在将其包含到节点中：

```
include memcached 
```

在 memcached 类中，引用了 memcached.conf 文件：

```
file { "/etc/memcached.conf":
    source => "puppet:///modules/memcached/memcached.conf",
} 
```

正如我们在 配置 Puppet 的文件服务器 一节讲到的， 上面代码中的 source 参数告诉 Puppet 在如下的路径寻找文件：

```
MODULEPATH/
    memcached/
        files/
            memcached.conf 
```

#### 更多用法

学习喜欢模块的组织方式，使用模块将使你的 Puppet 管理生活便得更轻松。模块并不复杂。 然而，实践和经验会帮助你判断何时应该将事物划分并组织成模块，以及如何更好的安排你的模块结构。 如下所述的一些技巧会对你有所帮助。

##### 模板

模板作为模块的一部分，如果你需要使用它，那么其放在 MODULE_NAME/templates 目录，参考如下的用法：

```
file { "/etc/memcached.conf":
    content => template("memcached/memcached.conf"),
} 
```

Puppet 会在如下目录搜索模板文件：

```
MODULEPATH/
    memcached/
        templates/
            memcached.conf 
```

##### Fact、函数、类型和提供者

模板中也可以包含自定义 fact、自定义函数、自定义资源类型和自定义提供者。 关于这些内容的详细信息请参阅 外部工具和 Puppet 的生态系统 一章的内容。

##### puppet-module

你也可以使用 puppet-module 工具为一个新模块创建目录布局，而不是使用手工方式。 请参考第九章的 使用公共模块 一节获得更详细的信息。

##### 第三方模块

你可以下载由其他人开发的模块，并在你的配置清单中使用这些模块， 就像这些模块是你自己写的一样。 请参考第九章的 使用公共模块 一节获得更详细的信息。

##### 模块的组织方法

有关如何组织模块的更多信息，参见 Puppet Labs 站点：[`docs.puppetlabs.com/guides/modules.html`](http://docs.puppetlabs.com/guides/modules.html) 。

#### 参见本书

*   第一章的 配置 Puppet 的文件服务器 一节

*   第九章的 创建 Facter 的自定义 fact 一节

*   第九章的 使用公共模块 一节

*   第九章的 创建自定义的资源类型 一节

*   第九章的 创建自定义的提供者 一节

# 使用标准的命名规范

### 使用标准的命名规范

> There are only two hard problems in computer science: cache invalidation, naming things, and off-by-one errors.
> 
> — Phil Karlton

当你维护代码时会发现，选择适当而有意义的信息为你的模块或类命名是有很大帮助的。 尤其是当其他人需要阅读或基于你编写的配置清单工作时，这种命名方法是十分必要的。

#### 操作步骤

1.  模块应该以被管理的软件或服务命名，例如：apache 或 haproxy。

    用跟在模块之后的功能或提供的服务为模块中的类命名，例如：apache::vhosts 或 rails::dependencies。

    如果在模块中提供了禁用服务的功能，将其命名为 disabled。例如一个禁用 Apache 的类应该命令为 apache::disabled。

2.  如果一个节点需要提供多种服务，请在节点的定义中包含一个类或者每个服务的类名。例如：

    ```
    node server014 inherits server {
        include puppet::server
        include mail::server
        include repo::gem
        include repo::apt
        include zabbix
    } 
    ```

3.  管理用户的模块应该命名为 user。

    1.  在 user 模块中，声明虚拟用户的类应命名为 user::virtual。

    2.  在 user 模块中，一个特定的用户组应该命名为子类，例如 user::sysadmins 或 user::contractors。

4.  当你需要覆盖某些特定节点或服务的类时，继承该类并以节点名作为子类名的前缀。 例如，如果节点 cartman 需要一个特殊的 SSH 配置且要覆盖 ssh 类，可以这样做：

    ```
    class cartman_ssh inherits ssh {
        [ override config here ]
    } 
    ```

5.  当使用 Puppet 为不同的服务部署配置时，被 Puppet 解析后传出的文件应该以服务开头， 随后跟上指示文件功能的后缀。例如：

    *   Apache init 脚本?—?apache.init

    *   Rails 的日志滚动配置片段?—?rails.logrotate

    *   Nginx 为 mywizzoapp 应用所做的 vhost 配置?—?mywizzoapp.vhost.nginx

    *   独立运行的 MySQL 服务器的配置?—?standalone.mysql

    如果你要管理不同的 Ruby 版本，可以使用添加了版本号的类名，表示由此类负责特定版本的管理， 例如 ruby192 或 ruby186。

#### 更多用法

Puppet 社区维护着一套 Puppet 基础设施的最佳实践准则，其中包括对命名的提示： [`projects.puppetlabs.com/projects/1/wiki/Puppet_Best_Practice2`](http://projects.puppetlabs.com/projects/1/wiki/Puppet_Best_Practice2) 。

一些人更愿意在一个节点中使用逗号分割的列表包含多个类，而不是使用分离的 include 语句。例如：

```
node server014 inherits server {
    include puppet::server,
            mail::server,
            repo::gem,
            repo::apt,
            zabbix
} 
```

这是个风格问题，但我更愿意使用分离的 include 语句，一行包含一个模块或类， 因为这样可以更容易地在节点之间复制或移动，而无需每次都要整理逗号和缩进的问题。

我在前面的例子中曾几次提到过继承。如果你不知道这是什么，不要担心：我会在下一章详细解释。

# 使用嵌入式 Ruby 代码

### 使用嵌入式 Ruby 代码

> Ruby, like fire, is a very useful friend, and a very dangerous enemy.
> 
> — Mikkel Bruun

在模板中使用嵌入式 Ruby 帮助构建动态的配置文件或实现数组遍历是一种强大的方式。 然而，你也可以在配置清单中使用 inline_template 函数直接嵌入 Ruby 而不必使用分离的模板文件。

#### 操作步骤

在 Puppet 的配置清单中将 Ruby 代码传递给 inline_template 函数：

```
cron { "nightly-job":
    command => "/usr/local/bin/nightly-job",
    hour => "0",
    minute => inline_template("<%= hostname.hash.abs % 60 %>"),
} 
```

#### 工作原理

传递给 inline_template 的字符串都会被执行，就像 ERB 模板一样。 也就是说，在分隔符 <%= 和 %> 之间的所有内容都被当做 Ruby 代码执行， 其余的将被视为字符串。

#### 参见本书

*   第五章的 使用 ERB 模板 一节

*   第五章的 在模板中遍历数组 一节

# 使用纯 Ruby 代码书写配置清单

### 使用纯 Ruby 代码书写配置清单

你会说西班牙语吗？学习一门语言可能会很有趣，但并非所有人都想这么做。 Puppet 有时会因为它使用自己专用的配置语言而不是现存的通用语言（如 Ruby）书写配置清单而遭到批评。

不是每个人都认为这是个缺点。计算机科学家 Dennis Ritchie 说：

> A language that doesn’t have everything is actually easier to program in than some that do.
> 
> — Dennis Ritchie

无论你是什么观点，这种批评已不再适用。因为 Puppet 已经拥有了使用 Ruby 语言书写配置清单的实验性支持，这在生产环境上是相当有用的，即使还处于早期开发阶段。 在你的配置清单中可以混合使用两种语言，Puppet 以文件扩展名进行区别，扩展名 .rb 表示用 Ruby 语言书写的配置清单文件，而扩展名 .pp 表示 Puppet 专用的配置语言文件。

为了书写配置清单使用的 Ruby 的 **特定领域语言**（**domain-specific language，DSL**） 看上去与标准的 Puppet 语言非常相似。 在下面的例子中，我将向你展示如何将典型的 Puppet 配置清单转换成 Ruby 的。 Puppet 语法的原始配置清单如下：

```
class admin::exim {
    package { "exim4": ensure => installed }

    service { "exim4":
        ensure  => running,
        require => Package["exim4"],
    }

    file { "/etc/exim4/exim4.conf":
        content => template("admin/exim4.conf"),
        notify  => Service["exim4"],
        require => Package["exim4"],
    }
} 
```

#### 操作步骤

1.  使用如下内容创建 /etc/puppet/modules/admin/manifests/exim.rb 文件：

    ```
    hostclass "admin::exim" do
        package "exim4", :ensure =&gt; :installed

        service "exim4",
            :ensure  =&gt; :running,
            :require =&gt; "Package[exim4]"

        file "/etc/exim4/exim4.conf",
            :content =&gt; template(["admin/exim4.conf"]),
            :notify  =&gt; "Service[exim4]",
            :require =&gt; "Package[exim4]"
    end 
    ```

2.  在一个节点中包含这个类并运行 Puppet。

#### 工作原理

1.  关键字 hostclass 声明一个类，就像 Puppet 中的 class：hostclass admin::exim do

2.  然后跟一个 do … end 语句块，这相当于 Puppet 的一对大括号。

3.  通过在资源类型后调用函数来声明资源： 例如 package 或 service：package "exim4", :ensure => :installed

4.  传递给函数的参数是一个用逗号分割的列表，参数必须要用双引号括起来， 或者在参数前使用前导的冒号使其成为 Ruby symbol 对象： :ensure => :running,

    又如，像 :installed 或 :running 这样的 Puppet 内置名字都是 Ruby 的 symbol 对象。

    | ![注记](img/note.png) | 译者注

    有关 Ruby Symbol 的详细介绍请参考： [`www.ibm.com/developerworks/cn/opensource/os-cn-rubysbl/`](http://www.ibm.com/developerworks/cn/opensource/os-cn-rubysbl/)

    |

5.  当我们需要引用资源来表示相互关系时，就要使用 :require， 资源标识符通过首字母大写的资源类型跟上写在一对方括号中的资源名给出，例如： require => "Package[exim4]"

    在调用像模板这样的函数时可以使用函数名跟一对圆括号，在圆括号中传递的参数是 一对用方括号声明的数组，例如：:content => template(["admin/exim4.conf"]),。

#### 更多用法

Ruby DSL 尚处于早期开发阶段。试验虽然有趣，除非你有使用 Ruby 的令人信服的理由否则先别用它， 至今我仍旧使用标准的 Puppet 语言。或许将来 Ruby DSL 会被广泛应用，但在此期间， 你会发现没有它的生活会更轻松。然而，若你坚持要使用它，下面介绍几个非常有用的提示。

##### 变量

当你像常规的 Ruby 程序一样使用 Ruby 变量时，可以使用 scope.lookupvar 访问你的 Puppet 变量，例如：

```
notice( "I am running on node %s" % scope.lookupvar("fqdn") ) 
```

将得到：

```
notice: I am running on node cookbook.bitfieldconsulting.com 
```

要在你的 Puppet 配置清单范围内设置变量，使用 scope.setvar，例如：

```
require 'time'
scope.setvar("now", Time.now)
notice( "At the third stroke, the time sponsored by Bitfield
Consulting will be: %s" % scope.lookupvar("now") ) 
```

上面的代码将获得如下结果：

```
notice: At the third stroke, the time sponsored by Bitfield Consulting
will be: Wed Mar 23 05:58:16 -0600 2011 
```

##### 文档

你可以在 Puppet Labs 网站找到更多如何使用 Ruby DSL 的详细资料，包括诸如 **virtual resources** 和 **collections** 这样的高级主题： [`projects.puppetlabs.com/projects/1/wiki/Ruby_Dsl`](http://projects.puppetlabs.com/projects/1/wiki/Ruby_Dsl) 。

Ken Barber 提供了一些语法例子，并对 Puppet 语法和 Ruby DSL 结构做了比较，网址在 [`github.com/bobsh/puppet-rubydsl-examples`](https://github.com/bobsh/puppet-rubydsl-examples) 。

最后，James Turnbull 发布过一篇 blog， 展示了使用 Ruby 连接 MySQL 服务器的高级方法： [`www.puppetlabs.com/blog/using-ruby-inthe-puppet-ruby-dsl/`](http://www.puppetlabs.com/blog/using-ruby-inthe-puppet-ruby-dsl/) 。

# 遍历多个项目

### 遍历多个项目

该死的东西一个接一个！ **数组**（**Arrays**）是 Puppet 的一个强大特性； 不论何时，你要对列表中的元素执行相同的操作，数组就会帮你的忙。 你可以创建一个数组，将所有的数组元素放在一对方括号中并以逗号间隔，例如：

```
$lunch = [ "franks", "beans", "mustard" ] 
```

#### 操作步骤

在你的配置清单中添加如下代码：

```
$packages = [ "ruby1.8-dev",
              "ruby1.8",
              "ri1.8",
              "rdoc1.8",
              "irb1.8",
              "libreadline-ruby1.8",
              "libruby1.8",
              "libopenssl-ruby" ]

package { $packages: ensure => installed } 
```

运行 Puppet，值得注意的是现在每个软件包都应该被安装。

#### 工作原理

当 Puppet 遇到数组作为资源名的情况时，它会对数组中的每个元素创建一个资源。 在前面的例子中，对 $packages 数组中的每一个包， 使用相同的参数（ensure => installed）创建了一个新的 package 资源。 这是对很多类似的资源进行实例化的一个非常紧凑的方式。

#### 更多用法

如果你听到哈希（hash），会比数组更兴奋。

##### 哈希

**哈希**（**hash**）与数组类似，但它的每个元素都可以通过名字存储和查找。例如：

```
$interface = { name    => 'eth0',
               address => '192.168.0.1' }

notice("Interface ${interface[name]} has address ${interface[address]}") 
```

的执行结果为：

```
Interface eth0 has address 192.168.0.1 
```

你可以给哈希赋任意的值：字符串、函数调用、表达式、甚至其它哈希或数组。

##### 使用 split 函数创建数组

你可以使用方括号来声明文字数组，例如：

```
define lunchprint() {
    notify { "Lunch included $name": }
}

$lunch = [ "egg", "beans", "chips" ]
lunchprint { $lunch: } 
```

执行结果为：

```
Lunch included egg
Lunch included beans
Lunch included chips 
```

但是 Puppet 还可以使用 split 函数从一个字符串创建数组，例如：

```
$menu = "egg beans chips"
$items = split($menu, ' ')
lunchprint { $items: } 
```

执行结果为：

```
Lunch included egg
Lunch included beans
Lunch included chips 
```

注意 split 函数携带两个参数：第一个参数是要被拆分的字符串；第二个参数是拆分间隔符， 在本例中是一个空格。当 Puppet 遍历字符串时，一遇到空格就将其视为一个元素的结束和下一个元素的开始。 所以，给定的字符串 "egg beans chips" 将被拆分为三个元素。

拆分间隔符可以是任意字符或字符串：

```
$menu = "egg and beans and chips"
$items = split($menu, ' and ') 
```

拆分间隔符也可以是正则表达式（regular expression），例如：一个多选一集合可以使用 | (pipe) 符号来做间隔：

```
$lunch = "egg:beans,chips"
$items = split($lunch, ':|,') 
```

# 书写强大的条件语句

### 书写强大的条件语句

生活充满选择。Puppet 的 if 语句允许基于变量或表达式的值应用不同的配置代码。 使用 if 语句，你可以根据当前节点的实际情况应用不同的资源或参数值，例如： 操作系统或内存大小。举例来说，数据中心 A 中的节点和数据中心 B 中的节点可能需要使用不同的 DNS 服务器；对 Ubuntu 系统和其他系统可能需要执行不同类的集合。

#### 操作步骤

在你的配置清单中添加如下的代码：

```
if $lsbdistid == "Ubuntu" {
    notice("Running on Ubuntu")
} else {
    notice("Non-Ubuntu system detected. Please upgrade to Ubuntu
immediately.")
} 
```

#### 工作原理

Puppet 对 if 关键字之后的内容视为表达式并对其进行评估。若表达式的值为 true， Puppet 将执行大括号内的代码。

另外，你还可以使用 else 分支，它将在表达式的值为 false 时执行。

#### 更多用法

虽然你可以在 Puppet 中写出很复杂的 if 语句，但是我不建议你这么做。 很多时候，改变你的设计思路（例如，使用模板）比使用 if 语句要好。

从我用于生产环境的一些配置清单实例来看，我惊讶地发现，在所有的几千行代码中都没有使用 if 语句。尽管如此，你的里程可能会有所不同，所以下面给出一些使用 if 的技巧。

##### elsif

你可以添加 elsif 关键字进行进一步的测试，例如：

```
if $lsbdistid == "Ubuntu" {
    notice("Running on Ubuntu")
} elsif $lsbdistid == "Debian" {
    notice("Close enough…")
} else {
    notice("Non-Ubuntu system detected. Please upgrade to Ubuntu
immediately.")
} 
```

##### 比较

你可以使用 == 语法检查两个值是否相同，例如：

```
if $lsbdistid == "Ubuntu" {
    ...
} 
```

或者使用 != 语法检查两个值是否不同，例如：

```
if $lsbdistid != "CentOS" {
    ...
} 
```

你也可以使用 < 和 > 比较两个数值，例如：

```
if $uptime_days > 365 {
    notice("Really .. there have been kernel security patches out
there for ages, you will so be 0wned!")
} 
```

还可以使用 <= 或 >= 对数值进行比较：

```
if $lsbmajdistrelease <= 9 {
    ...
} 
```

##### 复杂表达式

你可以把上述代码片段中的一些简单表达式组合成更复杂的逻辑表达式， 逻辑表达式使用与（and）、或（or）和非（not）连接简单表达式，例如：

```
if ($uptime_days > 365) and ($lsbdistid == "Ubuntu") {
     ...
}

if ($role == "webserver") and ( ($datacenter == "A") or ($datacenter == "B") ) {
     ...
} 
```

#### 参见本书

*   第三章的 在 if 语句中使用正则表达式 一节

*   第三章的 检测字符串中是否包含指定的值 一节

*   第三章的 使用选择器和 case 语句 一节

# 在 if 语句中使用正则表达式

### 在 if 语句中使用正则表达式

> Some people, when confronted with a problem think; *I know, I’ll use regular expressions.* Now they have two problems.
> 
> — Jamie Zawinski

你可以在 if 语句中使用另一种类型的表达式，即 **正则表达式**（**regular expression**）。 正则表达式是一种使用模式匹配的强大的字符串比较方式。

#### 操作步骤

在你的配置清单中添加如下代码：

```
if $lsbdistdescription =~ /LTS/ {
    notice("Looks like you are using a Long Term Support version of
 Ubuntu.")
} else {
    notice("You might want to upgrade to a Long Term Support version
 of Ubuntu...")
} 
```

#### 工作原理

Puppet 将两个斜线之间的文本当做正则表达式对待，两个各斜线之间的文本就是要匹配的内容。 如果正则表达式匹配成功，if 表达式为真，第一个大括号之间的代码就会被执行。

若要使用匹配的反逻辑，即不匹配，需要使用 !~ 替代 =~ ，例如：

```
if $lsbdistdescription !~ /LTS/ { 
```

#### 更多用法

正如 Jamie Zawinski 指出的，正则表达式虽然强大，但难于理解和调试。 如果你发现自己所写的正则表达式相当复杂，以至于一眼看上去不能被理解， 就应该考虑简化设计使正则表达式更易懂。 然而，正则表达式的一个特别有用的功能是能够捕获模式。

##### 捕捉模式

你不仅可以使用正则表达式匹配文本，还可以捕获匹配的文本并将其存储在变量中：

```
$input = "Puppet is better than manual configuration"
if $input =~ /(.*) is better than (.*)/ {
    notice("You said '$0'. Looks like you're comparing $1 to $2!")
} 
```

结果为：

```
You said 'Puppet is better than manual configuration'. Looks like you're
comparing Puppet to manual configuration! 
```

变量 $0 存储了所有匹配的文本（假设整体匹配成功）。 如果你将一部分正则表达式置于一对圆括号中，就会创建一个 **组**（**group**）， 并且所有被匹配的组都会存储在变量中。 第一个被匹配的组是 $1，第二个是 $2，以此类推，正如上例展示的那样。

##### 正则表达式语法

Puppet 使用了 Ruby 正则表达式语法（Regular expression syntax）的一个子集，如果你还不熟悉正则表达式， 下面网址中的解释会对你有用： [`gnosis.cx/publish/programming/regular_expressions.html`](http://gnosis.cx/publish/programming/regular_expressions.html) 。

#### 参见本书

*   本章的 使用正则表达式替换 一节

# 使用选择器和 case 语句

### 使用选择器和 case 语句

> Smarts is the most exclusive club in town. Everyone welcome.
> 
> — Sign

有时选择性是很重要的。尽管你可以使用 if 书写任何条件语句， 但 Puppet 还提供了帮助你更容易地表达条件的额外形式，例如： 选择器（selector）和 case 语句。

#### 操作步骤

1.  在你的配置清单中添加如下代码：

    ```
    $systemtype = $operatingsystem ? {
        "Ubuntu" =&gt; "debianlike",
        "Debian" =&gt; "debianlike",
        "RedHat" =&gt; "redhatlike",
        "Fedora" =&gt; "redhatlike",
        "CentOS" =&gt; "redhatlike",
        default  =&gt; "redhatlike",
    }

    notify { "You have a ${systemtype} system": } 
    ```

2.  接下来，再添加如下代码：

    ```
    class debianlike {
        notify { "Special manifest for Debian-like systems": }
    }

    class redhatlike {
        notify { "Special manifest for RedHat-like systems": }
    }

    case $operatingsystem {
        "Ubuntu",
        "Debian": {
            include debianlike
        }
        "RedHat",
        "Fedora",
        "CentOS": {
            include redhatlike
        }
    } 
    ```

#### 工作原理

上面的例子同时演示了选择器（selector）和 case 语句，让我们来看看它们是如何工作的。

*   **selector**

    在第一个例子中，我们使用了一个选择器（操作符 ?）根据变量 $operatingsystem 的值为 $systemtype 变量赋值。 这类似于 C 语言和 Ruby 语言中的三元运算符， 不同之处在于三元运算符只能在两个可选值之间选择，而此处的选择器则可以提供你想要的多个值。

    Puppet 会与 $operatingsystem 的每一个可能的值（如 Ubuntu、Debian 等）一一做比较。 这些值可以是正则表达式（例如，部分字符串匹配，或使用通配符），但在本例中我们仅仅使用了文本字符串。 一旦发现匹配的值，选择器表达式就会返回与其相关的匹配字符串。 例如，如果 $operatingsystem 的值是 Fedora，选择器表达式就会返回字符串 redhatlike 并将其赋予变量 $systemtype。

*   **Case** 语句

    与选择器不同，case 语句不会返回任何值。 case 语句适用于， 当你想根据一个表达式的不同的值执行不同代码的情况。 在我们的第二个例子中，使用 case 语句根据 $operatingsystem 的值或者包含 debianlike 类，或者包含 redhatlike 类。

    再次指出，Puppet 会根据 $operatingsystem 值与潜在的匹配列表进行比较。 这些匹配列表可以是正则表达式，或者是字符串，或者像我们的例子中一样使用 逗号间隔的字符串列表。 当 Puppet 在匹配列表中找到一个匹配值，就会执行与此匹配项相关的大括号之间的代码。所以， 如果 $operatingsystem 的值是 Ubuntu，那么 include debianlike 就会被执行。

#### 更多用法

一旦你掌握了选择器和 case 语句的基本用法，你会发现下面的技巧非常有用。

##### 正则表达式

与 if 语句一样，你可以在选择器和 case 语句中使用正则表达式，并且你也可以 捕获匹配组的值，并使用 $1、$2 等引用它们的值。

```
case $lsbdistdescription {
    /Ubuntu (.+)/: {
        notify { "You have Ubuntu version $1": }
    }
    /CentOS (.+)/: {
        notify { "You have CentOS version $1": }
    }
} 
```

##### 默认值

选择器和 case 语句都可以让你指定一个 default 值，当没有其他匹配项时就使用这个默认值：

```
$lunch = "Sausage and chips"
$lunchtype = $lunch ? {
    /chips/ => "unhealthy",
    /salad/ => "healthy",
    default => "unknown",
}

notify { "Your lunch was ${lunchtype}": } 
```

结果为：

```
Your lunch was unhealthy 
```

# 检测字符串中是否包含指定的值

### 检测字符串中是否包含指定的值

想知道什么在什么不在吗？Puppet 的 in 可以帮你， 如下面的表达式：

```
if "foo" in $bar 
```

如果 foo 是 $bar 的子串，表达式的值为 true。 如果 $bar 是个数组，并且 foo 是这个数组中的一个元素，表达式的值为 true。 如果 $bar 是一个哈希，foo 是 $bar 的一个键值，表达式的值为 true。

#### 操作步骤

1.  在你的配置清单中添加如下代码：

    ```
    if $operatingsystem in [ "Ubuntu", "Debian" ] {
        notify { "Debian-type operating system detected": }
    } elsif $operatingsystem in [ "RedHat", "Fedora", "SuSE", "CentOS"
    ] {
        notify { "RedHat-type operating system detected": }
    } else {
        notify { "Some other operating system detected": }
    } 
    ```

2.  运行 Puppet：

    ```
    # puppet agent --test
    Debian-type operating system detected 
    ```

#### 更多用法

in 表达式既可以使用在 if 语句或其他条件语句中，还可以在任何表达式能出现的地方使用。 例如，你可以像下面这样为一个变量赋值：

```
$debianlike = $operatingsystem in [ "Debian", "Ubuntu" ]

if $debianlike {
    $ntpservice = "ntp"
} else {
    $ntpservice = "ntpd"
} 
```

# 使用正则表达式替换

### 使用正则表达式替换

> Change is inevitable, except from vending machines.
> 
> — Robert C. Gallagher

Puppet 的 regsubst 函数提供了一种处理文本的简单方法， 用于在字符串中查找和替换，或者从字符串提取匹配的模式。 例如，我们通常需要对从 facter 或者从外部程序获得的数据做这样的处理。

在本例中将会看到如何使用 regsubst 提取一个 IP 地址的前三个字节 （即网络地址部分，假定此 IP 地址是一个 C 类地址）。

#### 操作步骤

1.  在配置清单中添加如下的代码：

    ```
    $class_c = regsubst($ipaddress, "(.*)\..*", "\1.0")
    notify { $ipaddress: }
    notify { $class_c: } 
    ```

2.  运行 Puppet：

    ```
    notice: 10.0.2.15
    notice: 10.0.2.0 
    ```

#### 工作原理

regsubst 函数需要携带三个参数： 源字符串、匹配模式（pattern）和替换结果。 在本例中，我们指定的源字符串是 $ipaddress，这恰好是：

```
 10.0.2.15 
```

我们还指定了 (.**)..** 作为匹配模式并且 \1.0 作为替换结果。

匹配模式将匹配整个 IP 地址，捕获的前三个字节放在一对圆括号内。 被捕获的文本可以在替换结果中使用 \1 来引用。

被匹配的全部文本（本例中是字符串 “10.0.2.15”）将使用替换结果（replacement）来替换。 这里是 \1 （从源字符串中捕获的文本）跟上字符串 .0， 最终获得：10.0.2.0。

#### 更多用法

匹配模式中可以使用任何正则表达式，与在 if 语句中使用的正则表达式 (Ruby) 语法一致。

#### 参见本书

*   第四章的 导入动态信息 一节

*   第四章的 获得系统的环境信息 一节

*   第三章的 在 if 语句中使用正则表达式 一节