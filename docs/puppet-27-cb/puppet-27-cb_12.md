# 应用程序

## 应用程序

> The best software in the world only sucks. The worst software is significantly worse than that.
> 
> — Luke Kanies

在本章中，我们将学习如下内容：

*   管理 Apache 服务

*   创建 Apache 虚拟主机

*   创建 Nginx 虚拟主机

*   创建 MySQL 数据库及用户

*   管理 Drupal 站点

*   管理 Rails 应用程序

如果没有应用程序，服务器仅仅是一个非常昂贵的空间加热器。 本章将介绍一些使用 Puppet 管理一些特定应用程序的处方：MySQL、Apache、Nginx、Rails 和 Drupal。 这些都是非常流行的应用程序， 所以这对你会很有用。 然而，它们使用的模式和技术几乎适用于任何软件， 所以适当改写这些处方以适应自己的目的并非难事。

# 管理 Apache 服务

### 管理 Apache 服务

Apache 是一款流行的 web 服务器，然而对于其配置者来说并不轻松。 Puppet 可以在一定程度上缓解配置者因管理 Apache 服务器所带来的痛苦。

#### 操作步骤

1.  如果 Apache 模块还不存在就创建它：

    ```
    # mkdir /etc/puppet/modules/apache
    # mkdir /etc/puppet/modules/apache/templates
    # mkdir /etc/puppet/modules/apache/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/apache/manifests/init.pp 文件：

    ```
    class apache {
        package { "apache2-mpm-prefork": ensure =&gt; installed }

        service { "apache2":
            enable  =&gt; true,
            ensure  =&gt; running,
            require =&gt; Package["apache2-mpm-prefork"],
        }

        file { "/etc/apache2/logs":
            ensure  =&gt; directory,
            require =&gt; Package["apache2-mpm-prefork"],
        }

        file { "/etc/apache2/conf.d/name-based-vhosts.conf":
            content =&gt; "NameVirtualHost *:80",
            require =&gt; Package["apache2-mpm-prefork"],
            notify  =&gt; Service["apache2"],
        }
    } 
    ```

3.  在一个节点中添加如下代码：

    ```
    include apache 
    ```

4.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1309189590'

    notice: /Stage[main]/Apache/Package[apache2-mpm-prefork]/ensure:
    ensure changed 'purged' to 'present'

    notice: /Stage[main]/Apache/File[/etc/apache2/logs]/ensure:
    created

    notice: /Stage[main]/Apache/File[/etc/apache2/conf.d/name-basedvhosts.
    conf]/ensure: defined content as '{md5}78465aacbd01eb537b94
    1b21ae0af8b8'

    info: /Stage[main]/Apache/File[/etc/apache2/conf.d/name-basedvhosts.
    conf]: Scheduling refresh of Service[apache2]

    notice: Finished catalog run in 39.45 seconds 
    ```

#### 更多用法

在下一节中，我们将看到如何为 Apache 定义虚拟主机的内容。 然而你可能会发现，你需要为整个 Apache 服务器（包括虚拟主机）提供特殊的配置选项。 你可以使用 Puppet 通过部署 apache2.conf 来配置这些选项， 但是将配置片段放在 /etc/apache2/conf.d 目录中会更加整洁清晰。 例如，你可以在 init.pp 中添加如下代码：

```
define snippet() {
    file { "/etc/apache2/conf.d/${name}":
        source => "puppet:///modules/apache/${name}",
        notify => Service["apache2"],
    }
} 
```

并在节点上包含如下的代码片段：

```
apache::snippet { "site-specific.conf": } 
```

# 创建 Apache 虚拟主机

### 创建 Apache 虚拟主机

使用 ERB 模板配置虚拟主机是一种常见的应用， 因为每个虚拟主机配置的实例通常都使用类似的样板代码，只有一两个变量的值不同而已。 显然，对于某些网站或应用程序来说，你需要在虚拟主机的定义中指定特殊的配置选项， 然而这些特殊选项又不能通过一个简单的模板来配置?—?但是，不管怎样， 使用一个模板配置一些简单的站点将会节省时间、避免重复劳动。

#### 操作步骤

1.  添加如下代码到 /etc/puppet/modules/apache/manifests/init.pp：

    ```
    define site( $sitedomain = "", $documentroot = "" ) {
        include apache

        if $sitedomain == "" {
            $vhost_domain = $name
        } else {
            $vhost_domain = $sitedomain
        }

        if $documentroot == "" {
            $vhost_root = "/var/www/${name}"
        } else {
            $vhost_root = $documentroot
        }

        file { "/etc/apache2/sites-available/${vhost_domain}.conf":
            content =&gt; template("apache/vhost.erb"),
            require =&gt; File["/etc/apache2/conf.d/name-basedvhosts.conf"],
            notify =&gt; Exec["enable-${vhost_domain}-vhost"],
        }

        exec { "enable-${vhost_domain}-vhost":
            command     =&gt; "/usr/sbin/a2ensite ${vhost_domain}.conf",
            require     =&gt; [ File["/etc/apache2/sites-available/${
     vhost_domain}.conf"], Package["apache2-mpm-prefork"] ],
            refreshonly =&gt; true,
            notify      =&gt; Service["apache2"],
        }
    } 
    ```

2.  使用如下内容创建 /etc/puppet/modules/apache/templates/vhost.erb 文件：

    ```
    &lt;VirtualHost *:80&gt;
        ServerName &lt;%= vhost_domain %&gt;
        ServerAdmin admin@&lt;%= vhost_domain %&gt;
        DocumentRoot &lt;%= vhost_root %&gt;
        ErrorLog logs/&lt;%= vhost_domain %&gt;-error_log
        CustomLog logs/&lt;%= vhost_domain %&gt;-access_log common

        &lt;Directory /var/www/&lt;%= vhost_domain %&gt;&gt;
            Allow from all
            Options +Includes +Indexes +FollowSymLinks
            AllowOverride all
        &lt;/Directory&gt;
    &lt;/VirtualHost&gt;

    &lt;VirtualHost *:80&gt;
        ServerName www.&lt;%= vhost_domain %&gt;
        Redirect 301 / http://&lt;%= vhost_domain %&gt;/
    &lt;/VirtualHost&gt; 
    ```

3.  添加如下代码到一个节点：

    ```
    apache::site { "keithlard.com": } 
    ```

4.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1309190720'

    notice: /Stage[main]//Node[cookbook]/Apache::Site[keithlard.com]/
    File[/etc/apache2/sites-available/keithlard.com.conf]/ensure:
    defined content as '{md5}f2a558c02beeaed4beb7da250821b663'

    info: /Stage[main]//Node[cookbook]/Apache::Site[keithlard.com]/
    File[/etc/apache2/sites-available/keithlard.com.conf]: Scheduling
    refresh of Exec[enable-keithlard.com-vhost]

    notice: /Stage[main]//Node[cookbook]/Apache::Site[keithlard.com]/
    Exec[enable-keithlard.com-vhost]: Triggered 'refresh' from 1
    events

    info: /Stage[main]//Node[cookbook]/Apache::Site[keithlard.
    com]/Exec[enable-keithlard.com-vhost]: Scheduling refresh of
    Service[apache2]

    notice: /Stage[main]/Apache/Service[apache2]: Triggered 'refresh'
    from 2 events
    notice: Finished catalog run in 3.79 seconds 
    ```

#### 工作原理

名为 apache::site 的 define 使用 vhost.erb 模板生成 Apache 虚拟主机的定义。 默认情况下，假设站点的域名与站点实例的名字相同，本例中是 keithlard.com。 所以当 Puppet 看到如下代码时：

```
apache::site { "keithlard.com": } 
```

它就使用 keithlard.com 作为站点域名。如果你要指定不同的域名，请添加 sitedomain 参数：

```
apache::site { "networkr_production":
    sitedomain => "networkr.com",
}

apache::site { "networkr_staging":
    sitedomain => "staging.networkr.com",
} 
```

模板系统的优秀之处在于：如果你想为所有站点重新配置一个值（例如，更改管理员的 e-mail 地址）， 你只需要修改一次模板，Puppet 就会根据模板相应地更新所有的虚拟主机。

同样地，如果你需要为虚拟主机指定与默认值（/var/www/${name}）不同的 DocumentRoot， 请添加如下的 documentroot 参数：

```
apache::site { "communitysafety.org":
    documentroot => "/var/apps/commsafe",
} 
```

#### 更多用法

在前面的例子中，我们只在模板中定义了一个变量，但只要你愿意，你可以使用更多的变量。 它们也可以是 **facts**，例如：

```
ServerName <%= fqdn %> 
```

或者 Ruby 表达式：

```
ServerAdmin<%= emails["admin"] %> 
```

或者任何你要执行的 Ruby 代码：

```
ServerAdmin <%= vhost_domain == 'coldcomfort.com' ? 'seth@coldcomfort.
com' : 'flora@poste.com' %> 
```

#### 参见本书

*   第五章的 在模板中遍历数组 一节

# 创建 Nginx 虚拟主机

### 创建 Nginx 虚拟主机

**Nginx** 是一个快速的、轻量级的 Web 服务器软件， 在许多情况下它已取代了 Apache，尤其是运行 Web 应用程序的情况。 然而，其配置语言与 Apache 相比并没有做什么重大改进，仍然不够简单清晰。 此外，大多数的配置文档只有俄语版，这也就解释了为什么你看到这么多份关于 "Understanding Russian for Nginx Administrators" 的拷贝。

#### 准备工作

你需要 当文件更新时运行命令 一节中曾使用的 Nginx 模块。 如果你像本章 管理 Apache 服务 一节中那样创建了 Apache 模块， 你还需要使用如下命令关闭 Apache 服务：

```
# service apache2 stop 
```

#### 操作步骤

1.  添加如下代码到 /etc/puppet/modules/nginx/manifests/init.pp：

    ```
    define site( $sitedomain = "" ) {
        include nginx

        if $sitedomain == "" {
            $vhost_domain = $name
        } else {
            $vhost_domain = $sitedomain
        }

        file { "/etc/nginx/sites-available/${vhost_domain}.conf":
            content =&gt; template("nginx/vhost.erb"),
            require =&gt; Package["nginx"],
        }

        file { "/etc/nginx/sites-enabled/${vhost_domain}.conf":
            ensure  =&gt; link,
            target  =&gt; "/etc/nginx/sites-available/${vhost_domain}.
     conf",
            require =&gt; File["/etc/nginx/sites-available/${vhost_
     domain}.conf"],
            notify  =&gt; Exec["reload nginx"],
        }
    } 
    ```

2.  使用如下内容创建 /etc/puppet/modules/nginx/templates/vhost.erb 文件：

    ```
    server {
        listen 80;
        server_name &lt;%= vhost_domain %&gt;;
        access_log /var/log/nginx/&lt;%= vhost_domain %&gt;-access_log;
        root /var/www/&lt;%= vhost_domain %&gt;;
    } 
    ```

3.  创建目录 /var/www/bbqrecipes.com，使用适当的信息创建此目录下的 index.html 文件：

    ```
    Welcome to the BBQ Recipes site! 
    ```

4.  添加如下代码到一个节点：

    ```
    nginx::site { "bbqrecipes.com": } 
    ```

5.  运行 Puppet：

    ```
    # puppet agent -test

    info: Retrieving plugin info: Caching catalog for cookbook.
    bitfieldconsulting.com info: Applying configuration version
    '1309198476'

    notice: /Stage[main]/Nginx/Package[nginx]/ensure: ensure changed
    'purged' to 'present'

    notice: /Stage[main]//Node[cookbook]/Nginx::Site[bbqrecipes.com]/
    File[/etc/nginx/sites-available/bbqrecipes.com.conf]/ensure:
    defined content as '{md5}fa92d2e7543b378e26827a063be34a31'

    notice: /Stage[main]//Node[cookbook]/Nginx::Site[bbqrecipes.com]/
    File[/etc/nginx/sites-enabled/bbqrecipes.com.conf]/ensure: created

    info: /Stage[main]//Node[cookbook]/Nginx::Site[bbqrecipes.com]/
    File[/etc/nginx/sites-enabled/bbqrecipes.com]: Scheduling refresh
    of Exec[reload nginx]

    notice: /Stage[main]/Nginx/Service[nginx]/ensure: ensure changed
    'stopped' to 'running'

    notice: /Stage[main]/Nginx/Exec[reload nginx]: Triggered 'refresh'
    from 1 event

    notice: Finished catalog run in 21.45 seconds 
    ```

#### 工作原理

以 bbqrecipes.com 为资源名对名为 nginx::site 的 define 创建一个实例， Puppet 会使用变量 vhost_domain 的值 bbqrecipes.com 调用 vhost.erb 模板生成相应的配置文件 bbqrecipes.com.conf。 此文件包含了 Nginx 为了响应对 domain 的请求所需要知道的一切， 并且指明了与 domain 对应的根文档目录所映射的文件系统位置。

#### 更多用法

与 Apache 不同，Nginx 直到现在还不支持动态模块。这就意味着， 如果想为其添加默认不包含的特殊功能，你必须自己重新编译 Nginx。 正确的做法是，使用你想要的选项编译 Nginx，然后创建一个软件包，将其放在自己的仓库中 （参见 配置 APT 软件仓库 一节的描述）提供给需要它的节点使用。

然而，一些使用 Puppet 的系统管理员希望省略这个步骤，直接提供源码包并在目标服务器上编译。 为了实现这点，可以使用 exec 资源通过类似于 从源码包自动构建软件 一节中描述的模式实现。 对于敏捷开发环境而言，被管理的有关产品几乎每隔几天就会有所变化，在这种情况下， 使用这种做法比不断重构软件包要迅速且节省由于重构软件包带来的成本。

#### 参见本书

*   本章的 管理 Rails 应用程序 一节

# 创建 MySQL 数据库及用户

### 创建 MySQL 数据库及用户

MySQL 是一个使用广泛的数据库服务器，你肯定会在某些节点上安装配置 MySQL 服务器。 本节将向你展示如何安装配置 MySQL 服务器，以及如何为应用程序自动创建数据库和用户。

#### 准备工作

1.  如果你还没有 MySQL 模块，先创建一个：

    ```
    # mkdir /etc/puppet/modules/mysql
    # mkdir /etc/puppet/modules/manifests
    # mkdir /etc/puppet/modules/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/mysql/manifests/server.pp 文件：

    ```
    class mysql::server {
        package { "mysql-server": ensure =&gt; installed }

        service { "mysql":
            enable =&gt; true,
            ensure =&gt; running,
            require =&gt; Package["mysql-server"],
        }

        file { "/etc/mysql/my.cnf":
            owner =&gt; "mysql", group =&gt; "mysql",
            source =&gt; "puppet:///mysql/my.cnf",
            notify =&gt; Service["mysql"],
            require =&gt; Package["mysql-server"],
        }

        exec { "set-mysql-password":
            unless =&gt; "/usr/bin/mysqladmin -uroot -p${mysql_password}
     status",
            command =&gt; "/usr/bin/mysqladmin -uroot password ${mysql_
     password}",
            require =&gt; Service["mysql"],
        }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/mysql/files/my.cnf 文件：

    ```
    [client]
    port = 3306
    socket = /var/run/mysqld/mysqld.sock

    [mysqld_safe]
    socket = /var/run/mysqld/mysqld.sock
    nice = 0

    [mysqld]
    user = mysql
    socket = /var/run/mysqld/mysqld.sock
    port = 3306
    datadir = /var/lib/mysql

    !includedir /etc/mysql/conf.d/ 
    ```

4.  添加如下代码到 /etc/puppet/manifests/site.pp 文件：

    ```
    $mysql_password = "secret" 
    ```

5.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1309448283'

    notice: /Stage[main]/Mysql::Server/Package[mysql-server]/ensure:
    ensure changed 'purged' to 'present'

    notice: /Stage[main]/Mysql::Server/File[/etc/mysql/my.cnf]/owner:
    owner changed 'root' to 'mysql'

    notice: /Stage[main]/Mysql::Server/File[/etc/mysql/my.cnf]/group:
    group changed 'root' to 'mysql'

    info: /Stage[main]/Mysql::Server/File[/etc/mysql/my.cnf]:
    Scheduling refresh of Service[mysql]

    info: /Stage[main]/Mysql::Server/File[/etc/mysql/my.cnf]:
    Scheduling refresh of Service[mysql]

    notice: /Stage[main]/Mysql::Server/Service[mysql]/enable: enable
    changed 'false' to 'true'

    notice: /Stage[main]/Mysql::Server/Service[mysql]: Triggered
    'refresh' from 2 events

    notice: Finished catalog run in 61.78 seconds 
    ```

#### 操作步骤

1.  添加如下代码到 /etc/puppet/modules/mysql/manifests/server.pp 文件：

    ```
    define db( $user, $password ) {
        include mysql::server

        exec { "create-${name}-db":
            unless  =&gt; "/usr/bin/mysql -u${user} -p${password}${name}",
            command =&gt; "/usr/bin/mysql -uroot -p${mysql_password} -e
      \"create database ${name}; grant all on ${name}.* to
      ${user}@localhost identified by '$password'; flush
     privileges;\"",
            require =&gt; Service["mysql"],
        }
    } 
    ```

2.  添加如下代码到一个节点：

    ```
    mysql::server::db { "johnstest":
        user =&gt; "john",
        password =&gt; "johnstest",
    } 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1309449259'

    notice: /Stage[main]//Node[cookbook]/Mysql::Server::Db[johnstest]/
    Exec[create-johnstest-db]/returns: executed successfully

    notice: Finished catalog run in 1.61 seconds 
    ```

4.  检查数据库是否已经创建，以及用户和权限的正确性：

    ```
    # mysql -ujohn -pjohnstest johnstest

    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A

    Welcome to the MySQL monitor. Commands end with ; or \g.
    Your MySQL connection id is 36
    Server version: 5.1.41-3ubuntu12.10 (Ubuntu)

    Type 'help;' or '\h' for help. Type '\c' to clear the current
    input statement.

    mysql&gt; 
    ```

#### 工作原理

mysql::server 类安装 MySQL，并使用你在 site.pp 文件中设置的 root 用户口令配置 MySQL。 名为 mysql::server::db 的 define 允许我们使用一个指定的名字创建数据库， 以及一个能访问此数据库的相关的 MySQL 用户。 例如，一个典型的 web 应用程序可能需要一个以应用程序命名的数据库， 以及一个可以登录数据库的特定用户名。

#### 更多用法

要创建多个数据库，只需添加多个 mysql::server::db 实例：

```
mysql::server::db { [ "test1", "test2", "test3" ]:
    user => "john",
    password => "johnstest",
} 
```

# 管理 Drupal 站点

### 管理 Drupal 站点

**Drupal** 是一个内容管理系统，它通过插拔组装一系列罐装的模块让你快速构建网站, 它使用户创建和编辑自己的内容变的相对容易。 Drupal 特别适合使用 Puppet 来管理，因为有一个强大的命令行工具 drush， 你可以使用这个工具安装、管理 Drupal 站点。

如果我们将自动化管理 Drupal 站点的 drush 工具与我们已经创建的用于管理 MySQL 数据库和 Apache 虚拟主机的 Puppet 处方相结合, 就可以使用单一资源创建一个安装 Drupal 站点所需一切的新处方。

#### 准备工作

1.  创建一个新的 drupal 模块如下：

    ```
    # mkdir /etc/puppet/modules/drupal
    # mkdir /etc/puppet/modules/drupal/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/drupal/manifests/init.pp 文件：

    ```
    class drupal {
        $drupalversion = "7.2"

        exec { "download-drush":
            cwd =&gt; "/root",
            command =&gt; "/usr/bin/wget http://ftp.drupal.org/files/
     projects/drush-7.x-4.4.tar.gz ",
            creates =&gt; "/root/drush-7.x-4.4.tar.gz",
            require =&gt; Package["php5-mysql"],
        }

        exec { "install-drush":
            cwd     =&gt; "/usr/local",
            command =&gt; "/bin/tar xvzf /root/drush-7.x-4.4.tar.gz",
            creates =&gt; "/usr/local/drush",
            require =&gt; Exec["download-drush"],
        }

        file { "/usr/local/bin/drush":
            ensure =&gt; link,
            target =&gt; "/usr/local/drush/drush",
            require =&gt; Exec["install-drush"],
        }

        exec { "install-drupal":
            cwd     =&gt; "/var/www",
            command =&gt; "/usr/local/drush/drush dl drupal-
      ${drupalversion}",
            creates =&gt; "/var/www/drupal-${drupalversion}",
            require =&gt; Exec["install-drush"],
        }

        file { "/var/www/drupal":
            ensure =&gt; link,
            target =&gt; "/var/www/drupal-${drupalversion}",
            require =&gt; Exec["install-drupal"],
        }

        package { [ "libapache2-mod-php5",
                    "php5-mysql" ]: ensure =&gt; installed }

        exec { "enable-mod-php5":
            command =&gt; "/usr/bin/a2enmod php5",
            creates =&gt; "/etc/apache2/mods-enabled/php5.conf",
            require =&gt; Package["libapache2-mod-php5"],
        }
    } 
    ```

#### 操作步骤

1.  在 init.pp 文件的 drupal 类中添加如下内容：

    ```
    define site( $password, $sitedomain = "" ) {
        include drupal

        if $sitedomain == "" {
            $drupal_domain = $name
        } else {
            $drupal_domain = $sitedomain
        }

        $dbname = regsubst( $drupal_domain, "\.", "" )
        mysql::server::db { $dbname:
            user =&gt; $dbname,
            password =&gt; $password,
        }

        exec { "site-install-${name}":
            cwd =&gt; "/var/www/drupal",
            command =&gt; "/usr/local/bin/drush site-install -y
     --site-name=${name} --sites-subdir=${drupal_domain}
     --db-url=mysql://${dbname}:${password}@localhost/${dbname}",
            creates =&gt; "/var/www/drupal/sites/${drupal_domain}",
            require =&gt; [ File["/var/www/drupal"], Exec["install-drupal"],
             Mysql::Server::Db[$dbname] ],
            logoutput =&gt; on_failure,
        }

        apache::site { $drupal_domain:
            documentroot =&gt; "/var/www/drupal",
        }
    } 
    ```

2.  添加如下内容到一个节点：

    ```
    drupal::site { "crispinfo.com":
        password =&gt; "crunch",
    } 
    ```

3.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1309783783'

    notice: /Stage[main]//Node[cookbook]/Drupal::Site[crispinfo.com]/
    Mysql::Server::Db[crispinfocom]/Exec[create-crispinfocom-db]/
    returns: executed successfully

    notice: /Stage[main]//Node[cookbook]/Drupal::Site[crispinfo.com]/
    Apache::Site[crispinfo.com]/File[/etc/apache2/sites-available/
    crispinfo.com.conf]/ensure: defined content as '{md5}15c5bbffa6128
    fce0b8a3996914af549'

    info: /Stage[main]//Node[cookbook]/Drupal::Site[crispinfo.com]/
    Apache::Site[crispinfo.com]/File[/etc/apache2/sites-available/
    crispinfo.com.conf]: Scheduling refresh of Exec[enable-crispinfo.
    com-vhost]

    notice: /Stage[main]//Node[cookbook]/Drupal::Site[crispinfo.com]/
    Apache::Site[crispinfo.com]/Exec[enable-crispinfo.com-vhost]:
    Triggered 'refresh' from 1 events

    info: /Stage[main]//Node[cookbook]/Drupal::Site[crispinfo.com]/
    Apache::Site[crispinfo.com]/Exec[enable-crispinfo.com-vhost]:
    Scheduling refresh of Service[apache2]

    notice: /Stage[main]/Apache/Service[apache2]: Triggered 'refresh'
    from 1 events

    notice: /Stage[main]//Node[cookbook]/Drupal::Site[crispinfo.com]/
    Exec[site-install-crispinfo.com]/returns: executed successfully
    notice: Finished catalog run in 22.51 seconds 
    ```

4.  在 /etc/hosts 文件中创建一个条目将 crispinfo.com 指向你正在使用的节点 IP （如果还没设置 DNS）：

    ```
    10.0.2.15 crispinfo.com 
    ```

5.  在浏览器中检查站点，以确保一切都已经正确创建。你应该看到 Drupal 的登录提示，在下图所示：

    ![img/ch07sec05.png](img/ch07sec05.png)

    使用由 drush site-install 创建的默认的管理员登录，用户名为 admin 其口令为 admin。 显然你应该为实际生产线上的站点设置强壮的口令 （查看 drush 文档获得如何使用命令行工具设置的信息）。

#### 工作原理

真是神奇呀！尤其是 drupal 类首先安装 drush，然后使用它安装 Drupal 的核心代码 （你可以通过修改 $drupalversion 的值改变版本）。

drupal::sitedefine 为你想要创建的每个站点运行 drush site-install 。 在我们的例子中，创建了一个名为 crispinfo.com 的站点并为其传递了站点数据库使用的口令， 其余的工作都由 drush 去完成。

drupal::site 也为我们的站点创建了所需的 **Apache 虚拟主机** （使用本章 创建 Apache 虚拟主机 一节中的处方) 和 **MySQL 数据库** （使用本章 创建 MySQL 数据库及用户 一节中的处方）。

#### 更多用法

要管理 Drupal 站点，drush 可以帮你做很多事，包括更新 Drupal 的核心代码、 安装模块和主题模板、管理用户以及备份站点数据库等。 你可以在 [`drush.ws/`](http://drush.ws/) 找到更多关于 drush 的信息。

# 管理 Rails 应用程序

### 管理 Rails 应用程序

**Rails** 是一个非常受欢迎的 Web 应用程序框架（从某种意义上说，是由于它被广泛应用而不是人们真正喜欢它）。 因此，在某些时候，你可能会被要求管理它。 本节要介绍的处方包含了安装一台运行 Rails 应用程序服务器所要做的绝大部分工作。 本处方假定你会使用 Nginx 和 **Passenger** 作为 Web 服务器， 然而你也可以轻松地修改本处方，使用 Apache 替换它。

#### 操作步骤

1.  创建 rails 模块的目录结构：

    ```
    # mkdir /etc/puppet/modules/rails
    # mkdir /etc/puppet/modules/rails/manifests
    # mkdir /etc/puppet/modules/rails/templates
    # mkdir /etc/puppet/modules/rails/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/rails/manifests/init.pp 文件：

    ```
    class rails {
        include rails::passenger

        package { "bundler":
            provider =&gt; gem,
            ensure   =&gt; installed,
        }

        define app( $sitedomain ) {
            include rails

            file { "/opt/nginx/sites-available/${name}.conf":
              content =&gt; template("rails/app.conf.erb"),
              require =&gt; File["/opt/nginx/sites-available"],
            }

            file { "/opt/nginx/sites-enabled/${name}.conf":
              ensure  =&gt; link,
              target  =&gt; "/opt/nginx/sites-available/${name}.conf",
              require =&gt; File["/opt/nginx/sites-enabled"],
              notify  =&gt; Exec["reload-nginx"],
            }

            file { "/opt/nginx/conf/includes/${name}.conf":
              source =&gt; [ "puppet:///modules/rails/${name}.conf",
                          "puppet:///modules/rails/empty.conf" ],
              notify =&gt; Exec["reload-nginx"],
            }

            file { [ "/var/www",
                     "/var/www/${name}",
                     "/var/www/${name}/releases",
                     "/var/www/${name}/shared",
                     "/var/www/${name}/shared/config",
                     "/var/www/${name}/shared/log",
                     "/var/www/${name}/shared/system" ]:
                ensure =&gt; directory,
                mode   =&gt; 775,
                owner  =&gt; "www-data",
                group  =&gt; "www-data",
            }
        }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/rails/manifests/passenger.pp 文件：

    ```
    class rails::passenger {
        $passenger_version = "3.0.7"
        $passenger_dependencies = [ "build-essential",
                                    "libcurl4-openssl-dev",
                                    "libssl-dev",
                                    "ruby",
                                    "rubygems" ]

        package { $passenger_dependencies: ensure =&gt; installed }
            exec { "install-passenger":
            command =&gt; "/usr/bin/gem install passenger
     --version=${passenger_version}",
             unless =&gt; "/usr/bin/gem list | /bin/grep passenger |/bin/
     grep ${passenger_version}",
            require =&gt; [ Package["rubygems"], Package[$passenger_
             dependencies] ],
            timeout =&gt; "-1",
        }

        exec { "install-passenger-nginx-module":
            command =&gt; "/usr/lib/ruby/gems/1.8/gems/passenger-
      ${passenger_version}/bin/passenger-install-nginx-module
     --auto --auto-download --prefix=/opt/nginx",
            creates =&gt; "/opt/nginx/sbin/nginx",
            require =&gt; Exec["install-passenger"],
            timeout =&gt; "-1",
        }

        file { [ "/opt/nginx",
                 "/opt/nginx/conf",
                 "/opt/nginx/conf/includes",
                 "/opt/nginx/sites-enabled",
                 "/opt/nginx/sites-available",
                 "/var/log/nginx" ]:
            ensure =&gt; directory,
            owner =&gt; "www-data",
            group =&gt; "www-data",
        }

        file { "/opt/nginx/sites-enabled/default":
            ensure =&gt; absent,
            require =&gt; Exec["install-passenger-nginx-module"],
        }

        file { "/opt/nginx/conf/nginx.conf":
            content =&gt; template("rails/nginx.conf.erb"),
            notify  =&gt; Exec["reload-nginx"],
            require =&gt; Exec["install-passenger-nginx-module"],
        }

        file { "/etc/init.d/nginx":
            source  =&gt; "puppet:///modules/rails/nginx.init",
            mode    =&gt; "700",
            require =&gt; Exec["install-passenger-nginx-module"],
        }

        service { "nginx":
            enable =&gt; true,
            ensure =&gt; running,
            require =&gt; File["/etc/init.d/nginx"],
        }

        exec { "reload-nginx":
            command     =&gt; "/opt/nginx/sbin/nginx -t && /etc/init.d/nginx reload",
            refreshonly =&gt; true,
            require     =&gt; Exec["install-passenger-nginx-module"],
        }
    } 
    ```

4.  使用如下内容创建 /etc/puppet/modules/rails/templates/app.conf.erb 文件：

    ```
    server {
        listen 80;
        root /var/www/&lt;%= name %&gt;/current/public;
        server_name &lt;%= sitedomain %&gt;;
        access_log /var/log/nginx/&lt;%= name %&gt;.access.log;
        error_log /var/log/nginx/&lt;%= name %&gt;.error.log;

        passenger_enabled on;
        passenger_min_instances 1;
    }

    passenger_pre_start http://&lt;%= sitedomain %&gt;; 
    ```

5.  使用如下内容创建 /etc/puppet/modules/rails/templates/nginx.conf.erb 文件：

    ```
    events {
      worker_connections 1024;
      use epoll;
    }

    http {
      passenger_root /usr/lib/ruby/gems/1.8/gems/passenger-&lt;%=
      passenger_version %&gt;;

      server_names_hash_bucket_size 64;

      sendfile on;
      tcp_nopush on;
      tcp_nodelay off;
      client_body_temp_path /var/spool/nginx-client-body 1 2;

      client_max_body_size 100m;

      include /opt/nginx/conf/mime.types;
      default_type application/octet-stream;

      log_format main '$remote_addr - $remote_user [$time_local] '
              $request" $status $body_bytes_sent "$http_referer" '
              '"$http_user_agent" "$http_x_forwarded_for"' ;

      access_log /var/log/nginx/access.log main;

      gzip on;
      gzip_http_version 1.0;
      gzip_comp_level 2;
      gzip_proxied any;
      gzip_min_length 1100;
      gzip_buffers 16 8k;
      gzip_types text/plain text/html text/css application/x-javascript
       text/xml application/xml application/xml+rss text/javascript;
      gzip_disable "MSIE [1-6].(?!.*SV1)";
      gzip_vary on;

      include /opt/nginx/sites-enabled/*;
    } 
    ```

6.  使用如下内容创建 /etc/puppet/modules/rails/files/nginx.init 文件：

    ```
    #!/bin/sh

    ### BEGIN INIT INFO
    # Provides: nginx
    # Required-Start: $all
    # Required-Stop: $all
    # Default-Start: 2 3 4 5
    # Default-Stop: 0 1 6
    # Short-Description: starts the nginx web server
    # Description: starts nginx using start-stop-daemon
    ### END INIT INFO

    PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
    DAEMON=/opt/nginx/sbin/nginx
    NAME=nginx
    DESC=nginx

    test -x $DAEMON || exit 0

    # Include nginx defaults if available
    if [ -f /etc/default/nginx ] ; then
       . /etc/default/nginx
    fi

    set -e

    # Return LSB status, grabbed from a newer lsb-base
    status_of_proc () {
        local pidfile daemon name status

        pidfile=
        OPTIND=1
        while getopts p: opt ; do
     case "$opt" in
                p) pidfile="$OPTARG";;
            esac
     done

      shift $(($OPTIND - 1))

        if [ -n "$pidfile" ]; then
      pidfile="-p $pidfile"
        fi

      daemon="$1"
        name="$2"

        status="0"
        pidofproc $pidfile $daemon &gt;/dev/null || status="$?"
        if [ "$status" = 0 ]; then
      log_success_msg "$name is running"
            return 0
        else
      log_failure_msg "$name is not running"
            return $status
        fi
    }

    . /lib/lsb/init-functions

    case "$1" in
      start)
        echo -n "Starting $DESC: "
        start-stop-daemon --start --quiet --pidfile /var/run/$NAME.pid \
         --exec $DAEMON -- $DAEMON_OPTS || true
     echo "$NAME."
        ;;
      stop)
        echo -n "Stopping $DESC: "
        start-stop-daemon --stop --quiet --pidfile /var/run/$NAME.pid \
         --exec $DAEMON || true
     echo "$NAME."
        ;;
      restart|force-reload)
        echo -n "Restarting $DESC: "
        start-stop-daemon --stop --quiet --pidfile \
         /var/run/$NAME.pid --exec $DAEMON || true
      sleep 1
        start-stop-daemon --start --quiet --pidfile \
         /var/run/$NAME.pid --exec $DAEMON -- $DAEMON_OPTS || true
     echo "$NAME."
        ;;
      reload)
        echo -n "Reloading $DESC configuration: "
        start-stop-daemon --stop --signal HUP --quiet --pidfile \
         /var/run/$NAME.pid --exec $DAEMON || true
     echo "$NAME."
        ;;
      status)
        status_of_proc -p /var/run/$NAME.pid "$DAEMON" nginx \
         && exit 0 || exit $?
        ;;
      *)
        N=/etc/init.d/$NAME
        echo "Usage: $N {start|stop|restart|reload|forcereload|
     status}" &gt;&2
        exit 1
        ;;
    esac
    exit 0 
    ```

7.  添加如下代码到一个节点：

    ```
    rails::app { "furiouspigs":
        sitedomain =&gt; "furiouspigs.com",
    } 
    ```

8.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1309960678'

    notice: /Stage[main]/Rails::Passenger/File[/opt/nginx]/ensure:
    created

    notice: /Stage[main]/Rails::Passenger/File[/opt/nginx/sitesenabled]/
    ensure: created

    notice: /Stage[main]//Node[cookbook]/Rails::App[furiouspigs]/
    File[/opt/nginx/sites-enabled/furiouspigs.conf]/ensure: created

    notice: /Stage[main]/Rails::Passenger/File[/opt/nginx/conf]/
    ensure: created

    notice: /Stage[main]/Rails::Passenger/File[/opt/nginx/conf/
    includes]/ensure: created

    notice: /Stage[main]//Node[cookbook]/Rails::App[furiouspigs]/
    File[/opt/nginx/conf/includes/furiouspigs.conf]/ensure: defined
    content as '{md5}d41d8cd98f00b204e9800998ecf8427e'

    notice: /Stage[main]/Rails::Passenger/File[/opt/nginx/sitesavailable]/
    ensure: created

    notice: /Stage[main]//Node[cookbook]/Rails::App[furiouspigs]/
    File[/opt/nginx/sites-available/furiouspigs.conf]/ensure: defined
    content as '{md5}c1a4c2bc4e7381b1c2f88dfee004a594'

    notice: /Stage[main]/Rails::Passenger/Exec[install-passenger]/
    returns: executed successfully

    notice: /Stage[main]/Rails::Passenger/Exec[install-passengernginx-
    module]/returns: executed successfully
    --- /opt/nginx/conf/nginx.conf 2011-07-06 14:04:33.231999538
    +0000
    +++ /tmp/puppet-file20110706-5343-k8ouds-0 2011-07-06
    14:04:34.246867124 +0000
    ...

    info: /Stage[main]/Rails::Passenger/File[/opt/nginx/conf/nginx.
    conf]: Filebucketed /opt/nginx/conf/nginx.conf to puppet with sum
    34d60856b6570e9d59cd6eecde5da000

    notice: /Stage[main]/Rails::Passenger/File[/opt/nginx/conf/nginx.
    conf]/content: content changed '{md5}34d60856b6570e9d59cd6eecde5
    da000' to '{md5}72132deeb45e6ee5b83cd246dffefc5f'
    info: /Stage[main]/Rails::Passenger/File[/opt/nginx/conf/nginx.
    conf]: Scheduling refresh of Exec[reload-nginx]
    notice: /Stage[main]/Rails::Passenger/Exec[reload-nginx]:
    Triggered 'refresh' from 1 events
    notice: Finished catalog run in 398.73 seconds 
    ```

#### 工作原理

这个处方比本书中其他的处方更长、更复杂，因此需要更详细的解释。 如果你觉得这有些烦人，尽管去使用这个配方吧，不必担心它是如何工作的。 稍后当你想学习更多详细工作过程时，可以再回过头来看这些解释。

上面所有代码的目的就是可以让你写出如下的实例化代码：

```
rails::app { "furiouspigs":
    sitedomain => "furiouspigs.com",
} 
```

这需要不少幕后工作。我们需要安装配置包含 Passenger 模块的 Nginx， 配置应用程序的虚拟主机，包括所有应用程序特定的配置（比如重定向以及其他服务配置）， 安装 Ruby 和 Rubygems，安装 Bundler，并创建要部署的应用程序所需的所有目录。

##### Nginx 和 Passenger

此处分离出的 passenger.pp 文件用于安装 Nginx 和 Passenger 所需的一切。 之前曾经提到过，Nginx 没有像 Apache 一样的动态模块概念， 因此，你不能仅仅通过安装发行版中的 Nginx 并安装提供 Passenger 功能的软件包来实现。 Nginx 必须与你想要的任何模块一起编译。

幸好，**Phusion** 社区里的好心人已经为我们提供了一个编译脚本（passengerinstall-nginx-module）。 一旦你已经安装了 Passenger 的 gem 包，这个脚本就会帮你完成编译工作。 所以首先需要做的事就是安装 Passenger 的 **gem**：

```
class rails::passenger {
    $passenger_version = "3.0.7"
    $passenger_dependencies = [ "build-essential",
                                "libcurl4-openssl-dev",
                                "libssl-dev",
                                "ruby",
                                "rubygems" ]
    package { $passenger_dependencies: ensure => installed }

    exec { "install-passenger":
        command => "/usr/bin/gem install passenger
 --version=${passenger_version}",
        unless => "/usr/bin/gem list | /bin/grep passenger \
 |/bin/grep ${passenger_version}",
        require => [ Package["rubygems"], Package[$passenger_
         dependencies] ],
        timeout => "-1",
    } 
```

我们把要安装的 Passenger 版本号设置在变量 $passenger_version 中， 因为 Nginx 需要知道 Passenger 的安装路径（路径中包括版本号）。 所以我们会在 nginx.conf 模板中引用 $passenger_version 变量。

下一步是运行 passenger-install-nginx-module 脚本：

```
 exec { "install-passenger-nginx-module":
        command => "/usr/lib/ruby/gems/1.8/gems/passenger-${passenger_
 version}/bin/passenger-install-nginx-module --auto --autodownload
 --prefix=/opt/nginx",

        creates => "/opt/nginx/sbin/nginx",
        require => Exec["install-passenger"],
        timeout => "-1",
    } 
```

> ![注记](img/note.png)
> 
> 你应该注意到了，此处 **gem** 的路径是固定的 /usr/lib/ruby/gems/1.8/gems。 这有些脆弱?—?在大部分生产基础设施中，我使用 **RVM** 管理 Ruby 版本和不同版本的 **gemsets**， 这就解决了由于使用固定路径带来的脆弱性。 然而，添加 RVM 会使本处方变得难于理解，所以我将使用 RVM 的部分放在 更多用法 小节中。 一旦你熟悉了这个处方，就可以修改它，使之适合你自己的需求，包括整合 RVM。

这也就意味着，如果你使用 Ruby 1.9，这个处方不会工作，当本书出版后你读到此处时很可能会发生这种情况。 如果是这样，或者你遇到其他问题，请手工运行 gem contents passenger 命令之后查找 passenger-installnginx-module 脚本的路径。

下一步，我们创建了 Nginx 配置文件所需的目录结构：

```
file { [ "/opt/nginx",
         "/opt/nginx/conf",
         "/opt/nginx/conf/includes",
         "/opt/nginx/sites-enabled",
         "/opt/nginx/sites-available",
         "/var/log/nginx" ]:
    ensure => directory,
    owner  => "www-data",
    group  => "www-data",
} 
```

我们要删除默认的 Nginx 虚拟主机配置，否则可能会干扰我们要创建的虚拟主机。 这可以通过如下代码实现：

```
file { "/opt/nginx/sites-enabled/default":
    ensure  => absent,
    require => Exec["install-passenger-nginx-module"],
} 
```

实际上，如果你从源代码构建 Nginx 或是通过 Passenger 构建（本例中的方法），这是不需要的， 但是如果要想使本处方也适用于发行版的 Nginx 软件包，这会是有益的。

下面是 Nginx 的主配置文件：

```
file { "/opt/nginx/conf/nginx.conf":
    content => template("rails/nginx.conf.erb"),
    notify  => Exec["reload-nginx"],
    require => Exec["install-passenger-nginx-module"],
} 
```

主配置文件使用 nginx.conf.erb 模板生成，因为我们需要插入之前定义的 Passenger 版本号：

```
passenger_root /usr/lib/ruby/gems/1.8/gems/passenger-<%= passenger_
version %>; 
```

否则，它就是一个标准的 Nginx 配置，你也可以在模板中添加任何你的服务器所需的特殊参数。

因为我们没有使用发行版提供的软件包，所以我们需要为节点应用一个 init 脚本 （改编自 Ubuntu 版本，仅做了轻微的修改）：

```
file { "/etc/init.d/nginx":
    source  => "puppet:///modules/rails/nginx.init",
    mode    => "700",
    require => Exec["install-passenger-nginx-module"],
} 
```

为了运行 Nginx 服务需要如下代码：

```
service { "nginx":
    enable  => true,
    ensure  => running,
    require => File["/etc/init.d/nginx"],
} 
```

要确保错误的配置不会使服务器宕机，配置文件的改变会通知如下的 “配置检查和重载” 资源：

```
exec { "reload-nginx":
    command     => "/opt/nginx/sbin/nginx -t \
 && /etc/init.d/nginx reload",
    refreshonly => true,
    require     => Exec["install-passenger-nginx-module"],
} 
```

##### Rails

你已经设置好了 Passenger 和 Nginx，接下来配置 **Rails** 需求的类：

```
class rails {
    include rails::passenger

    package { "bundler":
        provider => gem,
        ensure => installed,
    }
} 
```

**Bundler** 是一个管理应用程序或解决 gem 依赖关系的工具。 你可以使用手工方式（或通过 Puppet）指定要安装的所有 gem 包以及依赖的包， 取代这种方式的更好办法是使用 Bundler 来实现，它是 Rails 部署的一部分。 例如，你应该注意到了，我们没有安装 rails 的 gem；这通常是通过 Bundler 安装的， 或者在应用程序的 vendor 目录中已经提供了一份特定版本的 rails。 如果你没有使用 Bundler，或者你需要为你的 Rails 应用程序设置一些额外的依赖， 请在这个类中使用 Puppet 的 package 资源做相应的配置来安装它们。

rails 类的的主要部分是 define 函数 app， 对于你要管理的每个应用程序，它都会被实例化一次：

```
define app( $sitedomain ) {
    include rails 
```

为应用程序做的第一件事是安装 Nginx 虚拟主机配置文件，它由 app.conf.erb 模板生成：

```
 file { "/opt/nginx/sites-available/${name}.conf":
        content => template("rails/app.conf.erb"),
        require => File["/opt/nginx/sites-available"],
    }

    file { "/opt/nginx/sites-enabled/${name}.conf":
        ensure => link,
        target => "/opt/nginx/sites-available/${name}.conf",
        require => File["/opt/nginx/sites-enabled"],
        notify => Exec["reload-nginx"],
    } 
```

虚拟主机配置文件的模板相当小：

```
server {
    listen 80;
    root /var/www/<%= name %>/current/public;
    server_name <%= sitedomain %>;
    access_log /var/log/nginx/<%= name %>.access.log;
    error_log /var/log/nginx/<%= name %>.error.log;

    passenger_enabled on;
    passenger_min_instances 1;
}

passenger_pre_start http://<%= sitedomain %>; 
```

通常一个应用程序需要特定的 Nginx 配置指令，比如 redirects。 你可以在 Rails 模块中添加一个名为 files/furiouspigs.conf 的文件来包含这些配置指令。 Puppet 会从如下的这段代码找到这个文件，并分发给节点：

```
 file { "/opt/nginx/conf/includes/${name}.conf":
        source => [ "puppet:///modules/rails/${name}.conf",
                    "puppet:///modules/rails/empty.conf" ],
        notify => Exec["reload-nginx"],
    } 
```

注意这个文件使用了多个源，第二个源 empty.conf 确保 Puppet 不会因为应用程序指定的配置文件不存在而抱怨。

最后我们要确保为准备部署的标准 Rails 目录结构配置 www-data 用户属主及适当的权限（775）。 如果你的 Nginx 以及部署的应用程序以不同的用户身份执行，请使用你的用户名替换所有的 www-data。

```
 file { [ "/var/www",
                "/var/www/${name}",
                "/var/www/${name}/releases",
                "/var/www/${name}/shared",
                "/var/www/${name}/shared/config",
                "/var/www/${name}/shared/log",
                "/var/www/${name}/shared/system" ]:
            ensure => directory,
            mode   => 775,
            owner  => "www-data",
            group  => "www-data",
        }
    }
} 
```

#### 更多用法

使用 Puppet 管理 Rails 应用程序，你可能还需要考虑一些其他的事情。

##### RVM

正如我之前所提到的，使用 RVM 管理多版本 Ruby 和多版本的 gemsets 是一种强大的解决方案。 当然，使用 RVM 也带来了自身的有趣问题?—?RVM 的开发活跃，其主体经常变化。 无论如何，RVM 为我们带来的好处还是比其带来的麻烦要多的多。 所以，我建议你对生产线上的 Rails 站点，或许可以通过类似这样的代码应用 RVM：

```
class rails::rvm {
  package { [ "autoconf",
              "bison",
              "curl",
              "libreadline-dev",
              "subversion",
              "zlib1g-dev" ]:
      ensure => installed
  }

  file { "/usr/local/bin/rvm-install-system-wide":
      source => "puppet:///modules/rails/rvm-install-system-wide",
      mode   => "700",
  }

  exec { "install-rvm":
      command => "/usr/local/bin/rvm-install-system-wide",
      creates => "/usr/local/bin/rvm",
      require => [ Package["curl"], Package["subversion"],
                   File["/usr/local/bin/rvm-install-system-wide"] ],
      logoutput => on_failure,
  }

  append_if_no_such_line { "setup-rvm-shell-environment":
      file => "/etc/bash.bashrc",
      line => "[[ -s /usr/local/rvm/scripts/rvm ]] \
 && . /usr/local/rvm/scripts/rvm",
  }
} 
```

rvm-install-system-wide 脚本来自 RVM 网站： [`rvm.beginrescueend.com/install/rvm`](https://rvm.beginrescueend.com/install/rvm) 。

##### 日志滚动

在生产环境中你可能会需要为 Nginx 和 Rails 生成的日志文件添加 logrotate 配置片段， 以确保这些日志不会逐步占满你的磁盘。因为篇幅限制和保持简单的原因，本例省略了对日志滚动的配置。

##### 数据库

本处方中，未对 Rails 应用程序创建任何数据库及访问数据库的用户； 配置何种数据库取决于你团队的开发者正在使用何种数据库（MySQL、Postgres、MongoDB 等）， 你需要自行添加管理数据库的 Puppet 代码。如果使用的是 MySQL， 你可以参考 创建 MySQL 数据库及用户 一节的内容并作适当的改写。

##### SSL 证书

一些应用程序会需要 SSL 证书并为 **vhost** 配置安全的 URLs，例如：处理网上支付。 这已超出了本处方的讲解范围，但你应该可以发现添加所需的代码并不困难。 你可以在 rails::app 函数的定义中添加一个可选参数，例如：

```
define app( $sitedomain, $ssl = false ) { 
```

并添加如下代码处理这个参数：

```
if $ssl {
   file { "/etc/ssl/certs/${name}.crt":
       source => "puppet:///modules/rails/${name}.crt",
   }
} 
```

然后，只要使用如下的代码对你的应用程序进行实例化即可：

```
rails::app { "irritatedbadgers":
    sitedomain => "irritatedbadgers.com",
    ssl        => true,
} 
```