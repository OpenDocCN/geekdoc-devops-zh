# gitkraken v1.0 在这里

> 原文：<https://www.gitkraken.com/blog/axosoft-gitkraken-v1-0>

对于 GitKraken 来说，这是多么疯狂、不可思议的几个月啊！二月初，我们发布了 v0.6，GitKraken 进入了公测。在那个版本发布之前，我们有一个专门的、可靠的开发人员池，他们通过发现各种各样的错误和问题来帮助我们，并为我们提供有价值的 UI/UX 反馈。

随着我们进入公开测试阶段，直到今天，用户对 GitKraken 的采用率一直是惊人的。

> **短短三个月不到，GitKraken 的日活跃用户增长了 2200%！(说真的，那个第二个零不是错别字。)**

因此，当我们兴奋地宣布我们的[1.0](https://www.gitkraken.com/download)版本时，我们想以一句响亮的“谢谢你”开始给所有下载并试用 GitKraken 的开发者，特别是那些花时间给出反馈的人。没有你们的帮助，我们不可能如此迅速和全面地继续完善和改进 GitKraken。

### **我们很自豪地分享我们收到的非常积极的反馈:**

*   接受调查的 GitKraken 用户中有 90%表示他们会向朋友或同事推荐 git kraken**。**
*   ****92%的人表示【GitKraken 在他们的日常运营中很有用或**非常有用**。****
*   **许多用户报告说 GitKraken 已经**通过一个直观的 UI 帮助他们学习 Git** ，该 UI 提供可视化的“提示”来指导他们。**

 **事实上，你的一些赞美如此感人，以至于我们*几乎*根据你的一些推文替换了我们的网站标语:

### **特性和修复**

以下是我们自 v0.6 以来在每个主要版本中实现的一些突出特性和修复。当然，您也可以查看我们的[发行说明](http://www.gitkraken.com/release-notes)以获得更全面的概述。

**0.7**

### 右键单击图形中的参考标签以更改上游。

*   将一个本地分支拖放到一个远程节点上，以推送到该远程节点(即使目标远程节点不是上游节点)。
*   现在，您可以右键单击图中的提交条目来恢复该提交。
*   32 位和 64 位版本的 Windows 内部版本都已更新，可以流畅运行。
*   应用程序中的“关于”菜单会告诉你更多信息，而不会过度分享。
*   **0.8**

文件历史:在比较面板中右键单击一个文件来显示提交的时间线。使用键盘快捷键导航，或单击短提交 SHA 快速跳转并显示差异。

### 责备修改:你逃脱了这么久，但你不能再躲了。查看文件是如何、何时以及由谁更改的！

*   从应用程序中发出 GitHub/Bitbucket pull 请求。
*   **0.9**
*   子模块！

**1.0**

### 就是它了！大的那个！

我们已经努力提高应用程序的可靠性，我们很高兴为您带来 GitKraken 的第 1 版。以下是亮点:

**固定**

### 有时，当在 GitKraken 中重置 diffs 时(如果您的工作目录中有一个新文件)，您会收到一条错误消息，提示您为一个不存在的文件更新 diffs。这个信息本身就是一个错误，让整个经历变成了一个噩梦，现在我们已经解决了。

责备视图:如果你重命名一个文件，责备会在以前的名字下显示“未知”。我们应该为此受到责备，我们现在已经对命名进行了分支，这样你就可以跟踪命名历史，并再次将矛头指向你的开发伙伴。

### `775b3d237e52df5b11e1f3f346b13fc7dbd80e39`
哦，你看不懂沙哈希？我们已经更新了子模块冲突，使人类可以理解更多的冲突信息。

如果拖放最新的合并失败，它不再无声地尖叫。你会收到一条信息告诉你。

白屏:当你启动 GitKraken 时，它应该做点什么而不是什么都不做。我们已经把触角伸到了骨头上，以确保白屏死亡不会在尽可能多的平台和配置上发生。

已知问题:我们正在努力为 GitKraken 添加代理支持，但目前，通过代理的用户可能会遇到问题。

*   Occasionally, when resetting diffs in GitKraken (if you have a new file in your working directory), you’d receive an error message about updating diffs for a file that does not exist. This very message was an error, making the whole experience a meta nightmare that we have now resolved.
*   随着每个主要版本的推出，我们一直在添加新的功能，使 GitKraken 变得更好——但我们也一直在努力提高稳定性和性能。
*   我们的目标是:
*   [@GitKraken](https://twitter.com/GitKraken) 加载仓库时还是死机！！啊啊啊！！GIT 客户端是怎么回事？源代码树..gitkraken [#gitClientFAIL](https://twitter.com/hashtag/gitClientFAIL?src=hash)
*   —保罗(@ majic code)[2016 年 2 月 27 日](https://twitter.com/majiccode/status/703633737699999744)
*   变成:

[@GitKraken](https://twitter.com/GitKraken) 万岁！谢谢大家！终于可以开回购了！！我以为这一天永远不会到来..#真正快乐

—保罗(@ majic code)[2016 年 3 月 22 日](https://twitter.com/majiccode/status/712252504982077441)

Our aim has been to turn:

亲自去看看 GitKraken[立即在您的操作系统上免费下载](https://www.gitkraken.com/)！请让我们知道你在[推特](https://twitter.com/gitkraken)上的想法。

> 附注:您的 Twitter 请求已得到回复；您现在可以在我们的新店[获得 GitKraken 赠品](http://www.zazzle.com/gitkraken)！
> 
> — paul (@majiccode) [February 27, 2016](https://twitter.com/majiccode/status/703633737699999744)

 <picture decoding="async" class="wp-image-3818" title="<script>">![<script>](img/d764d8724113ff71a4fd8910f219a822.png)</picture> window.addEventListener('DOMContentLoaded', function() {

into:

> [@GitKraken](https://twitter.com/GitKraken) hurrah! thank you! finally i can open repo’s!! i thought this day would never come.. #trulyHappy
> 
> — paul (@majiccode) [March 22, 2016](https://twitter.com/majiccode/status/712252504982077441)

Check out GitKraken for yourself; [download it now for free](https://www.gitkraken.com/)  on your OS! And please, let us know what you think on [Twitter](https://twitter.com/gitkraken).

P.S. Your Twitter requests have been answered; you can now [get GitKraken swag](http://www.zazzle.com/gitkraken) in our new store!**