# 7\. 附录：轻量级标记语言

没有标记语言就没有 Web 和丰富多彩的互联网，但创造了 Web 的 HTML 语言并非尽善尽美，存在诸如难读、难写、难以向其他格式转换的问题。究其根源是因为 HTML 语言是一种“重”标记语言，对机器友好而并非对人友好。

下面这段 HTML 源码，非技术控阅读起来会遇到困难。

```
<html>
<head>
  <meta content='application/xhtml+xml;charset=utf-8' http-equiv='Content-type' />
  <title>轻量级标记语言</title>
</head>
<body>
  <h1 id='id1'>轻量级标记语言</h1>

  <p><strong>轻量级标记语言</strong> 是一种 <em>语法简单</em> 的标记语言。
  它使用易于理解的格式标记，没有古怪的 <code>&lt;标签&gt;</code> 。</p>

  <ul>
  <li>可以使用最简单的文本编辑器编辑。</li>
  <li>所见即所得，非技术控亦可直接阅读源码。</li>
  <li>可版本控制。</li>
  <li>实现单一源文件出版。</li>
  </ul>
<body>
</html> 
```

同样的信息如果换用轻量级标记语言来表达，就非常直观了。如下所示：

```
轻量级标记语言
==============

**轻量级标记语言** 是一种 *语法简单* 的标记语言。
它使用易于理解的格式标记，没有古怪的 `` 。

- 可以使用最简单的文本编辑器编辑。
- 所见即所得，非技术控亦可直接阅读源码。
- 可版本控制。
- 实现单一源文件出版。 
```

GitHub 令人着迷的一个因素就在于 GitHub 为用户提供更为便捷地创建 UGC（用户生成内容）的方法，其奥秘就在于使用了轻量级标记语言。无论是代码提交说明、提交评注、问题描述、项目的 README 文件、维基页面、用户主页和项目主页都可以使用 Markdown(Markdown 是在 Ruby 应用中广泛使用的标记语言，语法简洁并可混用 HTML。标准的 Markdown 语法缺乏如表格等关键特性的支持，虽然不同的解析器都对其语法进行了扩展，但实现各有不同，造成一定的混乱。网址：[`daringfireball.net/projects/markdown/`](http://daringfireball.net/projects/markdown/) 。等轻量级标记语言来撰写。轻量级标记语言如 Markdown 是对人友好的标记语言，一些语法参照了我们写电子邮件时的习惯，即使第一次接触用轻量级标记语言撰写的文件，也可以毫无障碍地理解其中的内容。

虽然 GitHub 更倾向于使用 Markdown 标记语言(GitHub 使用 Redcarpet 作为 Markdown 的解析工具，并添加了额外的语法扩展。网址：[`github.github.com/github-flavored-markdown/`](http://github.github.com/github-flavored-markdown/)，但很多地方也提供对其他轻量级标记语言的支持。包括为 Python 程序员所熟悉的 reStructedText(reStructuredText 可简写为 reST 或 RST，是在 Python 中广泛使用的标记语言。reST 的语法简洁严谨，本书就是使用 Sphinx 扩展的 reST 语法和工具撰写的。网址：[`docutils.sourceforge.net/rst.html`](http://docutils.sourceforge.net/rst.html)，为 Ruby 程序员所熟悉的 Textile(Textile 是在 Ruby 应用中广泛使用的标记语言，例如 Redmine 就将 Textile 作为内置的标记语言。网址：[`redcloth.org/textile`](http://redcloth.org/textile)、RDoc(RDoc 是内嵌于 Ruby 代码中用于维护软件文档的标记语言。网址：[`rdoc.sourceforge.net/doc/`](http://rdoc.sourceforge.net/doc/)，为 Perl 程序员所熟悉的 POD(POD 是内嵌于 Perl 代码中用于维护软件文档的标记语言。网址：[`perldoc.perl.org/perlpod.html`](http://perldoc.perl.org/perlpod.html)，为 Emacs 用户所熟悉的 Org-mode(Org-mode 是 Emacs 的一种编辑模式，除文档外还被广泛应用于维护 TODO 列表、项目计划等。网址：[`orgmode.org/org.html`](http://orgmode.org/org.html)，为维基用户所熟悉的 MediaWiki(MediaWiki 是著名的维基百科（WikiPedia）所使用的维基语言。网址：[`www.mediawiki.org/wiki/Help:Formatting`](http://www.mediawiki.org/wiki/Help:Formatting)和 Creole(维基的实现有上百种，语法各不相同。Creole 试图建立统一的维基语法标准。网址：[`www.wikicreole.org/`](http://www.wikicreole.org/)，以及可作为 DocBook(DocBook 是著名的用于文档撰写的标记语言，采用 XML 文件格式及大量的面向出版的格式标签，能够实现单一源文件出版（Single-Source Publishing），即一次撰写多种格式输出（Write once, publish many）。但复杂的 XML 标签给写作过程带来不小的负担。网址：[`www.docbook.org/`](http://www.docbook.org/)前端的颇有前途的 AsciiDoc(AsciiDoc 的轻量级标签和 DocBook 的 XML 标签语法有着清晰的对应关系，既解决了 DocBook 语言标签复杂、难读难写的问题，又可利用 DocBook 丰富的工具链实现单一源文件向多种格式的输出转换。网址：[`www.methods.co.nz/asciidoc`](http://www.methods.co.nz/asciidoc)标记语言。

下面通过一张表格对几种常用的轻量级标记语言加以对照，供有不同标记语言偏好的用户参考，便于在 GitHub 某些不能随意更换标记语言而只能使用 GFM（GitHub 风格的 Markdown）的场合可以自如地转换。

详细对照请[参考原文](http://www.worldhello.net/gotgithub/appendix/markups.html)