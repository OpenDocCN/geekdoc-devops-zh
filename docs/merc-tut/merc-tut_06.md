# 教程 - 和别人分享改变

## 教程 - 和别人分享改变

在《教程 - 和别人分享改变》中我们学到如何把变更集从一个仓库传递到另一个仓库去。有很多其它的方式在人和仓库之间分享改变，其中最常见的一种是通过电子邮件。

我们提交(Commit)改变后， 我们可以导出(Export) 它到一个文件里，并把这个文件作为附件 email 给其它人。

我们用 `export` 命令来 导出(Export) 改变。我们必需提供一个 Tag, 版本号或 变更集号 来告诉 Mercurial 有什么进入了 导出(Export)。 在我们的这个案例中，我们希望导出(Export) Tip。假设我们还在 `my-hello-share` 这个目录里，让我们做。

```
$ hg export tip
# HG changeset patch
# User mpm@selenic.com
# Node ID a58809af174d89a3afbbbb48008d34deb30d8574
# Parent  82e55d328c8ca4ee16520036c0aaace03a5beb65
Express great joy at existence of Mercurial

diff -r 82e55d328c8c -r a58809af174d hello.c
--- a/hello.c   Fri Aug 26 08:21:28 2005
+++ b/hello.c   Fri Aug 26 08:26:28 2005
@@ -12,5 +12,6 @@
 int main(int argc, char **argv)
 {
        printf("hello, world!\n");
+       printf("sure am glad I'm using Mercurial!\n");
        return 0;
 } 
```

缺省情况下，导出(Export)只显示补丁，所以我们通常把输出重定向到一个文件中(或使用参数-o)。这个文件是一个 UnifiedDiff 格式的补丁文件，这个文件还带了一些扩展的信息告诉 Mercurial 如何 导入(Import) 它.

当收件人收到我们的邮件，他们将保存附件并使用 `import` 命令来把变更集导入(Import)到他们的仓库中去。 (在 0.7 版本中， Mercurial 忽略了其中的一些信息，做导入(import)会引起一合并的问题。)

让我们站在现有的基础上在《教程 - 合并改变》里来学习如何 合并 一个改变。