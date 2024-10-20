# Ubuntu - Samba 客户端设置和持久共享

> 原文：<https://acloudguru.com/blog/engineering/ubuntu-samba-client-setup-and-persistent-shares>

今天的主题是关于 Samba 客户端的设置以及在 Ubuntu 桌面上安装 Windows 共享(包括 Windows 8)的能力。虽然你将获得安装和配置 Ubuntu 系统访问 Windows 共享所需的基本信息，但如果你想更详细地了解它的运行(包括 Windows 端的那些部分)，请访问我们的姐妹网站 [Linux Academy](https://linuxacademy.com) 。在那里，你不仅可以看到如何配置你的 Ubuntu 系统来访问 Windows 共享，还可以看到如何将你的 Ubuntu 服务器设置为文件服务器。Linux Academy 提供了大量认证级别的课程，涵盖了广泛的 Linux 主题。除了演示视频之外，您还可以访问自己专用的 Amazon Web Services Linux 服务器来跟进每堂课！**站在一个地方**大多数家庭网络的默认设置使用 DHCP。DHCP 的问题在于，它会使管理单个系统变得更加困难，特别是当您没有运行和自动更新内部 DNS 服务时(大多数家庭用户不会这样做)。让你的生活更简单，用静态 IP 地址设置你的 Ubuntu 桌面。这很容易从你的“设置”应用程序中完成，特别是“网络”设置，看起来应该是这样的:你会注意到在这个特殊的例子中，我有一个本地 DNS 服务器用于我的网络和一个 Windows 域。对于共享或挂载 Samba 共享来说，这两者都不是必需的，这只是我的配置。**奠定基础**我们旅程的下一步是安装 Samba 客户端的所有组件。我们将执行:`sudo apt-get install cifs-utils samba-common system-config-samba samba winbind`严格地说，只需要“cifs-utils samba 和 samba-common”包，因为我们将在命令行配置所有内容，并且“system-config-samba”为我们提供了一个 GUI 管理工具，我们在这里包含它是为了以后更加灵活。此外，“winbind”软件包将有助于在查看您的本地网络时进行完整的主机名解析。系统准备是时候编辑我们的配置文件了。打开控制台并编辑“/etc/nsswitch.conf”文件，确保内容如下所示:

> passwd:compat group:compat shadow:compat hosts:files mdns 4 _ minimal[not found = return]DNS wins mdns 4 networks:files protocols:db files services:db files ethers:db files RPC:db files net group:NIS

此配置与默认安装的唯一区别是在“hosts:”行中间添加了“wins”参数。确保存在，保存文件并重新启动系统。**挂载合作伙伴**此时，打开你的任何文件管理软件(Dolphin、Gnome Commander 等)将允许你扫描你的网络，并查看任何可用的 Windows 共享，以及你拥有的凭证(如果你想看到这一点，请访问 [Linux Academy](https://linuxacademy.com) 观看详细展示所有这些的视频)。但是，我们将获取一个已知共享并手动装载它。首先，让我们创建一个目录，将我们的 Windows 共享附加到:`sudo mkdir -p /mnt/tmpshare`…现在将我们的共享连接到这个新目录:`sudo mount -t cifs //shareserver/sharename /mnt/tmpshare -o username=user`现在将发生两件事之一——( 1)系统将提示您输入密码，以便共享允许您装载它，或者(2)装载命令将会成功(如果您没有受密码限制的共享)。一个简单的“df -h”命令将检查共享现在是否显示为本地装载。在这一点上，让我们做一个“sudo umount /mnt/tmpshare ”,并准备好成为我们系统上的永久共享。**永久承诺**我们有了挂载位置，但我们希望每次重启桌面时都能看到该共享，我们不希望每次尝试连接时都必须键入密码(这会导致重启挂载失败)，也不希望在 fstab 文件中明文包含该共享的密码。第一件事是在我们的主目录中创建一个名为'的文件。“smbcredentials”，应该是这样的:

> username=userpassword=pwd

接下来是:`chmod 700 .smbcredentials`最后，对于我们的示例来说，/etc/fstab 文件应该是这样的:

> # /在安装期间位于/dev/sdb5 上 uuid = 0 D5 cc 054-7 DBE-4799-83d 8-e 19160 f 748 aa/ext 4 errors = remount-ro 0 1 #/boot 在安装期间位于/dev/sdc1 上 uuid = a76 dc3a 1-933 c-4c 08-9e4f-7563d 447 CDF 2/boot ext3 默认值 0 2# swap 在安装期间位于/dev/sdb1 上 uuid = 4769 b8smbcredentials，rw，noauto，用户 0 0

一旦我们保存了该文件，我们就可以通过执行:`sudo mount -a`来测试一切是否按预期工作，这将挂载当前没有挂载的'/etc/fstab '配置文件中引用的文件系统。快速“df -h”将检查装载命令是否成功。**最后的想法** Samba 是一个强大的客户端(正如你在 Linux Academy 上看到的，也是一个强大的 Samba 服务器)，它允许你轻松快速地远程共享和编辑文件。你可能需要更多的细节，所以请访问我们的姐妹网站或给我写信，我会一如既往地尽我所能提供帮助。