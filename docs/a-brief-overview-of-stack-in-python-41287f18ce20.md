# Python 中堆栈的简要概述

> 原文：<https://medium.com/analytics-vidhya/a-brief-overview-of-stack-in-python-41287f18ce20?source=collection_archive---------14----------------------->

![](img/dab9e60b5bd4974066c6d047fbeedcab.png)

贝基尔·登梅兹在 [Unsplash](https://unsplash.com/s/photos/stack?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

# 堆

堆栈是根据后进先出(LIFO)原则插入和移除的对象的集合。

参考上面的图片和移除物体的确切原理。我们从顶部移除并在顶部插入，因此，后进先出。

**栈上的主要操作**

*   推送:将元素添加到堆栈中。
*   从堆栈中删除最近添加的条目。
*   Peek:在不修改堆栈的情况下访问顶层元素。

# 使用带有包装类的列表进行堆栈

```
class Stack():

    def __init__(self):
        self.stack = list()

    def push(self, item):
        self.stack.append(item)

    def pop(self):
        if len(self.stack) > 0:
            return self.stack.pop()
        else:
            return None

    def peek(self):
        if len(self.stack) > 0:
            return self.stack[len(self.stack)-1]
        else:
            return None

    def __str__(self):
        return str(self.stack)
```

# 运行代码:

```
# creating stack object
my_stack = Stack()

my_stack.push(1)
my_stack.push(2)
my_stack.push(3)
print('Complete stack as of now')
print(my_stack)

print('\nStack after applying peek')
print(my_stack.peek())
print(my_stack)

print('\nStack after applying pop once')
print(my_stack.pop())
print(my_stack)

print('\nStack after applying peek')
print(my_stack.peek())
print(my_stack)# Results: *******************************************************Complete stack as of now
[1, 2, 3]

Stack after applying peek
3
[1, 2, 3]

Stack after applying pop once
3
[1, 2]

Stack after applying peek
2
[1, 2]
```

# 有趣的细节

*   推入和弹出操作只发生在结构的一端，称为堆栈的顶部。
*   这种数据结构使得将堆栈实现为一个**单链表和一个指向顶层元素**的指针成为可能。
*   堆栈可以被实现为具有有限的容量。
*   如果堆栈已满，没有足够的空间来接受要推送的实体，则认为堆栈处于溢出状态。

## 为您做的事情:

**添加实现来检查堆栈是否为空，并返回其大小。**

# 用例

*   堆栈用于实现深度优先搜索(回溯*)。
*   表达式可以用前缀、后缀或中缀来表示，从一种形式到另一种形式的转换可以使用堆栈来完成。
*   许多编译器使用堆栈来解析表达式、程序块等的语法。在翻译成低级代码之前。
*   最近邻链算法是一种基于维护簇堆栈的凝聚层次聚类方法，每个簇都是堆栈上其前一个簇的最近邻。

既然您已经熟悉了堆栈的概念，那么请访问 [Leet code](https://leetcode.com/tag/stack/) 来练习一些现实生活中的问题，并使用本概述中总结的思想来解决它们。万事如意！

特别感谢 Wikipedia*和 python 文档*提供了宝贵的信息，我在本文中使用了这些信息来概括这个至关重要的概念。

## 谢谢你看我的帖子。我希望你学到了一些有用的东西*🙌 🎉