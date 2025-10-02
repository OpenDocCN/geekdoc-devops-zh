# 第一章: 介绍 AAA 和 RADIUS

我很荣幸展示给你一个 FreeRADIUS 入门. 这本书将会帮助你部署一个可靠的, 稳定的, 和可扩展的 RADIUS 服务器在你的环境.

这个章节是用来介绍 RADIUS 和 FreeRADIUS. 我们将会介绍许多理论并且推荐你要留意他. 这将会提供给你关于 RADIUS 协议工作原理的一个好的基础, 并且将会在后续的章节中提供很多帮助.

## 在这一章我们将会

*   了解什么是 AAA, 和我们为什么需要他.
*   学习 RADIUS 从哪开始, 和他为什么如今如此有意义.
*   了解为什么 FreeRADIUS 作为一个 RADIUS 服务器确实表现突出.
*   理解 AAA, RADIUS 和 FreeRADIUS 之间的关系.

# Authentication, Authorization 和 Accounting

用户通过许多设备获取数据网络和网络资源的权限. 这个发生在一个大范围的硬件上. 以太网交换机, Wi-Fi access points, 和 VPN 服务器都提供网络访问.

当这些设备用来控制网络访问, 例如一个带有 WPA2 企业版安全实现的 WiFi access point 或者一个带有 802.1x(EAP)的基于端口的认证的以太网交换机, 他们被称做一个 NAS(Network Access Server).

所有这些设备需要进行一些形式的控制来确保合适的安全和使用. 这个要求通常被描述为 AAA(Authentication, Authorization 和 Accounting). AAA 有时也被称为"三 A 框架"(Triple A Framework). AAA 是一个高层次的架构模型, 可以被用各种方法实现.

AAA 是通过许多 RFC 来指定的. 一般的 AAA 架构是在 RFC2903 上指定的. 也有许多 RFC 涵盖不同的 AAA 方面.

## Authentication

Authentication 通常是考虑的第一步, 为了获取网络访问和他提供的服务. 这是一个过程用来确认是否 Alice 提供的 credential 是有效的. 提供 credential 的最常见的方式是通过用户名和密码. 其他方式像 one-time token, certificate, PIN numbers, 或者甚至 biometric scanning 也可以使用.

在成功 authentication 之后一个 session 被初始化. 这个 session 持续直到网络连接被终止掉.

### Alice 是谁?

Alice 和 Bob 是占位名. 事实上, 有一个完整的角色集合, 每一个代表一个指定的角色. 我们将会使用下面的占位名:

*   Alice: 一个想要访问我们网络的用户.
*   Bob: 另一个想要访问我们网络的用户.
*   Isaac: ISP(Internet Service Provider)/我们的网络

更多的介绍: [`en.wikipedia.org/wiki/Alice_and_Bob`](http://en.wikipedia.org/wiki/Alice_and_Bob)

下面的图片阐述了一个 authentication 过程通过使用常见的活动, 从 ATM 机中取钱. 这个实质上让你获取访问银行网络的权限(尽管他非常有限制).

## Authorization

Authorization 是一种 Isaac 控制资源使用的方式. 在 Alice 已经 authenticate 了她自己之后, Isaac 可以强加某些限制或者授予某些权利. Isaac 可以例如检查 Alice 从哪台设备访问网络和基于这点来做出决定. 他可以限制 Alice 可以拥有的打开的 session 的个数, 给她一个预先决定的 IP 地址, 只允许某些流量通过, 或者甚至执行 QoS 基于一个 SLA(Service Level Agreement, 服务级别协议).

Authorization 通常包含逻辑. 如果 Alice 是学生组的一部分那么在工作时间没有网络访问权限. 如果 Bob 通过一个 captive portal 访问网络, 那么一个带宽限制被强加来阻止他超量使用网络连接.

逻辑可以基于许多事情. Authorization 决定例如可以基于组成员或者你连接的 NAS 或者甚至你访问资源是星期几.

如果我们举前面 ATM 的例子, 我们可以看到如果 Alice 没有一个透支机制, 她会被限制她可以取出的钱的数量.

## Accounting

Accounting 是一种测量资源使用量的方式. 在 Isaac 已经确定谁是 Alice 并且在建立的 session 上强加合适的控制, 他也可以测量她的使用量. Accounting 是测量使用量的不间断的过程.

这允许 Isaac 来跟踪 Alice 花费了多少时间和资源在一个建立的 session 上. 获取 accounting 数据允许 Isaac 来对 Alice 的资源进行计费, 趋势分析, 和行为监视.

当 Alice 想要检查她的使用量和可取出的钱, ATM 提供这个功能. Isaac 银行也可以监视他的账户和发现是否她经常在月底前花光. 他们然后可以提供给他一个透支机制.

RADIUS 是一种协议用来提供在 TCP/IP 网络上的 AAA. 下一节将会继续更多关于 RADIUS 协议的内容.

# RADIUS

RADIUS 是 Remote Access Dial In User Service 的缩写. RADIUS 是一个 AAA 解决方案的一部分, 由 Livingston 企业发布到 Merit Neowrk 在 1991 年. Merit Network 是一个非盈利互联网提供商, 需要一种创新的方式来管理通过他们的网络 dial-in access to various Points-Of-Presence (POPs).

Livingston 企业提供的解决方案有一个中心用户存储用来 authentication. 这个可以被许多 RAS(Radio Access Station)(dial-in)服务器使用. Authorization 和 accounting 也可以在 AAA 满足的条件下完成. 另外一个 Livingston 解决方案的关键方面是包括代理来支持扩展.

RADIUS 协议随后在 1997 年作为 RFC 发布, 一些修改被提交, 并且现在我们有了 RFC2865 涵盖了 RADIUS 协议, 还有 RFC2866 涵盖了 RADIUS accounting. 也有许多额外的 RFC 涵盖某些 RADIUS 方面的增强. 让 RFC 工作从任何个人或供应商到在他们的设备或软件上实现 RADIUS 协议. 这个导致 RADIUS 的广泛采用来处理在 TCP/IP 网络上的 AAA. 你将会发现术语 RADIUS 既表示 RADIUS 协议也表示完整的 RADIUS C/S 系统. 含义应该根据上下文可以明确.

支持 RADIUS 协议和标准成为 NAS 供应商的事实标准需求. RADIUS 在广泛的地方被使用, 从拥有百万级用户的蜂窝移动网络提供商到小的 WISP 创业公司提供 local neighborhood with Internet 到实现使用 802.1x 来围栏他们的网路的 NAC(Network Access Control)企业网络. RADIUS 在所有这些地方和更多的地方可以被发现.

ISP 和网络管理员们应该熟悉 RADIUS, 由于他被许多控制访问 TCP/IP 网络的设备使用. 下面是一些例子:

*   一个带有 VPN 服务器的防火墙可以使用 RADIUS.
*   带有 WPA-2 企业加密的 Wi-Fi AP 包含 RADIUS.
*   当 Alice 通过 DSL 连接一个现有的电信设施时, 电信设备将会使用 RADIUS 协议来联系 Isaac 的 RADIUS 服务器为了决定她是否可以通过 DSL(代理)获得网络访问权限.

下一节将会总结在 RFC2865 上指定的 RADIUS 协议.

## RADIUS 协议[RFC2865]

这一节在技术层面探索发布在 RFC2865 上的 RADIUS 协议. RADIUS accounting 是排除在外的. 这个是发布在 RFC2866 并且有个关于他自己的章节会详细探索.

RADIUS 协议是一个 C/S 协议, 利用 UDP 来通信. 使用 UDP 而不是 TCP 暗示通信不是严格 on state. 在客户端和服务器端的一个典型的数据流有一个来自客户端的单一请求, 紧接着一个来自服务器端的单一回复构成. 这让 RADIUS 成为一个非常轻量级的协议并且对在低网速环境下的效率有帮助.

在客户端和服务器端之间可以建立成功的通信之前, 每一端必须定义一个 shared secret. 这个是用来 authenticate 客户端.

一个 NAS 作为一个 RADIUS 客户端. 因此当你读到关于一个 RADIUS 客户端, 他表示一个 NAS.

RADIUS 包有一个指定的格式, 定义在 RFC 里面. 在一个 RADIUS 包中, 两个关键的组件是:

*   code: 表示包的类型.
*   attribute: 携带 RADIUS 使用的必要数据.

让我们来研究一下 RADIUS 数据报的组成.

## 数据包

了解一个 RADIUS 数据包的格式将会大大帮助理解 RADIUS 协议. 让我们更细致的查看 RADIUS 数据包. 我们将会看一个简单的 authentication 请求. 一个客户端发送一个 Access-Request 包到服务器. 服务器返回一个 Access-Accept 包来表示成功.

显示在这里的 RADIUS 数据包只是一个 UDP 数据包的 payload. UDP 和 IP 协议的讨论已经超出这本书的范围了.

屏幕截图是捕捉在 RADIUS 客户端和服务器端之间的流量得到的. 这里的屏幕截图是一个简单的 Authentication 请求发送到一个 RADIUS 服务器的结果.

下面的截图显示来自 RADIUS 客户端的 Access-Request 包.

下面的截图显示 RADIUS 服务器返回给这个请求一个 Access-Accept 数据包.

让我们来研究一下这些数据包.

## Code

每一个数据包由一个 code 来识别. 这个域是一个字节. 这个 code 的值决定这个包的某些特性和需求. 下面的表格可以被用作一个索引来列出当前 RADIUS 数据包的定义的 code.

| RADIUS code(decimal) | Packet type | Sent by |
| --- | --- | --- |
| 1 | Access-Request | NAS |
| 2 | Access-Accept | RADIUS server |
| 3 | Access-Reject | RADIUS server |
| 4 | Accounting-Request | NAS |
| 5 | Accounting-Response | RADIUS server |
| 11 | Access-Challenge | RADIUS server |
| 12 | Status-Server(Experimental) |  |
| 13 | Status-Client(Experimental) |  |
| 255 | Reserved |  |

当跟 RADIUS 工作时, 了解这些 code 是有利的.

## Identifier

每个包的第二个字节包含一个唯一的标识符. 他是由客户端生成的, 并且用做辅助来匹配请求和回复. RADIUS 数据包是通过无连接的 UDP 传输的. 这样需要 RADIUS 来实现他自己的算法来提交从客户端的重试请求. 当一个客户端重发一个请求给服务器, 数据包的 identifier 将会保持不变. 服务器将会回复请求通过匹配在回复数据包中的 identifier.

## Length

这个是数据包的第 3, 第 4 个 byte. 他表示一直到哪里在数据包中是有用的数据. 在这个边界之外的字节被认为是填充和被默默地忽略.

## Authenticator

这个域占用 2 个 byte, 根据包是来自客户端还是服务器端有不同的格式. 他也依赖包的类型, 例如, Access-Request 或者 Accounting-Request. 如果他是一个请求, 这个域作为一个 Redquest Authenticator. 如果他是一个回复, 这个域作为一个 Response Authenticator.

一个 Request Authenticator 的值一个不会重复的随机数. Response Authenticator 的值是在回复包的许多域的 MD5 哈希值, 带有客户端和服务器端之间的共享密钥(shared secret).

如果请求包含 User-Password attribute, 那么这个 attribute 的值会被加密. 这个加密的值通常是把共享的密钥(shared secret)结合 authenticator 生成 MD5 哈希值, 然后与用户的密码进行异或生成的. 这就是为什么共享密钥(shared secret)要在客户端和服务端保持一致, 为了解密用户的密码.

## Attributes

RADIUS 包的其余部分包含 0 或者更多 attribute, 作为 AVP(Attribute Value Pairs). 这些 AVPs 的结尾是由包的 length 域指定的.

## Conclusion

RADIUS 包是通过 UDP 传输. code 域表示 RADIUS 包的类型. attribute 是用来提供指定的信息用于 authentication, authorization 和 accounting. 例如为了 authenticate 一个用户, User-Name 和 User-Password AVPs 将会和一些其他的 attribute 一起被包含在 Access-Request 包中.

## AVPs

AVPs 是 RADIUS 协议的苦力. AVPs 可以被分类为 check 或者 reply attribute. check attribute 是从客户端发到服务器端. reply attribute 是从服务器端发到客户端.

attribute 作为信息的载体在客户端和服务器端之间服务. 他们被客户端使用来提供他们自身的信息和连接到他上的用户. 他们也被使用当服务器端回复客户端时. 客户端然后可以使用这个回复来控制用户的连接基于在服务器端回复接收到的 AVPs.

下一节将会描述一个 AVP 的格式.

## Type

AVP 的第一个 byte 是 type 域. 这个域的数值是和一个 attribute 名称联系在一起的, 所以我们人类也可以理解. 这些 attribute 和数值的关系是由 IANA([`www.iana.org)控制的`](http://www.iana.org)控制的). attribute 的名称通常是有描述性的, 例如 User-Name(1), User-Password(2), 或者 NAS-IP-Address(4).

RADIUS 也允许扩展协议. attribute 类型 26(叫做 Vendor-Specific)允许这样做. Vendor-Specific 的 attribute 的值可以反过来包含由一个 vendor 管理的 Vendor Specific Attributes(VSAs).

## Length

length 域是 AVP 的第二个 byte. 这个与 RADIUS 包中的 length 一个含义, 用来表示 AVP 的长度. 这个方法允许 AVPs 带有不同大小的值由于 length 域将会标记 AVP 的结尾.

## Value

AVP 的 value 可以在大小上不同. 这个 value 域可以是 0 或者更多 byte. 这个 value 域可以包含一下数据类型中的一个: test, string, address, integer 或者 time.

Text 和 string 可以多达 253byte. Address, integer 和 time 是 4 个 byte 大小.

如果我们取出一个请求包中的 NAS-IP-Address AVP, 我们会看到长度是 6 byte. 一个 byte 表示类型, 一个 byte 表示长度, 然后 4 个 byte 表示 IP 地址, 总共 6byte.

下一节将会讨论 Vendor-Specific Attribute, 是标准的 AVPs 的扩展.

## Vendor-Specific Attributes(VSAs)

VSAs 允许 vendor 来定义他们自己的 attribute. attribute 定义的格式基本上是普通的 AVPs 带有额外的一个 vendor 域名. VSAs 是通过 AVP 类型 26 发送. 这表示 VSAs 是 AVPs 的一个扩展, 并且包含在 AVPs 之内.

这让 RADIUS 非常灵活, 并且允许一个 vendor 来创建扩展来自定义他们的 RADIUS 实现. 例如 CoovaChilli 有一个 VSA attribute 叫做 ChilliSpot-Max-Total-Octets. 当 CoovaChilli 客户端从 RADIUS 服务器接收到这个 attribute 的回复时, 他使用这个值来限制通过 captive portal 的数据.

NAS 将会默默地忽略任何他不支持的 VSAs. 一些 vendors 发布了他们的 VSAs, 但是这不是需要的. 其他仅仅列出他们在网站或者文档里. 这个然后可以被用来决定他们设备实现 RADIUS 的能力.

## Proxying 和 realms

RADIUS 协议允许扩展. Proxying 允许一个 RADIUS 服务器来作为一个客户端连接到另一个 RADIUS 服务器. 这个最终形成一个链条.

在 proxying 上的讨论也包含 realms. Realms 是名字用来组织用户和形成用户名的一部分. 一个用户名区分于 realm name 带有一个指定的分隔符. realm name 可以是用户名加上前缀或者后缀. 今天的流行的标准使用玉米作为后缀, 并且用@分割, 例如:`alice@freeradius.org`. 这个然而只是一个约定. realm 可以是任何名字, 分隔符也可以是任何符号. windows 用户通常用一个前缀指定域名, 用`\`作为分隔符, 例如: `my_domain\alice`.

当 RADIUS 服务器接收到一个请求带有用户名包含一个 realm 时, 他决定是否处理这个请求或者转发这个请求给另一个 RADIUS 服务器被设计用来处理这个指定的 realm. 这将会需要第二台 RADIUS 服务器应该有转发 RADIUS 服务器作为一个客户端定义, 并且他们也有一个共享的密钥(shared secret).

## RADIUS 服务器端

RADIUS 协议是基于 C/S 架构. RADIUS 服务器将会监听 UDP 端口 1812 和 1813\. 1812 端口是用于 authentication. 这将会包含 Access-Request, Access-Accept, Access-Reject, 和 Access-Challenge 包. 1813 端口用于 accounting. 这个将会包含 Accounting-Request 和 Accounting-Response 包.

一个客户端和服务器需要一个共享的密钥(shared secret)为了加密和解密在 RADIUS 包中的某些域.

## RADIUS 客户端

RADIUS 客户端通常提供访问 TCP/IP 网络. 客户端作为一个 RADIUS 服务器和一个想要网络权限的用户/设备之间的中间人.

RADIUS 的 proxying 功能也允许一个 RADIUS 服务器成为另一台 RADIUS 服务器的客户端, 将会最终形成一个链条.

来自 RADIUS 服务器的反馈不仅仅决定是否一个用户允许访问网络(authentication), 也可以引导客户端来强加某些限制在用户上(authorization). 限制的例子有在 session 上的一个时间限制或者限制连接速度.

是否强加推荐的对于用户 sessiion 的调整取决于客户端. 域名 RADIUS 协议的无状态的本质, 对于 RADIUS 服务器没有方法来指导是否客户单在执行推荐的限制.

Accounting 定义在一个单独的 RFC. 下一节将会总结在 RFC2866 里面的 RADIUS accounting.

## RADIUS accounting [RFC2866]

这一节探索 RADIUS 协议的 accounting 功能. accounting 是一种方法来跟踪资源的使用和通常用于计费.

## Operation

RADIUS accounting 服务器运行在 1813 端口. 当一个用户的 session 开始, NAS 发送一个 Accounting-Request 包给 RADIUS 服务器. 这个包必须包含某些 AVPs. 他是在成功 authentication 之后发送的第一个包. 服务器端将会确定接收通过发送一个匹配的 Accounting-Response 包.

在整个 session, NAS 可以发送可选的 update 报告关于一个指定用户的时间和数据使用. 当用户的 session 结束, NAS 通知服务器. 这个将关闭在用户的 session 期间的 accounting 细节.

RADIUS 客户端的功能为未来做准备比如服务器下线. NAS 将会然后依赖他是如何配置的, 重试或者联系另一个 RADIUS 服务器.

当一个 RADIUS 服务器作为一个转发代理到另一个 RADIUS 服务器时, 他将会作为 accounting 数据的中继. 他也可能在转发前本地记录 accounting 数据.

## 包格式

Accounting 包括 RADIUS code 4(Accounting-Request)和 code 5(Accounting-Response)包. Accounting 包像 authentication 包一样使用相同的 RADIUS 协议. 一个独特的 accounting 包的特性是 User-Password attribute 不在请求中发送.

看下面的 Wireshark 输出, 显示一个典型的 accounting 交易. 他从一个来自客户端的 Accounting-Request 开始.

服务器然后回复给客户端一个 Accounting-Response.

Accounting-Request 包也需要包含某些 AVPs. 让我们看一下在 accounting 中使用的重要的 AVPs.

## Acct-Status-Type [Type40]

这个包表示用户或者 NAS 的状态. 一个 NAS 可能发送临时的更新关于某个 session 的使用. 为了做这个, NAS 设置类型为 Interim-Update. 这个允许我们来近似实时地跟踪使用趋势.

RADIUS 服务器不会在一个 NAS 上检查. 如果一个 NAS 已经通知 RADIUS 服务器关于一个新连接的用户(status 类型 Start)并且然后 NAS 完全关闭, RADIUS 服务器上的记录将会仍然指示那个用户连接在 NAS 上, 而事实上没有. 这些记录被称为欺骗条目(rogue entry). 为了减少欺骗条目, 最好让 NAS 发送一个 Accounting-Off 紧接着一个 Accounting-On 包, 仅仅在 boot-up 之后, 并且也发送一个 Accounting-Off 包在关闭之前. 这个动作将会导致 RADIUS 关闭所有打开的记录对于任何连接到指定 NAS 的用户允许一个干净的启动.

欺骗条目尤其称为问题当你限制一个用户可以拥有的 session 的时候. 如果限制一个用户 session 的组件利用包含欺骗条目的数据, 那么计算将会不准确.

下面的数字到类型表格可以被用来索引可能的状态值.

| Decimal value | Status type |
| --- | --- |
| 1 | Start |
| 2 | Stop |
| 3 | Interim-Update |
| 7 | Accounting-On |
| 8 | Accounting-Off |
| 9-14 | Reserved for tunnel accounting |
| 15 | Reserved for failed |

尽管 Acct-Status-Type AVP 不是必须的, 他总是被包含.

## Acct-Input-Octets [Type42]

这个表示在 session 期间接收到的 byte 并且带有 Acct-Status-Type 有一个值 Interim-Update 或者 Stop.

注意值的限制. 4 个 byte 限制他到 4294967296\. 大多数现代 RADIUS 实现已经满足这个需要.

## Acct-Output-Octets [Type43]

这个表示在 session 期间发送的 byte, 并且带有 Acct-Status-Type, 值为 Interim-Update 或者 Stop.

注意值的限制. 4 个 byte 限制他为 4294967296\. 大多数现代 RADIUS 实现已经满足这个需要.

## Acct-Session-Id [Type44]

对于所有的 Accounting-Request 包的规定动作, 这个是一个唯一的值用来匹配 Start, Interim 和 Stop 记录. 所有的一个 session 的 Start, Interm, 和 Stop 记录应该有对于 Acct-Session-Id 相同的值.

## Acct-Session-Time [Type46]

这个名字是自解释的. 以秒为时间表示 session 的长度, 并且带有 Acct-Status-Type, 值为 Interim-Update 或者 Stop.

## Acct-Terminate-Cause [Type49]

这个伴随着一个 Acct-Status-Type AVP, 他的值设置为 Stop. 这个 AVP 的值为十进制数. 他与 Acct-Status-Type 的行为类似, 用一个指定的数值对应解析一个终止原因.

## Conclusion

这个带给我们 RADIUS accounting 的结束. 下一节我们将看某些 RFC, 添加功能和增强给 RFC2865 和 RFC2866 的 RADIUS 定义.

## RADIUS 扩展

在初始的 RFC 定义了通常的 RADIUS 和 RADIUS accounting, 许多扩展被建议来扩展 RADIUS 的使用或者提高一些弱点.

也有一个改进的 RADIUS 协议叫做 Diameter(一个文字游戏--两倍好于 RADIUS). 然后 Diameter 的接受非常缓慢, RADIUS 仍然保持事实的标准对于可预见的未来. 一个主要的原因可能是事实上许多 Diameter 的增强已经被许多 RADIUS 的扩展实现. 例如, RadSec 协议通过 TCP 和 TLS 传输 RADIUS 协议. 这个让 RADIUS 再漫游环境更容易扩展.

尽管有许多, 我们将只研究两个可能被使用的扩展.

## Dynamic Authorization extension [RFC5176]

这个扩展帮助创建从 RADIUS 服务器到 NAS 的反馈回环. 这个实际上交换了客户端和服务器端之间的角色. RADIUS 服务器成为 NAS 的一个客户端.

动态 authorization 允许 RADIUS 服务器来通知 NAS 关于已经做的改变给在 NAS 上一个用户的现有 session. 有 2 个这种扩展的流行应用.

## Disconnect-Message [DM]

也叫做一个 POD(Packet Of Disconnect), 这个用来断开一个现有用户的 session. RADIUS 服务器发送断开连接请求, 并且 NAS 必须回复无论断开是否成功.

## Change-of-Authorizaion Message [CoA]

这个消息提供数据来改变一个先有用户 session 的 authorization. 例如, 我们现在可以动态改变每个 session 的带宽限制. 这让我们可以当互联网连接下降时可以增加每个 session 的带宽. 反之, 也可以.

MikroTik RouterOS 包括这个功能在一些使用 RADIUS 的服务上.

下表列出包含的 RADIUS 包的 code 和名称

| RADIUS code(decimal) | Packet type | Sent by |
| --- | --- | --- |
| 40 | Disconnect-Request | RADIUS server |
| 41 | Disconnect-ACK | NAS |
| 42 | Disconnect-NAK | NAS |
| 43 | CoA-Request | RADIUS server |
| 44 | CoA-ACK | NAS |
| 45 | CoA-NAK | NAS |

## RADIUS support for EAP [RFC3579]

EAP 代表 Extensible Authentication Protocol. 他大多用于以太网交换机和 Wi-Fi Access Point 的安全.

EAP 支持一个外部 authorization 服务器的使用. RADIUS 可以是这样的一个服务器. EAP 将会使用 RADIUS 协议来 wrap EAP 数据在 AVP 内部为了 authenticate 一个连接.

这本书有个专门的章节关于 EAP, 由于他是 Wi-Fi 企业安全的如此一个重要的部分.

在下一节, 将会研究 FreeRADIUS 项目. FreeRADIUS 是一种 RADIUS 协议的实现, 并且他的许多扩展包括 2 个在这里提到的.

# FreeRADIUS

FreeRADIUS 是一个开源项目提供一个非常功能丰富的 RADIUS 协议的实现带有他的许多增强. 当人们提到 FreeRADIUS 的时候, 他们通常讨论服务器软件.

## 历史

FreeRADIUS 的开发在 1999 年开始, 在原始的 Livingston RADIUS 服务器的未来变得不确定的时候. 这需要创建一个新的 RADIUS 服务器, 开源并且包含活跃的社区.

FreeRADIUS 成功的获得一个可靠的名声, 并且能够打败大多数商业竞争者. 座右铭"世界上最流行的 RADIUS 服务器"已经没有可挑战德尔了, 使得他成为一个有效的陈述.

## 特色

FreeRADIUS 有许多特色, 促进了他的流行. 让我们看一下:

*   开源: 不仅仅是免费. 你可以自由的适配, 修改, 扩展和需要的修复. FreeRADIUS 在 GPL 协议下发布.
*   模块化: FreeRADIUS 包含许多模块. 你可以创建的模块给 FreeRADIUS 使用. 模块包括 LDAP 集成或者 SQL 后端支持. 也有 Perl 和 Python 模块, 让你在 FreeRADIUS 上使用这两种强大的脚本语言.
*   被大量使用: 一个人不会因为选择了 FreeRADIUS 而被解雇. 很容易从 ISP 和在部署的 FreeRADIUS 上有大量用户的大公司得到帮助. FreeRADIUS 执行了一个调查来决定 FreeRADIUS 的使用和部署大小. 调查的详细结果可以从他们的请求中得到.
*   活跃的社区: 因为 FreeRaDIUS 有这样大的用户基础, 很大的机会有其他人跟你遇到同样的问题. FreeRADIUS 有活跃的邮件列表带有可搜索的文档.
*   可得到的信息: 信息可能不在一个地方, 但是可以得到, 只是需要被发现. 有许多的 wiki 包含很多细节. 也有许多 man page 和配置文件, 写的很详细和容易 follow.
*   活跃地开发: FreeRADIUS 遵循"release early, release often"的座右铭. RADIUS 协议的新进展通常最先在 FreeRADIUS 中支持. 你可以期待每年有一个或者更多的 FreeRADIUS release.
*   商业支持: FreeRADIUS 的核心成员提供商业支持. 在 FreeRADIUS 有许多知识丰富的人可以提供付费支持. Network RADIUS SARL 是一个好的网站带有更多的关于付费支持的细节: [`networkradius.com`](http://networkradius.com)
*   可用性: FreeRADIUS 可以被许多操作系统使用. 所有的流行的 linux 发行版包括他作为他们的可得到的包. 他甚至支持 windows. FreeRADIUS 网站上有二进制下载链接.

## 缺点

没有软件是完美的. FreeRADIUS 也不例外. 这有一些他的缺点:

*   复杂: 这是唯一真的缺点. FreeRADIUS 提供一个包含一切的软件, 带有许多配置选项. 如果你不小心, 你的系统会出问题.
*   漏洞: 过去一些漏洞被报道, 但是他们从那时已经被修复. 你可以了解每个 FreeRADIUS 的漏洞, 从他们的官网.

## 竞争者

当 FreeRADIUS 陈述他是最流行的服务器, 他和谁是竞争者呢? 有许多竞争 RADIUS 服务器和竞争技术. 竞争服务器包括 Cisco's ACS, Microsoft's IAS, 和 Radiator. 竞争 AAA 技术包括 Diameter, TACACS+(CISCO 专有, 尽管也被其他的企业网络设备支持), 和 LDAP(LDAP 只支持 authentication).

# 总结

这一章是引言和我们要将的基础. 作为讨论的重点, 确定知道下面的事实.

| 名称 | 代表 | 简称 |
| --- | --- | --- |
| AAA | Authentication, Authorization, Accounting | 对于访问和使用的合适控制的必要的三个组件 |
| NAS | Network Access Server | 例如一个控制网络访问设备, 一个 VPN 服务器, 作为一个 RADIUS 客户端. |
| AVP | Attribute Value Pair | 在 RADIUS 包中一个 3 域的组件来包含一个特殊的域和他的数据 |
| VSA | Vendor-Specific Attributes | 由指定 vendor 管理的扩展的 AVP |

*   AAA 是一个安全架构模型.
*   RADIUS 是 AAA 的一种特定实现.
*   FreeRADIUS 是 RADIUS 的实用应用.
*   因此我们有 AAA -> RADIUS -> FreeRADIUS
*   RADIUS 是关于中心控制和 NAS vendor 支持的事实标准.
*   RADIUS 是一个 C/S 协议. 他使用 UDP, 监听在 1812 端口用于 authentication, 在 1813 端口用于 accounting 请求.
*   RADIUS 数据包有 code 域, 指定 RADIUS 包的类型.
*   RADIUS 数据包有 0 或者更多的 AVPs, 包含用在 RADIUS 中的数据.
*   FreeRADIUS 实现了 RADIUS 协议, 还有在 RFC 中的许多扩展.
*   FreeRADIUS 是一个非常流行的, 广泛使用的, 非常灵活的 RADIUS 服务器.

这一章是 FreeRADIUS 的开始. 主要课程从下一章开始, 我们将安装和开始使用 FreeRADIUS.

## 课堂作业 - RADIUS 知识

1.  解释 NAS device 术语.
2.  什么是一个 session 的起点和终点?
3.  RADIUS 使用哪种协议和端口?
4.  RADIUS 客户端和服务器端需要成功的通信?
5.  当 authenticate 一个用户时, RADIUS 客户端会发送什么?
6.  谁初始化一个 Disconnect Request 包并且谁接受他?
7.  命名 AVP 的三个组件?
8.  Alice 使用用户名`alice@freeradius.org`连接到一个网络. Alice 术语的 realm 的名称是什么?