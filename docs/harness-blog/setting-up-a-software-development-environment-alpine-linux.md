# 在 Alpine Linux | Harness 上设置软件开发环境

> 原文：<https://www.harness.io/blog/setting-up-a-software-development-environment-alpine-linux>

在这篇文章中，我将讲述我为 C++和 Java 开发设置 Alpine Linux 工作站的经验，以及一些有用的 Alpine 知识、技巧和资源。

无论您是从事软件开发、DevOps 还是测试工程，如果您有一些 Docker 的工作经验，您很可能已经熟悉了 Alpine Linux。

Alpine 在最近几年很受欢迎，现在可能是码头工人最喜欢的 Linux。它最初是为路由器设计的，是一个安全、快速、轻量级的 Linux:一个基本的 Alpine 基础映像只需要 5 MB，比其他流行的 Linux 发行版少几个数量级(相比之下，Ubuntu 是 188 MB)。这一事实使它成为 Docker 图像的基础系统的理想选择，在这种情况下，小尺寸是可取的，特别是对于 OpenJDK Docker 图像，否则会占用数百 MB。

我是一个狂热的 Linux 粉丝，喜欢尝试新的环境，所以当我负责将 OverOps 代理移植到本地 Alpine Linux 时，我非常兴奋！但是在开始任何实际的移植工作之前，我必须为自己设置一个合适的 Alpine 开发环境。

在这篇文章中，我将讲述我为 C++和 Java 开发设置 Alpine Linux 工作站的经验，以及一些有用的 Alpine 知识、技巧和资源。

## 那么，Alpine Linux 给你带来了什么？

首先，我们来看一下 Alpine Linux 的主要特性。

项目主页很好地总结了这一点:“Alpine Linux 是一个独立的、非商业的、通用的 Linux 发行版，是为重视安全性、简单性和资源效率的高级用户设计的。”

最显著的安全特性是一个加固的、专门打了补丁的 Linux 内核。

### 饼图可执行文件。

所有 Alpine 可执行文件都是作为独立于位置的可执行文件构建的，它们的正确操作不依赖于绝对内存地址，就像共享库一样。此类可执行文件受地址空间布局随机化(ASLR)的约束，这是一种内核在随机内存位置之间动态转移程序的技术，有助于防止某些类型的攻击。

### 堆叠粉碎保护

这个特性在所有 Alpine 二进制文件中都实现了，它允许堆栈溢出的程序优雅地退出，而不是可怕地崩溃。它通常是通过使用特殊的 C/C++编译器选项(-fstack-protector-fstrong)构建来实现的，该选项向程序代码添加了堆栈金丝雀。这些是编译器生成的变量，标记每个函数中分配的堆栈帧的结束，当被覆盖时，指示堆栈溢出，麻烦就在前面，很像“煤矿中的金丝雀”。

### “少即是多”

虽然 Alpine 本身并不是一个特性，但是它的极简特性也使得它更安全，更不容易受到攻击，因为它几乎不携带额外的行李，因此可能的攻击面要小得多，特别是与其他 Linuxes 相比。不幸的是，与任何其他系统一样，它不是 100%防弹的，正如最近发现并修复的漏洞所揭示的那样。

因此，Alpine 尽可能地安全，但是它的小内存占用是它与其他 Linuxes 的真正区别。它是围绕两个组件构建的，这使得它特别紧凑。

### 巴布克斯

Busybox 是一个多合一、多用途的二进制文件。它的作者将其命名为“嵌入式 Linux 的瑞士军刀”，它为许多标准程序提供了核心功能，如 awk、cp、grep、gzip、sh 和 top。所有程序都用符号链接到/bin/busybox，它根据程序执行时使用的名称来标识要运行的程序。它提供了与 GNU 对等物的高度兼容性，通常具有减少的特性集，但不用担心——您通常可以通过安装相同名称的包(如 grep 或 tar)或捆绑包(如 coreutils)来获得完整的 GNU 功能。

Busybox 对 Alpine 的小尺寸非常重要——使用单个静态二进制文件减少了多个可执行文件的开销，并允许生成的二进制文件得到有效的大小优化。在 Alpine 3.8 上，它被压缩到只有 780 KB 的优度。

### 音乐图书馆。

musl libc 库是 GNU 的 libc 库 glibc 的一个紧凑和引人注目的替代，glibc 是大多数 Linux 发行版中使用的事实上的 libc 实现。与 glibc 相比，musl 的大小很小:它在 Alpine 3.8 上只有 572 KB，相比之下，例如 glibc 在 Ubuntu 16.04 上的大小为 3mb(3mb 是 libc.so 和 libm.so 的大小之和；在阿尔卑斯山中，libm.so 与 musl.so 符号相连。但是，musl 比它的小尺寸更重要:它提供了更严格的 POSIX 一致性、更好的安全性和更具竞争力的性能。musl 的作者 ETA labs 提供的这张对比图，非常详细地描述了 libc 实现之间的差异，并强调了 musl 的优势。

遗憾的是，这里有一个问题:将 musl 作为默认的 libc 实现会引入一个与其他基于 glibc 的 Linuxes 的兼容性问题。几乎任何 Linux 程序都与 libc 动态链接，而 glibc Linux 二进制文件将与 libc.so 链接，Alpine 二进制文件将与 musl.so 链接。因此，构建在 glibc 发行版(如 Ubuntu 和 RedHat)上的 Linux 可执行文件将无法在 Alpine 上运行，至少不能开箱即用。

## 阿尔卑斯山还有什么？

一个简洁的包管理器，叫做 apk。如果你已经熟悉 apt 包管理器，那就不要习惯 apk:基本的命令是 add、del、search 和 update，你可以开始了。如果你写过 Alpine Dockerfiles，你应该已经对 apk 相当了解了。

Alpine 中默认的 shell 是 busybox 提供的 ash。和 Alpine 中的任何东西一样，它也非常简洁，缺少一些 shell 的便利特性，比如自动完成。如果您像我一样喜欢 bash，只需安装 bash apk 包，并将其用作默认 shell。

## 让我们安装阿尔卑斯山

因此，在四处搜寻之后，是时候开始建立一个高山工作环境了。但是应该是什么环境呢？

## Docker 还是工作站？

在我看来，有两个选择:第一，在我的开发工作中使用 Alpine Docker，第二，在我的笔记本电脑上建立一个合适的 Alpine 工作站。这是我第一次真正接触 Docker，所以我不确定我是否应该朝那个方向深入，因为我想快速地站起来并运行。

此外，在那一点上，我认为 Docker 本质上是一个可任意使用的环境，对于日常开发来说可能并不理想(只是后来我才意识到 Docker 对于这一目的也很有用)。Alpine 工作站选项似乎更有吸引力，但是我也没有使用 Alpine 的经验，我不确定它对于开发来说有多舒适；有了 Docker 选项，至少我还有 Ubuntu 桌面可以使用。

最后，我选择了第二个选项，并匆忙在我已经双启动的 Ubuntu-Windows 戴尔 XPS 15 上安装了 Alpine。

## 安装阿尔卑斯 Linux

我兴高采烈地前往阿尔卑斯山下载页面。它提供了最新的 Alpine 版本(在撰写本文时是 3.8.1)，有 8 种不同的 ISO 版本。事实证明，Alpine 几乎可以在任何东西上运行，从服务器、dockers 和虚拟机，到嵌入式 ARM 设备和 Raspberry Pi。对于 x86_64 映像，我在“标准”和“扩展”之间进行了考虑。由于我的目标是一个成熟的桌面环境，我认为 Extended 会是一个更好的起点(后来证明是正确的),于是我点击了下载按钮。阿尔卑斯 ISO–检查。

现在真正有趣的部分来了——安装时间！谷歌搜索到的第一个“安装 alpine”是 Alpine Linux Wiki 上的安装页面。维基是一个不可思议的信息来源，几乎包含了所有关于阿尔卑斯山的信息。高山团队的工作真的很棒。

如安装页面所述，Alpine 可以三种不同的模式安装:

1.  无盘模式–在这种模式下，Alpine 从安装介质(如 CD 或 u 盘)启动，并加载到 RAM 中。没有数据写入磁盘，会话在重新启动时被丢弃。
2.  数据模式—类似于无盘，但/var 文件夹装载到可写介质上，在重新引导之间保持不变。
3.  Sys 模式–传统的硬盘安装。

很明显，选项 3 是最适合我的。因此，我在 Ubuntu 上按照以下步骤安装 Alpine:

1.  创建了一个可启动的阿尔卑斯山安装 USB(指南)；
2.  使用 gparted 收缩我现有的卷，为新的 Alpine 分区腾出空间，并将其格式化为 ext4
3.  将 Alpine 系统复制到新分区(指南)；
4.  通过运行 update-grub2，重新生成了我的 GRUB 菜单项；
5.  调整了 GRUB 条目，允许引导到 Alpine(见下文)。

大概就是这样。只需几个小时的工作，我现在就可以登录到我全新的 Alpine Linux 系统，三重启动 Ubuntu 和 Windows…太棒了！

## GRUB 调整

不过，食物部分有点棘手。安装 Alpine 并更新 GRUB 后，出现了一个新的菜单项:“未知 Linux 发行版(on /dev/nvme0n1p9)”。乐观地说，我点击了它，但是没有预期的“欢迎来到 Alpine”提示，在启动几秒钟后，我得到了一个内核恐慌…呀！

我做了一些实验来找出问题所在:我没有为我的文件系统加载合适的内核模块，特别是 ext4，update-grub2 也没有做到这一点。因此，我使用 Ubuntu GRUB 条目作为起点，并将以下菜单项放在/etc/grub.d 下的一个新文件中:

###### Alpine(和其他 Linuxes)的 GRUB 条目示例。
#将[my-uuid]和[my-partition]替换为以 root 用户身份运行的“blkid”输出的正确值。
menu entry " Alpine Linux " {
search-no-floppy-fs-uuid-set = root[my-uuid]
Linux/boot/vmlinuz-vanilla root =【my-partition】modules = SD-mod，usb-storage，ext 4
initrd/boot/initramfs-vanilla
}

## 开始使用 Alpine

现在我已经能够成功地引导到 Alpine，是时候进行一些基本的设置步骤了。

## 连接到 Wi-Fi

首先，我需要一个正常工作的互联网连接。

我的笔记本电脑没有有线网络插座，我的 USB RJ45 适配器也坏了，所以我唯一的选择就是 Wi-Fi。这个 Alpine Wiki 页面“连接到无线接入点”非常清楚地描述了 Wi-Fi 设置过程，因此我很快就能够连接到我的工作 Wi-Fi 网络(具体来说，我遵循了“手动配置”步骤)。

要在 Alpine 上开始使用 Wi-Fi，需要安装两个基本软件包:wireless-tools 和 wpa_supplicant。事实证明，我对扩展 ISO 的押注得到了回报:与标准 ISO 相比，它在其离线 apk 数据库中包含了这些包。这省去了我用一个同事的电脑从 Alpine packages 目录下载这些包的麻烦(顺便说一句，非常棒！)，将它们复制到 u 盘，安装到 Alpine 并安装。手动创建 apk 文件。

现在我有了网络连接，我可以更新 apk 存储库索引并安装一些便利的包，比如 bash、coreutils 和 nano。

已经有家的感觉了！

## 准备构建环境

在这一点上，我有了一个不错的 Alpine 命令行，所以我可以准备构建环境了。

与 Ubuntu 的 build-essential 类似，build-base 元包是安装最常见的构建工具和实用程序(包括 g++、make 和 binutils)的良好起点。为了构建我们的 C++栈，我需要其他几个包，包括 cmake 和 linux 头文件。

有些 C++项目只兼容某些编译器版本，具体来说，Alpine 3.8 附带了 gcc 6.4.0。据我所知，官方的 Alpine 资源库不包含以前的主要 gcc 版本，所以如果您需要 Alpine 上的不同 gcc 版本，您可以在 musl-cross-make 项目页面找到它(或兼容版本): https://github . com/just-containers/musl-cross-make/releases。我亲自测试过 gcc 4.9.4，效果非常好。这些人做得很好，这对一些人来说可能是真正的救命稻草。

接下来，安装 Java。官方的阿尔卑斯 JDK 实现是 OpenJDK，由 OpenJDK IcedTea 项目带给我们。OpenJDK 版本 7 和 8 是可用的，可以从 Alpine apk 库下载。不幸的是，目前还没有 Java 9、10 和 11 的 GA 版本(请参见 project Portola 主页和 github 的当前状态讨论)。目前，我对 openjdk8 包很满意。

最后，我期待着激烈的调试，并安装了 gdb 和 strace，以及 musl 和 OpenJDK 调试符号，它们分别位于 musl-dbg 和 openjdk8-dbg 包中。来吧，阿尔卑斯山！

提示:对于最新的 edge 包，您可能希望将 edge 存储库添加到/etc/apk/repositories 文件中，如包管理页面中所述。例如，lldb 目前仅在 edge 中可用。然而，边缘包是实验性的，所以要小心使用。

## 安装桌面和 IDE

我必须承认，我第一次把 Alpine 想象成一个奇特的命令行环境，甚至没有看到自己使用鼠标，所以我很高兴了解到 Alpine 也是一个有能力的桌面环境！

Alpine 有多种桌面可供选择，包括 GNOME、MATE 和 Xfce。我对 Xfce 还不熟悉，所以我决定试一试。一如既往，Alpine Wiki 是一个很好的起点。“Xfce setup”页面详细介绍了安装和启动 Xfce 所需的所有步骤。最重要的是，要安装的包是 xfce 4——桌面本身，以及 alpine-desktop。后者是一个非常全面的元包，提供默认的 Alpine 桌面体验，包括 Firefox web 浏览器、AbiWord 编辑器、Audacious 声音播放器等。

提示:不要忘记安装一个字体包，比如 font-noto。否则，你将会看到奇怪的空白方块，一些 GUI 应用程序可能会崩溃或运行不正常。这花了我相当多的时间才弄明白！

我的 Alpine 难题中缺少的最后一块是为 C/C++和 Java 开发找到合适的 ide。在这里，musl-glibc 兼容性问题变得非常棘手。例如，找不到与 musl 兼容的 Eclipse CDT 版本，所以没有 glibc 兼容层，就不可能在 Alpine 上运行 Eclipse。我对命令行很满意，但是我真的希望手边有一个合适的 IDE，特别是用于调试。

幸运的是，我们有 JetBrains IDE 家族。来自 JetBrains 的 IntelliJ、CLion 和其他 ide 都运行在 JVM 上，并不直接依赖于底层的 libc 实现。由于 OpenJDK JVM 与 musl 完全兼容，任何标准的 Java 应用程序都应该可以开箱即用，IntelliJ 和 CLion 也是如此。我是 IntelliJ 的忠实粉丝，所以我很高兴看到它在 Alpine 上也工作得很好。真是松了一口气！

值得注意的是，虽然 IntelliJ 的社区版对每个人都是免费的，但 CLion 只有 30 天的评估期是免费的(尽管学生可以申请免费的学术许可证)。现在，这就足够了。

就这样，我已经设置好了我的 Alpine 桌面环境！

## 摘要

由于在容器中的广泛使用，Alpine 已经成为最重要的 Linux 发行版之一。虽然它的核心非常纤薄和简约，但它可以很容易地与我们熟悉的其他流行 Linux 发行版的大多数程序和功能堆叠在一起。经过一些努力，Alpine 安装可以变成一个合适的桌面环境，没有什么可羞愧的。

然而，阿尔卑斯山有一个学习曲线，所以它肯定不适合每个人。因为它的用户群相对较小，所以可用的兼容软件较少，而且你可能会发现 musl Alpine 还不支持需求包。Alpine 附带了优秀的文档，但另一方面，故障排除资源和讨论要少得多(想想 StackOverflow)，所以您可能最终不得不自己诊断和解决问题。

总的来说，Alpine 有很多很棒的东西，我目前很乐意为它工作。希望你也会！

### 奖励–Alpine 3.8 IntelliJ docker 文件

几周后，随着我对 docker 越来越熟悉，我开始熟悉数据卷，这对于在主机和 Docker 容器之间共享文件夹很有用。我还了解到，如果设置得当，Docker 也能愉快地运行 GUI 应用程序。

因此，本质上，整个 Alpine 构建和桌面环境可以封装在一个可重用的 Docker 映像中，我们的项目源代码可以与 Docker 共享，并用于 Alpine 容器上的开发。

所以，事不宜迟，这里有一个 Alpine 3.8 Dockerfile，带有 IntelliJ Community edition +前面提到的工具和实用程序。虽然不完美或不完整，但这张图片可能是您自己的 Alpine dockered 桌面的良好起点。要运行映像并启动 IntelliJ，请遵循 docker 文件顶部的说明。通过授予 Docker 对主机 X11 服务器的访问权限来启用 GUI。

尽情享受吧！

GitHub 上 Dockerfile 的链接:https://GitHub . com/shah ARV/docker/blob/master/alpine/dev/docker file