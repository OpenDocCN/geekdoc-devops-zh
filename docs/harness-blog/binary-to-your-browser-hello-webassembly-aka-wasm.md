# 浏览器的二进制代码- Hello WebAssembly 又名 WASM | Harness

> 原文：<https://www.harness.io/blog/binary-to-your-browser-hello-webassembly-aka-wasm>

玩笑归玩笑，上一次我不得不在企业环境中处理浏览器是决定什么样的 Internet Explorer 安全设置适合我们的应用程序，这毫无乐趣可言。在我与 IE 竞争的几年前，我非常幸运地在 IBM 获得了一个轮流在[合作的](https://en.wikipedia.org/wiki/Cooperative_education)职位，这个职位持续了我大学的大部分时间，直到 2000 年代中期。在第二次轮流期间，我在做[WebSphere Application Server](https://www.ibm.com/cloud/websphere-application-platform/)的管理控制台(你知道你登录的那个东西)。我们正在重写大约 15，000 个左右的 Struts 动作[来表示管理控制台，因为 WebSphere 是 JAVA 转化为 HTML 的地方。我们一直在努力解决 web 问题，在浏览器之间测试新的控制台项目。当我为一个新组件](https://struts.apache.org/core-developers/action-configuration.html)[的几个问题争论不休时，一位团队领导向我提到了我写的“浏览器是我们最好的朋友和最坏的敌人”。又过了几分钟，“Ravi 想象一下，如果我们能把二进制文件流式传输到浏览器就好了”。这将是令人惊讶的，因为我用 JAVA 完成的事情比我在浏览器中使用像 JSF 这样的 MVC 技术要多得多。十年过去了，变化不断，我偶然发现了 InfoQ 的一篇文章，总结了 Richard Feldman 关于网络未来的演讲。理查德提到 WebAssembly 是他观察网络未来的一个视角。我以为 Richard 提到的是一个](https://javaee.github.io/tutorial/jsf-custom005.html) [WebPack](https://webpack.js.org/) ，并决定进一步调查 WebAssembly。在经历了几件抵押品之后，我的 2000 年中期团队领导的梦想实现了！

## 浏览器的二进制(ish)

你通往世界的门户，你的网络浏览器，只懂几种语言； [HTML](https://en.wikipedia.org/wiki/HTML) ， [CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) ，以及 [JS](https://en.wikipedia.org/wiki/JavaScript) 的风味。您的网络浏览器将解释这些语言，并在渲染引擎(例如 [Webkit](https://en.wikipedia.org/wiki/WebKit) / [Blink](https://en.wikipedia.org/wiki/Blink_(browser_engine)) )的帮助下渲染您所看到的内容。在某个时候，[二进制文件](https://en.wikipedia.org/wiki/Binary_code)被创建并在你的计算机上执行，作为你的机器的低级指令，也就是“最接近事实的指令”。之所以在渲染上存在 web 浏览器差异，是因为渲染引擎都不一样。容器技术[上升的一个原因是容器允许的可移植性和一致性。有了容器，您不必担心运行映像内部的不同解释，因为您正在运送您需要的一切。尽管如此，如果你的应用程序有一个基于浏览器的组件，比如一个](https://harness.io/blog/what-is-kubernetes-container/) [UI](https://en.wikipedia.org/wiki/User_interface) ，不管你的后端是否被容器化，浏览器仍然是一个不确定的领域。随着我们的应用程序变得越来越复杂，从有不止一个 web 浏览器的那一天起，我们一直在努力解决的问题似乎变得越来越复杂。另一个我自从大学上了[编译器课](https://www.udacity.com/course/compilers-theory-and-practice--ud168)(现在是免费的， [Go Jackets](https://www.cc.gatech.edu/) )之后就没接触过的技术！)是 [LLVM](https://en.wikipedia.org/wiki/LLVM) 。如果你对 LLVM 不熟悉，很多[编译器](https://en.wikipedia.org/wiki/Compiler)都是基于 LLVM 的。WebAssembly 支持 LLVM 语言，因此可以放入 WebAssembly 的语言列表[非常丰富，包括](https://github.com/appcypher/awesome-wasm-langs) [Go](https://en.wikipedia.org/wiki/Go_(programming_language)) 、 [Rust](https://en.wikipedia.org/wiki/Rust_(programming_language)) 和 [OCaml](https://en.wikipedia.org/wiki/OCaml) 。在[客户机-服务器](https://en.wikipedia.org/wiki/Client%E2%80%93server_model)模型中，大量的语言通常是为服务器保留的，处理过程再次转向你的浏览器，例如客户机。

## 客户端-无服务器？

WebAssembly 可能代表处理发生的根本转变。客户机(也称为浏览器)成为通常驻留在服务器上的语言的执行点。那么这是通过移除更多远程呼叫的新的无服务器吗？当然，更多的处理可以转移到浏览器。WebAssembly 如此新颖(2017)仍在不断发展。当我们开始探索向 web 浏览器发送低级命令时，肯定会有很多限制和顾虑。我们都知道 Adobe Flash 和 T2 JAVA 小程序在其生命周期中存在的安全隐患；Chrome 仍然让我知道 [Flash 支持将于 2020 年](https://www.chromium.org/flash-roadmap#TOC-Flash-Disabled-by-Default-Target:-Chrome-76---July-2019-)结束。运输预编译项目并不新鲜。在 JAVA 服务器页面的好日子里，你可以告诉应用服务器预编译 JSP。甚至更现代的语言如 [NodeJS](https://nodejs.org/en/) 提前编译 JavaScript 以产生[服务器端 JavaScript](https://en.wikipedia.org/wiki/List_of_server-side_JavaScript_implementations) 。最好的学习方法是尝试一些网络组件。

## 给我打包一个 web 程序集

WebAssembly 是一个新兴的标准，由于语言列表很长[感谢 LLVM]，底层项目/语言有很多变化。查看 WebAssembly 运行情况的快速方法是浏览 WebAssembly 项目的教程。好消息是先决条件步骤可以在 Linux [Harness Delegate](https://docs.staging-devharnessio.kinsta.cloud/article/h9tkwmkrm7-delegate-installation#where_do_i_install_the_delegate) 上运行。本教程的核心是 [Emscripten SDK](https://github.com/emscripten-core/emsdk) 的安装，它可以通过代理配置文件轻松地安装在线束代理上。由于我有一个基于 Ubuntu 的代理，我需要在我的亚马逊 Ubuntu AMI 上安装一些工具。*设置* > *驾驭代理* > *管理代理配置文件* > *添加代理配置文件*

在 Harness Delegate 配置文件中设置 bootstrap 命令是确保以模板化和一致的格式应用基础设施的一种好方法。一旦完成，你就可以启动一个终端并创建一个[“复杂的”C 应用程序](https://en.wikipedia.org/wiki/C_(programming_language))，它非常接近 WebAssembly 教程。我在这里直接登录了我的代理人。

一旦你有了你的 C 应用程序，你就可以使用 EMCC 来构建可分发的然后运行。有一个来自 [Spring One](https://content.pivotal.io/youtube-springone-platform-2019/webassembly-revolution-not-evolution) 的优秀视频展示了 [WASM 提琴](https://wasdk.github.io/WasmFiddle/)这是一个基于网络的编辑器，可以快速创建 [WAT/WASM](https://developer.mozilla.org/en-US/docs/WebAssembly/Text_format_to_wasm) 包装。WebAssmbley 肯定会发展起来，随着标准变得更加成熟，实践和模式肯定会改变。有了所有的活动部件，Harness 就有了你的后盾。

## 柔性线束平台

对于一个还没有过时的编译过程来说，用一个复杂的脚本驱动的管道来生成 WebAssmbley WAT 包似乎相当沉重。Harness 平台的美妙之处在于，您可以让一个 Harness 代表以代表身份运行 [Emscripten SDK 安装](https://github.com/emscripten-core/emsdk)，同时享受 Harness 提供的所有好处。随着 WebAssembly 标准和生态系统的不断成熟，对包含创建(别忘了测试)WebAssembly 的[利用工作流](https://docs.staging-devharnessio.kinsta.cloud/article/m220i1tnia-workflow-configuration)的更改可以很快被纳入考虑范围。2014 年使用 Kubernetes 成像与今天相比，我们已经走了多远。期待 WebAssembly 如何在未来塑造浏览器功能的创造性方法。如果你有任何有创意的用例/挖掘更多，请随意在[线束社区](https://community.harness.io)上发帖。干杯！拉维