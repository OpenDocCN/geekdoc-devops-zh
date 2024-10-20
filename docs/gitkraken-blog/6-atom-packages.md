# 6 个流行的 Atom 包

> 原文：<https://www.gitkraken.com/blog/6-atom-packages>

毫不奇怪，在 Axosoft，最常用的工具之一是不起眼的文本编辑器。不管你是哪种类型的开发人员，你都有自己的编辑器设置:主题、键盘快捷键、插件；一切都按照你喜欢的方式配置。

在 Axosoft，迄今为止最受欢迎的编辑器是 [Atom](https://atom.io/) : GitHub 的功能性、灵活性、开源、跨平台编辑器。开箱即用，Atom 是一个非常有能力的应用程序，不需要很长时间就能感觉到熟悉。但是它真正的优势在于它的可扩展性和可攻击性。对于有自己独特需求、语言、偏好和习惯的开发者来说，这是一个真正的福音。如果需要，您可以调整配置文件，或者创建整个插件来扩展 Atom 的基本功能。

如今，有很多适用于 Atom 的软件包，它们很容易在应用程序中浏览和安装。以下是 Axosoft office 中一些最常用的软件包的综述。

## **我们最常用的 6 个 Atom 包**

### **1。linter/linter-eslint**

[https://atom.io/packages/linter](https://atom.io/packages/linter) https://atom.io/packages/linter-eslint

我们开发的最流行的插件，是两个林挺代码插件的组合。linter 是一个程序，它评估代码中可能存在的错误，并强制执行一致的编码风格。基础 linter 与安装在基础之上的 JavaScript linter 相结合，是 Axosoft 开发人员的首选组合。语法错别字滚蛋！

### **2。高亮选择**

[https://atom.io/packages/highlight-selected](https://atom.io/packages/highlight-selected)

你可能已经猜到了，“高亮选择”包高亮你的选择；事实上，它突出显示了文档中该选择的所有实例。简单，但如果您需要快速引用一个字符串的所有可见实例，这是非常有效的。

**3。颜料**

### [https://atom.io/packages/pigments](https://atom.io/packages/pigments)

如果你正在处理颜色和变量，颜料是一个不可缺少的包。颜料突出显示所有颜色值(十六进制、rgb、rgba)以及该值所代表的颜色。在其他设置中，您可以选取“标记类型”，例如，选择颜色值旁边的颜色点，而不是用该颜色高亮显示它。

Pigments 也支持预编译器，所以您可以使用 Stylus、Less、Sass 或 Scss，它会突出显示代表颜色的函数和变量。需要例子？嗯，我们都知道 Chuck Norris 的十六进制值是#bada55，但这里有几个手写笔建议给你:

**4。迷你地图**

```
rocky                  = #ba1b0a
ringo-starr            = alpha(#bea71e, 0.5)
whats-that-smell       = #badc0d
whats-that-other-smell = #faece5 
```

[https://atom.io/packages/minimap](https://atom.io/packages/minimap)

### Minimap 提供了当前文档全部源代码的一目了然的图形预览。该软件包的主页显示了丰富的配置选项，但即使没有设置这些选项，Minimap 也提供了一种非常简单的方法来找出您在代码中的位置，并快速到达您需要的位置。

将此包与[小地图高亮选择](https://atom.io/packages/minimap-highlight-selected)包结合，查看您在地图中的所有选择！

Minimap offers an at-a-glance graphical preview of the current document’s entire source code. The package’s home page shows the copious configuration options, but even without setting those, Minimap offers a really easy way to work out where you are in your code, and quickly get to where you need to be.

**5。铁路图**

[https://atom.io/packages/regex-railroad-diagram](https://atom.io/packages/regex-railroad-diagram)

### 正则表达式是否让你对牙科手术、高中同学聚会、去车管所，或者在当地车管所由你的一个高中老朋友进行牙科手术充满恐惧？如果是这样，Regex 铁路图可能是你祈祷的答案。这个包提供了正则表达式的可视化视图，因此更容易调试它们的行为。

**6 .规则制定者**

[https://atom.io/packages/rulerz](https://atom.io/packages/rulerz)

我的光标去了哪里？我在这一排的什么位置？我很害怕！如果屏幕上有一个小竖条跟随我的光标，巧妙地给我一个我在哪里的视觉参考就好了！

### 啊，这有个包裹。Rulerz 给你一个简单的垂直规则，帮助你知道你在哪里。更重要的是，由于 Atom 的样式表，它是可定制的。这里有一个简单的例子:

给你:

啊，这样好多了。非常欢迎你。

Ah, there’s a package for that. Rulerz gives you a simple vertical rule to help you know where you are. What’s more, it’s customizable, thanks to Atom’s stylesheet. Here’s a modest example:

```
atom-text-editor.is-focused::shadow {
    ruler-view.rulerz {
        border: 10px solid #5234d4;
        width: 500px;
        box-shadow: 0px 0px 10px 10px #ff0, 0px 0px 100px 10px #f00;
        background-image: url('http://i.amz.mshcdn.com/AMO_a36WMI0rtWvQfnvIO8M_PZA=/1200x627/2012%2F12%2F04%2Fa9%2Fnyancatstar.aDm.jpg');
        background-size: contain;
    }
}
```

Gives you:

Ah, that’s better. And you are most welcome.