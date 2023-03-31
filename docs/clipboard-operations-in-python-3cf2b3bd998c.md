# python 中的剪贴板操作。

> 原文：<https://medium.com/analytics-vidhya/clipboard-operations-in-python-3cf2b3bd998c?source=collection_archive---------0----------------------->

使用 **ctrl+c** 和 **ctrl+v，**可以非常容易地执行剪贴板的复制/粘贴操作。您可能认为使用编程语言执行剪贴板操作可能很困难，但是我们可以使用 python 通过几行代码非常容易地做到这一点。Python 有专门用于剪贴板操作的库。在这篇短文中，我们将看到三个这样的 python 库。

# **pyperclip:**

pyperclip 有方法 copy()和 paste()来执行复制/粘贴操作。它是一个跨平台的库，这意味着我们可以在不同的操作系统上使用这个库。让我们先来看看 pyperclip 在不同操作系统中的依赖关系。

**在 Windows** 上，不需要额外的模块。
**在 Mac** 上，使用 pyobjc 模块，回退到 pbcopy 和 pbpaste cli
命令。(这些命令应该是 OS X 附带的)。
**在 Linux** 上，通过包管理器安装 xclip、xsel 或 wl-clipboard(用于“wayland”会话)。
*比如在 Debian 中:
sudo apt-get install xclip
sudo apt-get install xsel
sudo apt-get install wl-clipboard*

**执行复制/粘贴的方法:**

Pyperclip 有 copy()和 paste()方法来执行这些操作。

```
import pyperclip as pc
x = "Data to be copied to clipboard"
pc.copy(x)
a = pc.paste()
print(a)
```

**输出:**

```
Data to be copied to clipboard
```

Pyperclip 会将所有数据类型转换为字符串

```
print(type(a))#output
<class 'str'>
```

**pyperclip 的其他方法:**

1.  **确定 _clipboard():**
    确定操作系统/平台，并相应设置 copy()和 paste()函数
    。

**2。waitForNewPaste(time out = None):**
该函数调用一直阻塞，直到
剪贴板上出现一个新的文本字符串，该字符串不同于第一次调用函数
时的文本。它返回这个文本。
如果超时被设置为
，并且非空文本没有被放入
剪贴板，则该函数引发 PyperclipTimeoutException。

**3。waitForPaste(time out = None):**
该函数调用阻塞，直到
剪贴板上存在非空文本字符串。它返回这个文本。
如果超时被设置为
，则该函数引发 PyperclipTimeoutException，即非空文本没有被放入
剪贴板的秒数。

**4。set_clipboard(剪贴板):**显式设置剪贴板机制。

# **pyperclip3**

这个模块类似于 pyperclip，所有在 pyperclip 中可用的方法也在这个模块中出现。唯一的区别是，它将每种数据类型转换为字节。

```
import pyperclip3 as pc
x = "Data to be copied to clipboard"
pc.copy(x)
a = pc.paste()
print(a)
print(type(a))
```

**输出:**

```
b'Data to be copied to clipboard'
<class 'bytes'>
```

# **剪贴板**

这个模块只有 copy()和 paste()方法。以前库中可用的其他方法在本模块中不可用。

```
import clipboard as c
x = "Data to be copied to clipboard"
c.copy(x)
a = c.paste()
print(a)
print(type(a))
```

输出:

```
Data to be copied to clipboard
<class 'str'>
```

**结论:**

我们已经看到了三个 python 模块(pyperclip、pyperclip3、clipboard ),它们专门用于执行剪贴板操作。但是，Python 中有一些包，有内置的方法来执行剪贴板操作，比如熊猫的 to_clipboard，类似的 tkinter，PyQT 都有自己的方法来执行剪贴板操作。