# Ansible 上手

怎么使用 Ansible，本章通过简单的例子还说明 Ansilbe 上手的基本步骤。

1.  安装 Ansible
2.  Ansible 管理哪些主机（主机目录管理）
3.  使用 Ansilbe 命令行管理主机（Ad-hoc command）
4.  使用 Ansilbe 脚本语言管理主机（脚本语言 Playbook）
5.  Ansible 的“命令”Module

# 安装 Ansile

这里以 RedHat 系 Linux 为例，其他系统请参考 ansible 的官网

## 管理员的电脑上：

*   安裝 Ansible 软件

```
$ # Redhat/CentOS Linux 上，Ansible 目前放在的 epel 源中
$ # Fedora 默认源中包含 ansible，直接安装包既可
$ sudo yum install epel-release 
$ sudo yum install ansible -y 
```

*   配置 Ansible 管理节点和主机的连接

其实就是配置从**管理节点到远程主机**之间基于 key（无密码的方式）的**SSH 连接**：

```
$ # 生成 ssh key
$ ssh-keygen
$ # 拷贝 ssh key 到远程主机，ssh 的时候就不需要输入密码了
$ ssh-copy-id remoteuser@remoteserver
$ # ssh 的时候不会提示是否保存 key
$ ssh-keyscan remote_servers >> ~/.ssh/known_hosts 
```

验证下有没有装好: 在管理节点执行下面的 ssh 命令，既**不需要输入密码**，也**不会提醒你存储 key**，那就成功啦。

```
$ ssh remoteuser@remoteserver 
```

## 被管理的远程主机：

不需要安装特殊的包，只需要 python>2.4，RedHat Linux 一般安装方式都自带。

# Ansible 管理哪些主机

# Ansible 的 Host Inventory

## 什么是 Host Inventory （ 主机目录 、主机清单）？

Host Inventory 是配置文件，用来告诉 Ansible 需要管理哪些主机。并且把这些主机根据按需分类。

可以根据用途分类：数据库节点，服务节点等；根据地点分类：中部，西部机房。

## Host Inventory 配置文件：

默认的文件是： **\/etc\/ansible\/hosts**

可以修改为其它的文件，下一章 Ansible 进阶中介绍。

## 例子

### 最简单的 hosts 文件：

```
192.168.1.50
aserver.example.org
bserver.example.org 
```

### 带分类的 hosts 文件:

```
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com 
```

# Ansible 用命令管理主机

Ansible 提供了一个命令行工具，在官方文档中起给命令行起了一个名字叫 Ad-Hoc Commands。

ansible 命令的格式是：

```
ansible <host-pattern> [options] 
```

## ansible 命令功能有哪些

先不用深纠命令的语法，讲完 module 那节，就可以理解语法。先从感官上，通过下面的命令认识下 ansible 的命令行都可以做什么。

#### 检查 ansible 安装环境

检查所有的 server，是否以 bruce 用户创建了 ansible 主机可以访问的环境。

`$ansible all -m ping -u bruce`

#### 执行命令

在所有的 server 上，以当前 bash 的同名用户，在远程主机执行“echo bash”

`$ansible all -a "/bin/echo hello"`

#### 拷贝文件

拷贝文件/etc/host 到远程机器（组）atlanta，位置为/tmp/hosts

`$ ansible web -m copy -a "src=/etc/hosts dest=/tmp/hosts"`

#### 安装包

远程机器（组）webservers 安装 yum 包

`$ ansible web -m yum -a "name=acme state=present"`

#### 添加用户

`$ ansible all -m user -a "name=foo password=<crypted password here>"`

#### 下载 git 包

`$ ansible web -m git -a "repo=git://foo.example.org/repo.git dest=/srv/myapp version=HEAD"`

#### 起服务

`$ ansible web -m service -a "name=httpd state=started"`

#### 并行执行

启动 10 个并行进行执行重起

`$ansible lb -a "/sbin/reboot" -f 10`

#### 查看远程主机的全部系统信息！！！

`$ ansible all -m setup`

# Ansible 用脚本管理主机

只有脚本才可以重用，避免总敲重复的代码。Ansible 脚本的名字叫 Playbook，使用的是 YAML 的格式，文件 以 yml 结尾。

注解：YAML 和 JSON 类似，是一种表示数据的格式。

## 执行脚本 playbook 的方法

```
$ansible-palybook deploy.yml 
```

## playbook 的例子

deploy.yml 的功能为 web 主机部署 apache, 其中包含以下部署步骤：

1.  安装 apache 包；
2.  拷贝配置文件 httpd，并保证拷贝文件后，apache 服务会被重启；
3.  拷贝默认的网页文件 index.html；
4.  启动 apache 服务；

playbook deploy.yml 包含下面几个关键字，每个关键字的含义：

*   **hosts**：为主机的 IP，或者主机组名，或者关键字 all
*   **remote_user**: 以哪个用户身份执行。
*   **vars**： 变量
*   **tasks**: playbook 的核心，定义顺序执行的动作 action。每个 action 调用一个 ansbile module。

*   > action 语法： `module： module_parameter=module_value`

*   > 常用的 module 有 yum、copy、template 等，module 在 ansible 的作用，相当于 bash 脚本中 yum，copy 这样的命令。下一节会介绍。

*   **handers**： 是 playbook 的 event，默认不会执行，在 action 里触发才会执行。多次触发只执行一次。

```
---
- hosts: web
 vars:
 http_port: 80
 max_clients: 200
 remote_user: root
 tasks:
 - name: ensure apache is at the latest version
 yum: pkg=httpd state=latest

 - name: Write the configuration file
 template: src=templates/httpd.conf.j2 dest=/etc/httpd/conf/httpd.conf
 notify:
 - restart apache

 - name: Write the default index.html file
 template: src=templates/index.html.j2 dest=/var/www/html/index.html

 - name: ensure apache is running
 service: name=httpd state=started
 handlers:
 - name: restart apache
 service: name=httpd state=restarted 
```

* * *

不懂 yml，没关系，上面的 deploy.yml 格式转化为 json 格式为：

```
[
  {
    "hosts": "web",
    "vars": {
      "http_port": 80,
      "max_clients": 200
    },
    "remote_user": "root",
    "tasks": [
      {
        "name": "ensure apache is at the latest version",
        "yum": "pkg=httpd state=latest"
      },
      {
        "name": "Write the configuration file",
        "template": "src=templates/httpd.conf.j2 dest=/etc/httpd/conf/httpd.conf",
        "notify": [
          "restart apache"
        ]
      },
      {
        "name": "Write the default index.html file",
        "template": "src=templates/index.html.j2 dest=/var/www/html/index.html"
      },
      {
        "name": "ensure apache is running",
        "service": "name=httpd state=started"
      }
    ],
    "handlers": [
      {
        "name": "restart apache",
        "service": "name=httpd state=restarted"
      }
    ]
  }
] 
```

提供 json 和 yml 互转的在线网站： [http:\/\/www.json2yaml.com\/](http://www.json2yaml.com/)

# Ansible 模块 Module

## 什么是 Ansible Module？

bash 无论在命令行上执行，还是 bash 脚本中，都需要调用 cd、ls、copy、yum 等命令；module 就是 Ansible 的“命令”，module 是 ansible 命令行和脚本中都需要调用的。常用的 Ansible module 有 yum、copy、template 等。

在 bash，调用命令时可以跟不同的参数，每个命令的参数都是该命令自定义的；同样，ansible 中调用 module 也可以跟不同的参数，每个 module 的参数也都是由 module 自定义的。

每个 module 的用法可以查阅文档。[`docs.ansible.com/ansible/modules_by_category.html`](http://docs.ansible.com/ansible/modules_by_category.html)

## Ansible 在命令行里使用 Module

在命令行中

> -m 后面接调用 module 的名字
> 
> -a 后面接调用 module 的参数

```
$ #使用 module copy 拷贝管理员节点文件/etc/hosts 到所有远程主机/tmp/hosts
$ ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts"
$ #使用 module yum 在远程主机 web 上安装 httpd 包
$ ansible web -m yum -a "name=httpd state=present" 
```

## Ansilbe 在 Playbook 脚本使用 Module

在 playbook 脚本中，tasks 中的每一个 action 都是对 module 的一次调用。在每个 action 中：

> 冒号:前面是 module 的名字
> 
> 冒号:后面是调用 module 的参数

```
---
  tasks:
  - name: ensure apache is at the latest version
    yum: pkg=httpd state=latest
  - name: write the apache config file
    template: src=templates/httpd.conf.j2 dest=/etc/httpd/conf/httpd.conf
  - name: ensure apache is running
    service: name=httpd state=started 
```

## Module 的特性

*   Module 是通过命令或者 Playbook 可以执行的 task 的插件

*   Module 是用 Python 写的。

*   Ansible 提供一些常用的 Module [`docs.ansible.com/ansible/modules_by_category.html`](http://docs.ansible.com/ansible/modules_by_category.html)

*   通过命令 ansible-doc 可以查看 module 的用法

*   Ansible 提供 API，用户可以自己写 Module