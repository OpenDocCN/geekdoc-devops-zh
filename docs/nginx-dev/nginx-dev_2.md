# 下篇：nginx 原理解析篇

*   高性能服务器设计
    *   c10k 问题
    *   高性能服务器编写的关键原则
    *   事件驱动的核心引擎
    *   定时器管理
*   nginx 架构详解(50%)
    *   nginx 的源码目录结构(100%)
    *   nginx 的 configure 原理(100%)
        *   auto 脚本
        *   模块编译顺序
    *   nginx 的事件机制
        *   event 框架及非阻塞模型
        *   定时器实现
        *   信号处理
        *   惊群问题
    *   nginx 的进程机制
        *   master 进程
        *   worker 进程
        *   进程间通讯
*   nginx 基础设施
    *   内存池
        *   简介:
        *   结构:
        *   实现:
    *   共享内存
        *   slab 算法
    *   buffer 管理
        *   buffer 重用机制
        *   buffer 防拷贝机制
    *   chain 管理
        *   chain 重用机制
    *   aio 原理
    *   锁实现
    *   基本数据结构
    *   时间缓存
    *   文件缓存
    *   log 机制
*   nginx 的启动阶段 (30%)
    *   概述 (100%)
    *   共有流程 (100%)
        *   配置解析接口 (100%)
    *   配置解析
        *   通用过程 (100%)
        *   http 的解析
            *   创建并初始化上下文环境
            *   调用通用解析流程解析
            *   根据解析结果进行后续合并处理
            *   善后工作
        *   server 的管理
        *   location 的管理
    *   模块初始化
    *   热代码部署
        *   reload 过程解析
        *   upgrade 过程解析
*   nginx 的请求处理阶段 (90%)
    *   接收请求流程 (99%)
        *   http 请求格式简介 (99%)
        *   请求头读取 (99%)
            *   解析请求行 (99%)
            *   解析请求头 (99%)
        *   请求体读取(100%)
            *   读取请求体
            *   丢弃请求体
    *   多阶段处理请求
        *   多阶段执行链
        *   POST_READ 阶段
        *   SERVER_REWRITE 阶段
        *   FIND_CONFIG 阶段
        *   REWRITE 阶段
        *   POST_REWRITE 阶段
        *   PREACCESS 阶段
        *   ACCESS 阶段
        *   POST_ACCESS 阶段
        *   TRY_FILES 阶段
        *   CONTENT 阶段
        *   LOG 阶段
    *   Nginx filter
        *   header filter 分析
        *   body filter 分析
            *   ngx_http_copy_filter_module 分析
            *   ngx_http_write_filter_module 分析
    *   subrequest 原理解析 (99%)
    *   https 请求处理解析
        *   nginx 支持 ssl 简介
        *   ssl 连接建立(ssl 握手)
            *   对 ssl 连接建立的准备
            *   实际 ssl 握手阶段
            *   ssl 协议接受数据
            *   ssl 协议发送数据
*   nginx 的 upstream 原理解析
    *   无缓冲的处理流程
    *   带缓冲的处理流程
    *   subrequest 访问 upstream 的处理流程
    *   负载均衡算法
        *   round-robin 算法
        *   基于 ip 的 hash 算法
    *   upstream 缓存机制
    *   常用 upstream 模块分析
        *   proxy 模块
        *   fastcgi 模块
*   nginx 的 script 处理
    *   正则表达式
    *   变量
    *   复杂变量
    *   if 的处理

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# 高性能服务器设计

## c10k 问题

## 高性能服务器编写的关键原则

## 事件驱动的核心引擎

## 定时器管理

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# nginx 架构详解(50%)

nginx 的下篇将会更加深入的介绍 nginx 的实现原理。上一章，我们了解到了如何设计一个高性能服务器，那这一章将会开始讲解，nginx 是如何一步一步实现高性能服务器的。

## nginx 的源码目录结构(100%)

nginx 的优秀除了体现在程序结构以及代码风格上，nginx 的源码组织也同样简洁明了，目录结构层次结构清晰，值得我们去学习。nginx 的源码目录与 nginx 的模块化以及功能的划分是紧密结合，这也使得我们可以很方便地找到相关功能的代码。这节先介绍 nginx 源码的目录结构，先对 nginx 的源码有一个大致的认识，下节会讲解 nginx 如何编译。

下面是 nginx 源码的目录结构：

```
.
├── auto            自动检测系统环境以及编译相关的脚本
│   ├── cc          关于编译器相关的编译选项的检测脚本
│   ├── lib         nginx 编译所需要的一些库的检测脚本
│   ├── os          与平台相关的一些系统参数与系统调用相关的检测
│   └── types       与数据类型相关的一些辅助脚本
├── conf            存放默认配置文件，在 make install 后，会拷贝到安装目录中去
├── contrib         存放一些实用工具，如 geo 配置生成工具（geo2nginx.pl）
├── html            存放默认的网页文件，在 make install 后，会拷贝到安装目录中去
├── man             nginx 的 man 手册
└── src             存放 nginx 的源代码
    ├── core        nginx 的核心源代码，包括常用数据结构的定义，以及 nginx 初始化运行的核心代码如 main 函数
    ├── event       对系统事件处理机制的封装，以及定时器的实现相关代码
    │   └── modules 不同事件处理方式的模块化，如 select、poll、epoll、kqueue 等
    ├── http        nginx 作为 http 服务器相关的代码
    │   └── modules 包含 http 的各种功能模块
    ├── mail        nginx 作为邮件代理服务器相关的代码
    ├── misc        一些辅助代码，测试 c++头的兼容性，以及对 google_perftools 的支持
    └── os          主要是对各种不同体系统结构所提供的系统函数的封装，对外提供统一的系统调用接口

```

## nginx 的 configure 原理(100%)

nginx 的编译旅程将从 configure 开始，configure 脚本将根据我们输入的选项、系统环境参与来生成所需的文件（包含源文件与 Makefile 文件）。configure 会调用一系列 auto 脚本来实现编译环境的初始化。

### auto 脚本

auto 脚本由一系列脚本组成，他们有一些是实现一些通用功能由其它脚本来调用（如 have），有一些则是完成一些特定的功能（如 option）。脚本之间的主要执行顺序及调用关系如下图所示（由上到下，表示主流程的执行）：

![`tengine.taobao.org/book/_images/chapter-9-1.jpg`](http://tengine.taobao.org/book/_images/chapter-9-1.jpg)

接下来，我们结合代码来分析下 configure 的原理:

1.  初始化

```
. auto/options
. auto/init
. auto/sources

```

这是 configure 源码开始执行的前三行，依次交由 auto 目录下面的 option、init、sources 来处理。

2.  auto/options 主是处理用户输入的 configure 选项，以及输出帮助信息等。读者可以结合 nginx 的源码来阅读本章内容。由于篇幅关系，这里大致列出此文件的结构：

```
##1\. 设置选项对应的 shell 变量以及他们的初始值
help=no
NGX_PREFIX=
NGX_SBIN_PATH=
NGX_CONF_PREFIX=
NGX_CONF_PATH=
NGX_ERROR_LOG_PATH=
NGX_PID_PATH=
NGX_LOCK_PATH=
NGX_USER=
NGX_GROUP=

...

## 2, 处理每一个选项值，并设置到对应的全局变量中
for option
do
    opt="$opt `echo $option | sed -e \"s/\(--[^=]*=\)\(.* .*\)/\1'\2'/\"`"

    # 得到此选项的 value 部分
    case "$option" in
        -*=*) value=`echo "$option" | sed -e 's/[-_a-zA-Z0-9]*=//'` ;;
            *) value="" ;;
    esac

    # 根据 option 内容进行匹配，并设置相应的选项
    case "$option" in
        --help)                          help=yes                   ;;
        --prefix=)                       NGX_PREFIX="!"             ;;
        --prefix=*)                      NGX_PREFIX="$value"        ;;
        --sbin-path=*)                   NGX_SBIN_PATH="$value"     ;;
        --conf-path=*)                   NGX_CONF_PATH="$value"     ;;
        --error-log-path=*)              NGX_ERROR_LOG_PATH="$value";;
        --pid-path=*)                    NGX_PID_PATH="$value"      ;;
        --lock-path=*)                   NGX_LOCK_PATH="$value"     ;;
        --user=*)                        NGX_USER="$value"          ;;
        --group=*)                       NGX_GROUP="$value"         ;;

        ...

        *)
            # 没有找到的对应选项
            echo "$0: error: invalid option \"$option\""
            exit 1
        ;;
    esac
done

## 3\. 对选项进行处理

# 如果有--help，则输出帮助信息
if [ $help = yes ]; then

    cat << END

        --help                             print this message

        --prefix=PATH                      set installation prefix
        --sbin-path=PATH                   set nginx binary pathname
        --conf-path=PATH                   set nginx.conf pathname
        --error-log-path=PATH              set error log pathname
        --pid-path=PATH                    set nginx.pid pathname
        --lock-path=PATH                   set nginx.lock pathname

        --user=USER                        set non-privileged user for
        worker processes
        --group=GROUP                      set non-privileged group for
                                 worker processes
END

    exit 1
fi

# 默认文件路径
NGX_CONF_PATH=${NGX_CONF_PATH:-conf/nginx.conf}
NGX_CONF_PREFIX=`dirname $NGX_CONF_PATH`
NGX_PID_PATH=${NGX_PID_PATH:-logs/nginx.pid}
NGX_LOCK_PATH=${NGX_LOCK_PATH:-logs/nginx.lock}

...

```

上面的代码中，我们选用了文件中的部分代码进行了说明。大家可结合源码再进行分析。auto/options 的目的主要是处理用户选项，并由选项生成一些全局变量的值，这些值在其它文件中会用到。该文件也会输出 configure 的帮助信息。

3.  auto/init

该文件的目录在于初始化一些临时文件的路径，检查 echo 的兼容性，并创建 Makefile。

```
# 生成最终执行编译的 makefile 文件路径
NGX_MAKEFILE=$NGX_OBJS/Makefile
# 动态生成 nginx 模块列表的路径，由于 nginx 的的一些模块是可以选择编译的，而且可以添加自己的模块，所以模块列表是动态生成的
NGX_MODULES_C=$NGX_OBJS/ngx_modules.c

NGX_AUTO_HEADERS_H=$NGX_OBJS/ngx_auto_headers.h
NGX_AUTO_CONFIG_H=$NGX_OBJS/ngx_auto_config.h

# 自动测试目录与日志输出文件
NGX_AUTOTEST=$NGX_OBJS/autotest
# 如果 configure 出错，可用来查找出错的原因
NGX_AUTOCONF_ERR=$NGX_OBJS/autoconf.err

NGX_ERR=$NGX_OBJS/autoconf.err
MAKEFILE=$NGX_OBJS/Makefile

NGX_PCH=
NGX_USE_PCH=

# 检查 echo 是否支持-n 或\c

# check the echo's "-n" option and "\c" capability

if echo "test\c" | grep c >/dev/null; then

    # 不支持-c 的方式，检查是否支持-n 的方式

    if echo -n test | grep n >/dev/null; then
        ngx_n=
        ngx_c=

    else
        ngx_n=-n
        ngx_c=
    fi

else
    ngx_n=
    ngx_c='\c'
fi

# 创建最初始的 makefile 文件
# default 表示目前编译对象
# clean 表示执行 clean 工作时，需要删除 makefile 文件以及 objs 目录
# 整个过程中只会生成 makefile 文件以及 objs 目录，其它所有临时文件都在 objs 目录之下，所以执行 clean 后，整个目录还原到初始状态
# 要再次执行编译，需要重新执行 configure 命令

# create Makefile

cat << END > Makefile

default:    build

clean:
    rm -rf Makefile $NGX_OBJS
END

```

4.  auto/sources

该文件从文件名中就可以看出，它的主要功能是跟源文件相关的。它的主要作用是定义不同功能或系统所需要的文件的变量。根据功能，分为 CORE/REGEX/EVENT/UNIX/FREEBSD/HTTP 等。每一个功能将会由四个变量组成，”_MODULES”表示此功能相关的模块，最终会输出到 ngx_modules.c 文件中，即动态生成需要编译到 nginx 中的模块；”INCS”表示此功能依赖的源码目录，查找头文件的时候会用到，在编译选项中，会出现在”-I”中；”DEPS”显示指明在 Makefile 中需要依赖的文件名，即编译时，需要检查这些文件的更新时间；”SRCS”表示需要此功能编译需要的源文件。

拿 core 来说：

```
CORE_MODULES="ngx_core_module ngx_errlog_module ngx_conf_module ngx_emp_server_module ngx_emp_server_core_module"

CORE_INCS="src/core"

CORE_DEPS="src/core/nginx.h \
 src/core/ngx_config.h \
 src/core/ngx_core.h \
 src/core/ngx_log.h \
 src/core/ngx_palloc.h \
 src/core/ngx_array.h \
 src/core/ngx_list.h \
 src/core/ngx_hash.h \
 src/core/ngx_buf.h \
 src/core/ngx_queue.h \
 src/core/ngx_string.h \
 src/core/ngx_parse.h \
 src/core/ngx_inet.h \
 src/core/ngx_file.h \
 src/core/ngx_crc.h \
 src/core/ngx_crc32.h \
 src/core/ngx_murmurhash.h \
 src/core/ngx_md5.h \
 src/core/ngx_sha1.h \
 src/core/ngx_rbtree.h \
 src/core/ngx_radix_tree.h \
 src/core/ngx_slab.h \
 src/core/ngx_times.h \
 src/core/ngx_shmtx.h \
 src/core/ngx_connection.h \
 src/core/ngx_cycle.h \
 src/core/ngx_conf_file.h \
 src/core/ngx_resolver.h \
 src/core/ngx_open_file_cache.h \
 src/core/nginx_emp_server.h \
 src/core/emp_server.h \
 src/core/task_thread.h \
 src/core/standard.h \
 src/core/dprint.h \
 src/core/ngx_crypt.h"

CORE_SRCS="src/core/nginx.c \
 src/core/ngx_log.c \
 src/core/ngx_palloc.c \
 src/core/ngx_array.c \
 src/core/ngx_list.c \
 src/core/ngx_hash.c \
 src/core/ngx_buf.c \
 src/core/ngx_queue.c \
 src/core/ngx_output_chain.c \
 src/core/ngx_string.c \
 src/core/ngx_parse.c \
 src/core/ngx_inet.c \
 src/core/ngx_file.c \
 src/core/ngx_crc32.c \
 src/core/ngx_murmurhash.c \
 src/core/ngx_md5.c \
 src/core/ngx_rbtree.c \
 src/core/ngx_radix_tree.c \
 src/core/ngx_slab.c \
 src/core/ngx_times.c \
 src/core/ngx_shmtx.c \
 src/core/ngx_connection.c \
 src/core/ngx_cycle.c \
 src/core/ngx_spinlock.c \
 src/core/ngx_cpuinfo.c \
 src/core/ngx_conf_file.c \
 src/core/ngx_resolver.c \
 src/core/ngx_open_file_cache.c \
 src/core/nginx_emp_server.c \
 src/core/emp_server.c \
 src/core/standard.c \
 src/core/task_thread.c \
 src/core/dprint.c \
 src/core/ngx_crypt.c"

```

如果我们自己写一个第三方模块，我们可能会引用到这些变量的值，或对这些变量进行修改，比如添加我们自己的模块，或添加自己的一个头文件查找目录(在第三方模块的 config 中)，在后面，我们会看到它是如何加框第三方模块的。 在继续分析执行流程之前，我们先介绍一些工具脚本。

5.  auto/have

```
cat << END >> $NGX_AUTO_CONFIG_H

#ifndef $have
#define $have  1
#endif

END

```

从代码中，我们可以看到，这个工具的作用是，将$have 变量的值，宏定义为 1，并输出到 auto_config 文件中。通常我们通过这个工具来控制是否打开某个特性。这个工具在使用前，需要先定义宏的名称 ，即$have 变量。

6.  再回到 configure 文件中来：

```
# NGX_DEBUG 是在 auto/options 文件中处理的，如果有--with-debug 选项，则其值是 YES
if [ $NGX_DEBUG = YES ]; then
    # 当有 debug 选项时，会定义 NGX_DEBUG 宏
    have=NGX_DEBUG . auto/have
fi

```

这段代码中，可以看出，configure 是如何定义一个特性的：通过宏定义，输出到 config 头文件中，然后在程序中可以判断这个宏是否有定义，来实现不同的特性。

configure 文件中继续向下：

```
# 编译器选项
. auto/cc/conf

# 头文件支持宏定义
if [ "$NGX_PLATFORM" != win32 ]; then
    . auto/headers
fi

# 操作系统相关的配置的检测
. auto/os/conf

# unix 体系下的通用配置检测
if [ "$NGX_PLATFORM" != win32 ]; then
    . auto/unix
fi

```

configure 会依次调用其它几个文件，来进行环境的检测，包括编译器、操作系统相关。

7.  auto/feature

nginx 的 configure 会自动检测不同平台的特性，神奇之处就是 auto/feature 的实现，在继续向下分析之前，我们先来看看这个工具的实现原理。此工具的核心思想是，输出一小段代表性 c 程序，然后设置好编译选项，再进行编译连接运行，再对结果进行分析。例如，如果想检测某个库是否存在，就在小段 c 程序里面调用库里面的某个函数，再进行编译链接，如果出错，则表示库的环境不正常，如果编译成功，且运行正常，则库的环境检测正常。我们在写 nginx 第三方模块时，也常使用此工具来进行环境的检测，所以，此工具的作用贯穿整个 configure 过程。

先看一小段使用例子：

```
ngx_feature="poll()"
ngx_feature_name=
ngx_feature_run=no
ngx_feature_incs="#include <poll.h>"
ngx_feature_path=
ngx_feature_libs=
ngx_feature_test="int  n; struct pollfd  pl;
                  pl.fd = 0;
                  pl.events = 0;
                  pl.revents = 0;
                  n = poll(&pl, 1, 0);
                  if (n == -1) return 1"
. auto/feature

if [ $ngx_found = no ]; then
    # 如果没有找到 poll，就设置变量的值
    EVENT_POLL=NONE
fi

```

这段代码在 auto/unix 里面实现，用来检测当前操作系统是否支持 poll 函数调用。在调用 auto/feature 之前，需要先设置几个输入参数变量的值，然后结果会存在$ngx_found 变量里面, 并输出宏定义以表示支持此特性:

```
$ngx_feature      特性名称
$ngx_feature_name 特性的宏定义名称，如果特性测试成功，则会定义该宏定义
$ngx_feature_path 编译时要查找头文件目录
$ngx_feature_test 要执行的测试代码
$ngx_feature_incs 在代码中要 include 的头文件
$ngx_feature_libs 编译时需要 link 的库文件选项
$ngx_feature_run  编译成功后，对二进制文件需要做的动作，可以是 yes value bug 其它

#ngx_found 如果找到，并测试成功，其值为 yes，否则其值为 no

```

看看 ngx_feature 的关键代码：

```
# 初始化输出结果为 no
ngx_found=no

#将特性名称小写转换成大写
if test -n "$ngx_feature_name"; then
    # 小写转大写
    ngx_have_feature=`echo $ngx_feature_name \
                   | tr abcdefghijklmnopqrstuvwxyz ABCDEFGHIJKLMNOPQRSTUVWXYZ`
fi

# 将所有 include 目录转换成编译选项
if test -n "$ngx_feature_path"; then
    for ngx_temp in $ngx_feature_path; do
        ngx_feature_inc_path="$ngx_feature_inc_path -I $ngx_temp"
    done
fi

# 生成临时的小段 c 程序代码。
# $ngx_feature_incs 变量是程序需要 include 的头文件
# $ngx_feature_test 是测试代码
cat << END > $NGX_AUTOTEST.c

#include <sys/types.h>
$NGX_INCLUDE_UNISTD_H
$ngx_feature_incs

int main() {
    $ngx_feature_test;
    return 0;
}

END

# 编译命令
# 编译之后的目标文件是 $NGX_AUTOTEST，后面会判断这个文件是否存在来判断是否编译成功
ngx_test="$CC $CC_TEST_FLAGS $CC_AUX_FLAGS $ngx_feature_inc_path \
      -o $NGX_AUTOTEST $NGX_AUTOTEST.c $NGX_TEST_LD_OPT $ngx_feature_libs"

# 执行编译过程
# 编译成功后，会生成$NGX_AUTOTEST 命名的文件
eval "/bin/sh -c \"$ngx_test\" >> $NGX_AUTOCONF_ERR 2>&1"

# 如果文件存在，则编译成功
if [ -x $NGX_AUTOTEST ]; then

    case "$ngx_feature_run" in

        # 需要运行来判断是否支持特性
        # 测试程序能否正常执行（即程序退出后的状态码是否是 0），如果正常退出，则特性测试成功，设置 ngx_found 为 yes，并添加名为 ngx_feature_name 的宏定义，宏的值为 1
        yes)
            # 如果程序正常退出，退出码为 0，则程序执行成功，我们可以在测试代码里面手动返回非 0 来表示程序出错
            # /bin/sh is used to intercept "Killed" or "Abort trap" messages
            if /bin/sh -c $NGX_AUTOTEST >> $NGX_AUTOCONF_ERR 2>&1; then
                echo " found"
                ngx_found=yes

                # 添加宏定义，宏的值为 1
                if test -n "$ngx_feature_name"; then
                    have=$ngx_have_feature . auto/have
                fi

            else
                echo " found but is not working"
            fi
        ;;

        # 需要运行程序来判断是否支持特性，如果支持，将程序标准输出的结果作为宏的值
        value)
        # /bin/sh is used to intercept "Killed" or "Abort trap" messages
        if /bin/sh -c $NGX_AUTOTEST >> $NGX_AUTOCONF_ERR 2>&1; then
            echo " found"
            ngx_found=yes

            # 与 yes 不一样的是，value 会将程序从标准输出里面打印出来的值，设置为 ngx_feature_name 宏变量的值
            # 在此种情况下，程序需要设置 ngx_feature_name 变量名
            cat << END >> $NGX_AUTO_CONFIG_H

#ifndef $ngx_feature_name
#define $ngx_feature_name  `$NGX_AUTOTEST`
#endif

END
            else
                echo " found but is not working"
            fi
        ;;

        # 与 yes 正好相反
        bug)
            # /bin/sh is used to intercept "Killed" or "Abort trap" messages
            if /bin/sh -c $NGX_AUTOTEST >> $NGX_AUTOCONF_ERR 2>&1; then
                echo " not found"

            else
                echo " found"
                ngx_found=yes

                if test -n "$ngx_feature_name"; then
                    have=$ngx_have_feature . auto/have
                fi
            fi
        ;;

        # 不需要运行程序，最后定义宏变量
        *)
            echo " found"
            ngx_found=yes

            if test -n "$ngx_feature_name"; then
                have=$ngx_have_feature . auto/have
            fi
        ;;

    esac
else
    # 编译失败
    echo " not found"

    # 编译失败，会保存信息到日志文件中
    echo "----------"    >> $NGX_AUTOCONF_ERR
    # 保留编译文件的内容
    cat $NGX_AUTOTEST.c  >> $NGX_AUTOCONF_ERR
    echo "----------"    >> $NGX_AUTOCONF_ERR
    # 保留编译文件的选项
    echo $ngx_test       >> $NGX_AUTOCONF_ERR
    echo "----------"    >> $NGX_AUTOCONF_ERR
fi

# 最后删除生成的临时文件
rm $NGX_AUTOTEST*

```

8.  auto/cc/conf

在了解了工具 auto/feature 后，继续我们的主流程，auto/cc/conf 的代码就很好理解了，这一步主要是检测编译器，并设置编译器相关的选项。它先调用 auto/cc/name 来得到编译器的名称，然后根据编译器选择执行不同的编译器相关的文件如 gcc 执行 auto/cc/gcc 来设置编译器相关的一些选项。

9.  auto/include

这个工具用来检测是头文件是否支持。需要检测的头文件放在$ngx_include 里面，如果支持，则$ngx_found 变量的值为 yes，并且会产生 NGX_HAVE_{ngx_include}的宏定义。

10.  auto/headers

生成头文件的宏定义。生成的定义放在 objs/ngx_auto_headers.h 里面：

```
#ifndef NGX_HAVE_UNISTD_H
#define NGX_HAVE_UNISTD_H  1
#endif

#ifndef NGX_HAVE_INTTYPES_H
#define NGX_HAVE_INTTYPES_H  1
#endif

#ifndef NGX_HAVE_LIMITS_H
#define NGX_HAVE_LIMITS_H  1
#endif

#ifndef NGX_HAVE_SYS_FILIO_H
#define NGX_HAVE_SYS_FILIO_H  1
#endif

#ifndef NGX_HAVE_SYS_PARAM_H
#define NGX_HAVE_SYS_PARAM_H  1
#endif

```

11.  auto/os/conf

针对不同的操作系统平台特性的检测，并针对不同的操作系统，设置不同的 CORE_INCS、CORE_DEPS、CORE_SRCS 变量。nginx 跨平台的支持就是在这个地方体现出来的。

12.  auto/unix

针对 unix 体系的通用配置或系统调用的检测，如 poll 等事件处理系统调用的检测等。

13.  回到 configure 里面

```
# 生成模块列表
. auto/modules
# 配置库的依赖
. auto/lib/conf

```

14.  auto/modules

该脚本根据不同的条件，输出不同的模块列表，最后输出的模块列表的文件在 objs/ngx_modules.c：

```
#include <ngx_config.h>
#include <ngx_core.h>

extern ngx_module_t  ngx_core_module;
extern ngx_module_t  ngx_errlog_module;
extern ngx_module_t  ngx_conf_module;
extern ngx_module_t  ngx_emp_server_module;

...

ngx_module_t *ngx_modules[] = {
    &ngx_core_module,
    &ngx_errlog_module,
    &ngx_conf_module,
    &ngx_emp_server_module,
    ...
    NULL
};

```

这个文件会决定所有模块的顺序，这会直接影响到最后的功能，下一小节我们将讨论模块间的顺序。这个文件会加载我们的第三方模块，这也是我们值得关注的地方：

```
if test -n "$NGX_ADDONS"; then

    echo configuring additional modules

    for ngx_addon_dir in $NGX_ADDONS
    do
        echo "adding module in $ngx_addon_dir"

        if test -f $ngx_addon_dir/config; then
            # 执行第三方模块的配置
            . $ngx_addon_dir/config

            echo " + $ngx_addon_name was configured"

        else
            echo "$0: error: no $ngx_addon_dir/config was found"
            exit 1
        fi
    done
fi

```

这段代码比较简单，确实现了 nginx 很强大的扩展性，加载第三方模块。$ngx_addon_dir 变量是在 configure 执行时，命令行参数–add-module 加入的，它是一个目录列表，每一个目录，表示一个第三方模块。从代码中，我们可以看到，它就是针对每一个第三方模块执行其目录下的 config 文件。于是我们可以在 config 文件里面执行我们自己的检测逻辑，比如检测库依赖，添加编译选项等。

15.  auto/lib/conf

该文件会针对 nginx 编译所需要的基础库的检测，比如 rewrite 模块需要的 PCRE 库的检测支持。

16.  configure 接下来定义一些宏常量，主要是是文件路径方面的：

```
case ".$NGX_PREFIX" in
    .)
        NGX_PREFIX=${NGX_PREFIX:-/usr/local/nginx}
        have=NGX_PREFIX value="\"$NGX_PREFIX/\"" . auto/define
    ;;

    .!)
        NGX_PREFIX=
    ;;

    *)
        have=NGX_PREFIX value="\"$NGX_PREFIX/\"" . auto/define
    ;;
esac

if [ ".$NGX_CONF_PREFIX" != "." ]; then
    have=NGX_CONF_PREFIX value="\"$NGX_CONF_PREFIX/\"" . auto/define
fi

have=NGX_SBIN_PATH value="\"$NGX_SBIN_PATH\"" . auto/define
have=NGX_CONF_PATH value="\"$NGX_CONF_PATH\"" . auto/define
have=NGX_PID_PATH value="\"$NGX_PID_PATH\"" . auto/define
have=NGX_LOCK_PATH value="\"$NGX_LOCK_PATH\"" . auto/define
have=NGX_ERROR_LOG_PATH value="\"$NGX_ERROR_LOG_PATH\"" . auto/define

have=NGX_HTTP_LOG_PATH value="\"$NGX_HTTP_LOG_PATH\"" . auto/define
have=NGX_HTTP_CLIENT_TEMP_PATH value="\"$NGX_HTTP_CLIENT_TEMP_PATH\""
. auto/define
have=NGX_HTTP_PROXY_TEMP_PATH value="\"$NGX_HTTP_PROXY_TEMP_PATH\""
. auto/define
have=NGX_HTTP_FASTCGI_TEMP_PATH value="\"$NGX_HTTP_FASTCGI_TEMP_PATH\""
. auto/define
have=NGX_HTTP_UWSGI_TEMP_PATH value="\"$NGX_HTTP_UWSGI_TEMP_PATH\""
. auto/define
have=NGX_HTTP_SCGI_TEMP_PATH value="\"$NGX_HTTP_SCGI_TEMP_PATH\""
. auto/define

```

17.  configure 最后的工作，生成编译安装的 makefile

```
# 生成 objs/makefile 文件
. auto/make

# 生成关于库的编译选项到 makefile 文件
. auto/lib/make
# 生成与安装相关的 makefile 文件内容，并生成最外层的 makefile 文件
. auto/install

# STUB
. auto/stubs

have=NGX_USER value="\"$NGX_USER\"" . auto/define
have=NGX_GROUP value="\"$NGX_GROUP\"" . auto/define

# 编译的最后阶段，汇总信息
. auto/summary

```

### 模块编译顺序

上一节中，提到过，nginx 模块的顺序很重要，会直接影响到程序的功能。而且，nginx 和部分模块，也有着自己特定的顺序要求，比如 ngx_http_write_filter_module 模块一定要在 filter 模块的最后一步执行。想查看模块的执行顺序，可以在 objs/ngx_modules.c 这个文件中找到，这个文件在 configure 之后生成，上一节中，我们看过这个文件里面的内容。

下面是一个 ngx_modules.c 文件的示例：

```
ngx_module_t *ngx_modules[] = {
    // 全局 core 模块
    &ngx_core_module,
    &ngx_errlog_module,
    &ngx_conf_module,
    &ngx_emp_server_module,
    &ngx_emp_server_core_module,

    // event 模块
    &ngx_events_module,
    &ngx_event_core_module,
    &ngx_kqueue_module,

    // 正则模块
    &ngx_regex_module,

    // http 模块
    &ngx_http_module,
    &ngx_http_core_module,
    &ngx_http_log_module,
    &ngx_http_upstream_module,

    // http handler 模块
    &ngx_http_static_module,
    &ngx_http_autoindex_module,
    &ngx_http_index_module,
    &ngx_http_auth_basic_module,
    &ngx_http_access_module,
    &ngx_http_limit_conn_module,
    &ngx_http_limit_req_module,
    &ngx_http_geo_module,
    &ngx_http_map_module,
    &ngx_http_split_clients_module,
    &ngx_http_referer_module,
    &ngx_http_rewrite_module,
    &ngx_http_proxy_module,
    &ngx_http_fastcgi_module,
    &ngx_http_uwsgi_module,
    &ngx_http_scgi_module,
    &ngx_http_memcached_module,
    &ngx_http_empty_gif_module,
    &ngx_http_browser_module,
    &ngx_http_upstream_ip_hash_module,
    &ngx_http_upstream_keepalive_module,
    //此处是第三方 handler 模块

    // http filter 模块
    &ngx_http_write_filter_module,
    &ngx_http_header_filter_module,
    &ngx_http_chunked_filter_module,
    &ngx_http_range_header_filter_module,
    &ngx_http_gzip_filter_module,
    &ngx_http_postpone_filter_module,
    &ngx_http_ssi_filter_module,
    &ngx_http_charset_filter_module,
    &ngx_http_userid_filter_module,
    &ngx_http_headers_filter_module,
    // 第三方 filter 模块
    &ngx_http_copy_filter_module,
    &ngx_http_range_body_filter_module,
    &ngx_http_not_modified_filter_module,
    NULL
};

```

http handler 模块与 http filter 模块的顺序很重要，这里我们主要关注一下这两类模块。

http handler 模块，在后面的章节里面会讲到多阶段请求的处理链。对于 content phase 之前的 handler，同一个阶段的 handler，模块是顺序执行的。比如上面的示例代码中，ngx_http_auth_basic_module 与 ngx_http_access_module 这两个模块都是在 access phase 阶段，由于 ngx_http_auth_basic_module 在前面，所以会先执行。由于 content phase 只会有一个执行，所以不存在顺序问题。另外，我们加载的第三方 handler 模块永远是在最后执行。

http filter 模块，filter 模块会将所有的 filter handler 排成一个倒序链，所以在最前面的最后执行。上面的例子中，&ngx_http_write_filter_module 最后执行，ngx_http_not_modified_filter_module 最先执行。注意，我们加载的第三方 filter 模块是在 copy_filter 模块之后，headers_filter 模块之前执行。

## nginx 的事件机制

### event 框架及非阻塞模型

### 定时器实现

### 信号处理

### 惊群问题

## nginx 的进程机制

### master 进程

### worker 进程

### 进程间通讯

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# nginx 基础设施

## 内存池

### 简介:

Nginx 里内存的使用大都十分有特色:申请了永久保存,抑或伴随着请求的结束而全部释放,还有写满了缓冲再从头接着写.这么做的原因也主要取决于 Web Server 的特殊的场景,内存的分配和请求相关,一条请求处理完毕,即可释放其相关的内存池,降低了开发中对内存资源管理的复杂度,也减少了内存碎片的存在.

所以在 Nginx 使用内存池时总是只申请,不释放,使用完毕后直接 destroy 整个内存池.我们来看下内存池相关的实现。

### 结构:

```
struct ngx_pool_s {
    ngx_pool_data_t       d;
    size_t                max;
    ngx_pool_t           *current;
    ngx_chain_t          *chain;
    ngx_pool_large_t     *large;
    ngx_pool_cleanup_t   *cleanup;
    ngx_log_t            *log;
};

struct ngx_pool_large_s {
    ngx_pool_large_t     *next;
    void                 *alloc;
};

typedef struct {
    u_char               *last;
    u_char               *end;
    ngx_pool_t           *next;
    ngx_uint_t            failed;
} ngx_pool_data_t;

```

![内存池](https://raw.github.com/yzprofile/nginx-book/master/source/images/chapter-10-1.PNG)

### 实现:

这三个数据结构构成了基本的内存池的主体.通过 ngx_create_pool 可以创建一个内存池,通过 ngx_palloc 可以从内存池中分配指定大小的内存。

```
ngx_pool_t *
ngx_create_pool(size_t size, ngx_log_t *log)
{
    ngx_pool_t  *p;

    p = ngx_memalign(NGX_POOL_ALIGNMENT, size, log);
    if (p == NULL) {
        return NULL;
    }

    p->d.last = (u_char *) p + sizeof(ngx_pool_t);
    p->d.end = (u_char *) p + size;
    p->d.next = NULL;
    p->d.failed = 0;

    size = size - sizeof(ngx_pool_t);
    p->max = (size < NGX_MAX_ALLOC_FROM_POOL) ? size : NGX_MAX_ALLOC_FROM_POOL;

    p->current = p;
    p->chain = NULL;
    p->large = NULL;
    p->cleanup = NULL;
    p->log = log;

    return p;
}

```

这里首申请了一块大小为 size 的内存区域，其前 sizeof(ngx_pool_t)字节用来存储 ngx_pool_t 这个结构体自身自身.所以若 size 小于 sizeof(ngx_pool_t)将会有 coredump 的可能性。

我们常用来分配内存的有三个接口:ngx_palloc，ngx_pnalloc，ngx_pcalloc。

分别来看下它们的实现：

```
 void *
 ngx_palloc(ngx_pool_t *pool, size_t size)
 {
     u_char      *m;
     ngx_pool_t  *p;

     if (size <= pool->max) {

         p = pool->current;

         do {
             m = ngx_align_ptr(p->d.last, NGX_ALIGNMENT);

             if ((size_t) (p->d.end - m) >= size) {
                 p->d.last = m + size;

                 return m;
             }

             p = p->d.next;

         } while (p);

         return ngx_palloc_block(pool, size);
     }

     return ngx_palloc_large(pool, size);
 }

 void *
 ngx_pnalloc(ngx_pool_t *pool, size_t size)
 {
     u_char      *m;
     ngx_pool_t  *p;

     if (size <= pool->max) {

         p = pool->current;

         do {
             m = p->d.last;

             if ((size_t) (p->d.end - m) >= size) {
                 p->d.last = m + size;

                 return m;
             }

             p = p->d.next;

         } while (p);

         return ngx_palloc_block(pool, size);
     }

     return ngx_palloc_large(pool, size);
 }

 void *
 ngx_pcalloc(ngx_pool_t *pool, size_t size)
 {
     void *p;

     p = ngx_palloc(pool, size);
     if (p) {
         ngx_memzero(p, size);
     }

     return p;
}

```

ngx_pcalloc 其只是 ngx_palloc 的一个封装，将申请到的内存全部初始化为 0。

ngx_palloc 相对 ngx_pnalloc，其会将申请的内存大小向上扩增到 NGX_ALIGNMENT 的倍数，以方便内存对齐，减少内存访问次数。

Nginx 的内存池不仅用于内存方面的管理，还可以通过`ngx_pool_cleanup_add`来添加内存池释放时的回调函数，以便用来释放自己申请的其他相关资源。

从代码中可以看出，这些由自己添加的释放回调是以链表形式保存的，也就是说你可以添加多个回调函数来管理不同的资源。

## 共享内存

### slab 算法

## buffer 管理

### buffer 重用机制

### buffer 防拷贝机制

## chain 管理

### chain 重用机制

## aio 原理

## 锁实现

## 基本数据结构

## 时间缓存

## 文件缓存

## log 机制

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# nginx 的启动阶段 (30%)

## 概述 (100%)

nginx 启动阶段指从 nginx 初始化直至准备好按最新配置提供服务的过程。

在不考虑 nginx 单进程工作的情况下，这个过程包含三种方式：

1.  启动新的 nginx
2.  reload 配置
3.  热替换 nginx 代码

三种方式有共同的流程，下面这幅图向我们展现了这个流程：

图 11-1

流程的开端是解析 nginx 配置、初始化模块，接着是初始化文件句柄，初始化共享内存，然后是监听端口，再后来创建 worker 子进程和其他辅助子进程，最后是 worker 初始化事件机制。以上步骤结束以后，nginx 各个子进程开始各司其职，比如 worker 进程开始 accept 请求并按最新配置处理请求，cache-manager 进程开始管理 cache 文件目录等等。

除了这些共同流程，这三种方式的差异也非常明显。第一种方式包含命令行解析的过程，同时输出有一段时间是输出到控制台。reload 配置有两种形式，一种是使用 nginx 命令行，一种是向 master 进程发送 HUP 信号，前者表面上与第一种方式无异，但实际上差别很大，后者则完全不支持控制台输出，无法直接查看 nginx 的启动情况。而且 reload 配置时，nginx 需要自动停止以往生成的子进程，所以还包含复杂的进程管理操作，这一点在启动新的 nginx 的方式中是不存在的。热替换 nginx 代码虽然使用上与 reload 配置的后一种形式相似，但在解析 nginx 配置方面，与 reload 配置的方式差距非常大。另外，热替换 nginx 代码时，对以往创建的子进程管理也不像 reload 配置那样，需要手工触发进行。所以，我们想弄懂 nginx 的启动阶段，就必须理解所有这三种方式下 nginx 都是如何工作的。

## 共有流程 (100%)

从概述中我们了解到，nginx 启动分为三种方式，虽然各有不同，但也有一段相同的流程。在这一节中，我们对 nginx 启动阶段的共用流程进行讨论。

共有流程的代码主要集中在 ngx_cycle.c、ngx_process.c、ngx_process_cycle.c 和 ngx_event.c 这四个文件中。我们这一节只讨论 nginx 的框架代码，而与 http 相关的模块代码，我们会在后面进行分析。

共有流程开始于解析 nginx 配置，这个过程集中在 ngx_init_cycle 函数中。ngx_init_cycle 是 nginx 的一个核心函数，共有流程中与配置相关的几个过程都在这个函数中实现，其中包括解析 nginx 配置、初始化 CORE 模块，接着是初始化文件句柄，初始化错误日志，初始化共享内存，然后是监听端口。可以说共有流程 80%都是现在 ngx_init_cycle 函数中。

在具体介绍以前，我们先解决一个概念问题——什么叫 cycle？

cycle 就是周期的意思，对应着一次启动过程。也就是说，不论发生了上节介绍的三种启动方式的哪一种，nginx 都会创建一个新的 cycle 与这次启动对应。

### 配置解析接口 (100%)

ngx_init_cycle 提供的是配置解析接口。接口是一个切入点，通过少量代码提供一个完整功能的调用。配置解析接口分为两个阶段，一个是准备阶段，另一个就是真正开始调用配置解析。准备阶段指什么呢？主要是准备三点：

1.  准备内存

nginx 根据以往的经验（old_cycle）预测这一次的配置需要分配多少内存。比如，我们可以看这段：

```
if (old_cycle->shared_memory.part.nelts) {
    n = old_cycle->shared_memory.part.nelts;
    for (part = old_cycle->shared_memory.part.next; part; part = part->next)
    {
        n += part->nelts;
    }

} else {
    n = 1;
}

if (ngx_list_init(&cycle->shared_memory, pool, n, sizeof(ngx_shm_zone_t))
    != NGX_OK)
{
    ngx_destroy_pool(pool);
    return NULL;
}

```

这段代码的意思是遍历 old_cycle，统计上一次系统中分配了多少块共享内存，接着就按这个数据初始化当前 cycle 中共享内存的规模。

2.  准备错误日志

nginx 启动可能出错，出错就要记录到错误日志中。而错误日志本身也是配置的一部分，所以不解析完配置，nginx 就不能了解错误日志的信息。nginx 通过使用上一个周期的错误日志来记录解析配置时发生的错误，而在配置解析完成以后，nginx 就用新的错误日志替换旧的错误日志。具体代码摘抄如下，以说明 nginx 解析配置时使用 old_cycle 的错误日志：

```
log = old_cycle->log;
pool->log = log;
cycle->log = log;

```

3.  准备数据结构

主要是两个数据结果，一个是 ngx_cycle_t 结构，一个是 ngx_conf_t 结构。前者用于存放所有 CORE 模块的配置，后者则是用于存放解析配置的上下文信息。具体代码如下：

```
for (i = 0; ngx_modules[i]; i++) {
    if (ngx_modules[i]->type != NGX_CORE_MODULE) {
        continue;
    }

    module = ngx_modules[i]->ctx;

    if (module->create_conf) {
        rv = module->create_conf(cycle);
        if (rv == NULL) {
            ngx_destroy_pool(pool);
            return NULL;
        }
        cycle->conf_ctx[ngx_modules[i]->index] = rv;
    }
}

conf.ctx = cycle->conf_ctx;
conf.cycle = cycle;
conf.pool = pool;
conf.log = log;
conf.module_type = NGX_CORE_MODULE;
conf.cmd_type = NGX_MAIN_CONF;

```

准备好了这些内容，nginx 开始调用配置解析模块，其代码如下：

```
if (ngx_conf_param(&conf) != NGX_CONF_OK) {
    environ = senv;
    ngx_destroy_cycle_pools(&conf);
    return NULL;
}

if (ngx_conf_parse(&conf, &cycle->conf_file) != NGX_CONF_OK) {
    environ = senv;
    ngx_destroy_cycle_pools(&conf);
    return NULL;
}

```

第一个 if 解析 nginx 命令行参数’-g’加入的配置。第二个 if 解析 nginx 配置文件。好的设计就体现在接口极度简化，模块之间的耦合非常低。这里只使用区区 10 行完成了配置的解析。在这里，我们先浅尝辄止，具体 nginx 如何解析配置，我们将在后面的小节做细致的介绍。

## 配置解析

### 通用过程 (100%)

配置解析模块在 ngx_conf_file.c 中实现。模块提供的接口函数主要是 ngx_conf_parse，另外，模块提供一个单独的接口 ngx_conf_param，用来解析命令行传递的配置，当然，这个接口也是对 ngx_conf_parse 的包装。

ngx_conf_parse 函数支持三种不同的解析环境：

1.  parse_file：解析配置文件；
2.  parse_block：解析块配置。块配置一定是由“{”和“}”包裹起来的；
3.  parse_param：解析命令行配置。命令行配置中不支持块指令。

我们先来鸟瞰 nginx 解析配置的流程，整个过程可参见下面示意图：

图 11-2

这是一个递归的过程。nginx 首先解析 core 模块的配置。core 模块提供一些块指令，这些指令引入其他类型的模块，nginx 遇到这些指令，就重新迭代解析过程，解析其他模块的配置。这些模块配置中又有一些块指令引入新的模块类型或者指令类型，nginx 就会再次迭代，解析这些新的配置类型。比如上图，nginx 遇到“events”指令，就重新调用 ngx_conf_parse()解析 event 模块配置，解析完以后 ngx_conf_parse()返回，nginx 继续解析 core 模块指令，直到遇到“http”指令。nginx 再次调用 ngx_conf_parse()解析 http 模块配置的 http 级指令，当遇到“server”指令时，nginx 又一次调用 ngx_conf_parse()解析 http 模块配置的 server 级指令。

了解了 nginx 解析配置的流程，我们来看其中的关键函数 ngx_conf_parse()。

ngx_conf_parse()解析配置分成两个主要阶段，一个是词法分析，一个是指令解析。

词法分析通过 ngx_conf_read_token()函数完成。指令解析有两种方式，其一是使用 nginx 内建的指令解析机制，其二是使用第三方自定义指令解析机制。自定义指令解析可以参见下面的代码：

```
if (cf->handler) {
    rv = (*cf->handler)(cf, NULL, cf->handler_conf);
    if (rv == NGX_CONF_OK) {
        continue;
    }

    if (rv == NGX_CONF_ERROR) {
        goto failed;
    }

    ngx_conf_log_error(NGX_LOG_EMERG, cf, 0, rv);

    goto failed;
}

```

这里注意 cf->handler 和 cf->handler_conf 两个属性，其中 handler 是自定义解析函数指针，handler_conf 是 conf 指针。

下面着重介绍 nginx 内建的指令解析机制。本机制分为 4 个步骤：

1.  只有处理的模块的类型是 NGX_CONF_MODULE 或者是当前正在处理的模块类型，才可能被执行。nginx 中有一种模块类型是 NGX_CONF_MODULE，当前只有 ngx_conf_module 一种，只支持一条指令“include”。“include”指令的实现我们后面再进行介绍。

```
ngx_modules[i]->type != NGX_CONF_MODULE && ngx_modules[i]->type != cf->module_type

```

2.  匹配指令名，判断指令用法是否正确。

1.  指令的 Context 必须当前解析 Context 相符；

```
!(cmd->type & cf->cmd_type)

```

2.  非块指令必须以“;”结尾；

```
!(cmd->type & NGX_CONF_BLOCK) && last != NGX_OK

```

3.  块指令必须后接“{”；

```
(cmd->type & NGX_CONF_BLOCK) && last != NGX_CONF_BLOCK_START

```

4.  指令参数个数必须正确。注意指令参数有最大值 NGX_CONF_MAX_ARGS，目前值为 8。

```
if (!(cmd->type & NGX_CONF_ANY)) {

    if (cmd->type & NGX_CONF_FLAG) {

        if (cf->args->nelts != 2) {
            goto invalid;
        }

    } else if (cmd->type & NGX_CONF_1MORE) {

        if (cf->args->nelts < 2) {
            goto invalid;
        }

    } else if (cmd->type & NGX_CONF_2MORE) {

        if (cf->args->nelts < 3) {
            goto invalid;
        }

    } else if (cf->args->nelts > NGX_CONF_MAX_ARGS) {

        goto invalid;

    } else if (!(cmd->type & argument_number[cf->args->nelts - 1])) {
        goto invalid;
    }
}

```

3.  取得指令工作的 conf 指针。

```
if (cmd->type & NGX_DIRECT_CONF) {
    conf = ((void **) cf->ctx)[ngx_modules[i]->index];

} else if (cmd->type & NGX_MAIN_CONF) {
    conf = &(((void **) cf->ctx)[ngx_modules[i]->index]);

} else if (cf->ctx) {
    confp = *(void **) ((char *) cf->ctx + cmd->conf);

    if (confp) {
        conf = confp[ngx_modules[i]->ctx_index];
    }
}

```

1.  NGX_DIRECT_CONF 常量单纯用来指定配置存储区的寻址方法，只用于 core 模块。
2.  NGX_MAIN_CONF 常量有两重含义，其一是指定指令的使用上下文是 main（其实还是指 core 模块），其二是指定配置存储区的寻址方法。所以，在代码中常常可以见到使用上下文是 main 的指令的 cmd->type 属性定义如下：

```
NGX_MAIN_CONF|NGX_DIRECT_CONF|...

```

表示指令使用上下文是 main，conf 寻址方式是直接寻址。

使用 NGX_MAIN_CONF 还表示指定配置存储区的寻址方法的指令有 4 个：“events”、“http”、“mail”、“imap”。这四个指令也有共同之处——都是使用上下文是 main 的块指令，并且块中的指令都使用其他类型的模块（分别是 event 模块、http 模块、mail 模块和 mail 模块）来处理。

```
NGX_MAIN_CONF|NGX_CONF_BLOCK|...

```

后面分析 ngx_http_block()函数时，再具体分析为什么需要 NGX_MAIN_CONF 这种配置寻址方式。

3.  除开 core 模块，其他类型的模块都会使用第三种配置寻址方式，也就是根据 cmd->conf 的值从 cf->ctx 中取出对应的配置。举 http 模块为例，cf->conf 的可选值是 NGX_HTTP_MAIN_CONF_OFFSET、NGX_HTTP_SRV_CONF_OFFSET、NGX_HTTP_LOC_CONF_OFFSET，分别对应“http{}”、“server{}”、“location{}”这三个 http 配置级别。

4.  执行指令解析回调函数

```
rv = cmd->set(cf, cmd, conf);

```

cmd 是词法分析得到的结果，conf 是上一步得到的配置存贮区地址。

### http 的解析

http 是作为一个 core 模块被 nginx 通用解析过程解析的，其核心就是“http”块指令回调，它完成了 http 解析的整个功能，从初始化到计算配置结果。

因为这是本书第一次提到块指令，所以在这里对其做基本介绍。

块指令的流程是：

1.  创建并初始化上下文环境；
2.  调用通用解析流程解析；
3.  根据解析结果进行后续合并处理；
4.  善后工作。

下面我们以“http”指令为例来介绍这个流程：

#### 创建并初始化上下文环境

```
ctx = ngx_pcalloc(cf->pool, sizeof(ngx_http_conf_ctx_t));

*(ngx_http_conf_ctx_t **) conf = ctx;

...

ctx->main_conf = ngx_pcalloc(cf->pool,
                             sizeof(void *) * ngx_http_max_module);

ctx->srv_conf = ngx_pcalloc(cf->pool, sizeof(void *) * ngx_http_max_module);

ctx->loc_conf = ngx_pcalloc(cf->pool, sizeof(void *) * ngx_http_max_module);

for (m = 0; ngx_modules[m]; m++) {
    if (ngx_modules[m]->type != NGX_HTTP_MODULE) {
        continue;
    }

    module = ngx_modules[m]->ctx;
    mi = ngx_modules[m]->ctx_index;

    if (module->create_main_conf) {
        ctx->main_conf[mi] = module->create_main_conf(cf);
    }

    if (module->create_srv_conf) {
        ctx->srv_conf[mi] = module->create_srv_conf(cf);
    }

    if (module->create_loc_conf) {
        ctx->loc_conf[mi] = module->create_loc_conf(cf);
    }
}

pcf = *cf;
cf->ctx = ctx;

for (m = 0; ngx_modules[m]; m++) {
    if (ngx_modules[m]->type != NGX_HTTP_MODULE) {
        continue;
    }

    module = ngx_modules[m]->ctx;

    if (module->preconfiguration) {
        if (module->preconfiguration(cf) != NGX_OK) {
            return NGX_CONF_ERROR;
        }
    }
}

```

http 模块的上下文环境 ctx（注意我们在通用解析流程中提到的 ctx 是同一个东西）非常复杂，它是由三个指针数组组成的：main_conf、srv_conf、loc_conf。根据上面的代码可以看到，这三个数组的元素个数等于系统中 http 模块的个数。想想我们平时三四十个 http 模块的规模，大家也应该可以理解这一块结构的庞大。nginx 还为每个模块分别执行对应的 create 函数分配空间。我们需要注意后面的这一句“cf->ctx = ctx;”，正是这一句将解析配置的上下文切换成刚刚建立的 ctx。最后一段代码通过调用各个 http 模块的 preconfiguration 回调函数完成了对应模块的预处理操作，其主要工作是创建模块用到的变量。

#### 调用通用解析流程解析

```
cf->module_type = NGX_HTTP_MODULE;
cf->cmd_type = NGX_HTTP_MAIN_CONF;
rv = ngx_conf_parse(cf, NULL);

```

基本上所有的块指令都类似上面的三行语句（例外是 map，它用的是 cf->handler），改变通用解析流程的工作状态，然后调用通用解析流程。

#### 根据解析结果进行后续合并处理

```
for (m = 0; ngx_modules[m]; m++) {
    if (module->init_main_conf) {
        rv = module->init_main_conf(cf, ctx->main_conf[mi]);
    }

    rv = ngx_http_merge_servers(cf, cmcf, module, mi);
}

for (s = 0; s < cmcf->servers.nelts; s++) {

    if (ngx_http_init_locations(cf, cscfp[s], clcf) != NGX_OK) {
        return NGX_CONF_ERROR;
    }

    if (ngx_http_init_static_location_trees(cf, clcf) != NGX_OK) {
        return NGX_CONF_ERROR;
    }
}

if (ngx_http_init_phases(cf, cmcf) != NGX_OK) {
    return NGX_CONF_ERROR;
}

if (ngx_http_init_headers_in_hash(cf, cmcf) != NGX_OK) {
    return NGX_CONF_ERROR;
}

for (m = 0; ngx_modules[m]; m++) {
    if (module->postconfiguration) {
        if (module->postconfiguration(cf) != NGX_OK) {
            return NGX_CONF_ERROR;
        }
    }
}

if (ngx_http_variables_init_vars(cf) != NGX_OK) {
    return NGX_CONF_ERROR;
}

if (ngx_http_init_phase_handlers(cf, cmcf) != NGX_OK) {
    return NGX_CONF_ERROR;
}

if (ngx_http_optimize_servers(cf, cmcf, cmcf->ports) != NGX_OK) {
    return NGX_CONF_ERROR;
}

```

以上是 http 配置处理最重要的步骤。首先，在这里调用了各个模块的 postconfiguration 回调函数完成了模块配置过程。更重要的是，它为 nginx 建立了一棵完整的配置树（叶子节点为 location，包含 location 的完整配置）、完整的 location 搜索树、一张变量表、一张完成的阶段处理回调表(phase handler)、一张 server 对照表和一张端口监听表。下面我们将分别介绍这些配置表的生成过程。

##### location 配置树

介绍这部分以前，先说明一个 nginx 的公理

公理 11-1：所有存放参数为 NGX_HTTP_SRV_CONF_OFFSET 的配置，配置仅在请求匹配的虚拟主机(server)上下文中生效，而所有存放参数为 NGX_HTTP_LOC_CONF_OFFSET 的配置，配置仅在请求匹配的路径(location)上下文中生效。

正因为有公理 11-1，所以 nginx 需要调用 merge_XXX 回调函数合并配置。具体的原因是很多配置指令可以放在不同配置层级，比如 access_log 既可以在 http 块中配置，又可以在 server 块中配置，还可以在 location 块中配置。 但是因为公理 11-1，access_log 指令配置只有在路径(location)上下文中生效，所以需要将在 http 块中配置的 access_log 指令的配置向路径上下文做两次传递，第一次从 HTTP(http)上下文到虚拟主机(server)上下文，第二次从虚拟主机上下文到路径上下文。

可能有人会疑惑，为什么需要传递和合并呢？难道它们不在一张表里么？对，在创建并初始化上下文环境的过程中，大家已经看到，nginx 为 HTTP 上下文创建了 main_conf，为虚拟主机上下文创建了 srv_conf，为路径上下文创建了 loc_conf。但是，这张表只是用于解析在 http 块但不包含 server 块中定义的指令。而后面我们会看到，在 server 块指令中，同样建立了 srv_conf 和 loc_conf，用于解析在 server 块但不含 location 块中定义的指令。所以 nginx 其实维护了很多张配置表，因此 nginx 必须将配置在这些表中从顶至下不断传递。

前面列出的

```
for (m = 0; ngx_modules[m]; m++) {
    if (module->init_main_conf) {
        rv = module->init_main_conf(cf, ctx->main_conf[mi]);
    }

    rv = ngx_http_merge_servers(cf, cmcf, module, mi);
}

```

就是初始化 HTTP 上下文，并且完成两步配置合并操作：从 HTTP 上下文合并到虚拟主机上下文，以及从虚拟主机上下文合并到路径上下文。其中，合并到路径上下问的操作是在 ngx_http_merge_servers 函数中进行的，见

```
if (module->merge_loc_conf) {

    /* merge the server{}'s loc_conf */

    /* merge the locations{}' loc_conf's */

}

```

大家注意观察 ngx_http_merge_servers 函数中的这段，先将 HTTP 上下文中的 location 配置合并到虚拟主机上下文，再将虚拟主机上下文中的 location 配置合并到路径上下文。

##### location 搜索树

公理 11-2：nginx 搜索路径时，正则匹配路径和其他的路径分开搜。

公理 11-3：nginx 路径可以嵌套。

所以，nginx 存放 location 的有两个指针，分别是

```
struct ngx_http_core_loc_conf_s {

    ...

    ngx_http_location_tree_node_t   *static_locations;
#if (NGX_PCRE)
    ngx_http_core_loc_conf_t       **regex_locations;
#endif

    ...
}

```

通过这段代码，大家还可以发现一点——nginx 的正则表达式需要 PCRE 支持。

正则表达式的路径是个指针数组，指针类型就是 ngx_http_core_loc_conf_t，所以数据结构决定算法，正则表达式路径的添加非常简单，就是在表中插入一项，这里不做介绍。

而其他路径，保存在 ngx_http_location_tree_node_t 指针指向的搜索树 static_locations，则是变态复杂，可以看得各位大汗淋漓。

为了说明这棵树的构建，我们先了解其他路径包含哪些：

1.  普通前端匹配的路径，例如 location / {}
2.  抢占式前缀匹配的路径，例如 location ^~ / {}
3.  精确匹配的路径，例如 location = / {}
4.  命名路径，比如 location @a {}
5.  无名路径，比如 if {}或者 limit_except {}生成的路径

我们再来看 ngx_http_core_loc_conf_t 中如何体现这些路径：

| 普通前端匹配的路径 | 无 |
| 抢占式前缀匹配的路径 | noregex = 1 |
| 精确匹配的路径 | exact_match = 1 |
| 命名路径 | named = 1 |
| 无名路径 | noname = 1 |
| 正则路径 | regex != NULL |

有了这些基础知识，可以看代码了。首先是 ngx_http_init_locations 函数

```
ngx_queue_sort(locations, ngx_http_cmp_locations);

for (q = ngx_queue_head(locations);
     q != ngx_queue_sentinel(locations);
     q = ngx_queue_next(q))
{
    clcf = lq->exact ? lq->exact : lq->inclusive;

    if (ngx_http_init_locations(cf, NULL, clcf) != NGX_OK) {
        return NGX_ERROR;
    }

    if (clcf->regex) {
        r++;

        if (regex == NULL) {
            regex = q;
        }

        continue;
    }

    if (clcf->named) {
        n++;

        if (named == NULL) {
            named = q;
        }

        continue;
    }

    if (clcf->noname) {
        break;
    }
}

if (q != ngx_queue_sentinel(locations)) {
    ngx_queue_split(locations, q, &tail);
}

if (named) {
    ...
    cscf->named_locations = clcfp;
    ...
}

if (regex) {
    ...
    pclcf->regex_locations = clcfp;
    ...
}

```

大家可以看到，这个函数正是根据不同的路径类型将 locations 分成多段，并以不同的指针引用。首先注意开始的排序，根据 ngx_http_cmp_locations 比较各个 location，排序以后的顺序依次是

1.  精确匹配的路径和两类前缀匹配的路径(字母序，如果某个精确匹配的路径的名字和前缀匹配的路径相同，精确匹配的路径排在前面)
2.  正则路径(出现序)
3.  命名路径(字母序)
4.  无名路径(出现序)

这样 nginx 可以简单的截断列表得到不同类型的路径，nginx 也正是这样处理的。

另外还要注意一点，就是 ngx_http_init_locations 的迭代调用，这里的 clcf 引用了两个我们没有介绍过的字段 exact 和 inclusive。这两个字段最初是在 ngx_http_add_location 函数（添加 location 配置时必然调用）中设置的：

```
    if (clcf->exact_match
#if (NGX_PCRE)
        || clcf->regex
#endif
        || clcf->named || clcf->noname)
    {
        lq->exact = clcf;
        lq->inclusive = NULL;

    } else {
        lq->exact = NULL;
        lq->inclusive = clcf;
    }

```

当然这部分的具体逻辑我们在介绍 location 解析是再具体说明。

接着我们看 ngx_http_init_static_location_trees 函数。通过刚才的 ngx_http_init_locations 函数，留在 locations 数组里面的还有哪些类型的路径呢？

还有普通前端匹配的路径、抢占式前缀匹配的路径和精确匹配的路径这三类。

```
if (ngx_http_join_exact_locations(cf, locations) != NGX_OK) {
    return NGX_ERROR;
}

ngx_http_create_locations_list(locations, ngx_queue_head(locations));

pclcf->static_locations = ngx_http_create_locations_tree(cf, locations, 0);
if (pclcf->static_locations == NULL) {
    return NGX_ERROR;
}

```

请注意除开这段核心代码，这个函数也有一个自迭代过程。

ngx_http_join_exact_locations 函数是将名字相同的精确匹配的路径和两类前缀匹配的路径合并，合并方法

```
lq->inclusive = lx->inclusive;

ngx_queue_remove(x);

```

简言之，就是将前缀匹配的路径放入精确匹配的路径的 inclusive 指针中，然后从列表删除前缀匹配的路径。

ngx_http_create_locations_list 函数将和某个路径名拥有相同名称前缀的路径添加到此路径节点的 list 指针域下，并将这些路径从 locations 中摘除。其核心代码是

```
ngx_queue_split(&lq->list, x, &tail);
ngx_queue_add(locations, &tail);

ngx_http_create_locations_list(&lq->list, ngx_queue_head(&lq->list));

ngx_http_create_locations_list(locations, x);

```

ngx_http_create_locations_tree 函数则将刚才划分的各个 list 继续细分，形成一个二分搜索树，每个中间节点代表一个 location，每个 location 有如下字段：

1.  exact：两类前缀匹配路径的 inclusive 指针域指向这两类路径的配置上下文；
2.  inclusive：精确匹配路径的 exact 指针域指向这些路径的配置上下文；
3.  auto_redirect：为各种 upstream 模块，比如 proxy、fastcgi 等等开启自动 URI 填充的功能；
4.  len：路径前缀的长度。任何相同前缀的路径的 len 等于该路径名长度减去公共前缀的长度。比如路径/a 和/ab，前者的 len 为 2，后者的 len 也为 1；
5.  name：路径前缀，任何相同前缀的路径的 name 是其已于公共前缀的部分。仍举路径/a 和/ab 为例，前者的 name 为/a，后者的 name 为 b；
6.  left：左子树，当然是长度短或者字母序小的不同前缀的路径；
7.  right：右子树，当然是长度长或者字母序大的不同前缀的路径。

通过上面三个步骤，nginx 就将 locations 列表中各种类型的路径分类处理并由不同的指针引用。对于前缀路径和精确匹配的路径，形成一棵独特的二分前缀树。

##### 变量表

变量表的处理相对简单，即对照变量名表，为变量表中的每一个元素设置对应的 get_handler 和 data 字段。在前面的章节大家已经知道，变量表 variables 用以处理索引变量，而变量名表 variables_keys 用于处理可按变量名查找的变量。对于通过 ngx_http_get_variable_index 函数创建的索引变量，在变量表 variables 中的 get_handler 初始为空，如果没有认为设置的话，将会在这里进行初始化。

特殊变量的 get_handler 初始化也在这里进行：

| 变量前缀 | get_handler | 标志 |
| http | ngx_http_variable_unknown_header_in |   |
| sent_http | ngx_http_variable_unknown_header_out |   |
| upstream_http | ngx_http_upstream_header_variable | NGX_HTTP_VAR_NOCACHEABLE |
| cookie | ngx_http_variable_cookie |   |
| arg | ngx_http_variable_argument | NGX_HTTP_VAR_NOCACHEABLE |

##### 阶段处理回调表

按照下表顺序将各个模块设置的 phase handler 依次加入 cmcf->phase_engine.handlers 列表，各个 phase 的 phase handler 的 checker 不同。checker 主要用于限定某个 phase 的框架逻辑，包括处理返回值。

| 处理阶段 PHASE | checker | 可自定义 handler |
| NGX_HTTP_POST_READ_PHASE | ngx_http_core_generic_phase | 是 |
| NGX_HTTP_SERVER_REWRITE_PHASE | ngx_http_core_rewrite_phase | 是 |
| NGX_HTTP_FIND_CONFIG_PHASE | ngx_http_core_find_config_phase | 否 |
| NGX_HTTP_REWRITE_PHASE | ngx_http_core_rewrite_phase | 是 |
| NGX_HTTP_POST_REWRITE_PHASE | ngx_http_core_post_rewrite_phase | 否 |
| NGX_HTTP_PREACCESS_PHASE | ngx_http_core_generic_phase | 是 |
| NGX_HTTP_ACCESS_PHASE | ngx_http_core_access_phase | 是 |
| NGX_HTTP_POST_ACCESS_PHASE | ngx_http_core_post_access_phase | 否 |
| NGX_HTTP_TRY_FILES_PHASE | ngx_http_core_try_files_phase | 否 |
| NGX_HTTP_CONTENT_PHASE | ngx_http_core_content_phase | 是 |

注意相同 PHASE 的 phase handler 是按模块顺序的反序加入回调表的。另外在 NGX_HTTP_POST_REWRITE_PHASE 中，ph->next 指向 NGX_HTTP_FIND_CONFIG_PHASE 第一个 phase handler，以实现 rewrite last 逻辑。

##### server 对照表

大家如果读过 nginx 的“Server names”这篇官方文档，会了解 nginx 对于 server name 的处理分为 4 中情况：精确匹配、前缀通配符匹配、后缀通配符匹配和正则匹配。那么，下面是又一个公理，

公理 11-4：nginx 对于不同类型的 server name 分别处理。

所以，所谓 server 对照表，其实是四张表，分别对应四种类型的 server。数据结构决定算法，四张表决定了 nginx 必须建立这四张表的行为。鉴于前三种类型和正则匹配可以分成两大类，nginx 使用两套策略生成 server 对照表——对正则匹配的虚拟主机名，nginx 为其建立一个数组，按照主机名在配置文件的出现顺序依次写入数组；而对于其他虚拟主机名，nginx 根据它们的类型为它们分别存放在三张 hash 表中。三张 hash 表的结构完全相同，但对于前缀通配或者后缀通配这两种类型的主机名，nginx 对通配符进行的预处理不同。其中“.taobao.com”这种特殊的前缀通配与普通的前缀通配处理又有不同。我们现在来介绍这些不同。

处理前缀通配是将字符串按节翻转，然后去掉通配符。举个例子，“*.example.com”会被转换成“com.example.\0”，而特殊的前缀通配“.example.com”会被转换成“com.example\0”。

处理后缀通配更简单，直接去掉通配符。也举个例子，“www.example.*”会被转换成“www.example\0”。

##### 端口监听表

对于所有写在 server 配置中的 listen 指令，nginx 开始会建立一张 server 和端口的对照索引表。虽然这不是本节的要点，但要说明索引表到监听表的转换过程，还是需要描述其结构。如图 11-3 所示，这张索引表是二级索引，第一级索引以 listen 指定的端口为键，第二级索引以 listen 指定的地址为键，索引的对象就是 server 上下文数据结构。而端口监视表是两张表，其结构如图 11-4 所示。 索引表和监听表在结构上非常类似，但是却有一个非常明显的不同。索引表中第一张表的各表项的端口是唯一的，而监听表的第一张表中的不同表项的端口却可能是相同的。之所以出现这样的差别，是因为 nginx 会为监听表第一张表中的每一项分别建立监听套接字，而在索引表中，如果配置显式定义了需要监听不同 IP 地址的相同端口，它在索引表中会放在同一个端口的二级索引中，而在监听表中必须存放为两个端口相同的不同监听表项。

说明了两张表的结构，现在可以介绍转换过程：

第一步，在 ngx_http_optimize_servers()函数中，对索引表一级索引中的所有 port 下辖的二级索引分别进行排序。排序的规则是

1.  含 wildcard 属性的二级索引最终会尽可能排到尾部。这些二级索引类似于

```
listen *:80;
listen 80;

```

2.  含 bind 属性的二级索引最终会尽可能排到首部。这些二级索引是由那些设置了”bind”、”backlog”、”rcvbuf”、”sndbuf”、”accept_filter”、”deferred”、”ipv6only”和”so_keepalive”参数的 listen 指令生成的。
3.  其他二级索引，其相对顺序不变，排在含 bind 属性的二级索引之后，而在含 wildcard 属性的二级索引之前。

第二步，将索引表转换为监听表，这是在 ngx_http_init_listening()函数中实现的。其步骤是

1.  得到是否有二级索引含有 wildcard 属性，只需要看看排序后的二级索引的最后一项就可以了。
2.  顺次将所有含有 bind 属性的二级索引以一对一的方式生成监听表的表项（第一级和第二级都只有一项）。
3.  如果第一步检测到不含 wildcard 属性，则顺次将后续所有二级索引以一对一的方式生成监听表的表项。
4.  如果第一步检测到含 wildcard 属性，则以含 wildcard 属性的二级索引创建监听表的一级表项，并将二级索引中从第一不含 bind 属性的表项开始的所有表项一同转换成为刚刚创建的监听表一级表项的下级表项。

#### 善后工作

善后工作基本的就是一件事，还原解析上下文。“http”指令是这个进行的

```
*cf = pcf;

```

### server 的管理

前面介绍的 http 处理逻辑在处理“server {}”时仍然适用。server 相对较为特殊的是两个指令，一个是”server_name”，一个是”listen”。

就在上一节，我们已经介绍了”server_name”

### location 的管理

## 模块初始化

## 热代码部署

### reload 过程解析

### upgrade 过程解析

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# nginx 的请求处理阶段 (90%)

## 接收请求流程 (99%)

### http 请求格式简介 (99%)

首先介绍一下 rfc2616 中定义的 http 请求基本格式：

```
Request = Request-Line
          * (( general-header
             | request-header
             | entity-header ) CRLF)
          CRLF
          [ message-body ]

```

第一行是请求行（request line），用来说明请求方法，要访问的资源以及所使用的 HTTP 版本：

```
Request-Line   = Method SP Request-URI SP HTTP-Version CRLF

```

请求方法（Method）的定义如下，其中最常用的是 GET，POST 方法：

```
Method = "OPTIONS"
| "GET"
| "HEAD"
| "POST"
| "PUT"
| "DELETE"
| "TRACE"
| "CONNECT"
| extension-method
extension-method = token

```

要访问的资源由统一资源地位符 URI(Uniform Resource Identifier)确定，它的一个比较通用的组成格式（rfc2396）如下：

```
<scheme>://<authority><path>?<query>

```

一般来说根据请求方法（Method）的不同，请求 URI 的格式会有所不同，通常只需写出 path 和 query 部分。

http 版本(version)定义如下，现在用的一般为 1.0 和 1.1 版本：

```
HTTP/<major>.<minor>

```

请求行的下一行则是请求头，rfc2616 中定义了 3 种不同类型的请求头，分别为 general-header，request-header 和 entity-header，每种类型 rfc 中都定义了一些通用的头，其中 entity-header 类型可以包含自定义的头。

### 请求头读取 (99%)

这一节介绍 nginx 中请求头的解析，nginx 的请求处理流程中，会涉及到 2 个非常重要的数据结构，ngx_connection_t 和 ngx_http_request_t，分别用来表示连接和请求，这 2 个数据结构在本书的前篇中已经做了比较详细的介绍，没有印象的读者可以翻回去复习一下，整个请求处理流程从头到尾，对应着这 2 个数据结构的分配，初始化，使用，重用和销毁。

nginx 在初始化阶段，具体是在 init process 阶段的 ngx_event_process_init 函数中会为每一个监听套接字分配一个连接结构（ngx_connection_t），并将该连接结构的读事件成员（read）的事件处理函数设置为 ngx_event_accept，并且如果没有使用 accept 互斥锁的话，在这个函数中会将该读事件挂载到 nginx 的事件处理模型上（poll 或者 epoll 等），反之则会等到 init process 阶段结束，在工作进程的事件处理循环中，某个进程抢到了 accept 锁才能挂载该读事件。

```
static ngx_int_t
ngx_event_process_init(ngx_cycle_t *cycle)
{
    ...

    /* 初始化用来管理所有定时器的红黑树 */
    if (ngx_event_timer_init(cycle->log) == NGX_ERROR) {
        return NGX_ERROR;
    }
    /* 初始化事件模型 */
    for (m = 0; ngx_modules[m]; m++) {
        if (ngx_modules[m]->type != NGX_EVENT_MODULE) {
            continue;
        }

        if (ngx_modules[m]->ctx_index != ecf->use) {
            continue;
        }

        module = ngx_modules[m]->ctx;

        if (module->actions.init(cycle, ngx_timer_resolution) != NGX_OK) {
            /* fatal */
            exit(2);
        }

        break;
    }

    ...

    /* for each listening socket */
    /* 为每个监听套接字分配一个连接结构 */
    ls = cycle->listening.elts;
    for (i = 0; i < cycle->listening.nelts; i++) {

        c = ngx_get_connection(ls[i].fd, cycle->log);

        if (c == NULL) {
            return NGX_ERROR;
        }

        c->log = &ls[i].log;

        c->listening = &ls[i];
        ls[i].connection = c;

        rev = c->read;

        rev->log = c->log;
        /* 标识此读事件为新请求连接事件 */
        rev->accept = 1;

        ...

#if (NGX_WIN32)

        /* windows 环境下不做分析，但原理类似 */

#else
        /* 将读事件结构的处理函数设置为 ngx_event_accept */
        rev->handler = ngx_event_accept;
        /* 如果使用 accept 锁的话，要在后面抢到锁才能将监听句柄挂载上事件处理模型上 */
        if (ngx_use_accept_mutex) {
            continue;
        }
        /* 否则，将该监听句柄直接挂载上事件处理模型 */
        if (ngx_event_flags & NGX_USE_RTSIG_EVENT) {
            if (ngx_add_conn(c) == NGX_ERROR) {
                return NGX_ERROR;
            }

        } else {
            if (ngx_add_event(rev, NGX_READ_EVENT, 0) == NGX_ERROR) {
                return NGX_ERROR;
            }
        }

#endif

    }

    return NGX_OK;
}

```

当一个工作进程在某个时刻将监听事件挂载上事件处理模型之后，nginx 就可以正式的接收并处理客户端过来的请求了。这时如果有一个用户在浏览器的地址栏内输入一个域名，并且域名解析服务器将该域名解析到一台由 nginx 监听的服务器上，nginx 的事件处理模型接收到这个读事件之后，会交给之前注册好的事件处理函数 ngx_event_accept 来处理。

在 ngx_event_accept 函数中，nginx 调用 accept 函数，从已连接队列得到一个连接以及对应的套接字，接着分配一个连接结构（ngx_connection_t），并将新得到的套接字保存在该连接结构中，这里还会做一些基本的连接初始化工作：

1, 首先给该连接分配一个内存池，初始大小默认为 256 字节，可通过 connection_pool_size 指令设置；

2, 分配日志结构，并保存在其中，以便后续的日志系统使用；

3, 初始化连接相应的 io 收发函数，具体的 io 收发函数和使用的事件模型及操作系统相关；

4, 分配一个套接口地址（sockaddr），并将 accept 得到的对端地址拷贝在其中，保存在 sockaddr 字段；

5, 将本地套接口地址保存在 local_sockaddr 字段，因为这个值是从监听结构 ngx_listening_t 中可得，而监听结构中保存的只是配置文件中设置的监听地址，但是配置的监听地址可能是通配符*，即监听在所有的地址上，所以连接中保存的这个值最终可能还会变动，会被确定为真正的接收地址；

6, 将连接的写事件设置为已就绪，即设置 ready 为 1，nginx 默认连接第一次为可写；

7, 如果监听套接字设置了 TCP_DEFER_ACCEPT 属性，则表示该连接上已经有数据包过来，于是设置读事件为就绪；

8, 将 sockaddr 字段保存的对端地址格式化为可读字符串，并保存在 addr_text 字段；

最后调用 ngx_http_init_connection 函数初始化该连接结构的其他部分。

ngx_http_init_connection 函数最重要的工作是初始化读写事件的处理函数：将该连接结构的写事件的处理函数设置为 ngx_http_empty_handler，这个事件处理函数不会做任何操作，实际上 nginx 默认连接第一次可写，不会挂载写事件，如果有数据需要发送，nginx 会直接写到这个连接，只有在发生一次写不完的情况下，才会挂载写事件到事件模型上，并设置真正的写事件处理函数，这里后面的章节还会做详细介绍；读事件的处理函数设置为 ngx_http_init_request，此时如果该连接上已经有数据过来（设置了 deferred accept)，则会直接调用 ngx_http_init_request 函数来处理该请求，反之则设置一个定时器并在事件处理模型上挂载一个读事件，等待数据到来或者超时。当然这里不管是已经有数据到来，或者需要等待数据到来，又或者等待超时，最终都会进入读事件的处理函数-ngx_http_init_request。

ngx_http_init_request 函数主要工作即是初始化请求，由于它是一个事件处理函数，它只有唯一一个 ngx_event_t *类型的参数，ngx_event_t 结构在 nginx 中表示一个事件，事件处理的上下文类似于一个中断处理的上下文，为了在这个上下文得到相关的信息，nginx 中一般会将连接结构的引用保存在事件结构的 data 字段，请求结构的引用则保存在连接结构的 data 字段，这样在事件处理函数中可以方便的得到对应的连接结构和请求结构。进入函数内部看一下，首先判断该事件是否是超时事件，如果是的话直接关闭连接并返回；反之则是指之前 accept 的连接上有请求过来需要处理。

ngx_http_init_request 函数首先在连接的内存池中为该请求分配一个 ngx_http_request_t 结构，这个结构将用来保存该请求所有的信息。分配完之后，这个结构的引用会被包存在连接的 hc 成员的 request 字段，以便于在长连接或 pipelined 请求中复用该请求结构。在这个函数中，nginx 根据该请求的接收端口和地址找到一个默认虚拟服务器配置（listen 指令的 default_server 属性用来标识一个默认虚拟服务器，否则监听在相同端口和地址的多个虚拟服务器，其中第一个定义的则为默认）。

nginx 配置文件中可以设置多个监听在不同端口和地址的虚拟服务器（每个 server 块对应一个虚拟服务器），另外还根据域名（server_name 指令可以配置该虚拟服务器对应的域名）来区分监听在相同端口和地址的虚拟服务器，每个虚拟服务器可以拥有不同的配置内容，而这些配置内容决定了 nginx 在接收到一个请求之后如何处理该请求。找到之后，相应的配置被保存在该请求对应的 ngx_http_request_t 结构中。注意这里根据端口和地址找到的默认配置只是临时使用一下，最终 nginx 会根据域名找到真正的虚拟服务器配置，随后的初始化工作还包括：

1, 将连接的读事件的处理函数设置为 ngx_http_process_request_line 函数，这个函数用来解析请求行，将请求的 read_event_handler 设置为 ngx_http_block_reading 函数，这个函数实际上什么都不做（当然在事件模型设置为水平触发时，唯一做的事情就是将事件从事件模型监听列表中删除，防止该事件一直被触发），后面会说到这里为什么会将 read_event_handler 设置为此函数；

2, 为这个请求分配一个缓冲区用来保存它的请求头，地址保存在 header_in 字段，默认大小为 1024 个字节，可以使用 client_header_buffer_size 指令修改，这里需要注意一下，nginx 用来保存请求头的缓冲区是在该请求所在连接的内存池中分配，而且会将地址保存一份在连接的 buffer 字段中，这样做的目的也是为了给该连接的下一次请求重用这个缓冲区，另外如果客户端发过来的请求头大于 1024 个字节，nginx 会重新分配更大的缓存区，默认用于大请求的头的缓冲区最大为 8K，最多 4 个，这 2 个值可以用 large_client_header_buffers 指令设置，后面还会说到请求行和一个请求头都不能超过一个最大缓冲区的大小；

3, 为这个请求分配一个内存池，后续所有与该请求相关的内存分配一般都会使用该内存池，默认大小为 4096 个字节，可以使用 request_pool_size 指令修改；

4, 为这个请求分配响应头链表，初始大小为 20；

5, 创建所有模块的上下文 ctx 指针数组，变量数据；

6, 将该请求的 main 字段设置为它本身，表示这是一个主请求，nginx 中对应的还有子请求概念，后面的章节会做详细的介绍；

7, 将该请求的 count 字段设置为 1，count 字段表示请求的引用计数；

8, 将当前时间保存在 start_sec 和 start_msec 字段，这个时间是该请求的起始时刻，将被用来计算一个请求的处理时间（request time），nginx 使用的这个起始点和 apache 略有差别，nginx 中请求的起始点是接收到客户端的第一个数据包的事件开始，而 apache 则是接收到客户端的整个 request line 后开始算起；

9, 初始化请求的其他字段，比如将 uri_changes 设置为 11，表示最多可以将该请求的 uri 改写 10 次，subrequests 被设置为 201，表示一个请求最多可以发起 200 个子请求；

做完所有这些初始化工作之后，ngx_http_init_request 函数会调用读事件的处理函数来真正的解析客户端发过来的数据，也就是会进入 ngx_http_process_request_line 函数中处理。

#### 解析请求行 (99%)

ngx_http_process_request_line 函数的主要作用即是解析请求行，同样由于涉及到网络 IO 操作，即使是很短的一行请求行可能也不能被一次读完，所以在之前的 ngx_http_init_request 函数中，ngx_http_process_request_line 函数被设置为读事件的处理函数，它也只拥有一个唯一的 ngx_event_t *类型参数，并且在函数的开头，同样需要判断是否是超时事件，如果是的话，则关闭这个请求和连接；否则开始正常的解析流程。先调用 ngx_http_read_request_header 函数读取数据。

由于可能多次进入 ngx_http_process_request_line 函数，ngx_http_read_request_header 函数首先检查请求的 header_in 指向的缓冲区内是否有数据，有的话直接返回；否则从连接读取数据并保存在请求的 header_in 指向的缓存区，而且只要缓冲区有空间的话，会一次尽可能多的读数据，读到多少返回多少；如果客户端暂时没有发任何数据过来，并返回 NGX_AGAIN，返回之前会做 2 件事情：

1，设置一个定时器，时长默认为 60s，可以通过指令 client_header_timeout 设置，如果定时事件到达之前没有任何可读事件，nginx 将会关闭此请求；

2，调用 ngx_handle_read_event 函数处理一下读事件-如果该连接尚未在事件处理模型上挂载读事件，则将其挂载上；

如果客户端提前关闭了连接或者读取数据发生了其他错误，则给客户端返回一个 400 错误（当然这里并不保证客户端能够接收到响应数据，因为客户端可能都已经关闭了连接），最后函数返回 NGX_ERROR；

如果 ngx_http_read_request_header 函数正常的读取到了数据，ngx_http_process_request_line 函数将调用 ngx_http_parse_request_line 函数来解析，这个函数根据 http 协议规范中对请求行的定义实现了一个有限状态机，经过这个状态机，nginx 会记录请求行中的请求方法（Method），请求 uri 以及 http 协议版本在缓冲区中的起始位置，在解析过程中还会记录一些其他有用的信息，以便后面的处理过程中使用。如果解析请求行的过程中没有产生任何问题，该函数会返回 NGX_OK；如果请求行不满足协议规范，该函数会立即终止解析过程，并返回相应错误号；如果缓冲区数据不够，该函数返回 NGX_AGAIN。

在整个解析 http 请求的状态机中始终遵循着两条重要的原则：减少内存拷贝和回溯。

内存拷贝是一个相对比较昂贵的操作，大量的内存拷贝会带来较低的运行时效率。nginx 在需要做内存拷贝的地方尽量只拷贝内存的起始和结束地址而不是内存本身，这样做的话仅仅只需要两个赋值操作而已，大大降低了开销，当然这样带来的影响是后续的操作不能修改内存本身，如果修改的话，会影响到所有引用到该内存区间的地方，所以必须很小心的管理，必要的时候需要拷贝一份。

这里不得不提到 nginx 中最能体现这一思想的数据结构，ngx_buf_t，它用来表示 nginx 中的缓存，在很多情况下，只需要将一块内存的起始地址和结束地址分别保存在它的 pos 和 last 成员中，再将它的 memory 标志置 1，即可表示一块不能修改的内存区间，在另外的需要一块能够修改的缓存的情形中，则必须分配一块所需大小的内存并保存其起始地址，再将 ngx_buf_t 的 temporary 标志置 1，表示这是一块能够被修改的内存区域。

再回到 ngx_http_process_request_line 函数中，如果 ngx_http_parse_request_line 函数返回了错误，则直接给客户端返回 400 错误； 如果返回 NGX_AGAIN，则需要判断一下是否是由于缓冲区空间不够，还是已读数据不够。如果是缓冲区大小不够了，nginx 会调用 ngx_http_alloc_large_header_buffer 函数来分配另一块大缓冲区，如果大缓冲区还不够装下整个请求行，nginx 则会返回 414 错误给客户端，否则分配了更大的缓冲区并拷贝之前的数据之后，继续调用 ngx_http_read_request_header 函数读取数据来进入请求行自动机处理，直到请求行解析结束；

如果返回了 NGX_OK，则表示请求行被正确的解析出来了，这时先记录好请求行的起始地址以及长度，并将请求 uri 的 path 和参数部分保存在请求结构的 uri 字段，请求方法起始位置和长度保存在 method_name 字段，http 版本起始位置和长度记录在 http_protocol 字段。还要从 uri 中解析出参数以及请求资源的拓展名，分别保存在 args 和 exten 字段。接下来将要解析请求头，将在下一小节中接着介绍。

#### 解析请求头 (99%)

在 ngx_http_process_request_line 函数中，解析完请求行之后，如果请求行的 uri 里面包含了域名部分，则将其保存在请求结构的 headers_in 成员的 server 字段，headers_in 用来保存所有请求头，它的类型为 ngx_http_headers_in_t：

```
typedef struct {
    ngx_list_t                        headers;

    ngx_table_elt_t                  *host;
    ngx_table_elt_t                  *connection;
    ngx_table_elt_t                  *if_modified_since;
    ngx_table_elt_t                  *if_unmodified_since;
    ngx_table_elt_t                  *user_agent;
    ngx_table_elt_t                  *referer;
    ngx_table_elt_t                  *content_length;
    ngx_table_elt_t                  *content_type;

    ngx_table_elt_t                  *range;
    ngx_table_elt_t                  *if_range;

    ngx_table_elt_t                  *transfer_encoding;
    ngx_table_elt_t                  *expect;

#if (NGX_HTTP_GZIP)
    ngx_table_elt_t                  *accept_encoding;
    ngx_table_elt_t                  *via;
#endif

    ngx_table_elt_t                  *authorization;

    ngx_table_elt_t                  *keep_alive;

#if (NGX_HTTP_PROXY || NGX_HTTP_REALIP || NGX_HTTP_GEO)
    ngx_table_elt_t                  *x_forwarded_for;
#endif

#if (NGX_HTTP_REALIP)
    ngx_table_elt_t                  *x_real_ip;
#endif

#if (NGX_HTTP_HEADERS)
    ngx_table_elt_t                  *accept;
    ngx_table_elt_t                  *accept_language;
#endif

#if (NGX_HTTP_DAV)
    ngx_table_elt_t                  *depth;
    ngx_table_elt_t                  *destination;
    ngx_table_elt_t                  *overwrite;
    ngx_table_elt_t                  *date;
#endif

    ngx_str_t                         user;
    ngx_str_t                         passwd;

    ngx_array_t                       cookies;

    ngx_str_t                         server;
    off_t                             content_length_n;
    time_t                            keep_alive_n;

    unsigned                          connection_type:2;
    unsigned                          msie:1;
    unsigned                          msie6:1;
    unsigned                          opera:1;
    unsigned                          gecko:1;
    unsigned                          chrome:1;
    unsigned                          safari:1;
    unsigned                          konqueror:1;
} ngx_http_headers_in_t;

```

接着，该函数会检查进来的请求是否使用的是 http0.9，如果是的话则使用从请求行里得到的域名，调用 ngx_http_find_virtual_server（）函数来查找用来处理该请求的虚拟服务器配置，之前通过端口和地址找到的默认配置不再使用，找到相应的配置之后，则直接调用 ngx_http_process_request（）函数处理该请求，因为 http0.9 是最原始的 http 协议，它里面没有定义任何请求头，显然就不需要读取请求头的操作。

```
if (r->host_start && r->host_end) {

    host = r->host_start;
    n = ngx_http_validate_host(r, &host,
                               r->host_end - r->host_start, 0);

    if (n == 0) {
        ngx_log_error(NGX_LOG_INFO, c->log, 0,
                      "client sent invalid host in request line");
        ngx_http_finalize_request(r, NGX_HTTP_BAD_REQUEST);
        return;
    }

    if (n < 0) {
        ngx_http_close_request(r, NGX_HTTP_INTERNAL_SERVER_ERROR);
        return;
    }

    r->headers_in.server.len = n;
    r->headers_in.server.data = host;
}

if (r->http_version < NGX_HTTP_VERSION_10) {

    if (ngx_http_find_virtual_server(r, r->headers_in.server.data,
                                     r->headers_in.server.len)
        == NGX_ERROR)
    {
        ngx_http_close_request(r, NGX_HTTP_INTERNAL_SERVER_ERROR);
        return;
    }

    ngx_http_process_request(r);
    return;
}

```

当然，如果是 1.0 或者更新的 http 协议，接下来要做的就是读取请求头了，首先 nginx 会为请求头分配空间，ngx_http_headers_in_t 结构的 headers 字段为一个链表结构，它被用来保存所有请求头，初始为它分配了 20 个节点，每个节点的类型为 ngx_table_elt_t，保存请求头的 name/value 值对，还可以看到 ngx_http_headers_in_t 结构有很多类型为 ngx_table_elt_t*的指针成员，而且从它们的命名可以看出是一些常见的请求头名字，nginx 对这些常用的请求头在 ngx_http_headers_in_t 结构里面保存了一份引用，后续需要使用的话，可以直接通过这些成员得到，另外也事先为 cookie 头分配了 2 个元素的数组空间，做完这些内存准备工作之后，该请求对应的读事件结构的处理函数被设置为 ngx_http_process_request_headers，并随后马上调用了该函数。

```
if (ngx_list_init(&r->headers_in.headers, r->pool, 20,
                  sizeof(ngx_table_elt_t))
    != NGX_OK)
{
    ngx_http_close_request(r, NGX_HTTP_INTERNAL_SERVER_ERROR);
    return;
}

if (ngx_array_init(&r->headers_in.cookies, r->pool, 2,
                   sizeof(ngx_table_elt_t *))
    != NGX_OK)
{
    ngx_http_close_request(r, NGX_HTTP_INTERNAL_SERVER_ERROR);
    return;
}

c->log->action = "reading client request headers";

rev->handler = ngx_http_process_request_headers;
ngx_http_process_request_headers(rev);

```

ngx_http_process_request_headers 函数循环的读取所有的请求头，并保存和初始化和请求头相关的结构，下面详细分析一下该函数：

因为 nginx 对读取请求头有超时限制，ngx_http_process_request_headers 函数作为读事件处理函数，一并处理了超时事件，如果读超时了，nginx 直接给该请求返回 408 错误：

```
if (rev->timedout) {
     ngx_log_error(NGX_LOG_INFO, c->log, NGX_ETIMEDOUT, "client timed out");
     c->timedout = 1;
     ngx_http_close_request(r, NGX_HTTP_REQUEST_TIME_OUT);
     return;
 }

```

读取和解析请求头的逻辑和处理请求行差不多，总的流程也是循环的调用 ngx_http_read_request_header（）函数读取数据，然后再调用一个解析函数来从读取的数据中解析请求头，直到解析完所有请求头，或者发生解析错误为主。当然由于涉及到网络 io，这个流程可能发生在多个 io 事件的上下文中。

接着来细看该函数，先调用了 ngx_http_read_request_header（）函数读取数据，如果当前连接并没有数据过来，再直接返回，等待下一次读事件到来，如果读到了一些数据则调用 ngx_http_parse_header_line（）函数来解析，同样的该解析函数实现为一个有限状态机，逻辑很简单，只是根据 http 协议来解析请求头，每次调用该函数最多解析出一个请求头，该函数返回 4 种不同返回值，表示不同解析结果：

1，返回 NGX_OK，表示解析出了一行请求头，这时还要判断解析出的请求头名字里面是否有非法字符，名字里面合法的字符包括字母，数字和连字符（-），另外如果设置了 underscores_in_headers 指令为 on，则下划线也是合法字符，但是 nginx 默认下划线不合法，当请求头里面包含了非法的字符，nginx 默认只是忽略这一行请求头；如果一切都正常，nginx 会将该请求头及请求头名字的 hash 值保存在请求结构体的 headers_in 成员的 headers 链表,而且对于一些常见的请求头，如 Host，Connection，nginx 采用了类似于配置指令的方式，事先给这些请求头分配了一个处理函数，当解析出一个请求头时，会检查该请求头是否有设置处理函数，有的话则调用之，nginx 所有有处理函数的请求头都记录在 ngx_http_headers_in 全局数组中：

```
typedef struct {
    ngx_str_t                         name;
    ngx_uint_t                        offset;
    ngx_http_header_handler_pt        handler;
} ngx_http_header_t;

ngx_http_header_t  ngx_http_headers_in[] = {
    { ngx_string("Host"), offsetof(ngx_http_headers_in_t, host),
                 ngx_http_process_host },

    { ngx_string("Connection"), offsetof(ngx_http_headers_in_t, connection),
                 ngx_http_process_connection },

    { ngx_string("If-Modified-Since"),
                 offsetof(ngx_http_headers_in_t, if_modified_since),
                 ngx_http_process_unique_header_line },

    { ngx_string("If-Unmodified-Since"),
                 offsetof(ngx_http_headers_in_t, if_unmodified_since),
                 ngx_http_process_unique_header_line },

    { ngx_string("User-Agent"), offsetof(ngx_http_headers_in_t, user_agent),
                 ngx_http_process_user_agent },

    { ngx_string("Referer"), offsetof(ngx_http_headers_in_t, referer),
                 ngx_http_process_header_line },

    { ngx_string("Content-Length"),
                 offsetof(ngx_http_headers_in_t, content_length),
                 ngx_http_process_unique_header_line },

    { ngx_string("Content-Type"),
                 offsetof(ngx_http_headers_in_t, content_type),
                 ngx_http_process_header_line },

    { ngx_string("Range"), offsetof(ngx_http_headers_in_t, range),
                 ngx_http_process_header_line },

    { ngx_string("If-Range"),
                 offsetof(ngx_http_headers_in_t, if_range),
                 ngx_http_process_unique_header_line },

    { ngx_string("Transfer-Encoding"),
                 offsetof(ngx_http_headers_in_t, transfer_encoding),
                 ngx_http_process_header_line },

    { ngx_string("Expect"),
                 offsetof(ngx_http_headers_in_t, expect),
                 ngx_http_process_unique_header_line },

#if (NGX_HTTP_GZIP)
    { ngx_string("Accept-Encoding"),
                 offsetof(ngx_http_headers_in_t, accept_encoding),
                 ngx_http_process_header_line },

    { ngx_string("Via"), offsetof(ngx_http_headers_in_t, via),
                 ngx_http_process_header_line },
#endif

    { ngx_string("Authorization"),
                 offsetof(ngx_http_headers_in_t, authorization),
                 ngx_http_process_unique_header_line },

    { ngx_string("Keep-Alive"), offsetof(ngx_http_headers_in_t, keep_alive),
                 ngx_http_process_header_line },

#if (NGX_HTTP_PROXY || NGX_HTTP_REALIP || NGX_HTTP_GEO)
    { ngx_string("X-Forwarded-For"),
                 offsetof(ngx_http_headers_in_t, x_forwarded_for),
                 ngx_http_process_header_line },
#endif

#if (NGX_HTTP_REALIP)
    { ngx_string("X-Real-IP"),
                 offsetof(ngx_http_headers_in_t, x_real_ip),
                 ngx_http_process_header_line },
#endif

#if (NGX_HTTP_HEADERS)
    { ngx_string("Accept"), offsetof(ngx_http_headers_in_t, accept),
                 ngx_http_process_header_line },

    { ngx_string("Accept-Language"),
                 offsetof(ngx_http_headers_in_t, accept_language),
                 ngx_http_process_header_line },
#endif

#if (NGX_HTTP_DAV)
    { ngx_string("Depth"), offsetof(ngx_http_headers_in_t, depth),
                 ngx_http_process_header_line },

    { ngx_string("Destination"), offsetof(ngx_http_headers_in_t, destination),
                 ngx_http_process_header_line },

    { ngx_string("Overwrite"), offsetof(ngx_http_headers_in_t, overwrite),
                 ngx_http_process_header_line },

    { ngx_string("Date"), offsetof(ngx_http_headers_in_t, date),
                 ngx_http_process_header_line },
#endif

    { ngx_string("Cookie"), 0, ngx_http_process_cookie },

    { ngx_null_string, 0, NULL }
};

```

ngx_http_headers_in 数组当前包含了 25 个常用的请求头，每个请求头都设置了一个处理函数，其中一部分请求头设置的是公共处理函数，这里有 2 个公共处理函数，ngx_http_process_header_line 和 ngx_http_process_unique_header_line。 先来看一下处理函数的函数指针定义：

```
typedef ngx_int_t (*ngx_http_header_handler_pt)(ngx_http_request_t *r,
    ngx_table_elt_t *h, ngx_uint_t offset);

```

它有 3 个参数，r 为对应的请求结构，h 为指向该请求头在 headers_in.headers 链表中对应节点的指针，offset 为该请求头对应字段在 ngx_http_headers_in_t 结构中的偏移。

再来看 ngx_http_process_header_line 函数：

```
static ngx_int_t
ngx_http_process_header_line(ngx_http_request_t *r, ngx_table_elt_t *h,
    ngx_uint_t offset)
{
    ngx_table_elt_t  **ph;

    ph = (ngx_table_elt_t **) ((char *) &r->headers_in + offset);

    if (*ph == NULL) {
        *ph = h;
    }

    return NGX_OK;
}

```

这个函数只是简单将该请求头在 ngx_http_headers_in_t 结构中保存一份引用。ngx_http_process_unique_header_line 功能类似，不同点在于该函数会检查这个请求头是否是重复的，如果是的话，则给该请求返回 400 错误。

ngx_http_headers_in 数组中剩下的请求头都有自己特殊的处理函数，这些特殊的函数根据对应的请求头有一些特殊的处理，下面拿 Host 头的处理函数 ngx_http_process_host 做一下介绍：

```
static ngx_int_t
ngx_http_process_host(ngx_http_request_t *r, ngx_table_elt_t *h,
    ngx_uint_t offset)
{
    u_char   *host;
    ssize_t   len;

    if (r->headers_in.host == NULL) {
        r->headers_in.host = h;
    }

    host = h->value.data;
    len = ngx_http_validate_host(r, &host, h->value.len, 0);

    if (len == 0) {
        ngx_log_error(NGX_LOG_INFO, r->connection->log, 0,
                      "client sent invalid host header");
        ngx_http_finalize_request(r, NGX_HTTP_BAD_REQUEST);
        return NGX_ERROR;
    }

    if (len < 0) {
        ngx_http_close_request(r, NGX_HTTP_INTERNAL_SERVER_ERROR);
        return NGX_ERROR;
    }

    if (r->headers_in.server.len) {
        return NGX_OK;
    }

    r->headers_in.server.len = len;
    r->headers_in.server.data = host;

    return NGX_OK;
}

```

此函数的目的也是保存 Host 头的快速引用，它会对 Host 头的值做一些合法性检查，并从中解析出域名，保存在 headers_in.server 字段，实际上前面在解析请求行时，headers_in.server 可能已经被赋值为从请求行中解析出来的域名，根据 http 协议的规范，如果请求行中的 uri 带有域名的话，则域名以它为准，所以这里需检查一下 headers_in.server 是否为空，如果不为空则不需要再赋值。

其他请求头的特殊处理函数，不再做介绍，大致都是根据该请求头在 http 协议中规定的意义及其值设置请求的一些属性，必备后续使用。

对一个合法的请求头的处理大致为如上所述；

2，返回 NGX_AGAIN，表示当前接收到的数据不够，一行请求头还未结束，需要继续下一轮循环。在下一轮循环中，nginx 首先检查请求头缓冲区 header_in 是否已满，如够满了，则调用 ngx_http_alloc_large_header_buffer（）函数分配更多缓冲区，下面分析一下 ngx_http_alloc_large_header_buffer 函数：

```
static ngx_int_t
ngx_http_alloc_large_header_buffer(ngx_http_request_t *r,
    ngx_uint_t request_line)
{
    u_char                    *old, *new;
    ngx_buf_t                 *b;
    ngx_http_connection_t     *hc;
    ngx_http_core_srv_conf_t  *cscf;

    ngx_log_debug0(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                   "http alloc large header buffer");

    /*
     * 在解析请求行阶段，如果客户端在发送请求行之前发送了大量回车换行符将
     * 缓冲区塞满了，针对这种情况，nginx 只是简单的重置缓冲区，丢弃这些垃圾
     * 数据，不需要分配更大的内存。
     */
    if (request_line && r->state == 0) {

        /* the client fills up the buffer with "\r\n" */

        r->request_length += r->header_in->end - r->header_in->start;

        r->header_in->pos = r->header_in->start;
        r->header_in->last = r->header_in->start;

        return NGX_OK;
    }

    /* 保存请求行或者请求头在旧缓冲区中的起始地址 */
    old = request_line ? r->request_start : r->header_name_start;

    cscf = ngx_http_get_module_srv_conf(r, ngx_http_core_module);

    /* 如果一个大缓冲区还装不下请求行或者一个请求头，则返回错误 */
    if (r->state != 0
        && (size_t) (r->header_in->pos - old)
                                     >= cscf->large_client_header_buffers.size)
    {
        return NGX_DECLINED;
    }

    hc = r->http_connection;

    /* 首先在 ngx_http_connection_t 结构中查找是否有空闲缓冲区，有的话，直接取之 */
    if (hc->nfree) {
        b = hc->free[--hc->nfree];

        ngx_log_debug2(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                       "http large header free: %p %uz",
                       b->pos, b->end - b->last);

    /* 检查给该请求分配的请求头缓冲区个数是否已经超过限制，默认最大个数为 4 个 */
    } else if (hc->nbusy < cscf->large_client_header_buffers.num) {

        if (hc->busy == NULL) {
            hc->busy = ngx_palloc(r->connection->pool,
                  cscf->large_client_header_buffers.num * sizeof(ngx_buf_t *));
            if (hc->busy == NULL) {
                return NGX_ERROR;
            }
        }

        /* 如果还没有达到最大分配数量，则分配一个新的大缓冲区 */
        b = ngx_create_temp_buf(r->connection->pool,
                                cscf->large_client_header_buffers.size);
        if (b == NULL) {
            return NGX_ERROR;
        }

        ngx_log_debug2(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                       "http large header alloc: %p %uz",
                       b->pos, b->end - b->last);

    } else {
        /* 如果已经达到最大的分配限制，则返回错误 */
        return NGX_DECLINED;
    }

    /* 将从空闲队列取得的或者新分配的缓冲区加入已使用队列 */
    hc->busy[hc->nbusy++] = b;

    /*
     * 因为 nginx 中，所有的请求头的保存形式都是指针（起始和结束地址），
     * 所以一行完整的请求头必须放在连续的内存块中。如果旧的缓冲区不能
     * 再放下整行请求头，则分配新缓冲区，并从旧缓冲区拷贝已经读取的部分请求头，
     * 拷贝完之后，需要修改所有相关指针指向到新缓冲区。
     * status 为 0 表示解析完一行请求头之后，缓冲区正好被用完，这种情况不需要拷贝
     */
    if (r->state == 0) {
        /*
         * r->state == 0 means that a header line was parsed successfully
         * and we do not need to copy incomplete header line and
         * to relocate the parser header pointers
         */

        r->request_length += r->header_in->end - r->header_in->start;

        r->header_in = b;

        return NGX_OK;
    }

    ngx_log_debug1(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                   "http large header copy: %d", r->header_in->pos - old);

    r->request_length += old - r->header_in->start;

    new = b->start;

    /* 拷贝旧缓冲区中不完整的请求头 */
    ngx_memcpy(new, old, r->header_in->pos - old);

    b->pos = new + (r->header_in->pos - old);
    b->last = new + (r->header_in->pos - old);

    /* 修改相应的指针指向新缓冲区 */
    if (request_line) {
        r->request_start = new;

        if (r->request_end) {
            r->request_end = new + (r->request_end - old);
        }

        r->method_end = new + (r->method_end - old);

        r->uri_start = new + (r->uri_start - old);
        r->uri_end = new + (r->uri_end - old);

        if (r->schema_start) {
            r->schema_start = new + (r->schema_start - old);
            r->schema_end = new + (r->schema_end - old);
        }

        if (r->host_start) {
            r->host_start = new + (r->host_start - old);
            if (r->host_end) {
                r->host_end = new + (r->host_end - old);
            }
        }

        if (r->port_start) {
            r->port_start = new + (r->port_start - old);
            r->port_end = new + (r->port_end - old);
        }

        if (r->uri_ext) {
            r->uri_ext = new + (r->uri_ext - old);
        }

        if (r->args_start) {
            r->args_start = new + (r->args_start - old);
        }

        if (r->http_protocol.data) {
            r->http_protocol.data = new + (r->http_protocol.data - old);
        }

    } else {
        r->header_name_start = new;
        r->header_name_end = new + (r->header_name_end - old);
        r->header_start = new + (r->header_start - old);
        r->header_end = new + (r->header_end - old);
    }

    r->header_in = b;

    return NGX_OK;
}

```

当 ngx_http_alloc_large_header_buffer 函数返回 NGX_DECLINED 时，表示客户端发送了一行过大的请求头，或者是整个请求头部超过了限制，nginx 会返回 494 错误，注意到 nginx 在返回 494 错误之前将请求的 lingering_close 标识置为了 1，这样做的目的是在返回响应之前丢弃掉客户端发过来的其他数据；

3，返回 NGX_HTTP_PARSE_INVALID_HEADER，表示请求头解析过程中遇到错误，一般为客户端发送了不符合协议规范的头部，此时 nginx 返回 400 错误；

4，返回 NGX_HTTP_PARSE_HEADER_DONE，表示所有请求头已经成功的解析，这时请求的状态被设置为 NGX_HTTP_PROCESS_REQUEST_STATE，意味着结束了请求读取阶段，正式进入了请求处理阶段，但是实际上请求可能含有请求体，nginx 在请求读取阶段并不会去读取请求体，这个工作交给了后续的请求处理阶段的模块，这样做的目的是 nginx 本身并不知道这些请求体是否有用，如果后续模块并不需要的话，一方面请求体一般较大，如果全部读取进内存，则白白耗费大量的内存空间，另一方面即使 nginx 将请求体写进磁盘，但是涉及到磁盘 io，会耗费比较多时间。所以交由后续模块来决定读取还是丢弃请求体是最明智的办法。

读取完请求头之后，nginx 调用了 ngx_http_process_request_header（）函数，这个函数主要做了两个方面的事情，一是调用 ngx_http_find_virtual_server（）函数查找虚拟服务器配置；二是对一些请求头做一些协议的检查。比如对那些使用 http1.1 协议但是却没有发送 Host 头的请求，nginx 给这些请求返回 400 错误。还有 nginx 现在的版本并不支持 chunked 格式的输入，如果某些请求申明自己使用了 chunked 格式的输入（请求带有值为 chunked 的 transfer_encoding 头部)，nginx 给这些请求返回 411 错误。等等。

最后调用 ngx_http_process_request（）函数处理请求,至此，nginx 请求头接收流程就介绍完毕。

### 请求体读取(100%)

上节说到 nginx 核心本身不会主动读取请求体，这个工作是交给请求处理阶段的模块来做，但是 nginx 核心提供了 ngx_http_read_client_request_body()接口来读取请求体，另外还提供了一个丢弃请求体的接口-ngx_http_discard_request_body()，在请求执行的各个阶段中，任何一个阶段的模块如果对请求体感兴趣或者希望丢掉客户端发过来的请求体，可以分别调用这两个接口来完成。这两个接口是 nginx 核心提供的处理请求体的标准接口，如果希望配置文件中一些请求体相关的指令（比如 client_body_in_file_only，client_body_buffer_size 等）能够预期工作，以及能够正常使用 nginx 内置的一些和请求体相关的变量（比如$request_body 和$request_body_file），一般来说所有模块都必须调用这些接口来完成相应操作，如果需要自定义接口来处理请求体，也应尽量兼容 nginx 默认的行为。

#### 读取请求体

请求体的读取一般发生在 nginx 的 content handler 中，一些 nginx 内置的模块，比如 proxy 模块，fastcgi 模块，uwsgi 模块等，这些模块的行为必须将客户端过来的请求体（如果有的话）以相应协议完整的转发到后端服务进程，所有的这些模块都是调用了 ngx_http_read_client_request_body()接口来完成请求体读取。值得注意的是这些模块会把客户端的请求体完整的读取后才开始往后端转发数据。

由于内存的限制，ngx_http_read_client_request_body()接口读取的请求体会部分或者全部写入一个临时文件中，根据请求体的大小以及相关的指令配置，请求体可能完整放置在一块连续内存中，也可能分别放置在两块不同内存中，还可能全部存在一个临时文件中，最后还可能一部分在内存，剩余部分在临时文件中。下面先介绍一下和这些不同存储行为相关的指令：

| client_body_buffer_size: |
|   | 设置缓存请求体的 buffer 大小，默认为系统页大小的 2 倍，当请求体的大小超过此大小时，nginx 会把请求体写入到临时文件中。可以根据业务需求设置合适的大小，尽量避免磁盘 io 操作; |
| client_body_in_single_buffer: |
|   | 指示是否将请求体完整的存储在一块连续的内存中，默认为 off，如果此指令被设置为 on，则 nginx 会保证请求体在不大于 client_body_buffer_size 设置的值时，被存放在一块连续的内存中，但超过大小时会被整个写入一个临时文件; |
| client_body_in_file_only: |
|   | 设置是否总是将请求体保存在临时文件中，默认为 off，当此指定被设置为 on 时，即使客户端显式指示了请求体长度为 0 时，nginx 还是会为请求创建一个临时文件。 |

接着介绍 ngx_http_read_client_request_body()接口的实现，它的定义如下：

```
ngx_int_t
ngx_http_read_client_request_body(ngx_http_request_t *r,
    ngx_http_client_body_handler_pt post_handler)

```

该接口有 2 个参数，第 1 个为指向请求结构的指针，第 2 个为一个函数指针，当请求体读完时，它会被调用。之前也说到根据 nginx 现有行为，模块逻辑会在请求体读完后执行，这个回调函数一般就是模块的逻辑处理函数。ngx_http_read_client_request_body()函数首先将参数 r 对应的主请求的引用加 1，这样做的目的和该接口被调用的上下文有关，一般而言，模块是在 content handler 中调用此接口，一个典型的调用如下：

```
static ngx_int_t
ngx_http_proxy_handler(ngx_http_request_t *r)
{
    ...
    rc = ngx_http_read_client_request_body(r, ngx_http_upstream_init);

    if (rc >= NGX_HTTP_SPECIAL_RESPONSE) {
        return rc;
    }

    return NGX_DONE;
}

```

上面的代码是在 porxy 模块的 content handler，ngx_http_proxy_handler()中调用了 ngx_http_read_client_request_body()函数，其中 ngx_http_upstream_init()被作为回调函数传入进接口中，另外 nginx 中模块的 content handler 调用的上下文如下：

```
ngx_int_t
ngx_http_core_content_phase(ngx_http_request_t *r,
    ngx_http_phase_handler_t *ph)
{
    ...
    if (r->content_handler) {
        r->write_event_handler = ngx_http_request_empty_handler;
        ngx_http_finalize_request(r, r->content_handler(r));
        return NGX_OK;
    }
    ...
}

```

上面的代码中，content handler 调用之后，它的返回值作为参数调用了 ngx_http_finalize_request()函数，在请求体没有被接收完全时，ngx_http_read_client_request_body()函数返回值为 NGX_AGAIN，此时 content handler，比如 ngx_http_proxy_handler()会返回 NGX_DONE，而 NGX_DONE 作为参数传给 ngx_http_finalize_request()函数会导致主请求的引用计数减 1，所以正好抵消了 ngx_http_read_client_request_body()函数开头对主请求计数的加 1。

接下来回到 ngx_http_read_client_request_body()函数，它会检查该请求的请求体是否已经被读取或者被丢弃了，如果是的话，则直接调用回调函数并返回 NGX_OK，这里实际上是为子请求检查，子请求是 nginx 中的一个概念，nginx 中可以在当前请求中发起另外一个或多个全新的子请求来访问其他的 location，关于子请求的具体介绍会在后面的章节作详细分析，一般而言子请求不需要自己去读取请求体。

函数接着调用 ngx_http_test_expect()检查客户端是否发送了 Expect: 100-continue 头，是的话则给客户端回复”HTTP/1.1 100 Continue”，根据 http 1.1 协议，客户端可以发送一个 Expect 头来向服务器表明期望发送请求体，服务器如果允许客户端发送请求体，则会回复”HTTP/1.1 100 Continue”，客户端收到时，才会开始发送请求体。

接着继续为接收请求体做准备工作，分配一个 ngx_http_request_body_t 结构，并保存在 r->request_body，这个结构用来保存请求体读取过程用到的缓存引用，临时文件引用，剩余请求体大小等信息，它的定义如下:

```
typedef struct {
    ngx_temp_file_t                  *temp_file;
    ngx_chain_t                      *bufs;
    ngx_buf_t                        *buf;
    off_t                             rest;
    ngx_chain_t                      *to_write;
    ngx_http_client_body_handler_pt   post_handler;
} ngx_http_request_body_t;

```

| temp_file: | 指向储存请求体的临时文件的指针； |
| bufs: | 指向保存请求体的链表头； |
| buf: | 指向当前用于保存请求体的内存缓存； |
| rest: | 当前剩余的请求体大小； |
| post_handler: | 保存传给 ngx_http_read_client_request_body()函数的回调函数。 |

做好准备工作之后，函数开始检查请求是否带有 content_length 头，如果没有该头或者客户端发送了一个值为 0 的 content_length 头，表明没有请求体，这时直接调用回调函数并返回 NGX_OK 即可。当然如果 client_body_in_file_only 指令被设置为 on，且 content_length 为 0 时，该函数在调用回调函数之前，会创建一个空的临时文件。

进入到函数下半部分，表明客户端请求确实表明了要发送请求体，该函数会先检查是否在读取请求头时预读了请求体，这里的检查是通过判断保存请求头的缓存(r->header_in)中是否还有未处理的数据。如果有预读数据，则分配一个 ngx_buf_t 结构，并将 r->header_in 中的预读数据保存在其中，并且如果 r->header_in 中还有剩余空间，并且能够容下剩余未读取的请求体，这些空间将被继续使用，而不用分配新的缓存，当然甚至如果请求体已经被整个预读了，则不需要继续处理了，此时调用回调函数后返回。

如果没有预读数据或者预读不完整，该函数会分配一块新的内存（除非 r->header_in 还有足够的剩余空间），另外如果 request_body_in_single_buf 指令被设置为 no，则预读的数据会被拷贝进新开辟的内存块中，真正读取请求体的操作是在 ngx_http_do_read_client_request_body()函数，该函数循环的读取请求体并保存在缓存中，如果缓存被写满了，其中的数据会被清空并写回到临时文件中。当然这里有可能不能一次将数据读到，该函数会挂载读事件并设置读事件 handler 为 ngx_http_read_client_request_body_handler，另外 nginx 核心对两次请求体的读事件之间也做了超时设置，client_body_timeout 指令可以设置这个超时时间，默认为 60 秒，如果下次读事件超时了，nginx 会返回 408 给客户端。

最终读完请求体后，ngx_http_do_read_client_request_body()会根据配置，将请求体调整到预期的位置(内存或者文件)，所有情况下请求体都可以从 r->request_body 的 bufs 链表得到，该链表最多可能有 2 个节点，每个节点为一个 buffer，但是这个 buffer 的内容可能是保存在内存中，也可能是保存在磁盘文件中。另外$request_body 变量只在当请求体已经被读取并且是全部保存在内存中，才能取得相应的数据。

#### 丢弃请求体

一个模块想要主动的丢弃客户端发过的请求体，可以调用 nginx 核心提供的 ngx_http_discard_request_body()接口，主动丢弃的原因可能有很多种，如模块的业务逻辑压根不需要请求体 ，客户端发送了过大的请求体，另外为了兼容 http1.1 协议的 pipeline 请求，模块有义务主动丢弃不需要的请求体。总之为了保持良好的客户端兼容性，nginx 必须主动丢弃无用的请求体。下面开始分析 ngx_http_discard_request_body()函数：

```
ngx_int_t
ngx_http_discard_request_body(ngx_http_request_t *r)
{
    ssize_t       size;
    ngx_event_t  *rev;

    if (r != r->main || r->discard_body) {
        return NGX_OK;
    }

    if (ngx_http_test_expect(r) != NGX_OK) {
        return NGX_HTTP_INTERNAL_SERVER_ERROR;
    }

    rev = r->connection->read;

    ngx_log_debug0(NGX_LOG_DEBUG_HTTP, rev->log, 0, "http set discard body");

    if (rev->timer_set) {
        ngx_del_timer(rev);
    }

    if (r->headers_in.content_length_n <= 0 || r->request_body) {
        return NGX_OK;
    }

    size = r->header_in->last - r->header_in->pos;

    if (size) {
        if (r->headers_in.content_length_n > size) {
            r->header_in->pos += size;
            r->headers_in.content_length_n -= size;

        } else {
            r->header_in->pos += (size_t) r->headers_in.content_length_n;
            r->headers_in.content_length_n = 0;
            return NGX_OK;
        }
    }

    r->read_event_handler = ngx_http_discarded_request_body_handler;

    if (ngx_handle_read_event(rev, 0) != NGX_OK) {
        return NGX_HTTP_INTERNAL_SERVER_ERROR;
    }

    if (ngx_http_read_discarded_request_body(r) == NGX_OK) {
        r->lingering_close = 0;

    } else {
        r->count++;
        r->discard_body = 1;
    }

    return NGX_OK;
}

```

由于函数不长，这里把它完整的列出来了，函数的开始同样先判断了不需要再做处理的情况：子请求不需要处理，已经调用过此函数的也不需要再处理。接着调用 ngx_http_test_expect() 处理 http1.1 expect 的情况，根据 http1.1 的 expect 机制，如果客户端发送了 expect 头，而服务端不希望接收请求体时，必须返回 417(Expectation Failed)错误。nginx 并没有这样做，它只是简单的让客户端把请求体发送过来，然后丢弃掉。接下来，函数删掉了读事件上的定时器，因为这时本身就不需要请求体，所以也无所谓客户端发送的快还是慢了，当然后面还会讲到，当 nginx 已经处理完该请求但客户端还没有发送完无用的请求体时，nginx 会在读事件上再挂上定时器。

客户端如果打算发送请求体，就必须发送 content-length 头，所以函数会检查请求头中的 content-length 头，同时还会查看其他地方是不是已经读取了请求体。如果确实有待处理的请求体，函数接着检查请求头 buffer 中预读的数据，预读的数据会直接被丢掉，当然如果请求体已经被全部预读，函数就直接返回了。

接下来，如果还有剩余的请求体未处理，该函数调用 ngx_handle_read_event()在事件处理机制中挂载好读事件，并把读事件的处理函数设置为 ngx_http_discarded_request_body_handler。做好这些准备之后，该函数最后调用 ngx_http_read_discarded_request_body()接口读取客户端过来的请求体并丢弃。如果客户端并没有一次将请求体发过来，函数会返回，剩余的数据等到下一次读事件过来时，交给 ngx_http_discarded_request_body_handler()来处理，这时，请求的 discard_body 将被设置为 1 用来标识这种情况。另外请求的引用数(count)也被加 1，这样做的目的是客户端可能在 nginx 处理完请求之后仍未完整发送待发送的请求体，增加引用是防止 nginx 核心在处理完请求后直接释放了请求的相关资源。

ngx_http_read_discarded_request_body()函数非常简单，它循环的从链接中读取数据并丢弃，直到读完接收缓冲区的所有数据，如果请求体已经被读完了，该函数会设置读事件的处理函数为 ngx_http_block_reading，这个函数仅仅删除水平触发的读事件，防止同一事件不断被触发。

最后看一下读事件的处理函数 ngx_http_discarded_request_body_handler，这个函数每次读事件来时会被调用，先看一下它的源码：

```
void
ngx_http_discarded_request_body_handler(ngx_http_request_t *r)
{
    ...

    c = r->connection;
    rev = c->read;

    if (rev->timedout) {
        c->timedout = 1;
        c->error = 1;
        ngx_http_finalize_request(r, NGX_ERROR);
        return;
    }

    if (r->lingering_time) {
        timer = (ngx_msec_t) (r->lingering_time - ngx_time());

        if (timer <= 0) {
            r->discard_body = 0;
            r->lingering_close = 0;
            ngx_http_finalize_request(r, NGX_ERROR);
            return;
        }

    } else {
        timer = 0;
    }

    rc = ngx_http_read_discarded_request_body(r);

    if (rc == NGX_OK) {
        r->discard_body = 0;
        r->lingering_close = 0;
        ngx_http_finalize_request(r, NGX_DONE);
        return;
    }

    /* rc == NGX_AGAIN */

    if (ngx_handle_read_event(rev, 0) != NGX_OK) {
        c->error = 1;
        ngx_http_finalize_request(r, NGX_ERROR);
        return;
    }

    if (timer) {

        clcf = ngx_http_get_module_loc_conf(r, ngx_http_core_module);

        timer *= 1000;

        if (timer > clcf->lingering_timeout) {
            timer = clcf->lingering_timeout;
        }

        ngx_add_timer(rev, timer);
    }
}

```

函数一开始就处理了读事件超时的情况，之前说到在 ngx_http_discard_request_body()函数中已经删除了读事件的定时器，那么什么时候会设置定时器呢？答案就是在 nginx 已经处理完该请求，但是又没有完全将该请求的请求体丢弃的时候（客户端可能还没有发送过来），在 ngx_http_finalize_connection()函数中，如果检查到还有未丢弃的请求体时，nginx 会添加一个读事件定时器，它的时长为 lingering_timeout 指令所指定，默认为 5 秒，不过这个时间仅仅两次读事件之间的超时时间，等待请求体的总时长为 lingering_time 指令所指定，默认为 30 秒。这种情况中，该函数如果检测到超时事件则直接返回并断开连接。同样，还需要控制整个丢弃请求体的时长不能超过 lingering_time 设置的时间，如果超过了最大时长，也会直接返回并断开连接。

如果读事件发生在请求处理完之前，则不用处理超时事件，也不用设置定时器，函数只是简单的调用 ngx_http_read_discarded_request_body()来读取并丢弃数据。

## 多阶段处理请求

读取完请求头后，nginx 进入请求的处理阶段。简单的情况下，客户端发送过的统一资源定位符(url)对应服务器上某一路径上的资源，web 服务器需要做的仅仅是将 url 映射到本地文件系统的路径，然后读取相应文件并返回给客户端。但这仅仅是最初的互联网的需求，而如今互联网出现了各种各样复杂的需求，要求 web 服务器能够处理诸如安全及权限控制，多媒体内容和动态网页等等问题。这些复杂的需求导致 web 服务器不再是一个短小的程序，而变成了一个必须经过仔细设计，模块化的系统。nginx 良好的模块化特性体现在其对请求处理流程的多阶段划分当中，多阶段处理流程就好像一条流水线，一个 nginx 进程可以并发的处理处于不同阶段的多个请求。nginx 允许开发者在处理流程的任意阶段注册模块，在启动阶段，nginx 会把各个阶段注册的所有模块处理函数按序的组织成一条执行链。

nginx 实际把请求处理流程划分为了 11 个阶段，这样划分的原因是将请求的执行逻辑细分，各阶段按照处理时机定义了清晰的执行语义，开发者可以很容易分辨自己需要开发的模块应该定义在什么阶段，下面介绍一下各阶段:

| NGX_HTTP_POST_READ_PHASE: |
|   | 接收完请求头之后的第一个阶段，它位于 uri 重写之前，实际上很少有模块会注册在该阶段，默认的情况下，该阶段被跳过； |
| NGX_HTTP_SERVER_REWRITE_PHASE: |
|   | server 级别的 uri 重写阶段，也就是该阶段执行处于 server 块内，location 块外的重写指令，前面的章节已经说明在读取请求头的过程中 nginx 会根据 host 及端口找到对应的虚拟主机配置； |
| NGX_HTTP_FIND_CONFIG_PHASE: |
|   | 寻找 location 配置阶段，该阶段使用重写之后的 uri 来查找对应的 location，值得注意的是该阶段可能会被执行多次，因为也可能有 location 级别的重写指令； |
| NGX_HTTP_REWRITE_PHASE: |
|   | location 级别的 uri 重写阶段，该阶段执行 location 基本的重写指令，也可能会被执行多次； |
| NGX_HTTP_POST_REWRITE_PHASE: |
|   | location 级别重写的后一阶段，用来检查上阶段是否有 uri 重写，并根据结果跳转到合适的阶段； |
| NGX_HTTP_PREACCESS_PHASE: |
|   | 访问权限控制的前一阶段，该阶段在权限控制阶段之前，一般也用于访问控制，比如限制访问频率，链接数等； |
| NGX_HTTP_ACCESS_PHASE: |
|   | 访问权限控制阶段，比如基于 ip 黑白名单的权限控制，基于用户名密码的权限控制等； |
| NGX_HTTP_POST_ACCESS_PHASE: |
|   | 访问权限控制的后一阶段，该阶段根据权限控制阶段的执行结果进行相应处理； |
| NGX_HTTP_TRY_FILES_PHASE: |
|   | try_files 指令的处理阶段，如果没有配置 try_files 指令，则该阶段被跳过； |
| NGX_HTTP_CONTENT_PHASE: |
|   | 内容生成阶段，该阶段产生响应，并发送到客户端； |
| NGX_HTTP_LOG_PHASE: |
|   | 日志记录阶段，该阶段记录访问日志。 |

### 多阶段执行链

nginx 按请求处理的执行顺序将处理流程划分为多个阶段，一般每个阶段又可以注册多个模块处理函数，nginx 按阶段将这些处理函数组织成了一个执行链，这个执行链保存在 http 主配置（ngx_http_core_main_conf_t）的 phase_engine 字段中，phase_engine 字段的类型为 ngx_http_phase_engine_t：

```
typedef struct {
    ngx_http_phase_handler_t  *handlers;
    ngx_uint_t                 server_rewrite_index;
    ngx_uint_t                 location_rewrite_index;
} ngx_http_phase_engine_t;

```

其中 handlers 字段即为执行链，实际上它是一个数组，而每个元素之间又被串成链表，从而允许执行流程向前，或者向后的阶段跳转，执行链节点的数据结构定义如下：

```
struct ngx_http_phase_handler_s {
    ngx_http_phase_handler_pt  checker;
    ngx_http_handler_pt        handler;
    ngx_uint_t                 next;
};

```

其中 checker 和 handler 都是函数指针，相同阶段的节点具有相同的 checker 函数，handler 字段保存的是模块处理函数，一般在 checker 函数中会执行当前节点的 handler 函数，但是例外的是 NGX_HTTP_FIND_CONFIG_PHASE，NGX_HTTP_POST_REWRITE_PHASE，NGX_HTTP_POST_ACCESS_PHASE 和 NGX_HTTP_TRY_FILES_PHASE 这 4 个阶段不能注册模块函数。next 字段为快速跳跃索引，多数情况下，执行流程是按照执行链顺序的往前执行，但在某些执行阶段的 checker 函数中由于执行了某个逻辑可能需要回跳至之前的执行阶段，也可能需要跳过之后的某些执行阶段，next 字段保存的就是跳跃的目的索引。

和建立执行链相关的数据结构都保存在 http 主配置中，一个是 phases 字段，另外一个是 phase_engine 字段。其中 phases 字段为一个数组，它的元素个数等于阶段数目，即每个元素对应一个阶段。而 phases 数组的每个元素又是动态数组（ngx_array_t），每次模块注册处理函数时只需要在对应阶段的动态数组增加一个元素用来保存处理函数的指针。由于在某些执行阶段可能需要向后，或者向前跳转，简单的使用 2 个数组并不方便，所以 nginx 又组织了一个执行链，保存在了 phase_engine 字段，其每个节点包含一个 next 域用来保存跳跃目的节点的索引，而执行链的建立则在 nginx 初始化的 post config 阶段之后调用 ngx_http_init_phase_handlers 函数完成，下面分析一下该函数：

```
static ngx_int_t
ngx_http_init_phase_handlers(ngx_conf_t *cf, ngx_http_core_main_conf_t *cmcf)
{
    ngx_int_t                   j;
    ngx_uint_t                  i, n;
    ngx_uint_t                  find_config_index, use_rewrite, use_access;
    ngx_http_handler_pt        *h;
    ngx_http_phase_handler_t   *ph;
    ngx_http_phase_handler_pt   checker;

    cmcf->phase_engine.server_rewrite_index = (ngx_uint_t) -1;
    cmcf->phase_engine.location_rewrite_index = (ngx_uint_t) -1;
    find_config_index = 0;
    use_rewrite = cmcf->phases[NGX_HTTP_REWRITE_PHASE].handlers.nelts ? 1 : 0;
    use_access = cmcf->phases[NGX_HTTP_ACCESS_PHASE].handlers.nelts ? 1 : 0;

    n = use_rewrite + use_access + cmcf->try_files + 1 /* find config phase */;

    for (i = 0; i < NGX_HTTP_LOG_PHASE; i++) {
        n += cmcf->phases[i].handlers.nelts;
    }

    ph = ngx_pcalloc(cf->pool,
                     n * sizeof(ngx_http_phase_handler_t) + sizeof(void *));
    if (ph == NULL) {
        return NGX_ERROR;
    }

    cmcf->phase_engine.handlers = ph;
    n = 0;

    for (i = 0; i < NGX_HTTP_LOG_PHASE; i++) {
        h = cmcf->phases[i].handlers.elts;

        switch (i) {

        case NGX_HTTP_SERVER_REWRITE_PHASE:
            if (cmcf->phase_engine.server_rewrite_index == (ngx_uint_t) -1) {
                cmcf->phase_engine.server_rewrite_index = n;
            }
            checker = ngx_http_core_rewrite_phase;

            break;

        case NGX_HTTP_FIND_CONFIG_PHASE:
            find_config_index = n;

            ph->checker = ngx_http_core_find_config_phase;
            n++;
            ph++;

            continue;

        case NGX_HTTP_REWRITE_PHASE:
            if (cmcf->phase_engine.location_rewrite_index == (ngx_uint_t) -1) {
                cmcf->phase_engine.location_rewrite_index = n;
            }
            checker = ngx_http_core_rewrite_phase;

            break;

        case NGX_HTTP_POST_REWRITE_PHASE:
            if (use_rewrite) {
                ph->checker = ngx_http_core_post_rewrite_phase;
                ph->next = find_config_index;
                n++;
                ph++;
            }

            continue;

        case NGX_HTTP_ACCESS_PHASE:
            checker = ngx_http_core_access_phase;
            n++;
            break;

        case NGX_HTTP_POST_ACCESS_PHASE:
            if (use_access) {
                ph->checker = ngx_http_core_post_access_phase;
                ph->next = n;
                ph++;
            }

            continue;

        case NGX_HTTP_TRY_FILES_PHASE:
            if (cmcf->try_files) {
                ph->checker = ngx_http_core_try_files_phase;
                n++;
                ph++;
            }

            continue;

        case NGX_HTTP_CONTENT_PHASE:
            checker = ngx_http_core_content_phase;
            break;

        default:
            checker = ngx_http_core_generic_phase;
        }

        n += cmcf->phases[i].handlers.nelts;

        for (j = cmcf->phases[i].handlers.nelts - 1; j >=0; j--) {
            ph->checker = checker;
            ph->handler = h[j];
            ph->next = n;
            ph++;
        }
    }

    return NGX_OK;
}

```

首先需要说明的是 cmcf->phases 数组中保存了在 post config 之前注册的所有模块函数，上面的函数先计算执行链的节点个数，并分配相应的空间，前面提到有 4 个阶段不能注册模块，并且 POST_REWRITE 和 POST_ACCESS 这 2 个阶段分别只有在 REWRITE 和 ACCESS 阶段注册了模块时才存在，另外 TRY_FILES 阶段只有在配置了 try_files 指令的时候才存在，最后 FIND_CONFIG 阶段虽然不能注册模块，但它是必须存在的，所以在计算执行链节点数时需要考虑这些因素。

分配好内存之后，开始建立链表，过程很简单，遍历每个阶段注册的模块函数，为每个阶段的节点赋值 checker 函数，handler 函数，以及 next 索引。最终建立好的执行链如下图：

(暂缺)

SERVER_REWRITE 阶段的节点的 next 域指向 FIND_CONFIG 阶段的第 1 个节点，REWRITE 阶段的 next 域指向 POST_REWRITE 阶段的第 1 个节点，而 POST_REWRITE 阶段的 next 域则指向 FIND_CONFIG，因为当出现 location 级别的 uri 重写时，可能需要重新匹配新的 location，PREACCESS 阶段的 next 域指向 ACCESS 域，ACCESS 和 POST_ACCESS 阶段的 next 域则是则是指向 CONTENT 阶段，当然如果 TRY_FILES 阶段存在的话，则是指向 TRY_FILES 阶段，最后 CONTENT 阶段的 next 域指向 LOG 阶段，当然 next 域是每个阶段的 checker 函数根据该阶段的需求来使用的，没有需要时，checker 函数可能都不会使用到它。

### POST_READ 阶段

POST_READ 阶段是 nginx 处理请求流程中第一个可以添加模块函数的阶段，任何需要在接收完请求头之后立刻处理的逻辑可以在该阶段注册处理函数。nginx 源码中只有 realip 模块在该阶段注册了函数，当 nginx 前端多了一个 7 层负载均衡层，并且客户端的真实 ip 被前端保存在请求头中时，该模块用来将客户端的 ip 替换为请求头中保存的值。realip 模块之所以在 POST_READ 阶段执行的原因是它需要在其他模块执行之前悄悄的将客户端 ip 替换为真实值，而且它需要的信息仅仅只是请求头。一般很少有模块需要注册在 POST_READ 阶段，realip 模块默认没有编译进 nginx。

POST_READ 阶段的 checker 函数是 ngx_http_core_generic_phase，这个函数是 nginx phase 默认的 checker 函数，后面的 PREACCESS phase 也是用 checker，下面对它做一下介绍：

```
ngx_int_t
ngx_http_core_generic_phase(ngx_http_request_t *r, ngx_http_phase_handler_t *ph)
{
    ngx_int_t  rc;

    /*
     * generic phase checker,
     * used by the post read and pre-access phases
     */

    ngx_log_debug1(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                   "generic phase: %ui", r->phase_handler);

    rc = ph->handler(r);

    if (rc == NGX_OK) {
        r->phase_handler = ph->next;
        return NGX_AGAIN;
    }

    if (rc == NGX_DECLINED) {
        r->phase_handler++;
        return NGX_AGAIN;
    }

    if (rc == NGX_AGAIN || rc == NGX_DONE) {
        return NGX_OK;
    }

    /* rc == NGX_ERROR || rc == NGX_HTTP_...  */

    ngx_http_finalize_request(r, rc);

    return NGX_OK;
}

```

这个函数逻辑非常简单，调用该 phase 注册的 handler 函数，需要注意的是该函数对 handler 返回值的处理，一般而言 handler 返回：

| NGX_OK: | 表示该阶段已经处理完成，需要转入下一个阶段； |
| NG_DECLINED: | 表示需要转入本阶段的下一个 handler 继续处理； |
| NGX_AGAIN, NGX_DONE: |
|   | 表示需要等待某个事件发生才能继续处理（比如等待网络 IO），此时 Nginx 为了不阻塞其他请求的处理，必须中断当前请求的执行链，等待事件发生之后继续执行该 handler； |
| NGX_ERROR: | 表示发生了错误，需要结束该请求。 |

checker 函数根据 handler 函数的不同返回值，给上一层的 ngx_http_core_run_phases 函数返回 NGX_AGAIN 或者 NGX_OK，如果期望上一层继续执行后面的 phase 则需要确保 checker 函数不是返回 NGX_OK，不同 checker 函数对 handler 函数的返回值处理还不太一样，开发模块时需要确保相应阶段的 checker 函数对返回值的处理在你的预期之内。

### SERVER_REWRITE 阶段

SERVER_REWRITE 阶段是 nginx 中第一个必须经历的重要 phase，请求进入此阶段时已经找到对应的虚拟主机（server）配置。nginx 的 rewrite 模块在这个阶段注册了一个 handler，rewrite 模块提供 url 重写指令 rewrite，变量设置指令 set，以及逻辑控制指令 if、break 和 return，用户可以在 server 配置里面，组合这些指令来满足自己的需求，而不需要另外写一个模块，比如将一些前缀满足特定模式的 uri 重定向到一个固定的 url，还可以根据请求的属性来决定是否需要重写或者给用户发送特定的返回码。rewrite 提供的逻辑控制指令能够满足一些简单的需求，针对一些较复杂的逻辑可能需要注册 handler 通过独立实现模块的方式来满足。

需要注意该阶段和后面的 REWRITE 阶段的区别，在 SERVER_REWRITE 阶段中，请求还未被匹配到一个具体的 location 中。该阶段执行的结果（比如改写后的 uri）会影响后面 FIND_CONFIG 阶段的执行。另外这个阶段也是内部子请求执行的第一个阶段。 SERVER_REWRITE 阶段的 checker 函数是 ngx_http_core_rewrite_phase：

```
ngx_int_t
ngx_http_core_rewrite_phase(ngx_http_request_t *r, ngx_http_phase_handler_t *ph)
{
    ngx_int_t  rc;

    ngx_log_debug1(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                   "rewrite phase: %ui", r->phase_handler);

    rc = ph->handler(r);

    if (rc == NGX_DECLINED) {
        r->phase_handler++;
        return NGX_AGAIN;
    }

    if (rc == NGX_DONE) {
        return NGX_OK;
    }

    /* NGX_OK, NGX_AGAIN, NGX_ERROR, NGX_HTTP_...  */

    ngx_http_finalize_request(r, rc);

    return NGX_OK;
}

```

这个函数和上面说的 ngx_http_core_generic_phase 函数流程基本一致，唯一的区别就是对 handler 返回值的处理稍有不同，比如这里对 NGX_OK 的处理是调用 ngx_http_finalize_request 结束请求，所以再强调一下，handler 函数的返回值一定要根据不同 phase 的 checker 函数来设置。Nginx 的 rewrite 模块会挂上一个名为 ngx_http_rewrite_handler 的 handler。

### FIND_CONFIG 阶段

FIND_CONFIG 阶段顾名思义就是寻找配置阶段，具体一点就是根据 uri 查找 location 配置，实际上就是设置 r->loc_conf，在此之前 r->loc_conf 使用的 server 级别的，查找 location 过程由函数 ngx_http_core_find_location 完成，具体查找流流程这里不再赘述，可以参考上一章关于 location 管理的内容，值得注意的是当 ngx_http_core_find_location 函数返回 NGX_DONE 时，Nginx 会返回 301，将用户请求做一个重定向，这种情况仅发生在该 location 使用了 proxy_pass/fastcgi/scgi/uwsgi/memcached 模块，且 location 的名字以/符号结尾，并且请求的 uri 为该 location 除/之外的前缀，比如对 location /xx/，如果某个请求/xx 访问到该 location，则会被重定向为/xx/。另外 Nginx 中 location 可以标识为 internal，即内部 location，这种 location 只能由子请求或者内部跳转访问。

找到 location 配置后，Nginx 调用了 ngx_http_update_location_config 函数来更新请求相关配置，其中最重要的是更新请求的 content handler，不同 location 可以有自己的 content handler。

最后，由于有 REWRITE_PHASE 的存在，FIND_CONFIG 阶段可能会被执行多次。

### REWRITE 阶段

REWRITE 阶段为 location 级别的重写，这个阶段的 checker 和 SERVER_REWRITE 阶段的是同一个函数，而且 Nginx 的 rewrite 模块对这 2 个阶段注册的是同一个 handler，2 者唯一区别就是执行时机不一样，REWRITE 阶段为 location 级别的重写，SERVER_REWRITE 执行之后是 FIND_CONFIG 阶段，REWRITE 阶段执行之后是 POST_REWRITE 阶段。

### POST_REWRITE 阶段

该阶段不能注册 handler，仅仅只是检查上一阶段是否做了 uri 重写，如果没有重写的话，直接进入下一阶段；如果有重写的话，则利用 next 跳转域往前跳转到 FIND_CONFIG 阶段重新执行。Nginx 对 uri 重写次数做了限制，默认是 10 次。

### PREACCESS 阶段

进入该阶段表明 Nginx 已经将请求确定到了某一个 location(当该 server 没有任何 location 时，也可能是 server），如论如何请求的 loc_conf 配置已经确定下来，该阶段一般用来做资源控制，默认情况下，诸如 ngx_http_limit_conn_module，ngx_http_limit_req_module 等模块会在该阶段注册 handler，用于控制连接数，请求速率等。PREACCESS 阶段使用的 checker 是默认的 ngx_http_core_generic_phase 函数。

### ACCESS 阶段

该阶段的首要目的是做权限控制，默认情况下，Nginx 的 ngx_http_access_module 和 ngx_http_auth_basic_module 模块分别会在该阶段注册一个 handler。

ACCESS 阶段的 checker 是 ngx_http_core_access_phase 函数，此函数对 handler 返回值的处理大致和 ngx_http_core_generic_phase 一致，特殊的地方是当 clcf->satisfy 为 NGX_HTTP_SATISFY_ALL，也就是需要满足该阶段注册的所有 handler 的验证时，某个 handler 返回 NGX_OK 时还需要继续处理本阶段的其他 handler。clcf->satisfy 的值可以使用 satisfy 指令指定。

### POST_ACCESS 阶段

POST_ACCESS 和 POST_REWRITE 阶段一样，只是处理一下上一阶段的结果，而不能挂载自己的 handler，具体为如果 ACCESS 阶段返回了 NGX_HTTP_FORBIDDEN 或 NGX_HTTP_UNAUTHORIZED（记录在 r->access_code 字段），该阶段会结束掉请求。

### TRY_FILES 阶段

TRY_FILES 阶段仅当配置了 try_files 指令时生效，实际上该指令不常用，它的功能是指定一个或者多个文件或目录，最后一个参数可以指定为一个 location 或一个返回码，当设置了该指令时，TRY_FILES 阶段调用 checker 函数 ngx_http_core_try_files_phase 来依此检查指定的文件或目录是否存在，如果本地文件系统存在某个文件或目录则退出该阶段继续执行下面的阶段，否则内部重定向到最后一个参数指定的 location 或返回指定的返回码。

该阶段也不能注册 handler。

### CONTENT 阶段

CONTENT 阶段可以说是整个执行链中最重要的阶段，请求从这里开始执行业务逻辑并产生响应，下面来分析一下它的 checker 函数：

```
ngx_int_t
ngx_http_core_content_phase(ngx_http_request_t *r,
    ngx_http_phase_handler_t *ph)
{
    size_t     root;
    ngx_int_t  rc;
    ngx_str_t  path;

    if (r->content_handler) {
        r->write_event_handler = ngx_http_request_empty_handler;
        ngx_http_finalize_request(r, r->content_handler(r));
        return NGX_OK;
    }

    ngx_log_debug1(NGX_LOG_DEBUG_HTTP, r->connection->log, 0,
                   "content phase: %ui", r->phase_handler);

    rc = ph->handler(r);

    if (rc != NGX_DECLINED) {
        ngx_http_finalize_request(r, rc);
        return NGX_OK;
    }

    /* rc == NGX_DECLINED */

    ph++;

    if (ph->checker) {
        r->phase_handler++;
        return NGX_AGAIN;
    }

    /* no content handler was found */

    if (r->uri.data[r->uri.len - 1] == '/') {

        if (ngx_http_map_uri_to_path(r, &path, &root, 0) != NULL) {
            ngx_log_error(NGX_LOG_ERR, r->connection->log, 0,
                          "directory index of \"%s\" is forbidden", path.data);
        }

        ngx_http_finalize_request(r, NGX_HTTP_FORBIDDEN);
        return NGX_OK;
    }

    ngx_log_error(NGX_LOG_ERR, r->connection->log, 0, "no handler found");

    ngx_http_finalize_request(r, NGX_HTTP_NOT_FOUND);
    return NGX_OK;
}

```

CONTENT 阶段有些特殊，它不像其他阶段只能执行固定的 handler 链，还有一个特殊的 content_handler，每个 location 可以有自己独立的 content handler，而且当有 content handler 时，CONTENT 阶段只会执行 content handler，不再执行本阶段的 handler 链。

默认情况下，Nginx 会在 CONTENT 阶段的 handler 链挂上 index 模块，静态文件处理模块等的 handler。另外模块还可以设置独立的 content handler，比如 ngx_http_proxy_module 的 proxy_pass 指令会设置一个名为 ngx_http_proxy_handler 的 content handler。

接下来看一下上面的 checker 函数的执行流程，首先检查是否设置了 r->content_handler，如果设置了的话，则执行它，需要注意的是在执行它之前，Nginx 将 r->write_event_handler 设置为了 ngx_http_request_empty_handler，先看一下设置 r->write_event_handler 之前的值是什么，在 ngx_http_handler 函数中它被设置为 ngx_http_core_run_phases，而 ngx_http_core_run_phases 会运行每个阶段的 checker 函数。正常流程中，如果某个阶段需要等待某个写事件发生时，该阶段的 handler 会返回 NGX_OK 来中断 ngx_http_core_run_phases 的运行，等到下次写事件过来时，会继续执行之前阶段的 handler；当执行 r->content_handler 的流程时，Nginx 默认模块会去处理 r->write_event_handler 的值，也就是假设 r->content_handler 只能执行 1 次，如果模块设置的 content handler 涉及到 IO 操作，就需要合理的设置处理读写事件的 handler（r->read_event_handler 和 r->write_event_handler）。

还有一个需要注意的点是 r->content_handler 执行之后，Nginx 直接用其返回值调用了 ngx_http_finalize_request 函数，Nginx 将一大堆耦合的逻辑都集中在了这个函数当中，包括长连接，lingering_close，子请求等的处理都涉及到该函数，后面会有一节单独介绍这个函数。这里需要提醒的是 r->content_handler 如果并未完成整个请求的处理，而只是需要等待某个事件发生而退出处理流程的话，必须返回一个合适的值传给 ngx_http_finalize_request，一般而言是返回 NGX_DONE，而且需要将请求的引用计数（r->count）加 1，确保 ngx_http_finalize_request 函数不会将该请求释放掉。

函数的其他部分处理走 handler 链的情况，特殊的地方是 CONTENT 阶段是 ngx_http_core_run_phases 函数跑的最后一个阶段，如果最后一个 handler 返回 NGX_DECLINED，此时 Nginx 会给客户端返回 NGX_HTTP_FORBIDDEN（403）或 NGX_HTTP_NOT_FOUND（404）。

### LOG 阶段

LOG 阶段主要的目的就是记录访问日志，进入该阶段表明该请求的响应已经发送到系统发送缓冲区。另外这个阶段的 handler 链实际上并不是在 ngx_http_core_run_phases 函数中执行，而是在释放请求资源的 ngx_http_free_request 函数中运行，这样做的原因实际是为了简化流程，因为 ngx_http_core_run_phases 可能会执行多次，而 LOG 阶段只需要再请求所有逻辑都结束时运行一次，所以在 ngx_http_free_request 函数中运行 LOG 阶段的 handler 链是非常好的选择。具体的执行的函数为 ngx_http_log_request：

```
static void
ngx_http_log_request(ngx_http_request_t *r)
{
    ngx_uint_t                  i, n;
    ngx_http_handler_pt        *log_handler;
    ngx_http_core_main_conf_t  *cmcf;

    cmcf = ngx_http_get_module_main_conf(r, ngx_http_core_module);

    log_handler = cmcf->phases[NGX_HTTP_LOG_PHASE].handlers.elts;
    n = cmcf->phases[NGX_HTTP_LOG_PHASE].handlers.nelts;

    for (i = 0; i < n; i++) {
        log_handleri;
    }
}

```

函数非常简单，仅仅是遍历 LOG 阶段的 handler 链，逐一执行，而且不会检查返回值。LOG 阶段和其他阶段的不同点有两个，一是执行点是在 ngx_http_free_request 中，二是这个阶段的所有 handler 都会被执行。

至此，Nginx 请求处理的多阶段执行链的各个阶段都已经介绍完毕，弄清楚每个阶段的执行时机以及每个阶段的不同特点对写模块非常重要。

## Nginx filter

在 CONTENT 阶段产生的数据被发往客户端（系统发送缓存区）之前，会先经过过滤。Nginx 的 filter 的工作方式和做鱼有些类似。比如一条鱼，可以把它切成鱼片（也可以切块，切泥），然后通过不同的烹饪方法就得到水煮鱼或者日式生鱼片或者废了等等。同样是一条鱼，加工得到的结果却截然不同，就是因为中间不同的工序赋予了这条鱼各种属性。Nginx 的 filter 也是一个道理，前面的 Handler 好比这条鱼，filter 负责加工，最后得到的 HTTP 响应就会各种各样，格式可以是 JSON 或者 YAML，内容可能多一些或者少一些，HTTP 属性可各异，可以选择压缩，甚至内容可以被丢弃。

对应 HTTP 请求的响应头和响应体，Nginx 分别设置了 header filter 和 body filter。两种机制都是采用链表的方式，不同过滤模块对应链表的一个节点，一般而言一个模块会同时注册 header filter 和 body filter。一个典型的 filter 模块，比如 gzip 模块使用类似如下的代码来注册：

```
static ngx_http_output_header_filter_pt  ngx_http_next_header_filter;
static ngx_http_output_body_filter_pt    ngx_http_next_body_filter;

...

static ngx_int_t
ngx_http_gzip_filter_init(ngx_conf_t *cf)
{
    ngx_http_next_header_filter = ngx_http_top_header_filter;
    ngx_http_top_header_filter = ngx_http_gzip_header_filter;

    ngx_http_next_body_filter = ngx_http_top_body_filter;
    ngx_http_top_body_filter = ngx_http_gzip_body_filter;

    return NGX_OK;
}

```

上面的代码中，gzip 模块首先在模块的开头声明了两个 static 类型的全局变量 ngx_http_next_header_filter 和 ngx_http_next_body_filter，在 ngx_http_gzip_filter_init 函数中，这二个变量分别被赋值为 ngx_http_top_header_filter 及 ngx_http_top_body_filter。而后二者定义在 ngx_http.c，并在 ngx_http.h 头文件中被导出。ngx_http_top_header_filter 和 ngx_http_top_body_filter 实际上是 filter 链表的头结点，每次注册一个新的 filter 模块时，它们的值先被保存在新模块的内部全局变量 ngx_http_next_header_filter 及 ngx_http_next_body_filter，然后被赋值为新模块注册的 filter 函数，而且 Nginx filter 是先从头节点开始执行，所以越晚注册的模块越早执行。

采用默认编译选项，Nginx 默认编译的模块如下：

```
ngx_module_t *ngx_modules[] = {
    &ngx_core_module,
    &ngx_errlog_module,
    &ngx_conf_module,
    &ngx_events_module,
    &ngx_event_core_module,
    &ngx_epoll_module,
    &ngx_regex_module,
    &ngx_http_module,
    &ngx_http_core_module,
    &ngx_http_log_module,
    &ngx_http_upstream_module,
    &ngx_http_static_module,
    &ngx_http_autoindex_module,
    &ngx_http_index_module,
    &ngx_http_auth_basic_module,
    &ngx_http_access_module,
    &ngx_http_limit_conn_module,
    &ngx_http_limit_req_module,
    &ngx_http_geo_module,
    &ngx_http_map_module,
    &ngx_http_split_clients_module,
    &ngx_http_referer_module,
    &ngx_http_rewrite_module,
    &ngx_http_proxy_module,
    &ngx_http_fastcgi_module,
    &ngx_http_uwsgi_module,
    &ngx_http_scgi_module,
    &ngx_http_memcached_module,
    &ngx_http_empty_gif_module,
    &ngx_http_browser_module,
    &ngx_http_upstream_ip_hash_module,
    &ngx_http_upstream_keepalive_module,
    &ngx_http_write_filter_module,          /* 最后一个 body filter，负责往外发送数据 */
    &ngx_http_header_filter_module,         /* 最后一个 header filter，负责在内存中拼接出完整的 http 响应头，
                                               并调用 ngx_http_write_filter 发送 */
    &ngx_http_chunked_filter_module,        /* 对响应头中没有 content_length 头的请求，强制短连接（低于 http 1.1）
                                               或采用 chunked 编码（http 1.1) */
    &ngx_http_range_header_filter_module,   /* header filter，负责处理 range 头 */
    &ngx_http_gzip_filter_module,           /* 支持流式的数据压缩 */
    &ngx_http_postpone_filter_module,       /* body filter，负责处理子请求和主请求数据的输出顺序 */
    &ngx_http_ssi_filter_module,            /* 支持过滤 SSI 请求，采用发起子请求的方式，去获取 include 进来的文件 */
    &ngx_http_charset_filter_module,        /* 支持添加 charset，也支持将内容从一种字符集转换到另外一种字符集 */
    &ngx_http_userid_filter_module,         /* 支持添加统计用的识别用户的 cookie */
    &ngx_http_headers_filter_module,        /* 支持设置 expire 和 Cache-control 头，支持添加任意名称的头 */
    &ngx_http_copy_filter_module,           /* 根据需求重新复制输出链表中的某些节点
                                              （比如将 in_file 的节点从文件读出并复制到新的节点），并交给后续 filter
                                               进行处理 */
    &ngx_http_range_body_filter_module,     /* body filter，支持 range 功能，如果请求包含 range 请求，
                                               那就只发送 range 请求的一段内容 */
    &ngx_http_not_modified_filter_module,   /* 如果请求的 if-modified-since 等于回复的 last-modified 值，
                                               说明回复没有变化，清空所有回复的内容，返回 304 */
    NULL
};

```

从模块的命名可以很容易看出哪些模块是 filter 模块，一般而言 Nginx 的 filter 模块名以 filter_module 结尾，普通的模块名以 module 结尾。上面的列表从下往上看，ngx_http_not_modified_filter_module 实际上 filter 链的第一个节点，而 ngx_http_write_filter_module 是最后一个节点。filter 模块的执行顺序特别重要，比如数据经过 gzip 模块后就变成了压缩之后的数据，如果在 gzip 模块后面运行的 filter 模块需要再查看数据的原始内容就不可能了（除非再做解压），第三方模块会被 Nginx 注册在 ngx_http_copy_filter_module 之后，ngx_http_headers_filter_module 之前。这样设定的原因是为了确保一些模块比如 gzip filter，chunked filter，copy filter 运行在 filter 链的开头或尾部。

### header filter 分析

通常 Nginx 调用 ngx_http_send_header 函数来发送响应头，看下它的实现：

```
ngx_int_t
ngx_http_send_header(ngx_http_request_t *r)
{
    if (r->err_status) {
        r->headers_out.status = r->err_status;
        r->headers_out.status_line.len = 0;
    }

    return ngx_http_top_header_filter(r);
}

```

上面的代码中调用了 ngx_http_top_header_filter，也就是 header filter 的头节点，按照上一节介绍的顺序，ngx_http_not_modified_filter_module 是最后一个注册的 filter 模块，而最后定义的会最先执行，初始化之后，它实际上是 ngx_http_not_modified_header_filter 函数：

```
static ngx_int_t
ngx_http_not_modified_header_filter(ngx_http_request_t *r)
{
    if (r->headers_out.status != NGX_HTTP_OK
        || r != r->main
        || r->headers_out.last_modified_time == -1)
    {
        return ngx_http_next_header_filter(r);
    }

    if (r->headers_in.if_unmodified_since) {
        return ngx_http_test_precondition(r);
    }

    if (r->headers_in.if_modified_since) {
        return ngx_http_test_not_modified(r);
    }

    return ngx_http_next_header_filter(r);
}

```

而在 ngx_http_not_modified_header_filter 函数中，它会调用模块内部定义的函数指针变量 ngx_http_next_header_filter，而该变量保存的是上一模块注册的 header filter 函数，同样的下一个 header filter 函数内部也会调用其模块内部的 ngx_http_next_header_filter，直到调用到最后一个 header filter - ngx_http_header_filter。

ngx_http_header_filter，这个 filter 负责计算响应头的总大小，并分配内存，组装响应头，并调用 ngx_http_write_filter 发送。Nginx 中，header filter 只会被调用一次，ngx_http_header_filter 函数中首先会检查 r->header_sent 标识是否已经被设置，如果是的话，则直接返回；否则设置该标识，并发送响应头。另外如果是子请求的话，也会直接退出函数。

### body filter 分析

Nginx 中通常调用 ngx_http_output_filter 函数来发送响应体，它的实现如下：

```
ngx_int_t
ngx_http_output_filter(ngx_http_request_t *r, ngx_chain_t *in)
{
    ngx_int_t          rc;
    ngx_connection_t  *c;

    c = r->connection;

    ngx_log_debug2(NGX_LOG_DEBUG_HTTP, c->log, 0,
                   "http output filter \"%V?%V\"", &r->uri, &r->args);

    rc = ngx_http_top_body_filter(r, in);

    if (rc == NGX_ERROR) {
        /* NGX_ERROR may be returned by any filter */
        c->error = 1;
    }

    return rc;
}

```

body filter 链调用的原理和 header filter 一样，和 ngx_http_send_header 函数不同的是，上面的函数多了一个类型为 ngx_chain_t *的参数，因为 Nginx 实现的是流式的输出，并不用等到整个响应体都生成了才往客户端发送数据，而是产生一部分内容之后将其组织成链表，调用 ngx_http_output_filter 发送，并且待发送的内容可以在文件中，也可以是在内存中，Nginx 会负责将数据流式的，高效的传输出去。而且当发送缓存区满了时，Nginx 还会负责保存未发送完的数据，调用者只需要对新数据调用一次 ngx_http_output_filter 即可。

#### ngx_http_copy_filter_module 分析

ngx_http_copy_filter_module 是响应体过滤链（body filter）中非常重要的一个模块，这个 filter 模块主要是来将一些需要复制的 buf（可能在文件中，也可能在内存中）重新复制一份交给后面的 filter 模块处理。先来看它的初始化函数：

```
static ngx_int_t
ngx_http_copy_filter_init(ngx_conf_t *cf)
{
    ngx_http_next_body_filter = ngx_http_top_body_filter;
    ngx_http_top_body_filter = ngx_http_copy_filter;

    return NGX_OK;
}

```

可以看到，它只注册了 body filter，而没有注册 header filter，也就是说只有 body filter 链中才有这个模块。

该模块有一个命令，命令名为 output_buffers，用来配置可用的 buffer 数和 buffer 大小，它的值保存在 copy filter 的 loc conf 的 bufs 字段，默认数量为 1，大小为 32768 字节。这个参数具体的作用后面会做介绍。

Nginx 中，一般 filter 模块可以 header filter 函数中根据请求响应头设置一个模块上下文（context），用来保存相关的信息，在 body filter 函数中使用这个上下文。而 copy filter 没有 header filter，因此它的 context 的初始化也是放在 body filter 中的，而它的 ctx 就是 ngx_output_chain_ctx_t，为什么名字是 output_chain 呢，这是因为 copy filter 的主要逻辑的处理都放在 ngx_output_chain 模块中，另外这个模块在 core 目录下，而不是属于 http 目录。

接下来看一下上面说到的 context 结构：

```
struct ngx_output_chain_ctx_s {
    ngx_buf_t                   *buf;              /* 保存临时的 buf */
    ngx_chain_t                 *in;               /* 保存了将要发送的 chain */
    ngx_chain_t                 *free;             /* 保存了已经发送完毕的 chain，以便于重复利用 */
    ngx_chain_t                 *busy;             /* 保存了还未发送的 chain */

    unsigned                     sendfile:1;       /* sendfile 标记 */
    unsigned                     directio:1;       /* directio 标记 */
#if (NGX_HAVE_ALIGNED_DIRECTIO)
    unsigned                     unaligned:1;
#endif
    unsigned                     need_in_memory:1; /* 是否需要在内存中保存一份(使用 sendfile 的话，
                                                      内存中没有文件的拷贝的，而我们有时需要处理文件，
                                                      此时就需要设置这个标记) */
    unsigned                     need_in_temp:1;   /* 是否需要在内存中重新复制一份，不管 buf 是在内存还是文件,
                                                      这样的话，后续模块可以直接修改这块内存 */
#if (NGX_HAVE_FILE_AIO)
    unsigned                     aio:1;

    ngx_output_chain_aio_pt      aio_handler;
#endif

    off_t                        alignment;

    ngx_pool_t                  *pool;
    ngx_int_t                    allocated;        /* 已经分别的 buf 个数 */
    ngx_bufs_t                   bufs;             /* 对应 loc conf 中设置的 bufs */
    ngx_buf_tag_t                tag;              /* 模块标记，主要用于 buf 回收 */

    ngx_output_chain_filter_pt   output_filter;    /* 一般是 ngx_http_next_filter,也就是继续调用 filter 链 */
    void                        *filter_ctx;       /* 当前 filter 的上下文，
                                                      这里是由于 upstream 也会调用 output_chain */
};

```

为了更好的理解 context 结构每个域的具体含义，接下来分析 filter 的具体实现：

```
static ngx_int_t
ngx_http_copy_filter(ngx_http_request_t *r, ngx_chain_t *in)
{
    ngx_int_t                     rc;
    ngx_connection_t             *c;
    ngx_output_chain_ctx_t       *ctx;
    ngx_http_core_loc_conf_t     *clcf;
    ngx_http_copy_filter_conf_t  *conf;

    c = r->connection;

    ngx_log_debug2(NGX_LOG_DEBUG_HTTP, c->log, 0,
                   "http copy filter: \"%V?%V\"", &r->uri, &r->args);

    /* 获取 ctx */
    ctx = ngx_http_get_module_ctx(r, ngx_http_copy_filter_module);

    /* 如果为空，则说明需要初始化 ctx */
    if (ctx == NULL) {
        ctx = ngx_pcalloc(r->pool, sizeof(ngx_output_chain_ctx_t));
        if (ctx == NULL) {
            return NGX_ERROR;
        }

        ngx_http_set_ctx(r, ctx, ngx_http_copy_filter_module);

        conf = ngx_http_get_module_loc_conf(r, ngx_http_copy_filter_module);
        clcf = ngx_http_get_module_loc_conf(r, ngx_http_core_module);

        /* 设置 sendfile */
        ctx->sendfile = c->sendfile;
        /* 如果 request 设置了 filter_need_in_memory 的话，ctx 的这个域就会被设置 */
        ctx->need_in_memory = r->main_filter_need_in_memory
                              || r->filter_need_in_memory;
        /* 和上面类似 */
        ctx->need_in_temp = r->filter_need_temporary;

        ctx->alignment = clcf->directio_alignment;

        ctx->pool = r->pool;
        ctx->bufs = conf->bufs;
        ctx->tag = (ngx_buf_tag_t) &ngx_http_copy_filter_module;
        /* 可以看到 output_filter 就是下一个 body filter 节点 */
        ctx->output_filter = (ngx_output_chain_filter_pt)
                                  ngx_http_next_body_filter;
        /* 此时 filter ctx 为当前的请求 */
        ctx->filter_ctx = r;

    ...

        if (in && in->buf && ngx_buf_size(in->buf)) {
            r->request_output = 1;
        }
    }

    ...

    for ( ;; ) {
        /* 最关键的函数，下面会详细分析 */
        rc = ngx_output_chain(ctx, in);

        if (ctx->in == NULL) {
            r->buffered &= ~NGX_HTTP_COPY_BUFFERED;

        } else {
            r->buffered |= NGX_HTTP_COPY_BUFFERED;
        }

        ...

        return rc;
    }
}

```

上面的代码去掉了 AIO 相关的部分，函数首先设置并初始化 context，接着调用 ngx_output_chain 函数，这个函数实际上包含了 copy filter 模块的主要逻辑，它的原型为：

ngx_int_t ngx_output_chain(ngx_output_chain_ctx_t *ctx, ngx_chain_t *in)

分段来看它的代码，下面这段代码是一个快捷路径（short path），也就是说当能直接确定所有的 in chain 都不需要复制的时，可以直接调用 output_filter 来交给剩下的 filter 去处理：

```
if (ctx->in == NULL && ctx->busy == NULL) {

    /*
     * the short path for the case when the ctx->in and ctx->busy chains
     * are empty, the incoming chain is empty too or has the single buf
     * that does not require the copy
     */

    if (in == NULL) {
        return ctx->output_filter(ctx->filter_ctx, in);
    }

    if (in->next == NULL
#if (NGX_SENDFILE_LIMIT)
            && !(in->buf->in_file && in->buf->file_last > NGX_SENDFILE_LIMIT)
#endif
        && ngx_output_chain_as_is(ctx, in->buf))
    {
        return ctx->output_filter(ctx->filter_ctx, in);
    }
}

```

上面可以看到了一个函数 ngx_output_chain_as_is，这个函数很关键，下面还会再次被调用，这个函数主要用来判断是否需要复制 buf。返回 1,表示不需要拷贝，否则为需要拷贝：

```
static ngx_inline ngx_int_t
ngx_output_chain_as_is(ngx_output_chain_ctx_t *ctx, ngx_buf_t *buf)
{
    ngx_uint_t  sendfile;

    /* 是否为特殊 buf（special buf），是的话返回 1，也就是不用拷贝 */
    if (ngx_buf_special(buf)) {
        return 1;
    }

    /* 如果 buf 在文件中，并且使用了 directio 的话，需要拷贝 buf */
    if (buf->in_file && buf->file->directio) {
        return 0;
    }

    /* sendfile 标记 */
    sendfile = ctx->sendfile;

#if (NGX_SENDFILE_LIMIT)
    /* 如果 pos 大于 sendfile 的限制，设置标记为 0 */
    if (buf->in_file && buf->file_pos >= NGX_SENDFILE_LIMIT) {
        sendfile = 0;
    }

#endif

    if (!sendfile) {
        /* 如果不走 sendfile，而且 buf 不在内存中，则我们就需要复制到内存一份 */
        if (!ngx_buf_in_memory(buf)) {
            return 0;
        }

        buf->in_file = 0;
    }

    /* 如果需要内存中有一份拷贝，而并不在内存中，此时返回 0，表示需要拷贝 */
    if (ctx->need_in_memory && !ngx_buf_in_memory(buf)) {
        return 0;
    }

    /* 如果需要内存中有可修改的拷贝，并且 buf 存在于只读的内存中或者 mmap 中，则返回 0 */
    if (ctx->need_in_temp && (buf->memory || buf->mmap)) {
        return 0;
    }

    return 1;
}

```

上面有两个标记要注意，一个是 need_in_memory ，这个主要是用于当使用 sendfile 的时候，Nginx 并不会将请求文件拷贝到内存中，而有时需要操作文件的内容，此时就需要设置这个标记。然后后面的 body filter 就能操作内容了。

第二个是 need_in_temp，这个主要是用于把本来就存在于内存中的 buf 复制一份可修改的拷贝出来，这里有用到的模块有 charset，也就是编解码 filter。

然后接下来这段是复制 in chain 到 ctx->in 的结尾，它是通过调用 ngx_output_chain_add_copy 来进行 add copy 的，这个函数比较简单，这里就不分析了，不过只有一个要注意的地方，那就是如果 buf 是存在于文件中，并且 file_pos 超过了 sendfile limit，此时就会切割 buf 为两个 buf，然后保存在两个 chain 中，最终连接起来：

```
/* add the incoming buf to the chain ctx->in */

if (in) {
    if (ngx_output_chain_add_copy(ctx->pool, &ctx->in, in) == NGX_ERROR) {
        return NGX_ERROR;
    }
}

```

然后就是主要的逻辑处理阶段。这里 nginx 做的非常巧妙也非常复杂，首先是 chain 的重用，然后是 buf 的重用。

先来看 chain 的重用。关键的几个结构以及域：ctx 的 free，busy 以及 ctx->pool 的 chain 域。

其中每次发送没有发完的 chain 就放到 busy 中，而已经发送完毕的就放到 free 中，而最后会调用 ngx_free_chain 来将 free 的 chain 放入到 pool->chain 中,而在 ngx_alloc_chain_link 中，如果 pool->chain 中存在 chain 的话，就不用 malloc 了，而是直接返回 pool->chain，相关的代码如下：

```
/* 链接 cl 到 pool->chain 中 */
#define ngx_free_chain(pool, cl)                                             \
    cl->next = pool->chain;                                                  \
    pool->chain = cl

/* 从 pool 中分配 chain */
ngx_chain_t *
ngx_alloc_chain_link(ngx_pool_t *pool)
{
    ngx_chain_t  *cl;

    cl = pool->chain;
    /* 如果 cl 存在，则直接返回 cl */
    if (cl) {
        pool->chain = cl->next;
        return cl;
    }
    /* 否则才会 malloc chain */
    cl = ngx_palloc(pool, sizeof(ngx_chain_t));
    if (cl == NULL) {
        return NULL;
    }

    return cl;
}

```

然后是 buf 的重用，严格意义上来说 buf 的重用是从 free 中的 chain 中取得的，当 free 中的 buf 被重用，则这个 buf 对应的 chain 就会被链接到 ctx->pool 中，从而这个 chain 就会被重用。也就是说首先考虑的是 buf 的重用，只有当这个 chain 的 buf 确定不需要被重用(或者说已经被重用)的时候，chain 才会被链接到 ctx->pool 中被重用。

还有一个就是 ctx 的 allocated 域，这个域表示了当前的上下文中已经分配了多少个 buf，output_buffer 命令用来设置 output 的 buf 大小以及 buf 的个数。而 allocated 如果比 output_buffer 大的话，则需要先发送完已经存在的 buf，然后才能再次重新分配 buf。

来看代码，上面所说的重用以及 buf 的控制，代码里面都可以看的比较清晰。下面这段主要是拷贝 buf 前所做的一些工作，比如判断是否拷贝，以及给 buf 分贝内存等：

```
/* out 为最终需要传输的 chain，也就是交给剩下的 filter 处理的 chain */
out = NULL;
/* last_out 为 out 的最后一个 chain */
last_out = &out;
last = NGX_NONE;

for ( ;; ) {

    /* 开始遍历 chain */
    while (ctx->in) {

        /* 取得当前 chain 的 buf 大小 */
        bsize = ngx_buf_size(ctx->in->buf);

        /* 跳过 bsize 为 0 的 buf */
        if (bsize == 0 && !ngx_buf_special(ctx->in->buf)) {
            ngx_debug_point();

            ctx->in = ctx->in->next;

            continue;
        }

        /* 判断是否需要复制 buf */
        if (ngx_output_chain_as_is(ctx, ctx->in->buf)) {

            /* move the chain link to the output chain */
            /* 如果不需要复制，则直接链接 chain 到 out，然后继续循环 */
            cl = ctx->in;
            ctx->in = cl->next;

            *last_out = cl;
            last_out = &cl->next;
            cl->next = NULL;

            continue;
        }

        /* 到达这里，说明我们需要拷贝 buf，这里 buf 最终都会被拷贝进 ctx->buf 中，
           因此这里先判断 ctx->buf 是否为空 */
        if (ctx->buf == NULL) {

            /* 如果为空，则取得 buf，这里要注意，一般来说如果没有开启 directio 的话，
               这个函数都会返回 NGX_DECLINED */
            rc = ngx_output_chain_align_file_buf(ctx, bsize);

            if (rc == NGX_ERROR) {
                return NGX_ERROR;
            }

            /* 大部分情况下，都会落入这个分支 */
            if (rc != NGX_OK) {

                /* 准备分配 buf，首先在 free 中寻找可以重用的 buf */
                if (ctx->free) {

                    /* get the free buf */
                    /* 得到 free buf */
                    cl = ctx->free;
                    ctx->buf = cl->buf;
                    ctx->free = cl->next;
                    /* 将要重用的 chain 链接到 ctx->poll 中，以便于 chain 的重用 */
                    ngx_free_chain(ctx->pool, cl);

                } else if (out || ctx->allocated == ctx->bufs.num) {
                    /* 如果已经等于 buf 的个数限制，则跳出循环，发送已经存在的 buf。
                       这里可以看到如果 out 存在的话，nginx 会跳出循环，然后发送 out，
                       等发送完会再次处理，这里很好的体现了 nginx 的流式处理 */
                    break;

                } else if (ngx_output_chain_get_buf(ctx, bsize) != NGX_OK) {
                    /* 上面这个函数也比较关键，它用来取得 buf。接下来会详细看这个函数 */
                    return NGX_ERROR;
                }
            }
        }
        /* 从原来的 buf 中拷贝内容或者从文件中读取内容 */
        rc = ngx_output_chain_copy_buf(ctx);

        if (rc == NGX_ERROR) {
            return rc;
        }

        if (rc == NGX_AGAIN) {
            if (out) {
                break;
            }

            return rc;
        }

        /* delete the completed buf from the ctx->in chain */

        if (ngx_buf_size(ctx->in->buf) == 0) {
            ctx->in = ctx->in->next;
        }
        /* 分配新的 chain 节点 */
        cl = ngx_alloc_chain_link(ctx->pool);
        if (cl == NULL) {
            return NGX_ERROR;
        }

        cl->buf = ctx->buf;
        cl->next = NULL;
        *last_out = cl;
        last_out = &cl->next;
        ctx->buf = NULL;
    }
    ...
}

```

上面的代码分析的时候有个很关键的函数，那就是 ngx_output_chain_get_buf，这个函数当没有可重用的 buf 时用来分配 buf。

如果当前的 buf 位于最后一个 chain，则需要特殊处理，一是 buf 的 recycled 域，另外是将要分配的 buf 的大小。

先来说 recycled 域，这个域表示当前的 buf 需要被回收。而一般情况下 Nginx(比如在非 last buf)会缓存一部分 buf(默认是 1460 字节)，然后再发送，而设置了 recycled 的话，就不会让它缓存 buf，也就是尽量发送出去，然后以供回收使用。 因此如果是最后一个 buf，则不需要设置 recycled 域的，否则的话，需要设置 recycled 域。

然后就是 buf 的大小。这里会有两个大小，一个是需要复制的 buf 的大小，一个是配置文件中设置的大小。如果不是最后一个 buf，则只需要分配配置中设置的 buf 的大小就行了。如果是最后一个 buf，则就处理不太一样，下面的代码会看到：

```
static ngx_int_t
ngx_output_chain_get_buf(ngx_output_chain_ctx_t *ctx, off_t bsize)
{
    size_t       size;
    ngx_buf_t   *b, *in;
    ngx_uint_t   recycled;

    in = ctx->in->buf;
    /* 可以看到这里分配的 buf，每个 buf 的大小是配置文件中设置的 size */
    size = ctx->bufs.size;
    /* 默认有设置 recycled 域 */
    recycled = 1;
    /* 如果当前的 buf 是属于最后一个 chain 的时候，需要特殊处理 */
    if (in->last_in_chain) {
        /* 如果 buf 大小小于配置指定的大小，则直接按实际大小分配，不设置回收标记 */
        if (bsize < (off_t) size) {

            /*
             * allocate a small temp buf for a small last buf
             * or its small last part
             */
            size = (size_t) bsize;
            recycled = 0;

        } else if (!ctx->directio
                   && ctx->bufs.num == 1
                   && (bsize < (off_t) (size + size / 4)))
        {
            /*
             * allocate a temp buf that equals to a last buf,
             * if there is no directio, the last buf size is lesser
             * than 1.25 of bufs.size and the temp buf is single
             */

            size = (size_t) bsize;
            recycled = 0;
        }
    }
    /* 开始分配 buf 内存 */
    b = ngx_calloc_buf(ctx->pool);
    if (b == NULL) {
        return NGX_ERROR;
    }

    if (ctx->directio) {
        /* directio 需要对齐 */

        b->start = ngx_pmemalign(ctx->pool, size, (size_t) ctx->alignment);
        if (b->start == NULL) {
            return NGX_ERROR;
        }

    } else {
        /* 大部分情况会走到这里 */
        b->start = ngx_palloc(ctx->pool, size);
        if (b->start == NULL) {
            return NGX_ERROR;
        }
    }

    b->pos = b->start;
    b->last = b->start;
    b->end = b->last + size;
    /* 设置 temporary */
    b->temporary = 1;
    b->tag = ctx->tag;
    b->recycled = recycled;

    ctx->buf = b;
    /* 更新 allocated,可以看到每分配一个就加 1 */
    ctx->allocated++;

    return NGX_OK;
}

```

分配新的 buf 和 chain，并调用 ngx_output_chain_copy_buf 拷贝完数据之后，Nginx 就将新的 chain 链表交给下一个 body filter 继续处理：

```
if (out == NULL && last != NGX_NONE) {

    if (ctx->in) {
        return NGX_AGAIN;
    }

    return last;
}

last = ctx->output_filter(ctx->filter_ctx, out);

if (last == NGX_ERROR || last == NGX_DONE) {
    return last;
}

ngx_chain_update_chains(ctx->pool, &ctx->free, &ctx->busy, &out,
                        ctx->tag);
last_out = &out;

```

在其他 body filter 处理完之后，ngx_output_chain 函数还需要更新 chain 链表，以便回收利用，ngx_chain_update_chains 函数主要是将处理完毕的 chain 节点放入到 free 链表，没有处理完毕的放到 busy 链表中，另外这个函数用到了 tag，它只回收 copy filter 产生的 chain 节点。

#### ngx_http_write_filter_module 分析

ngx_http_write_filter_module 是最后一个 body filter，可以看到它的注册函数的特殊性：

```
static ngx_int_t
ngx_http_write_filter_init(ngx_conf_t *cf)
{
    ngx_http_top_body_filter = ngx_http_write_filter;

    return NGX_OK;
}

```

ngx_http_write_filter_module 是第一个注册 body filter 的模块，于是它也是最后一个执行的 body filter 模块。

直接来看 ngx_http_write_filter，下面的代码中去掉了一些调试代码：

```
ngx_int_t
ngx_http_write_filter(ngx_http_request_t *r, ngx_chain_t *in)
{
    off_t                      size, sent, nsent, limit;
    ngx_uint_t                 last, flush;
    ngx_msec_t                 delay;
    ngx_chain_t               *cl, *ln, **ll, *chain;
    ngx_connection_t          *c;
    ngx_http_core_loc_conf_t  *clcf;

    c = r->connection;

    if (c->error) {
        return NGX_ERROR;
    }

    size = 0;
    flush = 0;
    last = 0;
    ll = &r->out;

    /* find the size, the flush point and the last link of the saved chain */

    for (cl = r->out; cl; cl = cl->next) {
        ll = &cl->next;

#if 1
        if (ngx_buf_size(cl->buf) == 0 && !ngx_buf_special(cl->buf)) {
            return NGX_ERROR;
        }
#endif

        size += ngx_buf_size(cl->buf);

        if (cl->buf->flush || cl->buf->recycled) {
            flush = 1;
        }

        if (cl->buf->last_buf) {
            last = 1;
        }
    }

    /* add the new chain to the existent one */

    for (ln = in; ln; ln = ln->next) {
        cl = ngx_alloc_chain_link(r->pool);
        if (cl == NULL) {
            return NGX_ERROR;
        }

        cl->buf = ln->buf;
        *ll = cl;
        ll = &cl->next;

#if 1
        if (ngx_buf_size(cl->buf) == 0 && !ngx_buf_special(cl->buf)) {
            return NGX_ERROR;
        }
#endif

        size += ngx_buf_size(cl->buf);

        if (cl->buf->flush || cl->buf->recycled) {
            flush = 1;
        }

        if (cl->buf->last_buf) {
            last = 1;
        }
    }

    *ll = NULL;

    clcf = ngx_http_get_module_loc_conf(r, ngx_http_core_module);

    /*
     * avoid the output if there are no last buf, no flush point,
     * there are the incoming bufs and the size of all bufs
     * is smaller than "postpone_output" directive
     */

    if (!last && !flush && in && size < (off_t) clcf->postpone_output) {
        return NGX_OK;
    }
    /* 如果请求由于被限速而必须延迟发送时，设置一个标识后退出 */
    if (c->write->delayed) {
        c->buffered |= NGX_HTTP_WRITE_BUFFERED;
        return NGX_AGAIN;
    }
    /* 如果 buffer 总大小为 0，而且当前连接之前没有由于底层发送接口的原因延迟，
       则检查是否有特殊标记 */
    if (size == 0 && !(c->buffered & NGX_LOWLEVEL_BUFFERED)) {
        /* last_buf 标记，表示请求体已经发送结束 */
        if (last) {
            r->out = NULL;
            c->buffered &= ~NGX_HTTP_WRITE_BUFFERED;

            return NGX_OK;
        }
        /* flush 生效，而且又没有实际数据，则清空当前的未发送队列 */
        if (flush) {
            do {
                r->out = r->out->next;
            } while (r->out);

            c->buffered &= ~NGX_HTTP_WRITE_BUFFERED;

            return NGX_OK;
        }

        return NGX_ERROR;
    }
    /*　请求有速率限制，则计算当前可以发送的大小 */
    if (r->limit_rate) {
        limit = r->limit_rate * (ngx_time() - r->start_sec + 1)
                - (c->sent - clcf->limit_rate_after);

        if (limit <= 0) {
            c->write->delayed = 1;
            ngx_add_timer(c->write,
                          (ngx_msec_t) (- limit * 1000 / r->limit_rate + 1));

            c->buffered |= NGX_HTTP_WRITE_BUFFERED;

            return NGX_AGAIN;
        }

        if (clcf->sendfile_max_chunk
            && (off_t) clcf->sendfile_max_chunk < limit)
        {
            limit = clcf->sendfile_max_chunk;
        }

    } else {
        limit = clcf->sendfile_max_chunk;
    }

    sent = c->sent;
    /* 发送数据 */
    chain = c->send_chain(c, r->out, limit);

    if (chain == NGX_CHAIN_ERROR) {
        c->error = 1;
        return NGX_ERROR;
    }
    /* 更新限速相关的信息 */
    if (r->limit_rate) {

        nsent = c->sent;

        if (clcf->limit_rate_after) {

            sent -= clcf->limit_rate_after;
            if (sent < 0) {
                sent = 0;
            }

            nsent -= clcf->limit_rate_after;
            if (nsent < 0) {
                nsent = 0;
            }
        }

        delay = (ngx_msec_t) ((nsent - sent) * 1000 / r->limit_rate);

        if (delay > 0) {
            limit = 0;
            c->write->delayed = 1;
            ngx_add_timer(c->write, delay);
        }
    }

    if (limit
        && c->write->ready
        && c->sent - sent >= limit - (off_t) (2 * ngx_pagesize))
    {
        c->write->delayed = 1;
        ngx_add_timer(c->write, 1);
    }
    /* 更新输出链，释放已经发送的节点 */
    for (cl = r->out; cl && cl != chain; /* void */) {
        ln = cl;
        cl = cl->next;
        ngx_free_chain(r->pool, ln);
    }

    r->out = chain;
    /* 如果数据未发送完毕，则设置一个标记 */
    if (chain) {
        c->buffered |= NGX_HTTP_WRITE_BUFFERED;
        return NGX_AGAIN;
    }

    c->buffered &= ~NGX_HTTP_WRITE_BUFFERED;
    /* 如果由于底层发送接口导致数据未发送完全，且当前请求没有其他数据需要发送，
       此时要返回 NGX_AGAIN，表示还有数据未发送 */
    if ((c->buffered & NGX_LOWLEVEL_BUFFERED) && r->postponed == NULL) {
        return NGX_AGAIN;
    }

    return NGX_OK;
}

```

Nginx 将待发送的 chain 链表保存在 r->out，上面的函数先检查之前未发送完的链表中是否有 flush，recycled 以及 last_buf 标识，并计算所有 buffer 的大小，接着对新输入的 chain 链表做同样的事情，并将新链表加到 r->out 的队尾。

如果没有输出链表中没有被标识为最后一块 buffer 的节点，而且没有需要 flush 或者急着回收的 buffer，并且当前队列中 buffer 总大小不够 postpone_output 指令设置的大小（默认为 1460 字节）时，函数会直接返回。

ngx_http_write_filter 会调用 c->send_chain 往客户端发送数据，c->send_chain 的取值在不同操作系统，编译选项以及协议下（https 下用的是 ngx_ssl_send_chain）会取不同的函数，典型的 linux 操作系统下，它的取值为 ngx_linux_sendfile_chain，也就是最终会调用这个函数来发送数据。它的函数原型为：

```
ngx_chain_t *
ngx_linux_sendfile_chain(ngx_connection_t *c, ngx_chain_t *in, off_t limit)

```

第一个参数是当前的连接，第二个参数是所需要发送的 chain，第三个参数是所能发送的最大值。

首先看一下这个函数定义的一些重要局部变量：

send 表示将要发送的 buf 已经已经发送的大小；

sent 表示已经发送的 buf 的大小；

prev_send 表示上一次发送的大小，也就是已经发送的 buf 的大小；

fprev 和 prev-send 类似，只不过是 file 类型的；

complete 表示是否 buf 被完全发送了，也就是 sent 是否等于 send - prev_send；

header 表示需要是用 writev 来发送的 buf，也就是 only in memory 的 buf；

struct iovec *iov, headers[NGX_HEADERS] 这个主要是用于 sendfile 和 writev 的参数，这里注意上面 header 数组保存的就是 iovec。

下面看函数开头的一些初始化代码：

```
wev = c->write;

if (!wev->ready) {
    return in;
}

/* the maximum limit size is 2G-1 - the page size */

if (limit == 0 || limit > (off_t) (NGX_SENDFILE_LIMIT - ngx_pagesize)) {
    limit = NGX_SENDFILE_LIMIT - ngx_pagesize;
}

send = 0;

/* 设置 header，也就是 in memory 的数组 */
header.elts = headers;
header.size = sizeof(struct iovec);
header.nalloc = NGX_HEADERS;
header.pool = c->pool;

```

下面这段代码就是处理 in memory 的部分，然后将 buf 放入对应的 iovec 数组，处理核心思想就是合并内存连续并相邻的 buf(不管是 in memory 还是 in file)：

```
for (cl = in; cl && send < limit;  cl = cl->next) {

    if (ngx_buf_special(cl->buf)) {
        continue;
    }

    /* 如果既不在内存中，又不在文件中，则返回错误 */
    if (!ngx_buf_in_memory(cl->buf) && !cl->buf->in_file) {
        return NGX_CHAIN_ERROR;
    }

    /* 如果不只是在 buf 中，这是因为有时 in file 的 buf 可能需要内存中也有拷贝，
       如果一个 buf 同时 in memoey 和 in file 的话，Nginx 会把它当做 in file 来处理 */
    if (!ngx_buf_in_memory_only(cl->buf)) {
        break;
    }

    /* 得到 buf 的大小 */
    size = cl->buf->last - cl->buf->pos;

    /* 大于 limit 的话修改为 size */
    if (send + size > limit) {
        size = limit - send;
    }

    /* 如果 prev 等于 pos，则说明当前的 buf 的数据和前一个 buf 的数据是连续的 */
    if (prev == cl->buf->pos) {
        iov->iov_len += (size_t) size;

    } else {
        if (header.nelts >= IOV_MAX) {
            break;
        }
        /* 否则说明是不同的 buf，因此增加一个 iovc */
        iov = ngx_array_push(&header);
        if (iov == NULL) {
            return NGX_CHAIN_ERROR;
        }

        iov->iov_base = (void *) cl->buf->pos;
        iov->iov_len = (size_t) size;
    }

    /* 这里可以看到 prev 保存了当前 buf 的结尾 */
    prev = cl->buf->pos + (size_t) size;
    /* 更新发送的大小 */
    send += size;
}

```

然后是 in file 的处理，这里比较核心的一个判断就是 fprev == cl->buf->file_pos，和上面的 in memory 类似，fprev 保存的就是上一次处理的 buf 的尾部。这里如果这两个相等，那就说明当前的两个 buf 是连续的(文件连续)：

```
/* 如果 header 的大小不为 0 则说明前面有需要发送的 buf，
   并且数据大小已经超过限制则跳过 in file 处理 */
if (header.nelts == 0 && cl && cl->buf->in_file && send < limit) {
    /* 得到 file
    file = cl->buf;

    /* 开始合并 */
    do {
        /* 得到大小 */
        size = cl->buf->file_last - cl->buf->file_pos;

        /* 如果太大则进行对齐处理 */
        if (send + size > limit) {
            size = limit - send;

            aligned = (cl->buf->file_pos + size + ngx_pagesize - 1)
                       & ~((off_t) ngx_pagesize - 1);

            if (aligned <= cl->buf->file_last) {
                size = aligned - cl->buf->file_pos;
            }
        }

        /* 设置 file_size */
        file_size += (size_t) size;
        /* 设置需要发送的大小 */
        send += size;
        /* 和上面的 in memory 处理一样就是保存这次的 last */
        fprev = cl->buf->file_pos + size;
        cl = cl->next;

    } while (cl
             && cl->buf->in_file
             && send < limit
             && file->file->fd == cl->buf->file->fd
             && fprev == cl->buf->file_pos);
}

```

然后就是发送部分，这里 in file 使用 sendfile，in memory 使用 writev。处理逻辑比较简单，就是发送后判断发送成功的大小

```
if (file) {
#if 1
    if (file_size == 0) {
        ngx_debug_point();
        return NGX_CHAIN_ERROR;
    }
#endif
#if (NGX_HAVE_SENDFILE64)
        offset = file->file_pos;
#else
        offset = (int32_t) file->file_pos;
#endif

    /* 数据在文件中则调用 sendfile 发送数据 */
    rc = sendfile(c->fd, file->file->fd, &offset, file_size);

    ...

    /* 得到发送成功的字节数 */
    sent = rc > 0 ? rc : 0;

} else {
    /* 数据在内存中则调用 writev 发送数据 */
    rc = writev(c->fd, header.elts, header.nelts);

    ...
    /* 得到发送成功的字节数 */
    sent = rc > 0 ? rc : 0;
}

```

接下来就是需要根据发送成功的字节数来更新 chain：

```
/* 如果 send - prev_send == sent 则说明该发送的都发完了 */
if (send - prev_send == sent) {
    complete = 1;
}
/* 更新 congnect 的 sent 域 */
c->sent += sent;

/* 开始重新遍历 chain，这里是为了防止没有发送完全的情况，
   此时我们就需要切割 buf 了 */
for (cl = in; cl; cl = cl->next) {

    if (ngx_buf_special(cl->buf)) {
        continue;
    }

    if (sent == 0) {
        break;
    }
    /* 得到 buf size */
    size = ngx_buf_size(cl->buf);

    /* 如果大于当前的 size，则说明这个 buf 的数据已经被完全发送完毕了，
       因此更新它的域 */
    if (sent >= size){
        /* 更新 sent 域 */
        sent -= size;
        /* 如果在内存则更新 pos */
        if (ngx_buf_in_memory(cl->buf)) {
            cl->buf->pos = cl->buf->last;
        }
        /* 如果在 file 中则更显 file_pos */
        if (cl->buf->in_file) {
            cl->buf->file_pos = cl->buf->file_last;
        }

        continue;
    }

    /* 到这里说明当前的 buf 只有一部分被发送出去了，因此只需要修改指针。
       以便于下次发送 */
    if (ngx_buf_in_memory(cl->buf)) {
        cl->buf->pos += (size_t) sent;
    }
    /* 同上 */
    if (cl->buf->in_file) {
        cl->buf->file_pos += sent;
    }

    break;
}

```

最后一部分是一些是否退出循环的判断。这里要注意，Nginx 中如果发送未完全的话，将会直接返回，返回的就是没有发送完毕的 chain，它的 buf 也已经被更新。然后 Nginx 返回去处理其他的事情，等待可写之后再次发送未发送完的数据：

```
if (eintr) {
    continue;
}
/* 如果未完成，则设置 wev->ready 为 0 后返回 */
if (!complete) {
    wev->ready = 0;
    return cl;
}
/* 发送数据超过限制，或没有数据了 */
if (send >= limit || cl == NULL) {
    return cl;
}
/* 更新 in，也就是开始处理下一个 chain */
in = cl;

```

## subrequest 原理解析 (99%)

子请求并不是 http 标准里面的概念，它是在当前请求中发起的一个新的请求，它拥有自己的 ngx_http_request_t 结构，uri 和 args。一般来说使用 subrequest 的效率可能会有些影响，因为它需要重新从 server rewrite 开始走一遍 request 处理的 PHASE，但是它在某些情况下使用能带来方便，比较常用的是用 subrequest 来访问一个 upstream 的后端，并给它一个 ngx_http_post_subrequest_t 的回调 handler，这样有点类似于一个异步的函数调用。对于从 upstream 返回的数据，subrequest 允许根据创建时指定的 flag，来决定由用户自己处理(回调 handler 中)还是由 upstream 模块直接发送到 out put filter。简单的说一下 subrequest 的行为，nginx 使用 subrequest 访问某个 location，产生相应的数据，并插入到 nginx 输出链的相应位置（创建 subrequest 时的位置），下面用 nginx 代码内的 addition 模块(默认未编译进 nginx 核心，请使用–with-http_addition_module 选项包含此模块)来举例说明一下：

```
location /main.htm {
    # content of main.htm: main
    add_before_body /hello.htm;
    add_after_body /world.htm;
}
location /hello.htm {
    #content of hello.htm: hello
}
location /world.htm {
    #content of world.htm: world
}

```

访问/main.htm，将得到如下响应：

```
hello
main
world

```

上面的 add_before_body 指令发起一个 subrequest 来访问/hello.htm，并将产生的内容(hello)插入主请求响应体的开头，add_after_body 指令发起一个 subrequest 访问/world.htm，并将产生的内容(world)附加在主请求响应体的结尾。addition 模块是一个 filter 模块，但是 subrequest 既可以在 phase 模块中使用，也可以在 filter 模块中使用。

在进行源码解析之前，先来想想如果是我们自己要实现 subrequest 的上述行为，该如何来做？subrequest 还可能有自己的 subrequest，而且每个 subrequest 都不一定按照其创建的顺序来输出数据，所以简单的采用链表不好实现，于是进一步联想到可以采用树的结构来做，主请求即为根节点，每个节点可以有自己的子节点，遍历某节点表示处理某请求，自然的可以想到这里可能是用后根(序)遍历的方法，没错，实际上 Igor 采用树和链表结合的方式实现了 subrequest 的功能，但是由于节点（请求）产生数据的顺序不是固定按节点创建顺序(左->右)，而且可能分多次产生数据，不能简单的用后根(序)遍历。Igor 使用了 2 个链表的结构来实现，第一个是每个请求都有的 postponed 链表，一般情况下每个链表节点保存了该请求的一个子请求，该链表节点定义如下：

```
struct ngx_http_postponed_request_s {
    ngx_http_request_t               *request;
    ngx_chain_t                      *out;
    ngx_http_postponed_request_t     *next;
};

```

可以看到它有一个 request 字段，可以用来保存子请求，另外还有一个 ngx_chain_t 类型的 out 字段，实际上一个请求的 postponed 链表里面除了保存子请求的节点，还有保存该请求自己产生的数据的节点，数据保存在 out 字段；第二个是 posted_requests 链表，它挂载了当前需要遍历的请求（节点）， 该链表保存在主请求（根节点）的 posted_requests 字段，链表节点定义如下：

```
struct ngx_http_posted_request_s {
    ngx_http_request_t               *request;
    ngx_http_posted_request_t        *next;
};

```

在 ngx_http_run_posted_requests 函数中会顺序的遍历主请求的 posted_requests 链表：

```
void
ngx_http_run_posted_requests(ngx_connection_t *c)
{
    ...
    for ( ;; ) {
        /* 连接已经断开，直接返回 */
        if (c->destroyed) {
            return;
        }

        r = c->data;
        /* 从 posted_requests 链表的队头开始遍历 */
        pr = r->main->posted_requests;

        if (pr == NULL) {
            return;
        }

        /* 从链表中移除即将要遍历的节点 */
        r->main->posted_requests = pr->next;
        /* 得到该节点中保存的请求 */
        r = pr->request;

        ctx = c->log->data;
        ctx->current_request = r;

        ngx_log_debug2(NGX_LOG_DEBUG_HTTP, c->log, 0,
                       "http posted request: \"%V?%V\"", &r->uri, &r->args);
        /* 遍历该节点（请求） */
        r->write_event_handler(r);
    }
}

```

ngx_http_run_posted_requests 函数的调用点后面会做说明。

了解了一些实现的原理，来看代码就简单多了，现在正式进行 subrequest 的源码解析， 首先来看一下创建 subrequest 的函数定义：

```
ngx_int_t
ngx_http_subrequest(ngx_http_request_t *r,
    ngx_str_t *uri, ngx_str_t *args, ngx_http_request_t **psr,
    ngx_http_post_subrequest_t *ps, ngx_uint_t flags)

```

参数 r 为当前的请求，uri 和 args 为新的要发起的 uri 和 args，当然 args 可以为 NULL，psr 为指向一个 ngx_http_request_t 指针的指针，它的作用就是获得创建的子请求，ps 的类型为 ngx_http_post_subrequest_t，它的定义如下：

```
typedef struct {
    ngx_http_post_subrequest_pt       handler;
    void                             *data;
} ngx_http_post_subrequest_t;

typedef ngx_int_t (*ngx_http_post_subrequest_pt)(ngx_http_request_t *r,
    void *data, ngx_int_t rc);

```

它就是之前说到的回调 handler，结构里面的 handler 类型为 ngx_http_post_subrequest_pt，它是函数指针，data 为传递给 handler 的额外参数。再来看一下 ngx_http_subrequest 函数的最后一个是 flags，现在的源码中实际上只有 2 种类型的 flag，分别为 NGX_HTTP_SUBREQUEST_IN_MEMORY 和 NGX_HTTP_SUBREQUEST_WAITED，第一个就是指定文章开头说到的子请求的 upstream 处理数据的方式，第二个参数表示如果该子请求提前完成(按后序遍历的顺序)，是否设置将它的状态设为 done，当设置该参数时，提前完成就会设置 done，不设时，会让该子请求等待它之前的子请求处理完毕才会将状态设置为 done。

进入 ngx_http_subrequest 函数内部看看：

```
{
    ...
    /* 解析 flags， subrequest_in_memory 在 upstream 模块解析完头部，
       发送 body 给 downsstream 时用到 */
    sr->subrequest_in_memory = (flags & NGX_HTTP_SUBREQUEST_IN_MEMORY) != 0;
    sr->waited = (flags & NGX_HTTP_SUBREQUEST_WAITED) != 0;

    sr->unparsed_uri = r->unparsed_uri;
    sr->method_name = ngx_http_core_get_method;
    sr->http_protocol = r->http_protocol;

    ngx_http_set_exten(sr);
    /* 主请求保存在 main 字段中 */
    sr->main = r->main;
    /* 父请求为当前请求 */
    sr->parent = r;
    /* 保存回调 handler 及数据，在子请求执行完，将会调用 */
    sr->post_subrequest = ps;
    /* 读事件 handler 赋值为不做任何事的函数，因为子请求不用再读数据或者检查连接状态；
       写事件 handler 为 ngx_http_handler，它会重走 phase */
    sr->read_event_handler = ngx_http_request_empty_handler;
    sr->write_event_handler = ngx_http_handler;

    /* ngx_connection_s 的 data 字段比较关键，它保存了当前可以向 out chain 输出数据的请求，
       具体意义后面会做详细介绍 */
    if (c->data == r && r->postponed == NULL) {
        c->data = sr;
    }
    /* 默认共享父请求的变量，当然你也可以根据需求在创建完子请求后，再创建子请求独立的变量集 */
    sr->variables = r->variables;

    sr->log_handler = r->log_handler;

    pr = ngx_palloc(r->pool, sizeof(ngx_http_postponed_request_t));
    if (pr == NULL) {
        return NGX_ERROR;
    }

    pr->request = sr;
    pr->out = NULL;
    pr->next = NULL;
    /* 把该子请求挂载在其父请求的 postponed 链表的队尾 */
    if (r->postponed) {
        for (p = r->postponed; p->next; p = p->next) { /* void */ }
        p->next = pr;

    } else {
        r->postponed = pr;
    }
    /* 子请求为内部请求，它可以访问 internal 类型的 location */
    sr->internal = 1;
    /* 继承父请求的一些状态 */
    sr->discard_body = r->discard_body;
    sr->expect_tested = 1;
    sr->main_filter_need_in_memory = r->main_filter_need_in_memory;

    sr->uri_changes = NGX_HTTP_MAX_URI_CHANGES + 1;

    tp = ngx_timeofday();
    r->start_sec = tp->sec;
    r->start_msec = tp->msec;

    r->main->subrequests++;
    /* 增加主请求的引用数，这个字段主要是在 ngx_http_finalize_request 调用的一些结束请求和
       连接的函数中使用 */
    r->main->count++;

    *psr = sr;
    /* 将该子请求挂载在主请求的 posted_requests 链表队尾 */
    return ngx_http_post_request(sr, NULL);
}

```

到这时，子请求创建完毕，一般来说子请求的创建都发生在某个请求的 content handler 或者某个 filter 内，从上面的函数可以看到子请求并没有马上被执行，只是被挂载在了主请求的 posted_requests 链表中，那它什么时候可以执行呢？之前说到 posted_requests 链表是在 ngx_http_run_posted_requests 函数中遍历，那么 ngx_http_run_posted_requests 函数又是在什么时候调用？它实际上是在某个请求的读（写）事件的 handler 中，执行完该请求相关的处理后被调用，比如主请求在走完一遍 PHASE 的时候会调用 ngx_http_run_posted_requests，这时子请求得以运行。

这时实际还有 1 个问题需要解决，由于 nginx 是多进程，是不能够随意阻塞的（如果一个请求阻塞了当前进程，就相当于阻塞了这个进程 accept 到的所有其他请求，同时该进程也不能 accept 新请求），一个请求可能由于某些原因需要阻塞（比如访问 io），nginx 的做法是设置该请求的一些状态并在 epoll 中添加相应的事件，然后转去处理其他请求，等到该事件到来时再继续处理该请求，这样的行为就意味着一个请求可能需要多次执行机会才能完成，对于一个请求的多个子请求来说，意味着它们完成的先后顺序可能和它们创建的顺序是不一样的，所以必须有一种机制让提前完成的子请求保存它产生的数据，而不是直接输出到 out chain，同时也能够让当前能够往 out chain 输出数据的请求及时的输出产生的数据。作者 Igor 采用 ngx_connection_t 中的 data 字段，以及一个 body filter，即 ngx_http_postpone_filter，还有 ngx_http_finalize_request 函数中的一些逻辑来解决这个问题。

下面用一个图来做说明，下图是某时刻某个主请求和它的所有子孙请求的树结构：

[![`tengine.taobao.org/book/_images/chapter-12-1.png`](http://tengine.taobao.org/book/_images/chapter-12-1.png)](http://tengine.taobao.org/book/_images/chapter-12-1.png)  [http://tengine.taobao.org/book/_images/chapter-12-1.png]

图中的 root 节点即为主请求，它的 postponed 链表从左至右挂载了 3 个节点，SUB1 是它的第一个子请求，DATA1 是它产生的一段数据，SUB2 是它的第 2 个子请求，而且这 2 个子请求分别有它们自己的子请求及数据。ngx_connection_t 中的 data 字段保存的是当前可以往 out chain 发送数据的请求，文章开头说到发到客户端的数据必须按照子请求创建的顺序发送，这里即是按后序遍历的方法（SUB11->DATA11->SUB12->DATA12->(SUB1)->DATA1->SUB21->SUB22->(SUB2)->(ROOT)），上图中当前能够往客户端（out chain）发送数据的请求显然就是 SUB11，如果 SUB12 提前执行完成，并产生数据 DATA121，只要前面它还有节点未发送完毕，DATA121 只能先挂载在 SUB12 的 postponed 链表下。这里还要注意一下的是 c->data 的设置，当 SUB11 执行完并且发送完数据之后，下一个将要发送的节点应该是 DATA11，但是该节点实际上保存的是数据，而不是子请求，所以 c->data 这时应该指向的是拥有改数据节点的 SUB1 请求。

下面看下源码具体是怎样实现的，首先是 ngx_http_postpone_filter 函数：

```
static ngx_int_t
ngx_http_postpone_filter(ngx_http_request_t *r, ngx_chain_t *in)
{
    ...
    /* 当前请求不能往 out chain 发送数据，如果产生了数据，新建一个节点，
       将它保存在当前请求的 postponed 队尾。这样就保证了数据按序发到客户端 */
    if (r != c->data) {

        if (in) {
            ngx_http_postpone_filter_add(r, in);
            return NGX_OK;
        }
        ...
        return NGX_OK;
    }
    /* 到这里，表示当前请求可以往 out chain 发送数据，如果它的 postponed 链表中没有子请求，也没有数据，
       则直接发送当前产生的数据 in 或者继续发送 out chain 中之前没有发送完成的数据 */
    if (r->postponed == NULL) {

        if (in || c->buffered) {
            return ngx_http_next_filter(r->main, in);
        }
        /* 当前请求没有需要发送的数据 */
        return NGX_OK;
    }
    /* 当前请求的 postponed 链表中之前就存在需要处理的节点，则新建一个节点，保存当前产生的数据 in，
       并将它插入到 postponed 队尾 */
    if (in) {
        ngx_http_postpone_filter_add(r, in);
    }
    /* 处理 postponed 链表中的节点 */
    do {
        pr = r->postponed;
        /* 如果该节点保存的是一个子请求，则将它加到主请求的 posted_requests 链表中，
           以便下次调用 ngx_http_run_posted_requests 函数，处理该子节点 */
        if (pr->request) {

            ngx_log_debug2(NGX_LOG_DEBUG_HTTP, c->log, 0,
                           "http postpone filter wake \"%V?%V\"",
                           &pr->request->uri, &pr->request->args);

            r->postponed = pr->next;

            /* 按照后序遍历产生的序列，因为当前请求（节点）有未处理的子请求(节点)，
               必须先处理完改子请求，才能继续处理后面的子节点。
               这里将该子请求设置为可以往 out chain 发送数据的请求。  */
            c->data = pr->request;
            /* 将该子请求加入主请求的 posted_requests 链表 */
            return ngx_http_post_request(pr->request, NULL);
        }
        /* 如果该节点保存的是数据，可以直接处理该节点，将它发送到 out chain */
        if (pr->out == NULL) {
            ngx_log_error(NGX_LOG_ALERT, c->log, 0,
                          "http postpone filter NULL output",
                          &r->uri, &r->args);

        } else {
            ngx_log_debug2(NGX_LOG_DEBUG_HTTP, c->log, 0,
                           "http postpone filter output \"%V?%V\"",
                           &r->uri, &r->args);

            if (ngx_http_next_filter(r->main, pr->out) == NGX_ERROR) {
                return NGX_ERROR;
            }
        }

        r->postponed = pr->next;

    } while (r->postponed);

    return NGX_OK;
}

```

再来看 ngx_http_finalzie_request 函数：

```
void
ngx_http_finalize_request(ngx_http_request_t *r, ngx_int_t rc)
{
  ...
    /* 如果当前请求是一个子请求，检查它是否有回调 handler，有的话执行之 */
    if (r != r->main && r->post_subrequest) {
        rc = r->post_subrequest->handler(r, r->post_subrequest->data, rc);
    }

  ...

    /* 子请求 */
    if (r != r->main) {
        /* 该子请求还有未处理完的数据或者子请求 */
        if (r->buffered || r->postponed) {
            /* 添加一个该子请求的写事件，并设置合适的 write event hander，
               以便下次写事件来的时候继续处理，这里实际上下次执行时会调用 ngx_http_output_filter 函数，
               最终还是会进入 ngx_http_postpone_filter 进行处理 */
            if (ngx_http_set_write_handler(r) != NGX_OK) {
                ngx_http_terminate_request(r, 0);
            }

            return;
        }
        ...

        pr = r->parent;

        /* 该子请求已经处理完毕，如果它拥有发送数据的权利，则将权利移交给父请求， */
        if (r == c->data) {

            r->main->count--;

            if (!r->logged) {

                clcf = ngx_http_get_module_loc_conf(r, ngx_http_core_module);

                if (clcf->log_subrequest) {
                    ngx_http_log_request(r);
                }

                r->logged = 1;

            } else {
                ngx_log_error(NGX_LOG_ALERT, c->log, 0,
                              "subrequest: \"%V?%V\" logged again",
                              &r->uri, &r->args);
            }

            r->done = 1;
            /* 如果该子请求不是提前完成，则从父请求的 postponed 链表中删除 */
            if (pr->postponed && pr->postponed->request == r) {
                pr->postponed = pr->postponed->next;
            }
            /* 将发送权利移交给父请求，父请求下次执行的时候会发送它的 postponed 链表中可以
               发送的数据节点，或者将发送权利移交给它的下一个子请求 */
            c->data = pr;

        } else {
            /* 到这里其实表明该子请求提前执行完成，而且它没有产生任何数据，则它下次再次获得
               执行机会时，将会执行 ngx_http_request_finalzier 函数，它实际上是执行
               ngx_http_finalzie_request（r,0），也就是什么都不干，直到轮到它发送数据时，
               ngx_http_finalzie_request 函数会将它从父请求的 postponed 链表中删除 */
            r->write_event_handler = ngx_http_request_finalizer;

            if (r->waited) {
                r->done = 1;
            }
        }
        /* 将父请求加入 posted_request 队尾，获得一次运行机会 */
        if (ngx_http_post_request(pr, NULL) != NGX_OK) {
            r->main->count++;
            ngx_http_terminate_request(r, 0);
            return;
        }

        return;
    }
    /* 这里是处理主请求结束的逻辑，如果主请求有未发送的数据或者未处理的子请求，
       则给主请求添加写事件，并设置合适的 write event hander，
       以便下次写事件来的时候继续处理 */
    if (r->buffered || c->buffered || r->postponed || r->blocked) {

        if (ngx_http_set_write_handler(r) != NGX_OK) {
            ngx_http_terminate_request(r, 0);
        }

        return;
    }

 ...
}

```

## https 请求处理解析

### nginx 支持 ssl 简介

nginx-1.2.0 编译时默认是不支持 ssl 协议的，需要通过编译指令来开启对其支持：

```
./configure --with-http_ssl_module

```

在 nginx 源码中，ssl 相关代码用宏定义变量 NGX_HTTP_SSL 来控制是否开启。这给我们查找和阅读 ssl 相关代码带来了方便，如下:

ssl 协议工作在 tcp 协议与 http 协议之间。nginx 在支持 ssl 协议时，需要注意三点，其他时候只要正常处理 http 协议即可:

1.  tcp 连接建立时，在 tcp 连接上建立 ssl 连接
2.  tcp 数据接收后，将收到的数据解密并将解密后的数据交由正常 http 协议处理流程
3.  tcp 数据发送前，对(http)数据进行加密，然后再发送

以下章节将分别介绍这三点。

### ssl 连接建立(ssl 握手)

#### 对 ssl 连接建立的准备

更具 ssl 协议规定，在正式发起数据收发前，需要建立 ssl 连接，连接建立过程既 ssl 握手。nginx 在创建和初始化 http 请求阶段的同时为 tcp 连接建立做准备，主要流程在 ngx_http_init_request 函数中实现:

```
static void
ngx_http_init_request(ngx_event_t *rev)
{

...

#if (NGX_HTTP_SSL)

    {
    ngx_http_ssl_srv_conf_t  *sscf;

    sscf = ngx_http_get_module_srv_conf(r, ngx_http_ssl_module);
    if (sscf->enable || addr_conf->ssl) {

        /* c->ssl 不为空时，表示请求复用长连接(已经建立过 ssl 连接) */
        if (c->ssl == NULL) {

            c->log->action = "SSL handshaking";

            /*
             * nginx.conf 中开启 ssl 协议(listen 443 ssl;)，
             * 却没用设置服务器证书(ssl_certificate <certificate_path>;)
             */
            if (addr_conf->ssl && sscf->ssl.ctx == NULL) {
                ngx_log_error(NGX_LOG_ERR, c->log, 0,
                              "no \"ssl_certificate\" is defined "
                              "in server listening on SSL port");
                ngx_http_close_connection(c);
                return;
            }

            /*
             * 创建 ngx_ssl_connection_t 并初始化
             * openssl 库中关于 ssl 连接的初始化
             */
            if (ngx_ssl_create_connection(&sscf->ssl, c, NGX_SSL_BUFFER)
                != NGX_OK)
            {
                ngx_http_close_connection(c);
                return;
            }

            rev->handler = ngx_http_ssl_handshake;
        }

        /* ssl 加密的数据必须读到内存中 */
        r->main_filter_need_in_memory = 1;
    }
    }

#endif

...

}

```

ngx_http_init_request 大部分流程已经在前面章节分析过了，这个函数主要负责初始化 http 请求，此时并没有实际解析 http 请求。若发来的请求是经由 ssl 协议加密的，直接解析 http 请求就会出错。ngx_http_init_request 中 ssl 协议相关处理流程:

1，首先判断 c->ssl 是否为空。若不为空：说明这里是 http 长连接的情况，ssl 连接已经在第一个请求进入时建立了。这里只要复用这个 ssl 连接即可，跳过 ssl 握手阶段。

2.(1)，若 c->ssl 为空：需要进行 ssl 握手来建立连接。此时调用 ngx_ssl_create_connection 为 ssl 连接建立做准备。

ngx_ssl_create_connection 简化代码如下:

```
ngx_int_t
ngx_ssl_create_connection(ngx_ssl_t *ssl, ngx_connection_t *c, ngx_uint_t flags)
{
    ngx_ssl_connection_t  *sc;

    /* ngx_ssl_connection_t 是 nginx 对 ssl 连接的描述结构，记录了 ssl 连接的信息和状态 */
    sc = ngx_pcalloc(c->pool, sizeof(ngx_ssl_connection_t));

    sc->buffer = ((flags & NGX_SSL_BUFFER) != 0);

    /* 创建 openssl 库中对 ssl 连接的描述结构 */
    sc->connection = SSL_new(ssl->ctx);

    /* 关联(openssl 库)ssl 连接到 tcp 连接对应的 socket */
    SSL_set_fd(sc->connection, c->fd);

    if (flags & NGX_SSL_CLIENT) {
        /* upstream 中发起对后端的 ssl 连接，指明 nginx ssl 连接是客户端 */
        SSL_set_connect_state(sc->connection);

    } else {
        /* 指明 nginx ssl 连接是服务端 */
        SSL_set_accept_state(sc->connection);
    }

    /* 关联(openssl 库)ssl 连接到用户数据(当前连接 c) */
    SSL_set_ex_data(sc->connection, ngx_ssl_connection_index, c);

    c->ssl = sc;

    return NGX_OK;
}

```

2.(2)，设置连接读事件处理函数为 ngx_http_ssl_handshake，这将改变后续处理 http 请求的正常流程为：先进行 ssl 握手，再正常处理 http 请求。

3，标明当前待发送的数据须在内存中，以此可以让 ssl 对数据进行加密。由于开启了 ssl 协议，对发送出去的数据要进行加密，这就要求待发送的数据必须在内存中。 标识 r->main_filter_need_in_memory 为 1，可以让后续数据发送前，将数据读取到内存中 (防止在文件中的数据通过 sendfile 直接发送出去，而没有加密）。

#### 实际 ssl 握手阶段

由于在 ngx_http_init_request 中将连接读事件处理函数设置成 ngx_http_ssl_handshake，当连接中有可读数据时，将会进入 ngx_http_ssl_handshake 来处理(若未开启 ssl，将进入 ngx_http_process_request_line 直接解析 http 请求）

在 ngx_http_ssl_handshake 中，来进行 ssl 握手:

1，首先判断连接是否超时，如果超时则关闭连接

```
static void
ngx_http_process_request(ngx_http_request_t *r)
{
    if (rev->timedout) {
        ngx_log_error(NGX_LOG_INFO, c->log, NGX_ETIMEDOUT, "client timed out");
        c->timedout = 1;
        ngx_http_close_request(r, NGX_HTTP_REQUEST_TIME_OUT);
        return;
    }

```

2，首字节预读：从 tcp 连接中查看一个字节(通过 MSG_PEEK 查看 tcp 连接中数据，但不会实际读取该数据)，若 tcp 连接中没有准备好的数据，则重新添加读事件退出等待新数据到来。

```
n = recv(c->fd, (char *) buf, 1, MSG_PEEK);

if (n == -1 && ngx_socket_errno == NGX_EAGAIN) {

    if (!rev->timer_set) {
        ngx_add_timer(rev, c->listening->post_accept_timeout);
    }

    if (ngx_handle_read_event(rev, 0) != NGX_OK) {
        ngx_http_close_request(r, NGX_HTTP_INTERNAL_SERVER_ERROR);
    }

    return;
}

```

3，首字节探测：若成功查看 1 个字节数据，通过该首字节来探测接受到的数据是 ssl 握手包还是 http 数据。根据 ssl 协议规定，ssl 握手包的首字节中包含有 ssl 协议的版本信息。nginx 根据此来判断是进行 ssl 握手还是返回正常处理 http 请求(实际返回应答 400 BAD REQUEST)。

```
if (n == 1) {
    if (buf[0] & 0x80 /* SSLv2 */ || buf[0] == 0x16 /* SSLv3/TLSv1 */) {
        ngx_log_debug1(NGX_LOG_DEBUG_HTTP, rev->log, 0,
                       "https ssl handshake: 0x%02Xd", buf[0]);

        /*
         * 调用 ngx_ssl_handshake 函数进行 ssl 握手，连接双方会在 ssl 握手时交换相
         * 关数据(ssl 版本，ssl 加密算法，server 端的公钥等) 并正式建立起 ssl 连接。
         * ngx_ssl_handshake 函数内部对 openssl 库进行了封装。
         * 调用 SSL_do_handshake()来进行握手，并根据其返回值判断 ssl 握手是否完成
         * 或者出错。
         */
        rc = ngx_ssl_handshake(c);

        /*
         * ssl 握手可能需要多次数据交互才能完成。
         * 如果 ssl 握手没有完成，ngx_ssl_handshake 会根据具体情况(如需要读取更
         * 多的握手数据包，或者需要发送握手数据包）来重新添加读写事件
         */
        if (rc == NGX_AGAIN) {

            if (!rev->timer_set) {
                ngx_add_timer(rev, c->listening->post_accept_timeout);
            }

            c->ssl->handler = ngx_http_ssl_handshake_handler;
            return;
        }

        /*
         * 若 ssl 握手完成或者出错，ngx_ssl_handshake 会返回 NGX_OK 或者 NGX_ERROR, 然后 ngx_http_ssl_handshake 调用
         * ngx_http_ssl_handshake_handler 以继续处理
         */

        ngx_http_ssl_handshake_handler(c);

        return;

    } else {
        ngx_log_debug0(NGX_LOG_DEBUG_HTTP, rev->log, 0,
                       "plain http");

        r->plain_http = 1;
    }
}

```

需要特别注意，如果 ssl 握手完成，ngx_ssl_handshake 会替换连接的读写接口。这样，后续需要读写数据时，替换的接口会对数据进行加密解密。详细代码见下:

```
ngx_int_t
ngx_ssl_handshake(ngx_connection_t *c)
{
    n = SSL_do_handshake(c->ssl->connection);
    /* 返回 1 表示 ssl 握手成功 */
    if (n == 1) {
...
        c->ssl->handshaked = 1;

        c->recv = ngx_ssl_recv;
        c->send = ngx_ssl_write;
        c->recv_chain = ngx_ssl_recv_chain;
        c->send_chain = ngx_ssl_send_chain;

        return NGX_OK;
    }
...
}

```

4，探测为 http 协议：正常的 http 协议包处理直接调用 ngx_http_process_request_line 处理 http 请求，并将读事件处理函数设置成 ngx_http_process_request_line。(实际处理结果是向客户端返回 400 BAD REQUET，在 ngx_http_process_request 中又对 r->plain_http 标志的单独处理。)

```
    c->log->action = "reading client request line";

    rev->handler = ngx_http_process_request_line;
    ngx_http_process_request_line(rev);

} /* end of ngx_http_process_request() */

```

5，当 ssl 握手成功或者出错时，调用 ngx_http_ssl_handshake_handler 函数。

5.(1)，若 ssl 握手完成 (c->ssl->handshaked 由 ngx_ssl_handshake()确定握手完成后设为 1)，设置读事件处理函数为 ngx_http_process_request_line，并调用此函数正常处理 http 请求。

5.(2)，若 ssl 握手没完成（则说明 ssl 握手出错），则返回 400 BAD REQUST 给客户端。

至此，ssl 连接已经建立，此后在 ngx_http_process_request 中会读取数据并解密然后正常处理 http 请求。

```
static void
ngx_http_ssl_handshake_handler(ngx_connection_t *c)
{
    ngx_http_request_t  *r;

    if (c->ssl->handshaked) {

        /*
         * The majority of browsers do not send the "close notify" alert.
         * Among them are MSIE, old Mozilla, Netscape 4, Konqueror,
         * and Links.  And what is more, MSIE ignores the server's alert.
         *
         * Opera and recent Mozilla send the alert.
         */

        c->ssl->no_wait_shutdown = 1;

        c->log->action = "reading client request line";

        c->read->handler = ngx_http_process_request_line;
        /* STUB: epoll edge */ c->write->handler = ngx_http_empty_handler;

        ngx_http_process_request_line(c->read);

        return;
    }

    r = c->data;

    ngx_http_close_request(r, NGX_HTTP_BAD_REQUEST);

    return;
}

```

#### ssl 协议接受数据

ngx_http_process_request 中处理 http 请求，需要读取和解析 http 协议。而实际数据读取是通过 c->recv()函数来读取的，此函数已经在 ngx_ssl_handshake 中被替换成 ngx_ssl_recv 了。

ngx_ssl_recv 函数中调用 openssl 库函数 SSL_read()来读取并解密数据，简化后如下：

```
ssize_t ngx_ssl_recv(ngx_connection_t *c, u_char *buf, size_t size)
{
...
    n = SSL_read(c->ssl->connection, buf, size);
...
    return n;
}

```

#### ssl 协议发送数据

当 nginx 发送数据时，如使用 ngx_output_chain 函数发送缓存的 http 数据缓存链时，通过调用 c->send_chain()来发送数据。这个函数已经在 ngx_ssl_handshake 中被设置成 ngx_ssl_send_chain 了。ngx_ssl_send_chain 会进一步调用 ngx_ssl_write。而 ngx_ssl_write 调用 openssl 库 SSL_write 函数来加密并发送数据。

```
/* ngx_output_chain
 *  -> ..
 *   -> ngx_chain_writer
 *     -> c->send_chain (ngx_ssl_send_chain)
 *      -> ngx_ssl_write
 */
ssize_t ngx_ssl_write(ngx_connection_t *c, u_char *data, size_t size)
{
...
    n = SSL_write(c->ssl->connection, data, size);
...
    return n;
}

```

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# nginx 的 upstream 原理解析

## 无缓冲的处理流程

## 带缓冲的处理流程

## subrequest 访问 upstream 的处理流程

## 负载均衡算法

### round-robin 算法

### 基于 ip 的 hash 算法

## upstream 缓存机制

## 常用 upstream 模块分析

### proxy 模块

### fastcgi 模块

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.

# nginx 的 script 处理

## 正则表达式

## 变量

## 复杂变量

## if 的处理

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.