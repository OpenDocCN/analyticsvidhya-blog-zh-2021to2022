# Python 中队列的简要概述

> 原文：<https://medium.com/analytics-vidhya/a-brief-overview-of-queue-in-python-7e9ea57a47e4?source=collection_archive---------12----------------------->

![](img/be7cd4d9c2930f36a19453a92b069f20.png)

阿德里安·德尔福吉在 [Unsplash](https://unsplash.com/s/photos/queue?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

# 长队

队列是根据先进先出(FIFO)原则插入和移除的对象的集合。

参考上图，移除对象的确切原则适用于队列。我们从队伍的前面移走，插在最后，因此，先进先出(来的早走的早)。

**队列的主要操作**

*   入队:将元素添加到队列中。
*   出列:删除队列中最长的元素。

# 使用 Deque 的单端队列包装类

```
from collections import deque
class Queue():

    def __init__(self):
        self.queue = deque()
        self.size = 0

    def enqueue(self, item):
        self.queue.append(item)
        self.size += 1

    def dequeue(self, item):
        if self.size > 0:
            self.size -= 1
            return self.queue.popleft()
        else: 
            return None

    def peek(self):
        if self.size > 0:
            ret_val = self.queue.popleft()
            self.queue.appendleft(ret_val)
            return ret_val
        else:
            return None
    def get_size(self):
        return self.size

    def __str__(self):
        queue_items = []
        if self.size > 0:
            counter=0
            while self.size > counter:
                val = self.queue.popleft()
                queue_items.append(val)
                counter+=1

            while counter>0:
                self.queue.appendleft(queue_items[counter-1])
                counter-=1
            return str(queue_items)
        else:
            return None
```

## 运行代码:

```
# creating queue object
my_queue = Queue()

my_queue.enqueue(1)
my_queue.enqueue(2)
my_queue.enqueue(3)
print('Complete queue as of now')
print(my_queue)
print('Size: ', my_queue.get_size()) print('\nQueue after applying peek')
print(my_queue.peek())
print(my_queue)
print('Size: ', my_queue.get_size())

print('\nQueue after applying dequeue once')
print(my_queue.dequeue(3))
print(my_queue)
print('Size: ', my_queue.get_size()) print('\nQueue after applying peek')
print(my_queue.peek())
print(my_queue)
print('Size: ', my_queue.get_size())# Results: *******************************************************Complete queue as of now
[1, 2, 3]
Size:  3

Queue after applying peek
1
[1, 2, 3]
Size:  3

Queue after applying dequeue once
1
[2, 3]
Size:  2

Queue after applying peek
2
[2, 3]
Size:  2
```

# 双端队列

一种支持在队列前端和后端插入和删除的数据结构。

**对 deque** 的主要操作

*   append(e):-将值插入到 deque 的右端。
*   appendleft(e):-将值插入到 deque 的左端。
*   pop( ) :-删除 deque 右端的值。
*   popleft( ) :-删除 deque 左端的值。

## 优势:

*   在我们需要从容器两端进行更快的追加和弹出操作的情况下，dequee 优于 list，因为与提供 O(n)时间复杂度的 list 相比，dequee 为追加和弹出操作提供 O(1)时间复杂度。

```
# Python code to demonstrate deque

from collections import deque

Dqueue = deque([1,2,3])
print ("\nThe original deque is : ")
print(Dqueue)

Dqueue.append(4)
print ("\nThe deque after appending '4' at right is : ")
print (Dqueue)

Dqueue.appendleft(0)
print ("\nThe deque after appending '0' at left is : ")
print (Dqueue)

Dqueue.pop()
print ("\nThe deque after deleting from right is : ")
print (Dqueue)

Dqueue.popleft()
print ("\nThe deque after deleting from left is : ")
print (Dqueue)# Results: *******************************************************The original deque is : 
deque([1, 2, 3])

The deque after appending '4' at right is : 
deque([1, 2, 3, 4])

The deque after appending '0' at left is : 
deque([0, 1, 2, 3, 4])

The deque after deleting from right is : 
deque([0, 1, 2, 3])

The deque after deleting from left is : 
deque([1, 2, 3])
```

# 优先队列

优先级队列是抽象的数据结构，其中队列中的每个数据/值都有特定的优先级。

## 优先级队列与队列:

*   在队列中，最老的元素首先出队，但是在优先级队列中，基于最高优先级的元素出队。
*   当元素从优先级队列中弹出时，所获得的结果或者按升序排序，或者按降序排序。而当从简单队列中弹出元素时，在结果中获得数据的 FIFO 顺序。

## 使用队列的优先级队列包装类。

```
class PriorityQueue():

    def __init__(self):
        self.queue = []

    def isEmpty(self):
        return len(self.queue) == 0

    def insert(self, data):
        self.queue.append(data)

    # popping an element based on Priority(using high value as high   priority in this implementation)
    def delete(self):
        try:
            max = 0
            for i in range(len(self.queue)):
                if self.queue[i] > self.queue[max]:
                    max = i
            item = self.queue[max]
            self.queue.pop(max)
            return item
        except IndexError:
            print('Index error: ')
            exit()

    def __str__(self):
        return ' '.join([str(i) for i in self.queue])
```

## 运行代码:

```
# creating PriorityQueue objectmyQueue = PriorityQueue()
myQueue.insert(4)
myQueue.insert(2)
myQueue.insert(3)

print ("\nThe original PriorityQueue is : ")
print(myQueue, '\n')

print('\nRemoving all items based on priority: ')
while not myQueue.isEmpty():
    print(myQueue.delete())# Results: *******************************************************The original PriorityQueue is : 
4 2 3 

Removing all items based on priority: 
4
3
2
```

## 使用 python 内置的 PriorityQueue 实现(最小值具有更高的优先级)

```
import queue as Q

queue = Q.PriorityQueue()
queue.put(4)
queue.put(2)
queue.put(3)

print('size:', queue.qsize())

print ("\nThe original PriorityQueue is : ")
print(queue.queue)print('\nRemoving all items based on priority(high to min value): ')

print(queue.get())
print(queue.get())
print(queue.get())print('size:', queue.qsize())# Results: *******************************************************size: 3

The original PriorityQueue is : 
[2, 4, 3]Removing all items based on priority(high to min value): 
2
3
4
size: 0
```

# 使用案例:

查看这篇文章，了解我们日常生活中队列数据结构的有趣用例。

[](https://www.geeksforgeeks.org/applications-of-queue-data-structure/) [## 队列数据结构的应用

### 当事情不是必须立即处理，而是必须以先进先出的顺序处理时，使用队列…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/applications-of-queue-data-structure/) 

既然您已经熟悉了队列的概念，那么请访问 [Leet code](https://leetcode.com/tag/queue/) 来练习一些现实生活中的问题，并使用本概述中总结的思想来解决它们。万事如意！

## 谢谢你看我的帖子。我希望你学到了一些有用的东西*🙌 🎉