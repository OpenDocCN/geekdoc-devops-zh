# 服务器和云基础设施

## 服务器和云基础设施

> Rest is not idleness, and to lie sometimes on the grass under trees on a summer’s day, listening to the murmur of the water, or watching the clouds float across the sky, is by no means a waste of time.
> 
> — J. Lubbock

在本章中，我们将学习如下内容：

*   部署 Nagios 监控服务器

*   使用 Heartbeat 构建高可用服务

*   管理 NFS 服务和文件共享

*   使用 HAProxy 为多个 web 服务器实现负载均衡

*   使用 iptables 管理防火墙

*   管理 Amazon 的 EC2 实例

*   使用 Vagrant 管理虚拟机

Puppet 的强大不仅在于它可以管理配置单台服务器还可以控制网络中的大量服务器， 只有控制大量服务器时才能真正显现其效益。 在本章中我们将探讨：如何使用 Puppet 帮助你监控基础设施， 创建高可用性集群，跨越网络分享文件，设立自动的防火墙， 为你的众多后端服务器实现负载均衡，以及在云中或桌面上创建新虚拟机。

# 部署 Nagios 监控服务器

### 部署 Nagios 监控服务器

> My roommate lost his pet elephant. It’s in the apartment somewhere.
> 
> — Steven Wright

我们无法持续关注所有的一切。问题是：你怎么知道一台服务器何时会出现故障呢？ 错误的答案是，“我的客户打电话给我，告诉我服务器宕机了。” 但是，你会惊奇地发现，许多组织的系统没有任何的自动监控。自动监控的设置非常简单。 目前有许多优秀的免费开源的自动监测工具可用，包括 Nagios、Icinga、Zabbix 和 Zenoss。 Nagios 是其中历史最久最复杂的一个，尽管它有一个难以配置的名声（基本上是实至名归的）。

本处方将向你展示如何使用 Puppet 创建一个基于 Nagios 的监控服务器以及 如何让 Puppet 配置由 Nagios 监控的每台机器。

#### 准备工作

你将需要我们已经在第七章的 管理 Apache 服务 一节中创建的 Apache 模块。

#### 操作步骤

1.  创建一个 nagios 模块：

    ```
    # mkdir /etc/puppet/modules/nagios
    # mkdir /etc/puppet/modules/nagios/files
    # mkdir /etc/puppet/modules/nagios/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/nagios/manifests/server.pp 文件：

    ```
    class nagios::server {
        include apache

        package { [ "nagios3",
                    "nagios-images",
                    "nagios-nrpe-plugin" ]:
            ensure =&gt; installed,
        }

        service { "nagios3":
            ensure =&gt; running,
            enable =&gt; true,
            require =&gt; Package["nagios3"],
        }

        exec { "nagios-config-check":
            command =&gt; "/usr/sbin/nagios3 -v /etc/nagios3/nagios.
     cfg && /usr/sbin/service nagios3 restart",
            refreshonly =&gt; true,
        }

        file { "/etc/apache2/sites-available/nagios.conf":
            source  =&gt; "puppet:///modules/nagios/nagios.conf",
            notify  =&gt; Service["apache2"],
            require =&gt; Package["apache2-mpm-prefork"],
        }

        file { "/etc/apache2/sites-enabled/nagios.conf":
            ensure  =&gt; symlink,
            target  =&gt; "/etc/apache2/sites-available/nagios.conf",
            require =&gt; Package["apache2-mpm-prefork"],
        }

        file { [ "/etc/nagios3/generic-service_nagios2.cfg",
                 "/etc/nagios3/services_nagios2.cfg",
                 "/etc/nagios3/hostgroups_nagios2.cfg",
                 "/etc/nagios3/extinfo_nagios2.cfg",
                 "/etc/nagios3/localhost_nagios2.cfg",
                 "/etc/nagios3/contacts_nagios2.cfg",
                 "/etc/nagios3/conf.d"
               ]:
            ensure =&gt; absent,
            force  =&gt; true,
        }

        define nagios-config() {
            file { "/etc/nagios3/${name}":
                source  =&gt; "puppet:///modules/nagios/${name}",
                require =&gt; Package["nagios3"],
                notify =&gt; Exec["nagios-config-check"],
            }
        }

        nagios-config { [ "htpasswd.nagios",
                          "nagios.cfg",
                          "cgi.cfg",
                          "hostgroups.cfg",
                          "hosts.cfg",
                          "host_templates.cfg",
                          "service_templates.cfg",
                          "services.cfg",
                          "timeperiods.cfg",
                          "contacts.cfg",
                          "commands.cfg" ]: }

        file { "/var/lib/nagios3":
            # see http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=478889
            mode    =&gt; 751,
            require =&gt; Package["nagios3"],
            notify  =&gt; Service["nagios3"],
        }

        file { "/var/lib/nagios3/rw":
            # see http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=478889
            mode    =&gt; 2710,
            require =&gt; Package["nagios3"],
            notify  =&gt; Service["nagios3"],
        }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/nagios/files/nagios.cfg 文件：

    ```
    # Config files to read
    cfg_file=/etc/nagios3/commands.cfg
    cfg_file=/etc/nagios3/service_templates.cfg
    cfg_file=/etc/nagios3/host_templates.cfg
    cfg_file=/etc/nagios3/timeperiods.cfg
    cfg_file=/etc/nagios3/contacts.cfg
    cfg_file=/etc/nagios3/hostgroups.cfg
    cfg_file=/etc/nagios3/hosts.cfg
    cfg_file=/etc/nagios3/services.cfg

    # Nagios settings
    log_file=/var/log/nagios3/nagios.log
    illegal_macro_output_chars=`~$&|'"&lt;&gt;
    check_result_path=/var/lib/nagios3/spool/checkresults
    nagios_user=nagios
    nagios_group=nagios
    command_file=/var/lib/nagios3/rw/nagios.cmd
    lock_file=/var/run/nagios3/nagios3.pid
    p1_file=/usr/lib/nagios3/p1.pl
    check_external_commands=1
    resource_file=/etc/nagios3/resource.cfg 
    ```

4.  使用如下内容创建 /etc/puppet/modules/nagios/files/service_templates.cfg 文件：

    ```
    define service{
        name                         generic_service
         ; The 'name' of this service template
        active_checks_enabled        1
         ; Active service checks are enabled
        passive_checks_enabled       1
         ; Passive service checks are enabled/accepted
        parallelize_check            1
         ; Active service checks should be parallelized
         ; (disabling this can lead to major performance problems)
        obsess_over_service          1
         ; We should obsess over this service (if necessary)
        check_freshness              0
         ; Default is to NOT check service 'freshness'
        notifications_enabled        1
         ; Service notifications are enabled
        event_handler_enabled        1
         ; Service event handler is enabled
        flap_detection_enabled       1
         ; Flap detection is enabled
        failure_prediction_enabled   1
         ; Failure prediction is enabled
        process_perf_data            1
         ; Process performance data
        retain_status_information    1
         ; Retain status information across program restarts
        retain_nonstatus_information 1
         ; Retain non-status information across program restarts
        notification_interval        0
         ; Only send notifications on status change by default.
        is_volatile                  0
        check_period                 24x7
        normal_check_interval        5
        retry_check_interval         2
        max_check_attempts           3
        notification_period          24x7
        notification_options         c,r
        contact_groups               sysadmin
        register                     0
         ; DONT REGISTER THIS DEFINITION
         ;   - ITS NOT A REAL SERVICE, JUST A TEMPLATE!
    }

    # Defaults
    define service {
        name                         every_5_mins
        normal_check_interval        5
        use                          generic_service
        register                     0
    }

    define service {
        name                         every_hour
        normal_check_interval        60
        use                          generic_service
        register                     0
    }

    define service {
        name                         every_day
        normal_check_interval        1440
        use                          generic_service
        register                     0
    } 
    ```

5.  使用如下内容创建 /etc/puppet/modules/nagios/files/services.cfg 文件：

    ```
    define service {
        hostgroup_name               all
        service_description          Disk
        check_command                check_nrpe!check_all_disks!20%!10%
        use                          every_day
    }

    define service {
        hostgroup_name               all
        service_description          Load
        check_command                check_nrpe!check_load!10,10,10!15,15,15
        use                          every_hour
    } 
    ```

6.  使用如下内容创建 /etc/puppet/modules/nagios/files/cgi.cfg 文件：

    ```
    main_config_file=/etc/nagios3/nagios.cfg
    physical_html_path=/usr/share/nagios3/htdocs
    url_html_path=/nagios3
    show_context_help=1
    use_pending_states=1
    nagios_check_command=/usr/lib/nagios/plugins/check_nagios
     /var/cache/nagios3/status.dat 5 '/usr/sbin/nagios3'
    use_authentication=1
    use_ssl_authentication=0
    authorized_for_system_information=nagios
    authorized_for_configuration_information=nagios
    authorized_for_system_commands=nagios
    authorized_for_all_services=nagios
    authorized_for_all_hosts=nagios
    authorized_for_all_service_commands=nagios
    authorized_for_all_host_commands=nagios
    default_statusmap_layout=5
    default_statuswrl_layout=4
    ping_syntax=/bin/ping -n -U -c 5 $HOSTADDRESS$
    refresh_rate=90
    escape_html_tags=1
    action_url_target=_blank
    notes_url_target=_blank
    lock_author_names=1 
    ```

7.  使用如下内容创建 /etc/puppet/modules/nagios/files/host_templates.cfg 文件：

    ```
    define host{
        name                          generic_host
        check_command                 check-host-alive
        max_check_attempts            3
        checks_enabled                1
        failure_prediction_enabled    1
        retain_status_information     1
        retain_nonstatus_information  1
        notification_interval         0
        notification_options          d,u,r
        check_interval                300
        contact_groups                sysadmin
        register                      0
    } 
    ```

8.  使用如下内容创建 /etc/puppet/modules/nagios/files/contacts.cfg 文件 （使用你自己的 e-mail 地址，或者至少是一个不介意从你的监控服务获得大量邮件的人的 e-mail 地址）：

    ```
    define contact {
        contact_name                    helen
        alias Helen                     Highwater
        service_notification_period     24x7
        host_notification_period        24x7
        service_notification_options    w,u,c,r
        host_notification_options       d,r
        service_notification_commands   notify-service-by-email
        host_notification_commands      notify-host-by-email
        email                           helen@example.com
    }

    define contactgroup {
        contactgroup_name               sysadmin
        alias                           Sysadmins
        members                         helen
    } 
    ```

9.  使用如下内容创建 /etc/puppet/modules/nagios/files/hostgroups.cfg 文件：

    ```
    define hostgroup {
        hostgroup_name                   all
        alias                            All Servers
        members                          *
    } 
    ```

10.  使用如下内容创建 /etc/puppet/modules/nagios/files/timeperiods.cfg 文件：

    ```
    define timeperiod {
       timeperiod_name      24x7
       alias                24 Hours A Day, 7 Days A Week
       sunday               00:00-24:00
       monday               00:00-24:00
       tuesday              00:00-24:00
       wednesday            00:00-24:00
       thursday             00:00-24:00
       friday               00:00-24:00
       saturday             00:00-24:00
    } 
    ```

11.  使用如下内容创建 /etc/puppet/modules/nagios/files/hosts.cfg 文件 （使用你自己的服务器信息替换相应的配置值）：

    ```
    define host {
       host_name           cookbook
       address             cookbook.bitfieldconsulting.com
       use                 generic_host
    } 
    ```

12.  使用如下内容创建 /etc/puppet/modules/nagios/files/commands.cfg 文件：

    ```
    define command {
       command_name   check_nrpe
       command_line   $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
        -a $ARG2$ $ARG3$ $ARG4$ $ARG5$
    }

    define command{
       command_name  check-host-alive
       command_line  $USER1$/check_ping -H '$HOSTADDRESS$' -w
        5000,100% -c 5000,100% -p 1
    }

    define command{
       command_name  check_all_disks
       command_line  /usr/lib/nagios/plugins/check_disk -w '$ARG1$'
        -c '$ARG2$' -e
    }

    define command{
       command_name   notify-host-by-email
       command_line   /usr/bin/printf "%b" "***** Nagios *****\n\
        nNotification Type: $NOTIFICATIONTYPE$\nHost: $HOSTNAME$\
        nState: $HOSTSTATE$\nAddress: $HOSTADDRESS$\nInfo:
        $HOSTOUTPUT$\n\nDate/Time: $LONGDATETIME$\n" | /usr/bin/mail
        -s "** $NOTIFICATIONTYPE$ Host Alert: $HOSTNAME$ is
        $HOSTSTATE$ **" $CONTACTEMAIL$
    }

    define command{
       command_name  notify-service-by-email
       command_line  /usr/bin/printf "%b" "***** Nagios
        *****\n\nNotification Type: $NOTIFICATIONTYPE$\n\nService:
        $SERVICEDESC$\nHost: $HOSTALIAS$\nAddress: $HOSTADDRESS$\
        nState: $SERVICESTATE$\n\nDate/Time: $LONGDATETIME$\n\
        nAdditional Info:\n\n$SERVICEOUTPUT$" | /usr/bin/mail -s "**
        $NOTIFICATIONTYPE$ Service Alert: $HOSTALIAS$/$SERVICEDESC$ is
        $SERVICESTATE$ **" $CONTACTEMAIL$
    } 
    ```

13.  使用如下内容创建 /etc/puppet/modules/nagios/files/nagios.conf 文件 （用你自己的服务器替换 ServerName 的配置值）：

    ```
    ScriptAlias /cgi-bin/nagios3 /usr/lib/cgi-bin/nagios3
    ScriptAlias /nagios3/cgi-bin /usr/lib/cgi-bin/nagios3

    Alias /nagios3/stylesheets /etc/nagios3/stylesheets
    Alias /nagios3 /usr/share/nagios3/htdocs
    Alias / /usr/share/nagios3/htdocs/

    &lt;DirectoryMatch (/usr/share/nagios3/htdocs|/usr/lib/cgi-bin/
    nagios3|/etc/nagios3/stylesheets)&gt;

       Options FollowSymLinks

       DirectoryIndex index.html

       AllowOverride AuthConfig

       Order Allow,Deny
       Allow From All

       AuthName "Nagios Access"
       AuthType Basic
       AuthUserFile /etc/nagios3/htpasswd.nagios
       require valid-user
    &lt;/DirectoryMatch&gt;

    &lt;VirtualHost *:80&gt;
       ServerName nagios.bitfieldconsulting.com
       ErrorLog /var/log/apache2/nagios-error_log
       CustomLog /var/log/apache2/nagios-access_log common
       DocumentRoot /usr/share/nagios3
    &lt;/VirtualHost&gt; 
    ```

14.  创建口令文件控制对 Nagios web 界面的访问：

    ```
    # htpasswd -c /etc/puppet/modules/nagios/files/htpasswd.nagios
    nagios
    Password: (type password) 
    ```

15.  如果你的系统中还没有 htpasswd 程序，运行如下的命令：

    ```
    # apt-get install apache2-utils 
    ```

16.  在前面的代码中指定的 ServerName 为你创建一个 /etc/hosts 条目或 DNS 记录，本例中的主机名是：

    ```
    nagios.bitfieldconsulting.com 
    ```

17.  在你的 Nagios 服务器的节点中定义中包含如下代码：

    ```
    include nagios::server 
    ```

18.  使用如下内容创建 /etc/puppet/modules/nagios/files/nrpe.cfg 文件 （使用你自己的监控服务器的主机名或 IP 地址替换 allowed_hosts 设置值）：

    ```
    log_facility=daemon
    pid_file=/var/run/nagios/nrpe.pid
    server_port=5666
    nrpe_user=nagios
    nrpe_group=nagios
    allowed_hosts=cookbook.bitfieldconsulting.com
    dont_blame_nrpe=1
    debug=0
    command_timeout=60
    connection_timeout=300
    command[check_load]=/usr/lib/nagios/plugins/check_load -w $ARG1$
    -c $ARG2$
    command[check_all_disks]=/usr/lib/nagios/plugins/check_disk -w
    $ARG1$ -c $ARG2$ -e -A -i '.gvfs' 
    ```

19.  使用如下内容创建 /etc/puppet/modules/nagios/manifests/target.pp 文件：

    ```
    class nagios::target {
       package { [ "nagios-nrpe-server",
                   "nagios-plugins",
                   "nagios-plugins-basic",
                   "nagios-plugins-standard",
                   "nagios-plugins-extra" ]:
           ensure =&gt; installed,
       }

       service { "nagios-nrpe-server":
           enable  =&gt; true,
           ensure  =&gt; running,
           pattern =&gt; "/usr/sbin/nrpe",
           require =&gt; Package["nagios-nrpe-server"],
       }

       file { "/etc/nagios/nrpe.cfg":
           source  =&gt; "puppet:///modules/nagios/nrpe.cfg",
           require =&gt; Package["nagios-nrpe-server"],
           notify  =&gt; Service["nagios-nrpe-server"],
       }
    } 
    ```

20.  在你要监控的所有节点（也应该包括 Nagios 服务器自己）上包含这个类：

    ```
    include nagios::target 
    ```

21.  在 Nagios 服务器上运行如下 Puppet 命令：

    ```
    # puppet agent --test 
    ```

22.  使用浏览器打开 Nagios 的 web 界面（使用用户名 nagios 和你在前面的代码中设置的口令登录） 检查你看到的 Nagios 欢迎界面，如图所示：

    ![img/ch08sec01_1.png](img/ch08sec01_1.png)

23.  点击 **Host detail** 菜单，在界面中你应该看到目标节点的列表。

24.  点击节点的名字，之后从 **Host commands** 菜单中选择 "Schedule a check of all services on this host"。

25.  在 "Force check" 复选框上打勾将其选中并单击 "Commit"。 这会花几秒钟运行 Nagios 的检查。 点击 "Service detail" 菜单，在界面中你应该看到显示为绿色的服务列表，如图所示：

    ![img/ch08sec01_2.png](img/ch08sec01_2.png)

#### 工作原理

尽管这个处方相当的长，但是 Puppet 的配置清单本身却很简单； 这里没有我们在其他处方中还没使用过的配置技巧。

本质上，我们所做的就是安装 Nagios 包，配置服务于它的 Apache 虚拟主机， 为 Nagios 部署一系列的配置文件，这些配置文件用于告诉 Nagios 要检查哪些主机、 检查哪些服务以及其他一些杂七杂八的配置。

在客户端，由 Nagios 监控的每个节点上都需要安装 nagios-nrpe-server 包 （**NRPE** 是使 Nagios 能在远程服务器上安全地执行命令的协议） 以及告诉 NRPE 服务允许执行哪些命令的配置文件。

> ![注记](img/note.png)
> 
> 你应该知道，nrpe.cfg 文件中的 dont_blame_nrpe 设置是一个潜在的安全漏洞， 因为它允许在远程主机上使用用户提供的参数去执行命令。 这是一个非常有用的功能，因为这意味着你可以在无需重新配置每一个监控机的情况下， 就可以改变警报阈值或其他参数。 然而，如果你不需要这个功能，禁用 dont_blame_nrpe 设置会更安全。

#### 更多用法

使用 Nagios 最为棘手的事情，就是获取、配置及首次执行。 虽然这里介绍的是非常基本的监控配置（仅包括对磁盘占用和 CPU 负载的检查）， 你可以使用这个配置作为 Nagios 设置工作的起点，添加更多要检查的服务和主机。 你可能想要添加如下的一些配置：

*   主机组（例如，web 服务器组或数据库服务器组）：你可以配置一个检查自动应用到主机组的每个成员。

*   Web 站点检查：配置 Nagios 的 check_http plugin 插件是相当复杂的， 它可以处理重定向、SSL、认证以及在一个 Web 页面中匹配文本。

*   进程检查：监视一个主机上指定的进程是一个常见的需求。使用 check_procs 插件可以实现。

*   不同的检查频率：我已经在 service_templates.cfg 模板中定义了 every_hour、 every_day 和 every_5_mins；你可能想要添加一些新的频率设置。

*   新的时间周期；在 timeperiods.cfg 中，当前仅定义了一个时间周期 24x7， 但你可能想要创建自己的时间周期。例如，如果要从午夜 0 点到凌晨 1 点执行数据库的维护工作， 你可以定义一个排除了这段时间的时间周期，而不会获得来自这个数据库服务器的误报。

要找到如何配置 Nagios 的更多内容, 请参考文档： [`nagios.sourceforge.net/docs/nagioscore/3/en/toc.html`](http://nagios.sourceforge.net/docs/nagioscore/3/en/toc.html) 。

在 Puppet 中也有一些对 Nagios 内置支持；你可以让 Puppet 从配置清单生成主机和服务的定义， 这是一个强大而有用的功能。尽管我在一些生产站点上会使用这个功能，但基于篇幅的原因， 我不得不遗憾地排除了对它的介绍。若你希望找到与这部分的相关的内容，请参考 Puppet 的官方文档以及 Mike Gurski 撰写的有关这个主题的优秀文章： [`blog.gurski.org/index.php/2010/01/28/automatic-monitoring-with-puppet-and-nagios/`](http://blog.gurski.org/index.php/2010/01/28/automatic-monitoring-with-puppet-and-nagios/) 。

# 使用 Heartbeat 构建高可用服务

### 使用 Heartbeat 构建高可用服务

> Even in the future, nothing works!
> 
> — Spaceballs

一切迟早都会发生故障。高可用服务就是指当一个主机或网络线路失效时仍旧能够提供服务。 高可用性的主要技术就是冗余，另外，这个问题的解决就是以投放更多硬件设备而著称的。

虽然最终肯定会有单独的一台服务器失效，但是两台服务器同时失效的概率是不太高的， 这对大多数的应用程序提供了一个良好的冗余水平。

最简单的方法之一是建立一对冗余服务器，它们共享一个 IP 地址，并使用心跳检测。 **心跳**（**Heartbeat**）是一个守护进程，它同时运行在两台机器上并且定期彼此交换信息（heartbeats）。 其中的一台是主服务器，通常它拥有资源：在本例中是一个 IP 地址。 如果辅助服务器无法从主服务器检测到心跳信号，它就接管地址以确保服务的连续性。

在下面的处方中，我们将使用 Puppet 的配置设置两台机器，并解释如何使用它提供一个高可用服务。

#### 操作步骤

1.  创建如下的 heartbeat 模块：

    ```
    # mkdir /etc/puppet/modules/heartbeat
    # mkdir /etc/puppet/modules/heartbeat/manifests
    # mkdir /etc/puppet/modules/heartbeat/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/heartbeat/manifests/init.pp 文件：

    ```
    class heartbeat {
      package { "heartbeat":
        ensure =&gt; installed,
      }

      service { "heartbeat":
        ensure  =&gt; running,
        require =&gt; Package["heartbeat"],
      }

      exec { "reload-heartbeat":
        command     =&gt; "/usr/sbin/service heartbeat reload",
        refreshonly =&gt; true,
      }

      file { "/etc/ha.d/authkeys":
        source  =&gt; "puppet:///modules/heartbeat/authkeys",
        mode    =&gt; "600",
        require =&gt; Package["heartbeat"],
        notify  =&gt; Exec["reload-heartbeat"],
      }

      file { "/etc/ha.d/haresources":
        source  =&gt; "puppet:///modules/heartbeat/haresources",
        notify  =&gt; Exec["reload-heartbeat"],
        require =&gt; Package["heartbeat"],
      }

      file { "/etc/ha.d/ha.cf":
        source  =&gt; "puppet:///modules/heartbeat/ha.cf",
        notify  =&gt; Exec["reload-heartbeat"],
        require =&gt; Package["heartbeat"],
      }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/heartbeat/files/haresources 文件。 将 cookbook 替换成你的主服务器的主机名。这可以通过在主服务器上运行 uname -n 命令获得。 将 10.0.2.100 替换成你要在两台主机上共享的 IP 地址（这应该是当前网络上还未使用的地址）。 最后列出的接口是分配给心跳检测的（本例中是 eth0:1）。

    ```
    cookbook IPaddr::10.0.2.100/24/eth0:1 
    ```

4.  使用如下内容创建 /etc/puppet/modules/heartbeat/files/authkeys 文件 （使用你自己选择的口令替换 topsecretpassword）：

    ```
    auth 1
    1 sha1 topsecretpassword 
    ```

5.  使用如下内容创建 /etc/puppet/modules/heartbeat/files/ha.cf 文件。 替换下面的两个 IP 地址为你自己的两台机器上 eth0 接口对应的 IP 地址。 同样需替换 cookbook 和 cookbook2 为你自己的两台机器的主机名（可以通过运行 uname -n 命令获得）。

    ```
    autojoin none
    ucast eth0 10.0.2.15
    ucast eth0 10.0.2.16
    keepalive 1
    deadtime 10
    warntime 5
    udpport 694
    auto_failback on
    node cookbook
    node cookbook2
    use_logd yes 
    ```

6.  在两台冗余服务器上都运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1311440876'

    notice: /Stage[main]/Heartbeat/Package[heartbeat]/ensure: created
    notice: /Stage[main]/Heartbeat/File[/etc/ha.d/authkeys]/ensure:
    defined content as '{md5}e908c869aabe519aa69acc9e51da3399'

    info: /Stage[main]/Heartbeat/File[/etc/ha.d/authkeys]: Scheduling
    refresh of Exec[reload-heartbeat]

    notice: /Stage[main]/Heartbeat/File[/etc/ha.d/ha.cf]/ensure:
    defined content as '{md5}a8d3fdd62a1172cdff150fc1d86d8a6b'

    info: /Stage[main]/Heartbeat/File[/etc/ha.d/ha.cf]: Scheduling
    refresh of Exec[reload-heartbeat]

    notice: /Stage[main]/Heartbeat/File[/etc/ha.d/haresources]/ensure:
    defined content as '{md5}0f25aefe7f6c4c8e81b3bb6c86a42d60'

    info: /Stage[main]/Heartbeat/File[/etc/ha.d/haresources]:
    Scheduling refresh of Exec[reload-heartbeat]

    notice: /Stage[main]/Heartbeat/Exec[reload-heartbeat]: Triggered
    'refresh' from 3 events

    notice: Finished catalog run in 27.01 seconds 
    ```

7.  在主服务器节点上，检查它的资源：

    ```
    # cl_status rscstatus -m
    This node is holding all resources. 
    ```

8.  在辅助服务器节点上，你应该可以看到如下信息：

    ```
    # cl_status rscstatus -m
    This node is holding none resources. 
    ```

9.  在主节点上禁用 Heartbeat 服务：

    ```
    # service heartbeat stop 
    ```

10.  辅助节点现在应该接管了资源：

    ```
    # cl_status rscstatus -m
    This node is holding all resources. 
    ```

#### 工作原理

两台服务器上都运行了心跳守护进程，彼此监听心跳信号。 如果主服务器检测到辅助服务器已宕机，什么也不会发生。 而相反地，如果辅助服务器检测到主服务器已宕机，它就接管 IP 地址。 当主服务器恢复运行后，辅助服务器将再次放弃此地址，重新由主服务器接管 IP 地址 （如果 auto_failback 设置成了 on）。 在某些情况下，例如：如果你在主数据库服务器和从数据库服务器之间共享 IP 地址， 你可能不希望这种行为发生，在这种情况下应该将 auto_failback 设置成了 off。

#### 更多用法

现在你有一个共享的 IP 地址（真是名不副实，因为这个地址不是“共享”的，而是在两个服务器之间切换）， 你可以用这个地址来提供高可用性的服务。例如，如果服务器是被托管的 web 站点， 你应该为 web 站点设置 DNS 记录指向这个共享的 IP 地址。 当主服务器宕机时，辅助服务器将接管 IP 地址并继续响应基于此地址的 HTTP 请求。

> ![提示](img/tip.png)
> 
> 如果你正在使用 SSL 站点，你需要配置基于共享 IP 地址的 SSL 虚拟主机， 否则将不能响应基于这个 IP 的 HTTPs 请求。

另外，如果这个 web 站点使用了 sessions，主服务器上的任何 sessions 在故障转移后将会丢失， 除非 sessions 存储在一个分离的共享数据库中。

共享 IP 地址也是实现双路冗余负载均衡器的一种好方式 （参考 使用 HAProxy 为多个 web 服务器实现负载均衡 一节）。 你也可以使用这种方式为 Puppetmaster 主机提供冗余服务。 在 Puppet Labs 站点给出了一个合适的模式： [`projects.puppetlabs.com/projects/1/wiki/High_Availability_Patterns`](http://projects.puppetlabs.com/projects/1/wiki/High_Availability_Patterns) 。

# 管理 NFS 服务和文件共享

### 管理 NFS 服务和文件共享

> There are three kinds of death in this world. There’s heart death, there’s brain death, and there’s being off the network.
> 
> — Guy Almes

网络文件系统（**Network File System**，**NFS**）是一种从远程服务器挂装共享目录的方法。 例如：一批 web 服务器可以挂装同一个 NFS 共享为客户供应静态资产文件，比如图片文件和 CSS 文件。 尽管 NFS 是一种比较旧的技术，但它仍被广泛使用着， 所以本处方将向你展示如何创建一个 NFS 服务器以及如何通过它来共享文件。

#### 操作步骤

1.  创建一个 nfs 模块：

    ```
    # mkdir /etc/puppet/modules/nfs
    # mkdir /etc/puppet/modules/nfs/manifests 
    ```

2.  使用如下内容创建 /etc/puppet/modules/nfs/manifests/init.pp 文件：

    ```
    class nfs {
        package { "nfs-kernel-server": ensure =&gt; installed }

        service { "nfs-kernel-server":
            ensure     =&gt; running,
            enable     =&gt; true,
            hasrestart =&gt; true,
            require    =&gt; Package["nfs-kernel-server"],
        }

        file { "/etc/exports.d":
            ensure =&gt; directory,
        }

        exec { "update-etc-exports":
            command     =&gt; "/bin/cat /etc/exports.d/* &gt;/etc/exports",
            notify      =&gt; Service["nfs-kernel-server"],
            refreshonly =&gt; true,
        }

        define share( $path, $allowed, $options = "" ) {
            include nfs
            file { $path:
                ensure =&gt; directory,
            }

            file { "/etc/exports.d/${name}":
                content =&gt; "${path}  ${allowed}(${options})\n",
                notify  =&gt; Exec["update-etc-exports"],
            }
        }
    } 
    ```

3.  在你需要输出 NFS 共享的节点上添加如下代码（修改 IP 地址范围以适应你的网络）：

    ```
    nfs::share { "data":
        path    =&gt; "/data",
        allowed =&gt; "10.0.2.0/24",
        options =&gt; "rw,sync,no_root_squash",
    }

    nfs::share { "data2":
        path    =&gt; "/data2",
        allowed =&gt; "10.0.2.0/24",
        options =&gt; "rw,sync,no_root_squash",
    } 
    ```

4.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1311526219'
    notice: /Stage[main]/Nfs/Package[nfs-kernel-server]/ensure:

    created
    notice: /Stage[main]/Nfs/Service[nfs-kernel-server]/ensure: ensure
    changed 'stopped' to 'running'

    notice: /Stage[main]//Node[cookbook]/Nfs::Share[data2]/File[/
    data2]/ensure: created

    notice: /Stage[main]//Node[cookbook]/Nfs::Share[data2]/File[/etc/
    exports.d/data]/ensure: defined content as '{md5}408f8b40815ff4b6e
    ec2f324ca7eafc4'

    info: /Stage[main]//Node[cookbook]/Nfs::Share[data]/File[/etc/
    exports.d/data]: Scheduling refresh of Exec[update-etc-exports]

    notice: /Stage[main]//Node[cookbook]/Nfs::Share[data2]/
    File[/etc/exports.d/data2]/ensure: defined content as '{md5}
    ec2f324ca7eafc4408f8b40815ff4b6e'

    info: /Stage[main]//Node[cookbook]/Nfs::Share[data2]/File[/etc/
    exports.d/data2]: Scheduling refresh of Exec[update-etc-exports]

    notice: /Stage[main]/Nfs/Exec[update-etc-exports]: Triggered
    'refresh' from 2 events

    info: /Stage[main]/Nfs/Exec[update-etc-exports]: Scheduling
    refresh of Service[nfs-kernel-server]

    notice: /Stage[main]/Nfs/Service[nfs-kernel-server]/ensure: ensure
    changed 'stopped' to 'running'

    notice: /Stage[main]/Nfs/Service[nfs-kernel-server]: Triggered
    'refresh' from 1 events

    notice: Finished catalog run in 3.13 seconds 
    ```

5.  通过在另一个服务器上挂装共享测试 NFS 的输出配置：

    ```
    # mkdir /mnt/data
    # mount cookbook:/data /mnt/data
    # ls /mnt/data 
    ```

#### 工作原理

nfs 类用于安装并启动 nfs-kernel-server 服务，此服务监听网络文件共享的连接。 还定义了一个 nfs::share 资源，你可以在配置清单中的任何位置使用它输出一个 NFS 目录：

```
nfs::share { "data":
    path    => "/data",
    allowed => "10.0.2.0/24",
    options => "rw,sync,no_root_squash",
} 
```

此资源的名字是任何你想要给出的一个标签：本例中为 data。path 用于指定要共享的目录。 allowed 参数可以使用一个 CIDR 网络地址（正向本例中的 10.0.2.0/24）、 一个 IP 地址、一个主机名或一个用空格间隔的 IP 地址或主机名列表。 NFS 服务器仅允许由指定的主机远程挂装此资源。

options 参数用于指定 NFS 的挂装选项（这些参数将出现在 /etc/exports 文件中， 可使用命令 man exports 查看这些选项的精确细节）。

请注意，我们使用了与 rsyncd.conf 例子相同的片断模式。 nfs::share 的任何一个实例都会创建 /etc/exports.d 目录下的一个配置文件片段， 这会触发一个 exec 资源将所有的配置文件片段串连成一个 /etc/exports 配置文件并通知 NFS 服务应用配置的改变。

#### 更多用法

NFS 共享只能应用于应用程序的非关键性数据，因为 NFS 服务器会造成单点故障。 你可以使用集群文件系统解决关键性数据的共享，应该考虑使用 GlusterFS 替代 NFS。

# 使用 HAProxy 为多个 web 服务器实现负载均衡

### 使用 HAProxy 为多个 web 服务器实现负载均衡

> The inside of a computer is as dumb as hell but it goes like mad!
> 
> — Richard Feynman

曾几何时，为缓慢的 Web 服务器加速的方式就是增加更多的 CPU 核心。 我记得一个老板买了一台 24 核的 Sun 工作站，它是个尺寸接近悍马的怪物， 以至于我们不得不扩修数据中心的大门才能将其放入。

时至今日，扩展 Web 站点规模仍旧是添加 CPU 核心的问题，但与从前不同的是： 要么他们自己拿来若干粉色的 PC 机作为服务器使用； 要么从云服务提供商那里租用服务器作为计算资源使用。 为了对所有这些核心进行分组，从而一起服务于单一的 Web 站点， 我们要使用负载均衡器（load balancer）。

曾经，负载均衡器是坐落在机架上的一个耗资八万美元的大盒子。 尽管你现在仍旧可以购买这种负载均衡器，但对于大多数组织来说， 使用 Linux 商品服务器的软件负载均衡解决方案可以大大削减成本。

对大多数管理员来说，**HAProxy** 是软件负载均衡器的一种选择：快速、强大、高度可配置。 在下面的处方中，将向你展示如何创建一个 HAProxy 服务器为两个现有的后端服务器实现对其 web 请求的负载均衡。

#### 操作步骤

1.  创建 loadbalancer 模块：

    ```
    # mkdir /etc/puppet/modules/loadbalancer
    # mkdir /etc/puppet/modules/loadbalancer/manifests
    # mkdir /etc/puppet/modules/loadbalancer/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/loadbalancer/manifests/init.pp 文件：

    ```
    class loadbalancer {
      package { "haproxy": ensure =&gt; installed }

      file { "/etc/default/haproxy":
        source  =&gt; "puppet:///modules/loadbalancer/haproxy.defaults",
        require =&gt; Package["haproxy"],
      }

      service { "haproxy":
        ensure  =&gt; running,
        enable  =&gt; true,
        require =&gt; Package["haproxy"],
      }

      file { "/etc/haproxy/haproxy.cfg":
        source  =&gt; "puppet:///modules/loadbalancer/haproxy.cfg",
        require =&gt; Package["haproxy"],
        notify  =&gt; Service["haproxy"],
      }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/loadbalancer/files/haproxy.defaults 文件：

    ```
    # Don't edit this file - it's managed by Puppet
    # Set ENABLED to 1 if you want the init script to start haproxy.
    ENABLED=1
    # Add extra flags here.
    #EXTRAOPTS="-de -m 16" 
    ```

4.  使用如下内容创建 /etc/puppet/modules/loadbalancer/files/haproxy.cfg 文件。 在 myapp 部分中，用你的后端服务器 IP 地址替换每个 server 行中的 IP 地址， 并用你的后端服务器监听端口替换端口 :8000。

    ```
    global
            daemon
            user haproxy
            group haproxy
            pidfile /var/run/haproxy.pid

    defaults
            log        global
            stats      enable
            mode       http
            option     httplog
            option     dontlognull
            option     dontlog-normal
            retries    3
            option     redispatch
            contimeout 4000
            clitimeout 60000
            srvtimeout 30000

    listen stats :8080
           mode  http
           stats uri /
           stats auth haproxy:topsecret

    listen myapp 0.0.0.0:80
           balance leastconn
           server  myapp1 10.0.2.30:8000    check maxconn 100
           server  myapp2 10.0.2.40:8000    check maxconn 100 
    ```

5.  在你的 HAProxy 节点中包含如下代码：

    ```
    include loadbalancer 
    ```

6.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1311616315'

    notice: /Stage[main]/Loadbalancer/Package[haproxy]/ensure: ensure
    changed 'purged' to 'present'

    --- /etc/haproxy/haproxy.cfg 2009-11-06 17:59:44.000000000

    +0000
    +++ /tmp/puppet-file20110725-16369-1b85cr8-0 2011-07-25
    18:09:03.749146699 +0000

    @@ -1,86 +1,28 @@
    -# this config needs haproxy-1.1.28 or haproxy-1.2.1
    ...

    info: /Stage[main]/Loadbalancer/File[/etc/haproxy/haproxy.
    cfg]: Filebucketed /etc/haproxy/haproxy.cfg to puppet with sum
    c3bfb0c86138552475dea458e8ab36f3

    notice: /Stage[main]/Loadbalancer/File[/etc/haproxy/haproxy.cfg]/
    content: content changed '{md5}c3bfb0c86138552475dea458e8ab36f3'
    to '{md5}fa5fac3cf31f043f0120d0d45cef3f54'

    info: /Stage[main]/Loadbalancer/File[/etc/haproxy/haproxy.cfg]:
    Scheduling refresh of Service[haproxy]

    notice: /Stage[main]/Loadbalancer/Service[haproxy]/ensure: ensure
    changed 'stopped' to 'running'

    notice: /Stage[main]/Loadbalancer/Service[haproxy]: Triggered
    'refresh' from 1 events

    --- /etc/default/haproxy 2009-11-06 17:59:21.000000000 +0000

    +++ /tmp/puppet-file20110725-16369-1ndfrti-0 2011-07-25
    18:09:05.749136866 +0000
    @@ -1,4 +1,5 @@

     # Set ENABLED to 1 if you want the init script to start haproxy.
    -ENABLED=0
    +ENABLED=1
     # Add extra flags here.
     #EXTRAOPTS="-de -m 16"
    +

    notice: /Stage[main]/Loadbalancer/File[/etc/default/haproxy]/
    content: content changed '{md5}a1f2deb7c7a10e55dc7c971a2288f5d4'
    to '{md5}2217d74d66bd72630268598b1f11f173'

    notice: Finished catalog run in 22.21 seconds 
    ```

7.  在你的浏览器中检查 HAProxy 的 **stats** 界面确保一切工作正常（注意我的 **Backend** 服务器显示的是 **DOWN** ，因为这些虚拟机还没有运行。当我启动它们之后，HAProxy 将会自动检测并重新标记它们的状态）。

    ![img/ch08sec04.png](img/ch08sec04.png)

#### 工作原理

haproxy 守护进程监听进入的请求并将其分发到一组后端服务器（本例中是 **myapp1** 和 **myapp2**）。 如果一个后端服务器已超载，HAProxy 将避免向其发送更多的流量直至它恢复。 这有助于防止单台 Web 服务器因为超载（排队等待的不能被处理的请求越来越多）而造成的响应速度大幅放缓。 如果一台后端服务器宕机，HAProxy 将不会为其分发任何请求，直至其重新可用。

**stats** 界面会显示：你的后端服务器如何执行，有多少会话正在处理， HAProxy 是否将后端服务器标记成了 **UP** 或 **DOWN**，等信息。

#### 更多用法

如果你想添加更多的后端服务器以处理不断增长的需求，只需在 haproxy.cfg 中添加更多的 server 行。 如果你发现现有的后端服务器响应速度越来越慢，请适当减少每个服务器的 maxconn 值。 HAProxy 有大量值得探索的配置参数，参考 HAProxy 的文档站点 [`haproxy.1wt.eu/#docs`](http://haproxy.1wt.eu/#docs) 。

如果你需要提供 SSL 的能力，可以将 Nginx 放在 HAProxy 的前端进行处理。

尽管 HAProxy 经常用于 Web 服务器，但它可以代理很多服务，不仅仅是 HTTP。 它可以处理任何 TCP 流量，所以你可以使用 HAProxy 为 MySQL 服务器、SMTP 服务器、 视频服务器以及任何你想要的服务器实现负载均衡。

# 使用 iptables 管理防火墙

### 使用 iptables 管理防火墙

> Programming can be fun, so can cryptography; however they should not be combined.
> 
> — Kreitzberg and Shneiderman

C 编程语言被形容为 “只写” 的语言；它是如此的简洁、高效， 甚至你自己读自己写过的代码都可能很难理解。 同样地，Linux 内核内置的包过滤防火墙的 iptables 的配置也是如此。 一条原始的 iptables 命令规则看上去像这样：

```
iptables -A INPUT -d 10.0.2.15/32 -p tcp -m tcp --dport 80 -j ACCEPT 
```

除非你会因为掌握了命令行的这些似乎毫无意义的字符串而获得男子气概 （诚然这是 UNIX 系统管理员的职业病）， 否则，能够以更加象征性和可读性的方式来表达防火墙规则是更好的选择。 Puppet 在这方面可以为我们提供帮助，因为我们可以用它对 iptables 的实现细节进行抽象， 并通过参考管理员所控制的服务角色来定义防火墙规则，例如：

```
iptables::role { "web-server": } 
```

#### 准备工作

你需要我们在第五章 为配置文件添加配置行 一节中创建的 append_if_no_such_line 函数。

#### 操作步骤

1.  创建一个 iptables 模块：

    ```
    # mkdir /etc/puppet/modules/iptables
    # mkdir /etc/puppet/modules/iptables/manifests
    # mkdir /etc/puppet/modules/iptables/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/iptables/manifests/init.pp 文件：

    ```
    class iptables {
        file { [ "/root/iptables",
                 "/root/iptables/hosts",
                 "/root/iptables/roles" ]:
            ensure =&gt; directory,
        }

        file { "/root/iptables/roles/common":
            source =&gt; "puppet:///modules/iptables/common.role",
            notify =&gt; Exec["run-iptables"],
        }

        file { "/root/iptables/names":
            source =&gt; "puppet:///modules/iptables/names",
            notify =&gt; Exec["run-iptables"],
        }

        file { "/root/iptables/iptables.sh":
            source =&gt; "puppet:///modules/iptables/iptables.sh",
            mode   =&gt; "755",
            notify =&gt; Exec["run-iptables"],
        }

        file { "/root/iptables/hosts/${hostname}":
            content =&gt; "export MAIN_IP=${ipaddress}\n",
            replace =&gt; false,
            require =&gt; File["/root/iptables/hosts"],
            notify  =&gt; Exec["run-iptables"],
        }

        exec { "run-iptables":
            cwd         =&gt; "/root/iptables",
            command     =&gt; "/usr/bin/test -f hosts/${hostname} &&
     /root/iptables/iptables.sh && /sbin/iptables-save &gt;
     /etc/iptables.rules",
            refreshonly =&gt; true,
        }

        append_if_no_such_line { "restore iptables rules":
            file =&gt; "/etc/network/interfaces",
            line =&gt; "pre-up iptables-restore &lt; /etc/iptables.rules",
        }

        define role() {
            include iptables

            file { "/root/iptables/roles/${name}":
                source  =&gt; "puppet:///modules/iptables/${name}.role",
                replace =&gt; false,
                require =&gt; File["/root/iptables/roles"],
                notify  =&gt; Exec["run-iptables"],
            }

            append_if_no_such_line { "${name} role":
                file    =&gt; "/root/iptables/hosts/${hostname}",
                line    =&gt; ". `dirname \$0`/roles/${name}",
                require =&gt; File["/root/iptables/hosts/${hostname}"],
                notify =&gt; Exec["run-iptables"],
            }
        }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/iptables/files/iptables.sh 文件：

    ```
    # Server names and ports
    . `dirname $0`/names

    # Interfaces (override in host-specific file if necessary)
    export EXT_INTERFACE=eth0

    # Flush and remove all chains
    iptables -P INPUT ACCEPT
    iptables -P OUTPUT ACCEPT
    iptables -F
    iptables -X

    # Allow all traffic on loopback interface
    iptables -I INPUT 1 -i lo -j ACCEPT
    iptables -I OUTPUT 1 -o lo -j ACCEPT

    # Allow established and related connections
    iptables -I INPUT 2 -m state --state ESTABLISHED,RELATED -j ACCEPT
    iptables -I OUTPUT 2 -m state --state ESTABLISHED,RELATED -j ACCEPT

    # Include machine specific settings
    HOST_RULES=`dirname $0`/hosts/`hostname -s`
    [ -f ${HOST_RULES} ] && . ${HOST_RULES}
    [ "${MAIN_IP}" == "" ] && ( echo No MAIN_IP was set, \
     please set the primary IP address in ${HOST_RULES}. ; exit 1 )

    # Include common settings
    . `dirname $0`/roles/common

    # Drop all non-matching packets
    iptables -A INPUT -j LOG --log-prefix "INPUT: "
    iptables -A INPUT -j DROP
    iptables -A OUTPUT -j LOG --log-prefix "OUTPUT: "
    iptables -A OUTPUT -j DROP

    echo -e "Test remote login and then:\n iptables-save \
     &gt;/etc/iptables.rules" 
    ```

4.  使用如下内容创建 /etc/puppet/modules/iptables/files/names 文件：

    ```
    # Servers
    export PUPPETMASTER=10.0.2.15

    # Well-known ports
    export DNS=53
    export FTP=21
    export GIT=9418
    export HEARTBEAT=694
    export IMAPS=993
    export IRC=6667
    export MONIT=2828
    export MYSQL=3306
    export MYSQL_MASTER=3307
    export NRPE=5666
    export NTP=123
    export POSTGRES=5432
    export PUPPET=8140
    export RSYNCD=873
    export SMTP=25
    export SPHINX=3312
    export SSH=22
    export STARLING=3307
    export SYSLOG=514
    export WEB=80
    export WEB_SSL=443
    export ZABBIX=10051 
    ```

5.  使用如下内容创建 /etc/puppet/modules/iptables/files/common.role 文件：

    ```
    # Common rules for all hosts
    iptables -A INPUT -p tcp -m tcp -d ${MAIN_IP} --dport ${SSH} -j ACCEPT

    iptables -A INPUT -p ICMP --icmp-type echo-request -j ACCEPT
    iptables -A OUTPUT -p ICMP --icmp-type echo-request -j ACCEPT

    iptables -A OUTPUT -p tcp --dport ${SSH} -j ACCEPT
    iptables -A OUTPUT -p tcp --dport ${SMTP} -j ACCEPT
    iptables -A OUTPUT -p udp --dport ${NTP} -j ACCEPT
    iptables -A OUTPUT -p tcp --dport ${NTP} -j ACCEPT
    iptables -A OUTPUT -p udp --dport ${DNS} -j ACCEPT
    iptables -A OUTPUT -p tcp --dport ${WEB} -j ACCEPT
    iptables -A OUTPUT -p tcp --dport ${WEB_SSL} -j ACCEPT
    iptables -A OUTPUT -p tcp -d ${PUPPETMASTER} --dport ${PUPPET} -j ACCEPT
    iptables -A OUTPUT -p tcp --dport ${MYSQL} -j ACCEPT

    # Drop some commonly probed ports
    iptables -A INPUT -p tcp --dport 23 -j DROP # telnet
    iptables -A INPUT -p tcp --dport 135 -j DROP # epmap
    iptables -A INPUT -p tcp --dport 139 -j DROP # netbios
    iptables -A INPUT -p tcp --dport 445 -j DROP # Microsoft DS
    iptables -A INPUT -p udp --dport 1433 -j DROP # SQL server
    iptables -A INPUT -p tcp --dport 1433 -j DROP # SQL server
    iptables -A INPUT -p udp --dport 1434 -j DROP # SQL server
    iptables -A INPUT -p tcp --dport 1434 -j DROP # SQL server
    iptables -A INPUT -p tcp --dport 2967 -j DROP # SSC-agent 
    ```

6.  使用如下内容创建 /etc/puppet/modules/iptables/files/web-server.role 文件：

    ```
    # Access to web
    iptables -A INPUT -p tcp -d ${MAIN_IP} --dport ${WEB} -j ACCEPT

    # Send mail from web applications
    iptables -A OUTPUT -p tcp --dport ${SMTP} -j ACCEPT 
    ```

7.  使用如下内容创建 /etc/puppet/modules/iptables/files/puppet-server.role 文件：

    ```
    # Access to puppet
    iptables -A INPUT -p tcp -d ${MAIN_IP} --dport ${PUPPET} -j ACCEPT 
    ```

8.  在你的 Puppetmaster 节点上包含如下内容：

    ```
    iptables::role { "web-server": }
    iptables::role { "puppet-server": } 
    ```

9.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1311682880'

    notice: /Stage[main]/Iptables/File[/root/iptables]/ensure: created
    notice: /Stage[main]/Iptables/File[/root/iptables/names]/ensure:
    defined content as '{md5}9bb004a7d2c6d70616b149d044c22669'

    info: /Stage[main]/Iptables/File[/root/iptables/names]: Scheduling
    refresh of Exec[run-iptables]

    notice: /Stage[main]/Iptables/File[/root/iptables/hosts]/ensure:
    created

    notice: /Stage[main]/Iptables/File[/root/iptables/hosts/cookbook]/
    ensure: defined content as '{md5}d00bc730514bbb74cdef3dad70058a81'

    info: /Stage[main]/Iptables/File[/root/iptables/hosts/cookbook]:
    Scheduling refresh of Exec[run-iptables]

    notice: /Stage[main]//Node[cookbook]/Iptables::Role[web-server]/
    Append_if_no_such_line[web-server role]/Exec[/bin/echo '. `dirname
    $0`/roles/web-server' &gt;&gt; '/root/iptables/hosts/cookbook']/returns:
    executed successfully

    info: /Stage[main]//Node[cookbook]/Iptables::Role[web-server]/
    Append_if_no_such_line[web-server role]/Exec[/bin/echo '. `dirname
    $0`/roles/web-server' &gt;&gt; '/root/iptables/hosts/cookbook']:
    Scheduling refresh of Exec[run-iptables]

    notice: /Stage[main]//Node[cookbook]/Iptables::Role[puppetserver]/
    Append_if_no_such_line[puppet-server role]/Exec[/bin/echo
    '. `dirname $0`/roles/puppet-server' &gt;&gt; '/root/iptables/hosts/
    cookbook']/returns: executed successfully

    info: /Stage[main]//Node[cookbook]/Iptables::Role[puppet-server]/
    Append_if_no_such_line[puppet-server role]/Exec[/bin/echo '.
    `dirname $0`/roles/puppet-server' &gt;&gt; '/root/iptables/hosts/
    cookbook']: Scheduling refresh of Exec[run-iptables]

    notice: /Stage[main]/Iptables/File[/root/iptables/roles]/ensure:
    created

    notice: /Stage[main]//Node[cookbook]/Iptables::Role[puppetserver]/
    File[/root/iptables/roles/puppet-server]/ensure: defined
    content as '{md5}c30a13f7792525c181e14e78c9a510cd'

    info: /Stage[main]//Node[cookbook]/Iptables::Role[puppet-server]/
    File[/root/iptables/roles/puppet-server]: Scheduling refresh of
    Exec[run-iptables]

    notice: /Stage[main]//Node[cookbook]/Iptables::Role[web-server]/
    File[/root/iptables/roles/web-server]/ensure: defined content as
    '{md5}11e5747cb2737903ffc34133f5fe2452'

    info: /Stage[main]//Node[cookbook]/Iptables::Role[web-server]/
    File[/root/iptables/roles/web-server]: Scheduling refresh of
    Exec[run-iptables]

    notice: /Stage[main]/Iptables/File[/root/iptables/roles/common]/
    ensure: defined content as '{md5}116f57d4e31f3e0b351da6679dca15e3'

    info: /Stage[main]/Iptables/File[/root/iptables/roles/common]:
    Scheduling refresh of Exec[run-iptables]

    notice: /Stage[main]/Iptables/File[/root/iptables/iptables.sh]/
    ensure: defined content as '{md5}340ff9fb5945e9fc7dd78b21f45dd823'

    info: /Stage[main]/Iptables/File[/root/iptables/iptables.sh]:
    Scheduling refresh of Exec[run-iptables]

    notice: /Stage[main]/Iptables/Exec[run-iptables]: Triggered
    'refresh' from 8 events

    notice: /Stage[main]/Iptables/Append_if_no_such_line[restore
    iptables rules]/Exec[/bin/echo 'pre-up iptables-restore &lt; /etc/
    iptables.rules' &gt;&gt; '/etc/network/interfaces']/returns: executed
    successfully

    notice: Finished catalog run in 4.86 seconds 
    ```

10.  检查要求的规则是否已被安装：

    ```
    # iptables -nL
    Chain INPUT (policy ACCEPT)
    target prot opt source     destination
    ACCEPT all  --  0.0.0.0/0  0.0.0.0/0
    ACCEPT all  --  0.0.0.0/0  0.0.0.0/0    state RELATED,ESTABLISHED
    ACCEPT tcp  --  0.0.0.0/0  10.0.2.15    tcp dpt:80
    ACCEPT tcp  --  0.0.0.0/0  10.0.2.15    tcp dpt:8140
    ACCEPT tcp  --  0.0.0.0/0  10.0.2.15    tcp dpt:22
    ACCEPT icmp --  0.0.0.0/0  0.0.0.0/0    icmp type 8
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:23
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:135
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:139
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:445
    DROP   udp  --  0.0.0.0/0  0.0.0.0/0    udp dpt:1433
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:1433
    DROP   udp  --  0.0.0.0/0  0.0.0.0/0    udp dpt:1434
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:1434
    DROP   tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:2967
    LOG    all  --  0.0.0.0/0  0.0.0.0/0    LOG
     flags 0 level 4 prefix `INPUT: '
    DROP   all  --  0.0.0.0/0  0.0.0.0/0

    Chain FORWARD (policy ACCEPT)
    target prot opt source destination
    Chain OUTPUT (policy ACCEPT)
    target prot opt source     destination
    ACCEPT all  --  0.0.0.0/0  0.0.0.0/0
    ACCEPT all  --  0.0.0.0/0  0.0.0.0/0    state RELATED,ESTABLISHED
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:25
    ACCEPT icmp --  0.0.0.0/0  0.0.0.0/0    icmp type 8
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:22
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:25
    ACCEPT udp  --  0.0.0.0/0  0.0.0.0/0    udp dpt:123
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:123
    ACCEPT udp  --  0.0.0.0/0  0.0.0.0/0    udp dpt:53
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:80
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:443
    ACCEPT tcp  --  0.0.0.0/0  10.0.2.15    tcp dpt:8140
    ACCEPT tcp  --  0.0.0.0/0  0.0.0.0/0    tcp dpt:3306
    LOG    all  --  0.0.0.0/0  0.0.0.0/0    LOG
    flags 0 level 4 prefix `OUTPUT: '
    DROP   all  --  0.0.0.0/0  0.0.0.0/0 
    ```

#### 工作原理

为了创建一套合适的防火墙规则，我们需要知道节点的主 IP 地址以及其运行了哪些服务。 我们还需要添加一些所有的机器都要设置的共同规则（例如，允许 SSH），并运行一系列的 iptables 命令以激活我们已经生成的规则。 之后我们还要保存这些规则，以便使这些规则可以在开机时恢复执行。 下面介绍所有的这一切是如何完成的。

首先，我们创建一个 names 文件为常用的端口定义 shell 变量。 这意味着，当我们定义防火墙规则时，可以引用变量，例如对于 MySQL 服务可以使用变量 ${MYSQL} 取代数值端口号 3306。

common.role 文件包含了一些对所有机器都有用的规则。编辑这个文件以适应你自己的所有机器 （例如，你可能仅允许从指定的 IP 范围访问 SSH）。

web-server.role 和 puppet-server.role 文件包含了两个特定角色的规则。 你可以添加更多的文件用于定义你的网络中所需的众多角色：例如，数据库服务器、应用服务器、 DNS 服务器，等等。文件中所有的规则都具有如下的格式：

```
iptables -A INPUT -p tcp -d ${MAIN_IP} --dport ${WEB} -j ACCEPT 
```

通常，你只需要修改 ${WEB} 这部分：将其替换为另一个端口名（定义在 names 文件中） 的变量引用（例如 ${POSTGRES}）。如果你需要更多的端口名，在 names 文件中添加相应的定义。

iptables.sh 脚本读取其他的所有文件并执行规则要求的 iptables 命令。 每当相关文件有任何改变，Puppet 就执行这个脚本，因此要想刷新防火墙， 你需要做的工作就是改变相关的配置并运行 Puppet。

Puppet 还会将当前的规则集保存在 /etc/iptables.rules 文件中。 为了让机器重启后加载规则集文件，Puppet 在 /etc/network/interfaces 文件中添加了如下一行：

```
pre-up iptables-restore < /etc/iptables.rules 
```

所有这一切意味着，在相关的模块中（例如 apache 模块）， 你只要简单地包含如下一行即可创建相应的防火墙：

```
iptables::role { "web-server": } 
```

一旦防火墙被激活，任何不符合规则的数据包将被阻止并记录到 /var/log/messages 日志文件。 检查这个文件，以帮助解决防火墙的任何问题。

#### 更多用法

如果在你的规则中引用了某些特定的机器（例如，你的监控服务器）， 可以在 names 文件中添加如下的定义：

```
MONITOR=10.0.2.15 
```

然后在适当的位置（例如 common.role 文件中），你可以允许来自这台机器的访问， 例如，允许来自监控服务器对指定主机 NRPE 端口的访问规则如下：

```
iptables -A INPUT -p tcp -m tcp -d ${MAIN_IP} -s ${MONITOR} --dport ${NRPE} -j ACCEPT 
```

你也可以用这种方法指定数据库服务器，以及在 .role 文件中需要引用特定的 IP 地址、 网络地址和地址范围等情况。

像这样动态生成防火墙规则集对于云基础设施是非常有用的， 云中的服务器列表会因为节点的创建和销毁而不断地变化。 对于需要触发防火墙重建的任何资源，你只要在此资源中添加如下代码即可：

```
notify => Exec["run-iptables"], 
```

你可能有一个由版本控制系统维护的或通过 **cloud API** （例如，Rackspace 或 Amazon EC2） 自动更新的 “主服务器列表（master server list）”。 可以在 Puppet 中将这个列表定义成一个 file 资源，通过在此资源中使用 notify 参数即可触发防火墙的重建，所以每次当你检入（check in）主服务器列表的变化， 每台机器上运行的 Puppet 将相应地更新其防火墙。

当然，这种高度的自动化意味着你需要对你检入的内容格外小心， 因为任何错误都可能会导致整个基础设施离线。

测试变更的一种好方法是对用于测试的 Puppet 配置清单使用一个单独的 Git 分支， 在分支中仅将变更应用到一到两台服务器。一旦你验证了变更的正确性， 就可以将其合并到主分支并回滚到主分支。

# 管理 Amazon 的 EC2 实例

### 管理 Amazon 的 EC2 实例

> The most amazing achievement of the computer software industry is its continuing cancellation of the steady and staggering gains made by the computer hardware industry.
> 
> — Henry Petroski

如果你觉得你的电脑近年来变得比较慢，这可能是正确的。 对于大多数应用程序而言，你已不必将所有的计算能力（computing power） 全部挤进你办公桌下的一个米色盒子里。 为了解决这个问题，计算能力已经成为一种可以在网上购买的商品。

亚马逊不只是卖书了：他们也卖首饰、摩托车、叶鼓风机，以及对我们当前目的有用的计算能力。 你可以用信用卡签约使用 **Amazon Web Services**，并根据你的需要创建众多的服务器实例， 每个服务器基于小时计费。如果你只是想试水，可以运行一个免费最长一年的微型实例。 如果你正在寻找将你的一部分基础设施迁移到公共云的方案，这是一个不错的实验方式。

本处方将向你展示一个使用 Puppet 创建并自动供应一个 **EC2** 实例的简单方法。 尽管还可以使用更强大的方式实现，包括使用 MCollective，但作为教学目的， 我们将只做最低限度的必要的工作：让一个实例运行起来并应用 Puppet 配置清单。 一旦你获得了基本思路，你就可以以此作为基础加入你自己的改良和改进。

#### 准备工作

你需要一个 **Amazon Web Services**（**AWS**）账号。如果还没有，请到 [`aws-portal.amazon.com/gp/aws/developer/subscription/index.html?productCode=AmazonEC2`](http://aws-portal.amazon.com/gp/aws/developer/subscription/index.html?productCode=AmazonEC2) 注册。

你需要 AWS 访问的 key ID，这个秘密访问 Key 对应于你的 AWS 账号。你可以在这个页面找到它： [`aws-portal.amazon.com/gp/aws/developer/account/index.html?action=access-key`](http://aws-portal.amazon.com/gp/aws/developer/account/index.html?action=access-key) 。

你还需要访问 EC2 实例的 SSH 密钥对。为此，你需要如下步骤：

1.  在 [`console.aws.amazon.com/ec2/home`](https://console.aws.amazon.com/ec2/home) 登录 AWS 管理控制台。

2.  选择 **Amazon EC2 标签**，在导航部分的标题 **Network & Security** 上单击 **Key Pairs**。

3.  单击 **Create key pair**，当提示出现后下载 keypair 文件。将此文件保存到一个安全的地方， 并使用如下命令为它设置模式为 0600 的权限：

    ```
    # chmod 600 bitfield.pem 
    ```

#### 操作步骤

1.  创建 fog 模块：

    ```
    # mkdir /etc/puppet/modules/fog
    # mkdir /etc/puppet/modules/fog/manifests
    # mkdir /etc/puppet/modules/fog/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/fog/manifests/init.pp 文件：

    ```
    class fog {
        package { "fog":
            ensure   =&gt; installed,
            provider =&gt; gem,
        }

        file { "/usr/local/etc/fog_credentials":
            source =&gt; "puppet:///modules/fog/fog_credentials",
        }

        file { "/usr/local/bin/boot-ec2":
            source =&gt; "puppet:///modules/fog/boot-ec2.rb",
            mode   =&gt; "755",
        }

        file { "/usr/local/bin/bootstrap-ec2":
            source =&gt; "puppet:///modules/fog/bootstrap-ec2.sh",
            mode   =&gt; "755",
        }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/fog/files/boot-ec2.rb 文件 （修改 :private_key_path 参数指向你自己的 AWS 私钥文件）：

    ```
    #!/usr/bin/ruby
    require 'rubygems'
    require 'fog'

    HOSTNAME = 'devbox'
    @server = ''
    Fog.credentials_path = '/usr/local/etc/fog_credentials'

    def command( cmdline )
        puts "Running command: #{cmdline}"
        res = @server.ssh( "sudo #{cmdline}" )[0]
        puts res.stdout
        puts res.stderr
    end

    def create()
        puts "Bootstrapping instance..."
        connection = Fog::Compute.new( { :provider =&gt; 'AWS' } )
        @server = connection.servers.bootstrap( :key_name =&gt;
                                                'bitfield',
                                                :private_key_path =&gt;
                                                '~/bitfield.pem',
                                                :username =&gt; 'ubuntu')
        @server.wait_for { ready? }
        @server.reload
        puts "Instance name: #{@server.dns_name}"
        puts "Setting hostname..."
        @server.ssh( "sudo hostname #{HOSTNAME}" )
    end

    def copy_bootstrap_files()
        puts "Copying bootstrap files..."
        @server.scp( "puppet.tar.gz", "/tmp" )
        @server.scp( "/usr/local/bin/bootstrap-ec2", "/tmp" )
    end

    def bootstrap()
        puts "Bootstrapping..."
        command( "sudo sh /tmp/bootstrap-ec2" )
    end

    create()
    copy_bootstrap_files()
    bootstrap() 
    ```

4.  使用如下内容创建 /etc/puppet/modules/fog/files/bootstrap-ec2.sh 文件：

    ```
    #!/bin/bash
    apt-get update
    apt-get -y install puppet
    apt-get -y install git-core
    cd /root
    tar xzf /tmp/puppet.tar.gz
    puppet --modulepath=/root/puppet/modules \
      /root/puppet/manifests/site.pp 
    ```

5.  使用如下内容创建 /etc/puppet/modules/fog/files/fog_credentials 文件 （使用你自己的 AWS 凭证替换相应的值）：

    ```
    :default:
      :aws_access_key_id: AKIAI5RGMC3QRPO3AJWR
      :aws_secret_access_key: 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 
    ```

6.  添加如下的节点声明，此节点将会应用 EC2 实例：

    ```
    node devbox {
        file { "/etc/motd":
            content =&gt; "Puppet power!\n",
        }
    } 
    ```

7.  添加如下代码到一个节点：

    ```
    include fog 
    ```

8.  运行 Puppet：

    ```
    # puppet agent --test
    info: Retrieving plugin
    info: Caching catalog for cookbook.bitfieldconsulting.com
    info: Applying configuration version '1313160844'

    notice: /Stage[main]/Fog/Package[fog]/ensure: ensure changed
    'purged' to 'present'

    notice: /Stage[main]/Fog/File[/usr/local/bin/bootstrap-ec2]/
    ensure: defined content as '{md5}5bc2ffb3b5aa94b33b17d419625ecbab'

    notice: /Stage[main]/Fog/File[/usr/local/bin/boot-ec2]/ensure:
    defined content as '{md5}dadc835c6e52c89cb928d60db7677713'

    notice: /Stage[main]/Fog/File[/usr/local/etc/fog_credentials]/
    ensure: defined content as '{md5}3b140aedac170bbfcc2837077e03bb93'

    notice: Finished catalog run in 1.67 seconds 
    ```

9.  在你的工作目录中，为分发 EC2 实例创建一个 Puppet 的 tar 包。 最简单的方法是对你现有的 Puppet 仓库（对于 git 的裸仓库需要先检出）执行 tar 命令：

    ```
    # cd /etc
    # tar czf /tmp/puppet.tar.gz --exclude .git puppet
    # cd -
    # mv /tmp/puppet.tar.gz . 
    ```

10.  运行 boot-ec2 脚本：

    ```
    # boot-ec2
    Bootstrapping instance...
    Instance name: ec2-107-20-59-174.compute-1.amazonaws.com
    Setting hostname...
    Copying bootstrap files...
    Bootstrapping...
    Running command: sudo sh /tmp/bootstrap.sh
    sudo: unable to resolve host devbox
    sudo: unable to resolve host devbox
    ...
    notice: //Node[devbox]/File[/etc/motd]/content: defined content as
    'unknown checksum' 
    ```

11.  登录到该实例检查你的配置清单已被正确应用：

    ```
    # ssh -i bitfield.pem ubuntu@ec2-107-20-59-174.compute-1.amazonaws.com
    Puppet power!
    ubuntu@devbox:~$ 
    ```

12.  你已经得到了一个由 Puppet 管理控制的云服务器！ 如果想要创建十个实例，运行此脚本十次即可。 不要忘记使用完毕之后，要在 AWS 管理控制台关闭这些实例（按使用时间付费的哦！）。

#### 工作原理

**Fog** 是一个用于管理云资源的 Ruby 库，包括 EC2 和其他一些供应商（如 Rackspace）。 尽管你可以使用 Amazon 自己提供的 ec2-tools 脚本启动和管理 EC2 实例， 但使用 Fog 可以轻而易举地将你的实例迁移到另一个供应商， 而且还不需要你为运行 ec2-tools 而安装 Java 以及其他依赖的软件。 对于创建 EC2 基础设施的这两种方法来说，我可以自信的说我更喜欢用 Fog， 尽管事实上它几乎没有文档（而 Amazon 却有很多）。

在 boot-ec2 脚本中，我们用自己的凭证使用 Fog 创建了一个新的 EC2 实例， 并为其传输了一份 Puppet 配置清单的拷贝。 然后我们复制 bootstrap-ec2 脚本，它用于安装 Puppet 以及应用程序的配置清单。

对于本例而言，配置清单相当简单：

```
file { "/etc/motd":
    content => "Puppet power!\n",
} 
```

你可以轻松地修改它，比如，与你的生产应用服务器同名。 这对于快速部署位于物理负载平衡器之后的大量应用程序服务器来说是一个好方法， 例如，处理突然飙升的需求。另外，你也可以使用 EC2 实例作为测试服务器或临时服务器 （这完全取决于你自己的需求）。

#### 更多用法

除了你的信用卡强加给你的限制之外，此处的脚本没有对你能部署的 EC2 实例数量做任何限制。 所以你可以尝试修改此处所示的脚本，启用一个由命令行参数设置的实例数量。

你或许想要创建不同类型的实例，例如：Web 服务器、人工队列服务器（queue worker servers）等。 你可以修改启动脚本，携带一个参数指定要启动的实例类型。

此处显示的脚本有一个重要的限制，就是它以 tar 包形式提供了一个包含你的 Puppet 配置清单快照的实例。 显然，当你在 Puppetmaster 上修改你的 Puppet 配置清单后，EC2 实例不受影响，即更改不会被应用到 EC2 实例。 基于简单的目的，本处方中的例子仅仅使用 Puppet 构建了初始的服务器，它没有运行 Puppet 守护进程也没与 Puppetmaster 服务器联系。

这对于短生存期或仅为指定目的而运行的 EC2 实例往往还是不错的。 如果你需要运行长生存期的服务器，或者需要通过 Puppet 更新 EC2 实例服务器， 就应该修改脚本使实例能与你的 Puppetmaster 服务器取得联系。 要解决的是证书签名问题，例如你可以对证书进行预签名并伴随 bootstrap 脚本一同部署到 EC2 实例。 另外一种方法是，在脚本中通过 SSH 或 MCollective 登录 Puppetmaster 服务器并对实例的证书请求进行签名。 这两种证书签名的机制或简单或复杂，随你选择。

你可能还想使用其他的云服务提供商，例如 Rackspace 或 Linode。 为此，你需要对脚本做轻微地修改。 请参考 Fog 文档获取相关的详细信息，网址为 [`fog.io`](http://fog.io) 。

> ![注记](img/note.png)
> 
> 你也可以使用 Puppet 新的云供应商扩展（Cloud Provisioner extension）来管理 EC2 实例； 要获取相关的详细信息请参考 Puppet Labs 的 [`docs.puppetlabs.com/guides/cloud_pack_getting_started.html`](http://docs.puppetlabs.com/guides/cloud_pack_getting_started.html) 页面。

#### 参见本书

*   本章的 使用 Vagrant 管理虚拟机 一节

# 使用 Vagrant 管理虚拟机

### 使用 Vagrant 管理虚拟机

> In 1974 computers were oppressive devices in far-off air-conditioned places. Now you can be oppressed in your own living room.
> 
> — Ted Nelson

虽然能够在云中部署虚拟机是一个创举，但若能将虚拟机运行在你自己的桌面系统中有时甚至是更方便的， 尤其对于测试来说更是如此。如果每个开发者都有一个克隆自生产系统的运行在自己机器上的虚拟机， 那么实际部署时就不太可能遇到问题。 同样地，每个系统管理员也可以在私人的虚拟机上测试配置管理的变化， 这是一种使错误配置实际影响客户之前捕捉错误的良好方式。

几年前出现的工具（如 VirtualBox 或 VMware）就已经能在桌面系统上创建虚拟机。 然而，**Vagrant** 的到来使桌面云技术真正实现了起飞，它是一个管理和供应虚拟化环境的自动化工具。 Vagrant 是 VirtualBox 的前端工具，它驱动 VirtualBox 实现自动创建虚拟机的过程， 并使用自动化管理配置工具（Chef 或 Puppet）为虚拟机调配所需的资源， 设置网络，端口转发，以及对运行着的虚拟机打包生成映像文件以便其他人使用。

你可以使用 Vagrant 管理你用于开发目的虚拟机，这些虚拟机既可以运行在你自己的桌面上， 也可以运行在一台共享的机器上，比如一台持续集成服务器（Continuous Integration Server）。 例如，你可以使用像 **Jenkins** 那样的 CI 工具启动一个由 Vagrant 管理的虚拟机， 部署你的应用程序，然后在虚拟机里运行测试实验，就好像是在生产环境中一样。

#### 操作步骤

1.  创建一个 vagrant 模块：

    ```
    # mkdir /etc/puppet/modules/vagrant
    # mkdir /etc/puppet/modules/vagrant/manifests
    # mkdir /etc/puppet/modules/vagrant/files 
    ```

2.  使用如下内容创建 /etc/puppet/modules/vagrant/manifests/init.pp 文件：

    ```
    class vagrant {
        $virtualbox_deps = [ "libgl1-mesa-glx",
                             "libqt4-network",
                             "libqt4-opengl",
                             "libqtcore4",
                             "libqtgui4",
                             "libsdl1.2debian",
                             "libxmu6",
                             "libxt6",
                             "gawk",
                             "linux-headers-${kernelrelease}" ]
            package { $virtualbox_deps: ensure =&gt; installed }

            exec { "download-virtualbox":
                cwd     =&gt; "/root",
                command =&gt; "/usr/bin/wget http://download.virtualbox.org/
     virtualbox/4.1.0/virtualbox-4.1_4.1.0-73009~Ubuntu~lucid_
     i386.deb",
                creates =&gt; "/root/virtualbox-4.1_4.1.0-73009~Ubuntu~lucid_
     i386.deb",
                timeout =&gt; "-1",
            }

            exec { "install-virtualbox":
                command =&gt; "/usr/bin/dpkg -i /root/virtualbox-4.1_4.1.0-
     73009~Ubuntu~lucid_i386.deb",
                unless  =&gt; "/usr/bin/dpkg -l |/bin/grep virtualbox-4.1",
                require =&gt; [ Exec["download-virtualbox"],
                Package[$virtualbox_deps] ],
            }

            $vagrant_deps = [ "build-essential",
                              "rubygems" ]

            package { $vagrant_deps: ensure =&gt; installed }

            exec { "install-rubygems-update":
                command =&gt; "/usr/bin/gem install -v 1.8.6 rubygemsupdate",
                unless  =&gt; "/usr/bin/gem -v |/bin/grep 1.8.6",
                require =&gt; Package["rubygems"],
            }

            exec { "run-rubygems-update":
                command =&gt; "/var/lib/gems/1.8/bin/update_rubygems",
                unless  =&gt; "/usr/bin/gem -v |/bin/grep 1.8.6",
                require =&gt; Exec["install-rubygems-update"],
            }

            package { "vagrant":
                provider =&gt; gem,
                ensure   =&gt; installed,
                require  =&gt; [ Package["build-essential"],
                              Exec["runrubygems-update"] ],
            }

            define devbox( $vm_user ) {
                include vagrant
                $vm_dir = "/home/${vm_user}/${name}"
                file { [ $vm_dir,
                         "${vm_dir}/data" ]:
                    ensure =&gt; directory,
                    owner  =&gt; $vm_user,
                }

                file { "${vm_dir}/Vagrantfile":
                    source =&gt; "puppet:///modules/vagrant/devbox.
     Vagrantfile",
                    require =&gt; File[$vm_dir],
                }
         }
    } 
    ```

3.  使用如下内容创建 /etc/puppet/modules/vagrant/files/devbox.Vagrantfile 文件：

    ```
    Vagrant::Config.run do |config|
      config.vm.box = "lucid32"
      config.vm.box_url = "http://files.vagrantup.com/lucid32.box"
      config.vm.forward_port "http", 80, 8080
      config.vm.share_folder "v-data", "/vagrant_data", "./data"

      config.vm.customize do |vm|
        vm.name = "devbox"
      end

      config.vm.provision :puppet,:module_path =&gt; "puppet/modules-0"
      do |puppet|
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file = "site.pp"
      end
    end 
    ```

4.  在一个你想要运行虚拟机的节点上包含如下代码（将 john 替换为你自己的用户名）：

    ```
    vagrant::devbox { "devbox":
        vm_user =&gt; "john",
    } 
    ```

5.  添加一个名为 devbox 的节点：

    ```
    node devbox {
        group { "puppet": ensure =&gt; present }
        file { "/etc/motd":
            content =&gt; "Puppet power!\n",
        }
    } 
    ```

6.  运行 Puppet：

    ```
    # puppet agent --test 
    ```

7.  你应该在要运行虚拟机的宿主机上找到用户 john 自家目录下已创建的 devbox 目录。 在此目录中需要拥有一套 Puppet 配置清单的子目录（名为 puppet）， 既可以从你的 Puppet 仓库检出到名为 puppet 的目录，也可以创建一个名为 puppet 的 **符号链接**（**symlink**）指向宿主机上已存在的 Puppet 配置清单目录：

    ```
    # cd ~/devbox
    # git clone git@github.com:Example/Puppet.git puppet 
    ```

    或者

    ```
    # ln -s /etc/puppet ~/devbox/puppet 
    ```

8.  在 devbox 目录中，运行如下命令行：

    ```
    # vagrant up
    [default] Box lucid32 was not found. Fetching box from specified
    URL...
    [default] Downloading with Vagrant::Downloaders::HTTP...
    [default] Downloading box: http://files.vagrantup.com/lucid32.box
    [default] Extracting box...
    [default] Verifying box...
    [default] Cleaning up downloaded box...
    [default] Importing base box 'lucid32'...
    [default] Matching MAC address for NAT networking...
    [default] Clearing any previously set forwarded ports...
    [default] Forwarding ports...
    [default] -- http: 80 =&gt; 8080 (adapter 1)
    [default] -- ssh: 22 =&gt; 2222 (adapter 1)
    [default] Creating shared folders metadata...
    [default] Running any VM customizations...
    [default] Booting VM...
    [default] Waiting for VM to boot. This can take a few minutes.
    [default] VM booted and ready for use!
    [default] Mounting shared folders...
    [default] -- v-root: /vagrant
    [default] -- v-data: /vagrant_data
    [default] -- manifests: /tmp/vagrant-puppet/manifests
    [default] Running provisioner: Vagrant::Provisioners::Puppet...
    [default] Running Puppet with site.pp...
    [default] stdin: is not a tty
    [default] notice: /Stage[main]//Node[devbox]/File[/etc/motd]/
    ensure: defined content as '{md5}0bdeca690dbb409d48391f3772d389b7'
    [default]
    [default] notice: /Group[puppet]/ensure: created
    [default]
    [default] notice: Finished catalog run in 0.36 seconds
    [default] 
    ```

    登录到 devbox 虚拟主机进行测试：

    ```
    # vagrant ssh
    Puppet power!
    Last login: Thu Jul 21 13:07:53 2011 from 10.0.2.2
    vagrant@devbox:~$ logout
    Connection to 127.0.0.1 closed. 
    ```

#### 工作原理

vagrant 类安装 Vagrant 和 VirtualBox 以及所有的依赖。它同时还定义了名为 devbox 的 define，你可以使用它为一台宿主机的多个用户创建 devbox 的多个实例。 devbox 的一个实例如下：

```
vagrant::devbox { "app-foo-devbox":
    vm_user => "john",
} 
```

此实例在用户（本例为 john）的家目录下创建一个名为 app-foo-devbox 的 Vagrant 项目目录 （此目录包含一个配置文件 Vagrantfile，它指定了一个虚拟机的配置定义）。

当 Vagrant 首次启动虚拟机，它会在项目目录的名为 puppet 的子目录中查找提供给本虚拟机的 Puppet 配置清单。 这可以是你当前 Puppet 工作副本的一个符号链接， 也可以是仅为 devbox 编制的独立的 Puppet 配置清单 （无论你使用哪一种方式，只要 Vagrant 能找到即可）。

一旦虚拟主机已经配置好，它就可以投入使用了。运行 vagrant up 命令即可启动虚拟机； vagrant ssh 命令用于登录虚拟机；vagrant halt 命令用于停止虚拟机的运行。

顺便指出，节点定义中名为 puppet 的 group 资源在 Vagrant 的 Puppet 供应时会引发一个错误， 当你看到本书时可能已经被修复。Vagrant 正处于开发活跃期，所以可能会有一两处无法正常工作： 如有疑问，请查看本节最后的文档链接。

你可能会发现有时虚拟机无法完全启动，Vagrant 只是处于超时等待状态。 这似乎也是由于 Vagrant 的一个错误引起的，当你看到本书时可能已经被修复。如果还没有修复， 你可以在 Vagrantfile 中通过添加如下的代码片段来解决这个问题：

```
config.vm.boot_mode = :gui 
```

修改之后重新启动虚拟机。现在虚拟机在 GUI 模式下启动，同时运行了一个控制台窗口。 在此窗口中，以用户名 vagrant（口令为 vagrant）登录，然后运行如下命令：

```
# sudo /etc/init.d/networking restart 
```

现在你发现 Vagrant 会完成配置阶段并且 vagrant ssh 命令也会工作正常。

#### 更多用法

在本例中，我们仅对 devbox 配置了一个极其简单的配置清单，它在 /etc/motd 文件中添加了消息。 为了使其更实用，可以让 devbox 提取与你要部署的实际服务器相同的配置清单。例如：

```
node production, devbox {
    include myapp::production
} 
```

因此，应用到生产服务器配置的任何改变将同时反映在你用于测试的机器上， 这样就可以在实际部署之前先解决出现的问题，如果你需要进行配置的变化以支持新的功能， 可以首先在虚拟机上测试它，看看是否有什么不正常。

如果你不再使用虚拟机，想要挂起或关闭它，只要运行：

```
# vagrant suspend 
```

或

```
# vagrant halt 
```

想要完全删除虚拟机，例如你要重新测试供应，运行：

```
# vagrant destroy 
```

Vagrant 的维护者为了使其使用简单做了相当多的工作，如果你需要阅读更多关于 Vagrant 的内容请访问其文档站点： [`vagrantup.com/docs/index.html`](http://vagrantup.com/docs/index.html) 。