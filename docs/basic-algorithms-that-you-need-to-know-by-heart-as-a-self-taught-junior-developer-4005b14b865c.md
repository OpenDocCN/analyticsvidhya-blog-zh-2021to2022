# 你需要牢记的基本算法来破解你的面试

> 原文：<https://medium.com/analytics-vidhya/basic-algorithms-that-you-need-to-know-by-heart-as-a-self-taught-junior-developer-4005b14b865c?source=collection_archive---------1----------------------->

![](img/196d06edb7ac8d08cad8f581e42b6fcc.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的 [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) 拍摄

大学毕业后，我自学了编程，并在过去的 6 年里一直作为一名软件工程师工作。我向我的朋友提到了我的背景和我是如何进入这个领域的，他们中的很多人问我他们应该学习什么，他们是否需要去训练营成为一名软件工程师。简单的答案是否定的。你不需要去学校成为一名软件工程师。没有接受过计算机科学的正规教育，你也可以涉足软件行业。你只需要做编码项目，尝试做安卓应用什么的。(不过，在最初的 2-3 年里，你的工资会比计算机专业的同行少得多。)那么，接受正规计算机科学教育的软件工程师和自学的软件工程师之间有什么区别呢？

首先，接受过正规教育的人已经接触到了计算机科学的更多方面，而你可能只是作为一名自学成才的程序员从事 Android/IOS 应用程序的工作。但是影响你的可雇佣性和你的实际软件工程技能的最大区别可能是对算法和数据结构的理解。你不会知道列表和字典之间的区别，那些数据结构的性能，甚至不知道如何以及何时使用它们。

因此，这里有一些基本算法，你绝对需要记住，以通过未来的编码面试。

1.  有效括号

[](https://leetcode.com/problems/valid-parentheses/) [## 有效括号- LeetCode

### 给定一个只包含字符'('，')'，' { '，' } '，'['和']'的字符串，确定输入字符串是否…

leetcode.com](https://leetcode.com/problems/valid-parentheses/) 

这个问题和解决方案给你一个如何使用栈和字典的想法。

字典用于以键/值对的形式存储数据。对于查找来说，它的平均(摊销)时间复杂度为 O(1 ),最差时间复杂度为 O(n ),这意味着如果您想检查一个值是否存在于具有特定键的字典中，您的计算机在大多数情况下可以很快完成，但如果您非常不幸，可能需要很长时间。

还有一种非常相似的数据结构叫做集合。集合主要用于存储多个值，以便跟踪特定数据值是否已经被添加。

在我解释上面提到的另一个数据结构之前，称为堆栈。这里有一篇很好的文章可以阅读，了解什么是 FIFO 和 LIFO。

[](https://www.guru99.com/python-queue-example.html#:~:text=There%20are%20two%20types%20of,the%20put%28item%29%20method.) [## Python 队列:FIFO、LIFO 示例

### 队列是保存数据的容器。首先输入的数据将首先被删除，因此队列也是…

www.guru99.com](https://www.guru99.com/python-queue-example.html#:~:text=There%20are%20two%20types%20of,the%20put%28item%29%20method.) 

*   对于 FIFO(先入先出队列)，先去的元素将最先出来。
*   对于 LIFO(后进先出队列)，最后进入的元素将最先出来。

(LIFO 队列数据结构通常也称为堆栈，因为其行为的可视化表示类似于一堆煎饼，其中最后放置的将是第一个取出的。您需要使用这些数据结构来有效地跟踪和处理算法中的数据。

2.卖出股票的时间到了

[](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) [## 买卖股票的最佳时机- LeetCode

### 给你一个价格数组，其中 prices[i]是当天给定股票的价格。你想最大化你的…

leetcode.com](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) 

有时候给你一个问题，解决了，面试官让你优化或者说“你一关就能搞定”之类的话。他们可能会告诉你，如果你不知道这个优化的解决方案(除非你足够聪明，当场就想出来。)

3.树遍历

[](https://leetcode.com/problems/binary-tree-inorder-traversal/) [## 二叉树有序遍历- LeetCode

### 给定二叉树的根，返回其节点值的有序遍历。示例 1:输入:root =…

leetcode.com](https://leetcode.com/problems/binary-tree-inorder-traversal/) 

如果你还不熟悉树结构和有序、前序、后序遍历算法，你应该对它们做一些研究，但是从上面的这些解决方案中得到的关键是，你总是可以将递归函数变成迭代函数，并且很多时候，迭代解决方案将更加节省内存。

[](https://stackoverflow.com/questions/931762/can-every-recursion-be-converted-into-iteration#:~:text=Can%20you%20always%20turn%20a,Turing%20machine%29%20and%20vice%20versa) [## 每一次递归都可以转化为迭代吗？

### 你总能把递归函数变成迭代函数吗？是的，绝对是，丘奇-图灵论题证明了这一点…

stackoverflow.com](https://stackoverflow.com/questions/931762/can-every-recursion-be-converted-into-iteration#:~:text=Can%20you%20always%20turn%20a,Turing%20machine%29%20and%20vice%20versa) 

4.二进位检索

[](https://leetcode.com/problems/binary-search/) [## 二分搜索法-李特码

### 给定一个按升序排列的整数数组 nums 和一个整数目标，写一个函数来搜索…

leetcode.com](https://leetcode.com/problems/binary-search/) 

这是一个广为人知的算法，你一定要在面试前复习一下(还有这篇文章中的所有其他算法…)

5.深度优先搜索

在我过去的面试中，我不得不在这个问题和类似的问题上花很多时间，我已经把它牢记在心，你可能也应该这样做。

[](https://leetcode.com/problems/number-of-islands/) [## 岛屿数量- LeetCode

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/number-of-islands/) [](https://leetcode.com/problems/number-of-islands/discuss/56340/Python-Simple-DFS-Solution) [## Python 简单 DFS 解决方案- LeetCode 讨论

### 提高你的编码技能，迅速找到工作。这是扩展你的知识和做好准备的最好地方…

leetcode.com](https://leetcode.com/problems/number-of-islands/discuss/56340/Python-Simple-DFS-Solution) 

6.斐波那契数列

[](https://realpython.com/fibonacci-sequence-python/) [## 斐波纳契数列的 Python 指南-真正的 Python

### 让我们开始吧！列奥纳多·斐波那契是一位意大利数学家，他能够很快给出这个问题的答案…

realpython.com](https://realpython.com/fibonacci-sequence-python/) 

他们总是让你用字典优化你的简单递归解决方案…

您想知道为什么需要使用字典并保留先前计算的值？
在这个 Youtube 视频中解释得非常好。

最后一个带 yield 关键字的只是 python 开发者的必选项。

额外收获:有效的 Python 和破解编码面试

[](https://www.amazon.com/Effective-Python-Specific-Software-Development-dp-0134853989/dp/0134853989/ref=mt_other?_encoding=UTF8&me=&qid=) [## 有效的 Python:写更好的 Python 的 90 种具体方法(有效的软件开发系列)

### 有效的 Python:写出更好 Python 的 90 种具体方法(有效软件开发系列):9780134853987…

www.amazon.com](https://www.amazon.com/Effective-Python-Specific-Software-Development-dp-0134853989/dp/0134853989/ref=mt_other?_encoding=UTF8&me=&qid=) 

如果你是 python 开发人员，我强烈推荐你去看看这本书。

[](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850/ref=sr_1_1?crid=ZRCT1RWE3AUN&keywords=cracking+the+coding+interview&qid=1645019553&s=books&sprefix=cracking+the+c%2Cstripbooks-intl-ship%2C425&sr=1-1) [## 破解编码面试:189 个编程问题和解决方案

### 破解编码面试:Amazon.com 的 189 个编程问题和解决方案。*免费*…

www.amazon.com](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850/ref=sr_1_1?crid=ZRCT1RWE3AUN&keywords=cracking+the+coding+interview&qid=1645019553&s=books&sprefix=cracking+the+c%2Cstripbooks-intl-ship%2C425&sr=1-1) 

此外，每个软件工程师可能也需要这本书。

(我不为亚马逊或其他什么公司工作…我希望如此。)

当然，要通过面试并高效地工作，你还需要知道更多的事情。您需要了解从排序算法到新框架的所有内容，以及什么是微服务，什么可能是您应用程序中的潜在瓶颈。但是就面试算法问题而言，一些公司似乎给他们的候选人出了难以置信的难题，当你遇到其中一家公司时，也许他们并不想雇用你。(还有一些公司会给你超长的编码任务，让你在一周左右的时间内完成。你只需要对那些说不)

![](img/ce6e7752d35cb3588010689a6ab7759d.png)