# shell 中的 heredoc 是什么？

> 原文：<https://medium.com/analytics-vidhya/what-is-heredoc-in-shell-bb1a2e35ed1b?source=collection_archive---------9----------------------->

heredoc 是一种向命令提供输入的**方式，只需沿着命令写就可以了，如下所示:**

```
$ cat <<delm
> hello world
> delm
hello world
```

heredoc 是**而不是**为命令提供选项或参数。该命令实际上必须能够从标准输入中读取，因此，例如在使用`echo`命令而不是`cat`编写之前的示例时，将只打印新的一行，因为`echo`不从标准输入中读取。

```
$ echo <<in
> Hello world
> in 
```

例如，在 heredoc 插值或**扩展**中，

```
$ X=1
$ cat <<in
> $X
> in
1
```

这可以通过引用 heredoc 字符串中的第一个分隔符`'`或`"`或`\`来**防止**，如下所示:

```
cat <<'in'
> $X
> in
$Xcat <<\in
> $X
> in
$Xcat <<"in"
> $X
> in
$X
```