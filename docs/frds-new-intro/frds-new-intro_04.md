# 第三章: 开始使用 FreeRADIUS

在 FreeRADIUS 被安装之后, 他需要按照我们的需要进行配置. 这一章将会帮助你熟悉 FreeRADIUS. 假定你已经了解第一章中讨论的 RADIUS 协议的基础了.

正在这一章我们将会:

*   对 FreeRADIUS 进行一个基本配置, 并测试他.
*   发现更多获取帮助的方法.
*   学会推荐的配置和测试 FreeRADIUS 的方法.
*   了解一起是怎么适配在一起与 FreeRADIUS 工作的.

## 开始之前

这章假定你有一个 FreeRADIUS 的干净的安装. 你将会需要 root 权限来修改 FreeRADIUS 的配置文件来基本配置和测试

# 一个简单的配置

我们以一个下面简单的 FreeRADIUS 配置来开始这一章

*   本机作为一个 NAS 设备(RADIUS 客户端)
*   Alice 定义为一个测试用户.

在我们已经定义客户端和测试用户之后, 我们将会使用`radtest`程序来作为 RADIUS 客户端的角色并且测试 Alice 的 authentication.

# 配置 FreeRADIUS

FreeRADIUS 是通过修改配置文件来配置的. 这些文件的位置依赖于 FreeRADIUS 是怎样被安装的:

*   如果你安装发行版提供的标准 FreeRADIUS 包, 在 CentOS 和 SLES 上, 将会在`/etc/raddb`. 在 Ubuntu 上将会在`/etc/freeradius`.
*   如果你使用发行版的包管理系统从源码编译和安装 FreeRADIUS, 在 CentOS 和 SLES 上, 将也会在`/etc/raddb`. 在 Ubuntu 上将会在`/etc/freeradius`.
*   如果你通过`configure`, `make`, `make install`来编译和安装 FreeRADIUS, 他将会在`/usr/local/etc/raddb`.

写明的命令假定 FreeRADIUS 的配置目录为你的当前工作目录.

1.  确保你是 root 用户为了能够编辑配置文件.
2.  FreeRADIUS 包括一个默认的客户端叫做`localhost`. 这个客户端被在 localhost 上 RADIUS 客户端程序所使用来帮助排查问题和测试. 确认下面的入口存在在`clients.conf`文件中:

    ```
    client localhost {
     ipaddr = 127.0.0.1
     secret = testing123
     require_message_authenticator = no
     nastype = other
    } 
    ```

3.  定义 Alice 作为一个 FreeRADIUS 测试用户. 添加下面的行到`users`文件的顶部. 确定第二行和第三行前面由一个单一的 tab 键缩进:

    ```
    "alice" Cleartext-Password := "passme"
     Framed-IP-Address = 192.168.1.65,
     Reply-Message = "Hello, %{User-Name}" 
    ```

4.  以 debug 模式启动 FreeRADIUS. 使用启动脚本来关闭 FreeRADIUS 来确保没有其他实例在运行. 在 Ubuntu 中我们这样做:

    ```
    # /etc/init.d/freeradius stop
    # freeradius -X 
    ```

    你也可以使用更暴力的方法: `kill -9 $(pidof freeradius)` 或者 `killall freeradius`在 Ubuntu 上和`kill -9 $(pidof radius)`或者`killall radiusd`在 CentOS 和 SLES 上, 如果启动脚本没能停止 FreeRADIUS.
5.  确保 FreeRADIUS 已经正确的启动通过确认在你的屏幕上的最后一行是: `Ready to process requests.` 如果这个没有发生, 阅读 FreeRADIUS 的 debug 输出.
6.  使用下面的命令来 Authenticate Alice.

    ```
    radtest alice passme 127.0.0.1 100 testing123 
    ```

7.  FreeRADIUS 的 debug 输出将会展示`Access-Request`包到达并且 FreeRADIUS 服务器如何响应这个请求.
8.  `radtest`将也会显示 FreeRADIUS 服务器的回复:

    ```
    Sending Access-Request of id 17 to 127.0.0.1 port 1812
     User-Name = "alice"
     User-Password = "passme"
     NAS-IP-Address = 127.0.0.1
     NAS-Port = 100
    rad_recv: Access-Accept packet from host 127.0.0.1 port 1812, id=147, length=40
     Framed-IP-Address = 192.168.1.65
     Reply-Message = "Hello, alice" 
    ```

## 发生了什么?

我们已经在 FreeRADIUS 上创建了一个测试用户. 我们也使用`radtest`命令作为一个客户端到 FreeRADIUS 服务器来测试 authentication.

让我们详细描述一些有趣和重要的点.

### 配置 FreeRADIUS

FreeRADIUS 服务器的配置逻辑上划分为不同的文件. 这些文件被修改来配置某个功能, 组件或者 FreeRADIUS 模块. 然而有一个主配置文件包含各种子配置文件, 叫做`radiusd.conf`.

默认的配置适合大多数安装. 只需要非常少修改来使 FreeRADIUS 在你的环境有用.

### 客户端

尽管在 FreeRADIUS 服务器的配置目录内有许多文件, 只有少量将来需要修改的. `clients.conf`文件是用来定义到 FreeRADIUS 服务器的客户端的.

在一个 NAS 可以使用 FreeRADIUS 服务器之前, 他必须被定义为在 FreeRADIUS 服务器上的一个客户端. 让我们看一些关于客户端定义的要点.

#### Sections

一个客户端通过一个`client` section 定义. FreeRADIUS 使用 section 来分组和定义许多东西. 一个 section 以一个关键字开始表示 section 名称. 然后跟着一个封闭的大括号. 在封闭的括号内部有许多配置指定到这个 section. section 可以被嵌套.

有时 section 的关键字跟着一个单一的词来区分通一个类型的不同的 section. 这允许我们在`clients.conf`中有不同客户端入口. 没一个客户端有一个简短的名称来区分他和其他的.

`clients.conf`文件不是唯一的文件, `client` section 可以被定义, 尽管他是常用和最符合逻辑的地方. 下图显示在一个 server section 内嵌套定义的客户端.

#### 客户端标识

FreeRADIUS 服务器通过他的 IP 地址标识一个客户端. 如果一个不知道的客户端发送给服务器一个请求, 这个请求将会被默默忽略.

### 共享密钥(shared secret)

客户端和服务器端也需要有一个共享的密钥, 将会用来加密和解密某些 AVPs. `User-Password` AVP 的值使用这个共享密钥加密. 当共享密钥在客户端和服务器端不同时, FreeRADIUS 服务器将会检测到他并且当运行的 debug 模式时警告你.

```
Failed to authenticate the user.
    WARNING: Unprintable characters in the password. Double-check the shared secret on the server and the NAS! 
```

### Message-Authenticator

当定义一个客户端你可以强制在所有请求中出现`Message-Authenticator` AVP. 由于我们将会使用`radtest`程序, 没有包含他, 我们对于`localhost`禁用他, 通过设置`require_message_authenticator`为`no`.

### Nastype

`nastype`设置为`other`. `nastype`的值将会剧而定`checkrad` Perl 脚本将会如何执行. `checkrad`是用来决定一个用户是否已经使用一个 NAS 的资源. 由于`localhost`没有这个功能或者我们需要定义他为`other`.

### 常见错误

如果服务器下线或者从`radtest`发出的包由于防火墙不能到达服务器, `radtest`将会尝试 3 次然后以下面信息放弃

```
radclient: no response from server for ID 133 socket 3 
```

如果你用普通用户运行`radtest`, 他可能会抱怨没有权限访问 FreeRADIUS 字典文件. 这对于普通操作是需要的. 解决这个的方法是修改报告的文件的权限或者用 root 运行`radtest`

### users 文件

用户定义在`users`文件在 FreeRADIUS 的配置目录. `users`文件的内容是用来 Authorization 和 Authentication. 这个文件不仅使用户的来源也是一个简单有效的方式开始. 让我们看一些关于 users 文件的关键点.

### 文件模块

`files`模块(`rlm_files`)读取`users`文件的内容来决定是否在`Access-Request`中指定的用户存在并且授权来使用 NAS. 他也决定了什么 attribute 应该被返回给客户端.

`files`模块也设置了`Auth-Type`. 这将会决定要使用的 authentication 方法. 如果一个用户定义了带有`Cleartext-Password` check item, 他将会设置`Auth-Type = PAP`.

`files`模块也提供其他的模块带有一个值"know good password"如果定义了. 在我们的例子中, 他提供`pap`模块(`rlm_pap`)带有值指定在`Cleartext-Password` check AVP.

### PAP 模块

`pap`模块(`rlm_pap`)是用来 authentication. 如果`Auth-Type`设置为`PAP`, 他将会寻找`known good password`并且将他跟`User-Password` AVP 的值比较. 如果是相同的, 这个模块将会通过 authentication 请求.

请求可能仍然失败由于在 FreeRADIUS authentication 链条的其他模块.

### Users 文件

`users`文件不包含任何 section 像`clients.conf`. 这是因为他是针对`files`模块并且不直接跟 FreeRADIUS 服务器自身相关的.

要在`users`文件中添加一个入口, 你可以定义一个用户名跟着 0 个或者更多逗号分隔的检查项. 接着跟着 0 个或者更多 tab 缩进的行用逗号分隔的回复项.

我们假定你在使用默认的配置没有对`sites-enabled/default`虚拟服务器做任何修改. 如果你改了, 例如, 激活了在`authorize` section 下面的 unix 模块并且 Alice 也被定义为一个系统用户, 系统用户和他们密码将会是优先的用户而不是在`users`文件中定义的那个. 结果将会是一个`Access-Reject`包被返回来回应`Access-Request`如果在 Alice(系统用户)和 Alice(在`user`文件中用户)不同.

### Check 项目

下面的入口需要`Access-Request`包, 包含一个 AVP 对于`NAS-IP-Address`带有值`127.0.0.1`.

```
"alice" Cleartext-Password := "passme", NAS-IP-Address == '127.0.0.1' 
```

`radtest`程序将会默认设置`NAS-IP-Address`的值为 hostname 的 IP 在`/etc/hosts`文件中指定的. 后面的章节中你将会看到这个值怎样修改.

一些 AVPs 有一个特殊的含义和被 FreeRADIUS 内部使用. 尽管进来的`Access-Request`没有包含一个 AVP 叫做`Cleartext-Password`, 这个`files`模块内部使用他来调整`Auth-Type`的值并且来创建一个好的密码, 可以被`pap`模块使用来 authentication.

另一个特殊的 AVP 的例子是当你想要拒绝或者接受一个基于用户的用户名, 无论他们的密码是什么. 之前的行将会改变为下面中的一个.

```
"alice" Cleartext-Password := "passme", Auth-Type := Reject
"alice" Cleartext-Password := "passme", Auth-Type := Accept 
```

尽管`Auth-Type`似乎是一个标准的 AVP 检查项, 他属于 FreeRADIUS 内部, 并且是用来控制哪种 authentication 将会被使用.

### 回复项

# 帮助你自己

# 挖掘 FreeRADIUS 的 man page

# 黄金法则

# 深入到 radiusd 内部

# 总结