# Nginx 开发从入门到精通

## 缘起

nginx 由于出色的性能，在世界范围内受到了越来越多人的关注，在淘宝内部它更是被广泛的使用，众多的开发以及运维同学都迫切的想要了解 nginx 模块的开发以及它的内部原理，但是国内却没有一本关于这方面的书，源于此我们决定自己来写一本。本书的作者为淘宝核心系统服务器平台组的成员，本书写作的思路是从模块开发逐渐过渡到 nginx 原理剖析。书籍的内容会定期在这里更新，欢迎大家提出宝贵意见，不管是本书的内容问题，还是字词错误，都欢迎大家提交 issue(章节标题的左侧有评注按钮)，我们会及时的跟进。

更新历史

| 日期 | 描述 |
| --- | --- |
| 2012/03/01 | 创建目录大纲 |
| 2012/03/28 | 增加了样章 |
| 2012/05/25 | 更新样章 |
| 2012/06/08 | 增加第五章 |
| 2012/06/11 | 增加第四章 |
| 2012/06/26 | 增加第六章(event module) |
| 2012/06/27 | 更新第五章部分内容 |
| 2012/07/04 | 更新第六章 event module 部分内容 |
| 2012/07/12 | 增加第十二章（请求头读取，subrequest 解析） |
| 2012/08/14 | 增加第二章(nginx 基础架构及基础概念) |
| 2012/08/14 | 增加第二章(ngx_str_t 数据结构介绍) |
| 2012/08/17 | 增加第七章(模块开发高级篇之变量) |
| 2012/08/25 | 增加第十一章(nginx 的启动阶段部分内容) |
| 2012/09/26 | 增加第二章(ngx_array_t,ngx_hash_t 及 ngx_pool_t 介绍) |
| 2012/10/08 | 增加第十一章(配置解析综述) |
| 2012/10/12 | 增加第二章(ngx_hash_wildcard_t,ngx_hash_combined_t 及 ngx_hash_keys_arrays_t 介绍) |
| 2012/10/21 | 增加第二章(ngx_chain_t,ngx_list_t 及 ngx_buf_t 介绍) |
| 2012/11/09 | 增加第十二章(请求体的读取和丢弃解析) |
| 2012/11/24 | 更新第二章(ngx_buf_t 的部分字段以及其他一些书写错误和表达) |
| 2012/12/18 | 更新第十一章(解析 http 块) |
| 2012/12/10 | 增加第三章的内容 |
| 2012/12/28 | 补充和完善了第三章的内容 |
| 2013/01/25 | 增加了第二章(nginx 的配置系统) |
| 2013/02/18 | 增加了第二章(nginx 的模块化体系结构, nginx 的请求处理) |
| 2013/03/05 | 增加了第十二章部分内容(多阶段请求处理) |
| 2013/03/08 | 完成第十一章第 1 节(配置解析综述、ngx_http_block) |
| 2013/04/16 | 完成第九章第 1 节(源码目录结构、configure 原理) |
| 2013/09/30 | 完成第十二章部分内容(多阶段执行链各个阶段解析) |
| 2013/10/11 | 完成第十二章部分内容(filter 解析) |
| 2013/10/11 | 完成第十二章部分内容(ssl 解析) |

## 版权申明

本书的著作权归作者淘宝核心系统服务器平台组成员所有。你可以：

*   下载、保存以及打印本书
*   网络链接、转载本书的部分或者全部内容，但是必须在明显处提供读者访问本书发布网站的链接
*   在你的程序中任意使用本书所附的程序代码，但是由本书的程序所引起的任何问题，作者不承担任何责任

你不可以：

*   以任何形式出售本书的电子版或者打印版
*   擅自印刷、出版本书
*   以纸媒出版为目的，改写、改编以及摘抄本书的内容

## 目录

*   上篇：nginx 模块开发篇
    *   背景介绍
        *   nginx 历史
        *   使用简介
        *   nginx 特点介绍
    *   nginx 平台初探(100%)
        *   初探 nginx 架构(100%)
        *   nginx 基础概念(100%)
            *   connection
            *   request
        *   基本数据结构(99%)
            *   ngx_str_t(100%)
            *   ngx_pool_t(100%)
            *   ngx_array_t(100%)
            *   ngx_hash_t(100%)
            *   ngx_hash_wildcard_t(100%)
            *   ngx_hash_combined_t(100%)
            *   ngx_hash_keys_arrays_t(100%)
            *   ngx_chain_t(100%)
            *   ngx_buf_t(99%)
            *   ngx_list_t(100%)
            *   ngx_queue_t(100%)
        *   nginx 的配置系统(100%)
            *   指令概述
            *   指令参数
            *   指令上下文
        *   nginx 的模块化体系结构
        *   模块概述
            *   模块的分类
        *   nginx 的请求处理
            *   请求的处理流程
    *   handler 模块(100%)
        *   handler 模块简介
        *   模块的基本结构
            *   模块配置结构
            *   模块配置指令
            *   模块上下文结构
            *   模块的定义
        *   handler 模块的基本结构
        *   handler 模块的挂载
            *   按处理阶段挂载
            *   按需挂载
        *   handler 的编写步骤
        *   示例: hello handler 模块
        *   handler 模块的编译和使用
            *   config 文件的编写
            *   编译
            *   使用
        *   更多 handler 模块示例分析
            *   http access module
            *   http static module
            *   http log module
    *   过滤模块 (90%)
        *   过滤模块简介 (90%)
            *   执行时间和内容 (90%)
            *   执行顺序 (90%)
            *   模块编译 (90%)
        *   过滤模块的分析 (90%)
            *   相关结构体 (90%)
            *   响应头过滤函数 (90%)
            *   响应体过滤函数 (90%)
    *   upstream 模块
        *   upstream 模块 (100%)
            *   upstream 模块接口
            *   memcached 模块分析
            *   本节回顾
        *   负载均衡模块 (100%)
            *   配置
            *   指令
            *   钩子
            *   初始化配置
            *   初始化请求
            *   peer.get 和 peer.free 回调函数
            *   本节回顾
    *   其他模块 (40%)
        *   core 模块 (40%)
            *   Nginx 的启动模块 (40%)
        *   event 模块 (40%)
            *   event 的类型和功能 (40%)
            *   accept 锁 (40%)
            *   定时器 (40%)
        *   mail 模块
            *   mail 模块的实现
            *   mail 模块的功能
    *   模块开发高级篇(30%)
        *   变量(80%)
            *   综述
            *   创建变量
            *   使用变量
        *   upstream
            *   使用 subrequest 访问 upstream
            *   超越 upstream
        *   使用 lua 模块
*   下篇：nginx 原理解析篇
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
            *   请求体读取(100%)
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
        *   subrequest 原理解析 (99%)
        *   https 请求处理解析
            *   nginx 支持 ssl 简介
            *   ssl 连接建立(ssl 握手)
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
*   附录 A 编码风格 (100%)
    *   Nginx 代码风格图示 (100%)
*   附录 B 常用 API
    *   B.1 字符串操作
    *   B.2 读取请求体
    *   B.3 操作请求头
    *   B.4 取参数
    *   B.5 URI 处理
    *   B.6 buffer 操作
    *   B.7 变量操作
    *   B.8 日志输出
    *   B.9 配置解析接口
    *   B.10 文件操作
    *   B.11 锁操作
*   附录 C 模块编译，调试与测试
    *   C.1 编译与安装
        *   环境要求
        *   下载
        *   Nginx 在 Windows 环境下安装
        *   Nginx 在 Linux 环境下安装
        *   测试
        *   在 Windows 环境下查看 nginx 进程
        *   在 Linux 环境下查看 Nginx 进程
    *   C.2 调试日志
        *   一，开启调试日志：
        *   二，日志级别分析：
        *   三，日志格式设置：
        *   四，调试日志的几个注意点：
    *   C.3 使用 GDB 调试
    *   C.4 功能测试
    *   C.5 性能/压力测试
    *   C.6 常见缺陷分析

## 团队成员

叔度 ([`blog.zhuzhaoyuan.com`](http://blog.zhuzhaoyuan.com))

雕梁 ([`www.pagefault.info`](http://www.pagefault.info))

文景 ([`yaoweibin.cn`](http://yaoweibin.cn))

李子 ([`blog.lifeibo.com`](http://blog.lifeibo.com))

卫越 ([`blog.sina.com.cn/u/1929617884`](http://blog.sina.com.cn/u/1929617884))

袁茁 ([`yzprofile.me`](http://yzprofile.me))

小熊 ([`dinic.iteye.com`](http://dinic.iteye.com))

吉兆 ([`jizhao.blog.chinaunix.net`](http://jizhao.blog.chinaunix.net))

静龙 ([`blog.csdn.net/fengmo_q`](http://blog.csdn.net/fengmo_q))

竹权 ([`weibo.com/u/2199139545`](http://weibo.com/u/2199139545))

公远 ([`100continue.iteye.com/`](http://100continue.iteye.com/))

布可 ([`weibo.com/sifeierss`](http://weibo.com/sifeierss))

© Copyright 2012, taobao. Created using [Sphinx](http://sphinx-doc.org/) 1.3.5.