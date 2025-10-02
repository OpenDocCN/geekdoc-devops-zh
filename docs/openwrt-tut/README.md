# OpenWrt 🪜🧱路由器教程

# 史上最详细的 OpenWrt shadowsocks 路由器自动🪜🧱教程

手把手教你路由器刷 OpenWrt 固件，自动穿越万里长城。

## 本教程🪜🧱方案的特点

放弃建立黑名单的方案吧，被🧱的网站每天在大量增加，有限的人生不能在无穷的手工添加黑名单、重启路由器中渡过。

大道至简，一劳永逸！

*   建立国内重要网站名单，在国内进行 dns 查询
*   其他网站通过 shadowsocks 客户端向 shadowsocks 服务端进行 dns 查询
*   国内或亚洲的 IP 流量走国内通道
*   其他流量通过 shadowsocks 服务端转发
*   屏蔽 ISP 劫持相关 IP
*   屏蔽国内外的广告

## 知识若不分享，实在没有意义

2014 年 6 月 Dropbox 壮烈被🧱。

查资料发现，著名的开源路由器固件 OpenWrt 支持家里的路由器 TP-Link WR2543N V1 ，于是就给路由器安装了 OpenWrt 并设置为自动智能🪜🧱。

自由的感觉真好:　youtube, hulu, twitter, facebook, google...

什么是圣人，圣人就是得到和付出比较均衡的人。天地生我，我敬天地；父母育我，我亦养父母；网上获得知识，也要在网上分享知识。于是，花了许多天，查资料，写教程，调试固件，不知不觉一天就过去了。

希望你应用本教程后，也把你的过程写下来，合并到这个项目中来:
[`github.com/softwaredownload/openwrt-fanqiang`](https://github.com/softwaredownload/openwrt-fanqiang)

## 如何编译 OpenWrt shadowsocks 自动🪜🧱固件

*   首先把本项目 clone 到本地目录，如 ~/Downloads/openwrt-fanqiang
*   原始配置文件
    *   ~/Downloads/openwrt-fanqiang/openwrt/default 默认配置文件夹
    *   ~/Downloads/openwrt-fanqiang/openwrt/wndr4300 针对特定路由器型号的配置文件，此处以 wndr4300 为例
*   复制配置文件
    *   本地建立配置文件目录，如 ~/Downloads/openwrt-wndr4300
    *   复制默认配置文件夹下面的文件到 ~/Downloads/openwrt-wndr4300/ 下
    *   如果有针对特定路由器的配置文件，也复制到~/Downloads/openwrt-wndr4300/，并覆盖同名文件
*   修改配置文件，编译后就直接可以用了。否则刷上固件后登录路由器再修改。主要修改：
    *   openwrt-wndr4300/etc/shadowsocks.json
    *   openwrt-wndr4300/usr/bin/shadowsocks-firewall
    *   openwrt-wndr4300/etc/uci-defaults/defaults
*   编译自定义固件，设置 FILES=~/Downloads/openwrt-wndr4300

## 本项目规定的默认值

```
shadowsocks server:            1.0.9.8
shadowsocks server_port:    1098
shadowsocks local_port:        7654
shadowsocks tunnel_port:    3210
shadowsocks password:        killgfw
shadowsocks method:            aes-256-cfb
root login password:        fanqiang
WIFI password:                icanfly9876　　(for DIR-505 and TLWR2543 before 2015: wsjdw,8181) 
```

## 如何使用预编译🪜🧱固件：

*   shadowsocks 服务端按照本项目规定的默认值进行设置（除了 server IP)
*   路由器刷 OpenWrt shadowsocks 🪜🧱固件
*   登录路由器修改 server IP：

    ```
     # Modify 1.0.9.8 to your server IP address
      vi /etc/shadowsocks.json    　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　
      # Modify 1.0.9.8 to your server IP address
      vi /usr/bin/shadowsocks-firewall

      /etc/init.d/shadowsocks restart 
    ```

*   以上修改测试通过后，建议再修改 shadowsocks password, 路由器 root password
*   不建议修改端口号
*   少数时候需要重启路由器才能使修改生效

## 相关资源

*   Netgear WNDR4300 预编译🪜🧱固件(2015-12-23):
    [`software-download.name/2015/netgear-wndr4300-openwrt-fanqiang-gujian/`](https://software-download.name/2015/netgear-wndr4300-openwrt-fanqiang-gujian/)

*   D-Link DIR-505 预编译🪜🧱固件(2015-12-24):
    [`software-download.name/2014/dlink-dir-505-openwrt-fanqiang/`](https://software-download.name/2014/dlink-dir-505-openwrt-fanqiang/)

*   TP-Link TLWR2543 预编译🪜🧱固件(2015-12-24):
    [`software-download.name/2014/openwrt-ar71xx-generic-tl-wr2543-v1-squashfs-sysupgrade-bin-with-shadowsocks/`](https://software-download.name/2014/openwrt-ar71xx-generic-tl-wr2543-v1-squashfs-sysupgrade-bin-with-shadowsocks/)

*   OpenWrt 自动🪜🧱教程电子书下载
    [`software-download.name/2014/fanqiang-jiaocheng/`](https://software-download.name/2014/fanqiang-jiaocheng/)

*   shadowsocks-libev-polarssl_2.4.3.ar71xx.ipk (2015-12-20):
    [`software-download.name/2014/shadowsocks-libev-polarssl-ar71xx-ipk-latest/`](https://software-download.name/2014/shadowsocks-libev-polarssl-ar71xx-ipk-latest/)

## 授权许可

除特别声明外，本书中的内容使用[CC BY-SA 3.0 License](http://creativecommons.org/licenses/by-sa/3.0/)（创作共用 署名-相同方式共享 3.0 许可协议）授权，代码遵循[BSD 3-Clause License](https://github.com/softwaredownload/openwrt-fanqiang/blob/master/LICENSE)（3 项条款的 BSD 许可协议）。

## 在线阅读 OpenWrt 🪜🧱路由器教程:

*   [`www.gitbook.com/book/softwaredownload/openwrt-fanqiang/details`](https://www.gitbook.com/book/softwaredownload/openwrt-fanqiang/details)
*   [`github.com/softwaredownload/openwrt-fanqiang/blob/master/SUMMARY.md`](https://github.com/softwaredownload/openwrt-fanqiang/blob/master/SUMMARY.md)