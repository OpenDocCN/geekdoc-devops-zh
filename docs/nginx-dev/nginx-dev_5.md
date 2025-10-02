# 附录 C 模块编译，调试与测试

## C.1 编译与安装

### 环境要求

操作系统：目前 Nginx 各版本在以下操作系统和平台测试通过：

> FreeBSD 3 — 10 / i386; FreeBSD 5 — 10 / amd64;
> 
> Linux 2.2 — 3 / i386; Linux 2.6 — 3 / amd64;
> 
> Solaris 9 / i386, sun4u; Solaris 10 / i386, amd64, sun4v;
> 
> AIX 7.1 / powerpc;
> 
> HP-UX 11.31 / ia64;
> 
> MacOS X / ppc, i386;
> 
> Windows XP, Windows Server 2003

磁盘空间：必须保证至少 10M 以上的磁盘空间，并且随着编译设置及第三方模块的安装而有所不同；

编译器及相关工具： 必须确保操作系统安装有 GCC 编译器；make 工具；用户可通过 yum 命令安装编译器及相关工具：yum -y install gcc gcc-c++ make；

模块依赖性：Nginx 的一些模块需要第三方库的支持，如 rewrite 模块需要 pcre 库，gzip 模块需要 zlib 模块，ssl 功能需要 openssl 库等。用户可通过 yum 命令安装这些依赖库：yum -y install pcre pcre-devel zlib zlib-devel openssl openssl-devel；

### 下载

Nginx 是开源软件，用户可以访问 [`nginx.org/`](http://nginx.org/) 网站获取源码包或 Windows 二进制文件下载。其中 1.3.x 版本为开发版本，1.2.x 版本为稳定版本。开发版本分支会较快的获得新功能和缺陷修复，但同时也可能会遇到新的缺陷。一旦更新稳定下来，就会被加入稳定版本分支。

作为生产环境，通常建议用户使用稳定版本。

### Nginx 在 Windows 环境下安装

Nginx 的 windows 版本使用原生 win32 API（非 Cygwin 模拟层）。当前存在的已知问题：1.采用 select 作为通知方法，所以不具备很高的性能和扩展性；2.虽然可以启动若干工作进程运行，实际上只有一个进程在处理所有请求；3.一个工作进程只能处理不超过 1024 个并发连接；4.缓存和其他需要共享内存支持的模块在 windows vista 及后续版本的操作系统中无法工作，因为在这些操作系统中，地址空间的布局是随机的；5.除了 XSLT 过滤器、图像过滤器、GeoIP 模块和嵌入 Perl 语言支持以外，Nginx 的 Windows 版本与 Unix 版本相比，功能几乎齐全。

安装 Nginx 的 Windows 版本，建议下载最新的 1.3.13 开发版本，因为开发分支上包含了所有已知的问题修复，尤其是针对 Windows 版本的问题修复。解压下载得到的 zip 文件，进入 nginx-1.3.13 目录，运行 nginx。

C 盘根目录下安装例子

```
cd c:\
unzip nginx-1.3.13.zip
cd nginx-1.3.13
start nginx

```

Nginx 的 Windows 版本的控制命令包含如下：

```
nginx -s stop 快速退出
nginx -s quit 优雅退出
nginx -s reload 更换配置，启动新的工作进程，优雅的关闭以往的工作进程
nginx -s reopen 重新打开日志文件

```

### Nginx 在 Linux 环境下安装

Nginx 在 Linux 环境下可以通过编译源码的方式安装，最简单的安装命令如下：

```
wget http://nginx.org/download/nginx-1.2.0.tar.gz
tar zxvf nginx-1.2.0.tar.gz
cd nginx-1.2.0
./configure
make
sudo make install

```

按照以上命令，Nginx 将被默认安装到/usr/local/nginx 目录下。用户可以通过./configure –help 命令查看 Nginx 可选择的编译选项进行自定义安装配置。

Nginx 的 configure 脚本支持以下选项:

```
--prefix=<PATH> #Nginx 安装路径。如果没有指定，默认为 /usr/local/nginx

--sbin-path=<PATH> #Nginx 可执行文件安装路径。只能安装时指定，如果没有指定，默认为<prefix>/sbin/nginx

--conf-path=<PATH> #在没有给定-c 选项下默认的 nginx.conf 的路径。如果没有指定，默认为<prefix>/conf/nginx.conf

--pid-path=<PATH> #在 nginx.conf 中没有指定 pid 指令的情况下，默认的 nginx.pid 的路径。如果没有指定，默认为 <prefix>/logs/nginx.pid

--lock-path=<PATH> #nginx.lock 文件的路径

--error-log-path=<PATH> #在 nginx.conf 中没有指定 error_log 指令的情况下，默认的错误日志的路径。如果没有指定，默认为 <prefix>/logs/error.log

--http-log-path=<PATH> #在 nginx.conf 中没有指定 access_log 指令的情况下，默认的访问日志的路径。如果没有指定，默认为 <prefix>/logs/access.log。

--user=<USER> #在 nginx.conf 中没有指定 user 指令的情况下，默认的 nginx 使用的用户。如果没有指定，默认为 nobody

--group=<GROUP> #在 nginx.conf 中没有指定 user 指令的情况下，默认的 nginx 使用的组。如果没有指定，默认为 nobody

--builddir=DIR #指定编译的目录

--with-rtsig_module #启用 rtsig 模块

--with-select_module(--without-select_module) #允许或不允许开启 SELECT 模式，如果 configure 没有找到合适的模式，比如，kqueue(sun os)、epoll(linux kenel 2.6+)、rtsig(实时信号)或/dev/poll（一种类似 select 的模式，底层实现与 SELECT 基本相同，都是采用轮询的方法），SELECT 模式将是默认安装模式

--with-poll_module(--without-poll_module) #允许或不允许开启 POLL 模式，如果没有合适的模式，比如：kqueue（sun os）、epoll（liunx kernel 2.6+），则开启该模式

--with-http_ssl_module #开启 HTTP SSL 模块，使 NGINX 可以支持 HTTPS 请求。这个模块需要已经安装了 OPENSSL，在 DEBIAN 上是 libssl

--with-http_realip_module #启用 ngx_http_realip_module

--with-http_addition_module #启用 ngx_http_addition_module

--with-http_sub_module #启用 ngx_http_sub_module

--with-http_dav_module #启用 ngx_http_dav_module

--with-http_flv_module #启用 ngx_http_flv_module

--with-http_stub_status_module #启用 "server status" 页

--without-http_charset_module #禁用 ngx_http_charset_module

--without-http_gzip_module #禁用 ngx_http_gzip_module. 如果启用，需要 zlib 。

--without-http_ssi_module #禁用 ngx_http_ssi_module

--without-http_userid_module #禁用 ngx_http_userid_module

--without-http_access_module #禁用 ngx_http_access_module

--without-http_auth_basic_module #禁用 ngx_http_auth_basic_module

--without-http_autoindex_module #禁用 ngx_http_autoindex_module

--without-http_geo_module #禁用 ngx_http_geo_module

--without-http_map_module #禁用 ngx_http_map_module

--without-http_referer_module #禁用 ngx_http_referer_module

--without-http_rewrite_module #禁用 ngx_http_rewrite_module. 如果启用需要 PCRE 。

--without-http_proxy_module #禁用 ngx_http_proxy_module

--without-http_fastcgi_module #禁用 ngx_http_fastcgi_module

--without-http_memcached_module #禁用 ngx_http_memcached_module

--without-http_limit_zone_module #禁用 ngx_http_limit_zone_module

--without-http_empty_gif_module #禁用 ngx_http_empty_gif_module

--without-http_browser_module #禁用 ngx_http_browser_module

--without-http_upstream_ip_hash_module #禁用 ngx_http_upstream_ip_hash_module

--with-http_perl_module #启用 ngx_http_perl_module

--with-perl_modules_path=PATH #指定 perl 模块的路径

--with-perl=PATH #指定 perl 执行文件的路径

--http-log-path=PATH #指定 http 默认访问日志的路径

--http-client-body-temp-path=PATH #指定 http 客户端请求缓存文件存放目录的路径

--http-proxy-temp-path=PATH #指定 http 反向代理缓存文件存放目录的路径

--http-fastcgi-temp-path=PATH #指定 http FastCGI 缓存文件存放目录的路径

--without-http #禁用 HTTP server

--with-mail #启用 IMAP4/POP3/SMTP 代理模块

--with-mail_ssl_module #启用 ngx_mail_ssl_module

--with-cc=PATH #指定 C 编译器的路径

--with-cpp=PATH #指定 C 预处理器的路径

--with-cc-opt=OPTIONS #设置 C 编译器的额外选项

--with-ld-opt=OPTIONS #设置链接的额外选项

--with-cpu-opt=CPU #为特定的 CPU 编译，有效的值包括：pentium, pentiumpro, pentium3, pentium4, athlon, opteron, amd64, sparc32, sparc64, ppc64

--without-pcre #禁止 PCRE 库的使用。同时也会禁止 HTTP rewrite 模块。在 "location" 配置指令中的正则表达式也需要 PCRE

--with-pcre=DIR #指定 PCRE 库的源代码的路径

--with-pcre-opt=OPTIONS #设置 PCRE 的额外编译选项

--with-md5=DIR #使用 MD5 汇编源码

--with-md5-opt=OPTIONS #设置 MD5 库的额外编译选项

--with-md5-asm #使用 MD5 汇编源码

--with-sha1=DIR #设置 sha1 库的源代码路径

--with-sha1-opt=OPTIONS #设置 sha1 库的额外编译选项

--with-sha1-asm #使用 sha1 汇编源码

--with-zlib=DIR #设置 zlib 库的源代码路径

--with-zlib-opt=OPTIONS #设置 zlib 库的额外编译选项

--with-zlib-asm=CPU #zlib 针对 CPU 的优化，合法的值是: pentium, pentiumpro

--with-openssl=DIR #设置 OpenSSL 库的源代码路径

--with-openssl-opt=OPTIONS #设置 OpenSSL 库的额外编译选项

--with-debug #启用调试日志

--add-module=PATH #添加一个在指定路径中能够找到的第三方模块

```

在不同版本间，选项可能会有些许变化，请总是使用./configure –help 命令来检查当前的选项列表。

### 测试

将 Nginx conf 文件的 server block 部分的配置如下：

```
server {
    listen 80;
    server_name localhost;

    location / {
        root html;
        index index.html index.htm;
    }

    # redirect server error pages to the static page /50x.html
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root html;
    }
}

```

用户可以通过访问“[`localhost:80/index.html`](http://localhost:80/index.html)”页面来查看 Nginx 的欢迎页面。

### 在 Windows 环境下查看 nginx 进程

用户还可以通过命令行运行 tasklist 命令来查看 Nginx 进程：

```
C:\>tasklist /fi "imagename eq nginx.exe"

映像名称 PID 会话名 会话# 内存使用
========================= ======== ================ =========== ============
nginx.exe 463024 Console 1 5,036 K
nginx.exe 462960 Console 1 5,280 K

```

如果 Nginx 没有启动或没有得到预期展示页面，可查看 error.log 文件以查看失败原因。如果日志文件不存在，可在 Windows 事件日志中查看。

### 在 Linux 环境下查看 Nginx 进程

用户可以通过执行 ps/top 命令来查看 nginx 进程：

```
ps aux|grep nginx
admin 24913 0.0 0.0 58596 1048 ? Ss Feb27 0:00 nginx: master process ./nginx
admin 24914 0.0 0.0 72772 5420 ? S Feb27 0:03 nginx: worker process

```

同上，如果 nginx 没有启动或者没有得到预期展示页面，可以查看 error.log 文件或调试来查看失败原因。

## C.2 调试日志

用户在使用 Nginx 的过程中，可能会遇到所请求的资源不正确，Nginx Core Dump，段错误等异常情况，这时需要有相应的机制来进行调试及问题定位，特别是面对大量的日志信息，合理的调试处理机制对用户来说是一件非常重要的事情。以下将着重为大家介绍调试日志。

### 一，开启调试日志：

要开启调试日志，首先需要在配置 Nginx 时打开调试功能，然后编译：

```
./configure --with-debug ...

```

然后在配置文件中设置 error_log 的级别为：

```
error_log /path/to/log debug;

```

Nginx 的 Windows 二进制版本总是将调试日志开启的，因此只需要设置 debug 的日志级别即可。

### 二，日志级别分析：

在此，我们通过分析 Nginx 源码了解下 Nginx 将日志分为几个等级及不同日志等级之间的相互关系：

> Ngx_log.h 代码

```
#define NGX_LOG_STDERR 0
#define NGX_LOG_EMERG 1
#define NGX_LOG_ALERT 2
#define NGX_LOG_CRIT 3
#define NGX_LOG_ERR 4
#define NGX_LOG_WARN 5
#define NGX_LOG_NOTICE 6
#define NGX_LOG_INFO 7
#define NGX_LOG_DEBUG 8

#define NGX_LOG_DEBUG_CORE 0x010
#define NGX_LOG_DEBUG_ALLOC 0x020
#define NGX_LOG_DEBUG_MUTEX 0x040
#define NGX_LOG_DEBUG_EVENT 0x080
#define NGX_LOG_DEBUG_HTTP 0x100
#define NGX_LOG_DEBUG_MAIL 0x200
#define NGX_LOG_DEBUG_MYSQL 0x400

#define NGX_LOG_DEBUG_FIRST NGX_LOG_DEBUG_CORE
#define NGX_LOG_DEBUG_LAST NGX_LOG_DEBUG_MYSQL
#define NGX_LOG_DEBUG_CONNECTION 0x80000000
#define NGX_LOG_DEBUG_ALL 0x7ffffff0

```

其中默认有效的第一级别日志是”stderr”，”emerg”，”alert”，”crit”，”error”，”warn”，”notice”，”info”，”debug”。 而 ngx_log.h 内列出的其他 debug 第二级别日志：”debug_core”，”debug_alloc”，”debug_mutex”，”debug_event”，”debug_http”，”debug_mail”，”debug_mysql”等则需要在配置 Nginx 时启动调试日志功能才能使用，并且用户可以通过修改 ngx_log.h 及 ngx_log.c 源码来更新 debug 第二级别。

我们再通过 ngx_log.c 的部分代码分析下可以如何使用这些日志级别：

> ngx_log.c 代码

```
char *
ngx_log_set_levels(ngx_conf_t *cf, ngx_log_t *log)
{
    ...

        for (n = 1; n <= NGX_LOG_DEBUG; n++) {
            if (ngx_strcmp(value[i].data, err_levels[n].data) == 0) {

                if (log->log_level != 0) {
                    ngx_conf_log_error(NGX_LOG_EMERG, cf, 0,
                                       "duplicate log level \"%V\"",
                                       &value[i]);
                    return NGX_CONF_ERROR;
                }

                log->log_level = n;
                found = 1;
                break;
            }
        }

        for (n = 0, d = NGX_LOG_DEBUG_FIRST; d <= NGX_LOG_DEBUG_LAST; d <<= 1) {
            if (ngx_strcmp(value[i].data, debug_levels[n++]) == 0) {
                if (log->log_level & ~NGX_LOG_DEBUG_ALL) {
                    ngx_conf_log_error(NGX_LOG_EMERG, cf, 0,
                                       "invalid log level \"%V\"",
                                       &value[i]);
                    return NGX_CONF_ERROR;
                }

                log->log_level |= d;
                found = 1;
                break;
            }
        }
 ...
 if (log->log_level == NGX_LOG_DEBUG) {
        log->log_level = NGX_LOG_DEBUG_ALL;
    }
 ...
}

```

按照以上代码逻辑，我们可以得出以下结论：

1\. 第一级别日志之间是互斥的，如果配置文件内加入如下配置项：

```
error_log path/logs/error.log warn;
error_log path/logs/error.log info;

```

那么启动 Nginx 将报错如下：

```
[emerg]: duplicate log level "info" in /path/conf/nginx.conf:XX

```

但是需要注意的是，在配置文件不同 block 中是允许重新定义错误日志的。但是当用户在重新定义错误日志时，如果没有指定相应的日志级别，那么调试日志将会被屏蔽。下面的例子里，在 server 层中重新定义的日志就屏蔽了这个虚拟主机的调试日志：

```
error_log  /path/to/log  debug;

http {
    server {
        error_log  /path/to/log;
        ...

```

为了避免这个问题，可以注释这行重新定义日志的配置，或者也给日志指定 debug 级别：

```
error_log  /path/to/log  debug;

http {
    server {
        error_log  /path/to/log  debug;
        ...

```

2\. 第二级别日志是多选的，用户可以根据项目需要配置多个第二级别日志：

```
error_log  logs/error.log debug_mysql;
error_log  logs/error.log debug_core;

```

3\. 在第一级别日志与第二级别日志组合配置时，仅有在第一级别日志为”debug”时才可以有第二级别的配置，其他第一级别日志的情况下指定第二级别日志将无法启动 Nginx，如：

```
error_log  logs/error.log error;
error_log  logs/error.log debug_core;

```

启动 Nginx 将获得如下错误信息：

```
[emerg]: invalid log level “debug_http” in /path/conf/nginx.conf:XX

```

当用户开启 debug 级别日志时，会输出所有 debug_ 开头的调试信息，因此可以通过上面组合 debug_core|debug_http 的形式来获取用户所需要的调试信息。

### 三，日志格式设置：

用户在使用 Nginx 提供 web 服务的时候，可能会有很多场景需要记录日志，如打点日志，访问日志，数据统计日志，性能分析日志等。为了更加方便的对日志进行分析，我们可以通过设置日志格式的方式来要求 Nginx 按照用户要求进行日志的展现。

控制 nginx 日志输出的指令如下：

```
log_format  customLog "$remote_addr^A$remote_user^A$time_local^A$request_method^A$uri^A$args^A$server_protocol"
        "^A$status^A$body_bytes_sent^A$http_referer"
        "^A$http_user_agent";
access_log /path/logs/access.log customLog;

```

上面例子中通过使用特殊字符（^A）来作为日志字段的分隔符，用户后续可以使用 sort 和 grep 之类的工具对特定 url 做分析，如统计各 url 请求量倒排取前 50 个：

```
awk -F^A '{print $5}' /path/logs/access.log | sort | uniq -c | sort -nr | head -50

```

类似上面的日志定制化设置，可以让用户在调试日志的过程中随心所欲，如鱼得水。 详细的 log_format 指令和 access_log 指令，用户可以访问 Nginx 官网的 HttpLog 模块 [`wiki.nginx.org/HttpLogModule`](http://wiki.nginx.org/HttpLogModule) 。

### 四，调试日志的几个注意点：

1\. 勘误：在 Nginx Wiki 里面 error log 相关部分（[`wiki.nginx.org/NginxHttpMainModule#error_log`](http://wiki.nginx.org/NginxHttpMainModule#error_log) ）的介绍中提到

```
Default values for the error level:
in the main section - error
in the HTTP section - crit
in the server section - crit

```

但是，我们从源码上看：

```
static char *
ngx_error_log(ngx_conf_t *cf, ngx_command_t *cmd, void *conf)
{
    ...

    if (cf->args->nelts == 2) {
        cf->cycle->new_log.log_level = NGX_LOG_ERR;
        return NGX_CONF_OK;
    }
 ...
}

```

当 error_log 的日志级别选项为配置时，默认日志级别为 error，无上面提及的三个 section 的区别。故特在此勘误。

2\. 配置 error_log off 并不能关闭日志记录——日志信息会被写入到文件名为 off 的文件当中。如果要关闭日志记录，用户可以做如下配置：

```
error_log /dev/null crit;

```

3\. 如果 nginx 进程没有权限将日志信息写入指定的 log 地址，那么 nginx 会在启动是报错：

```
[alert]: could not open error log file: open() "/path/log/nginx/error.log" failed (13: Permission denied)

```

4\. 通过 debug_connection 配置项，用户可以针对某些地址开启调试日志：

```
error_log  /path/to/log;

events {
    debug_connection   10.232.10.1;
    debug_connection   10.232.10.0/24;
}

```

## C.3 使用 GDB 调试

## C.4 功能测试

## C.5 性能/压力测试

## C.6 常见缺陷分析

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# Index

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.