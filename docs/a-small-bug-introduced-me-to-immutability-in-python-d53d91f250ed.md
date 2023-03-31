# 一个小 bug 让我了解了 Python 中的不变性

> 原文：<https://medium.com/analytics-vidhya/a-small-bug-introduced-me-to-immutability-in-python-d53d91f250ed?source=collection_archive---------28----------------------->

> python 中的一切都是对象。不变性与对象的内存位置密切相关。

在我们讨论不变性之前，让我们看一下这些例子。

## 实施例 1)

## 输出:

```
list1 before mutation:  ['1', '2', '3', '4']
list2 before mutation:  ['1', '2', '3', '4']list1 after mutation:  ['1', '8', '3', '4']
list2 after mutation:  ['1', '8', '3', '4']
```

***这里，在函数中，我们更新了 list1 的索引 1。列表 2 没有变化，但列表 2 的索引 1 已更新。***

现在，让我们对一个整数做同样的尝试。

## 实施例 2)

## 输出:

```
param1 before mutation:  7
param2 before mutation:  7param1 after mutation:  9
param2 after mutation:  7
```

***在上面的例子中，我们正在更新函数中的 param1。与前面的例子不同，param2 没有变化。***

> 为什么不同于整数的行为？

整数被认为是不可变的对象。顾名思义，不可变对象是那些一旦创建就不能再更新的对象。

但我们一直都是这么做的，不是吗？我们一直在改变整数值，就像例 2 一样。那它怎么会符合不可变对象的定义。

要回答这个问题，我们必须遵循更多的例子。

## 实施例 3)

*(这里，“id”指的是各自对象的存储位置)*

```
#integers
num1 = num2 = 4print('num1: ',num1)
print('num2: ',num2)
print('id of num1: ',id(num1))
print('id of num2: ',id(num2))#mutatingnum1 += 1
num2 += 2print('num1: ',num1)
print('num2: ',num2)
print('id of num1: ',id(num1))
print('id of num2: ',id(num2))
```

## 输出:

```
num1:  4
num2:  4
id of num1:  4534237584
id of num2:  4534237584num1:  5
num2:  6
id of num1:  4534237616
id of num2:  4534237648
```

## 观察结果:

*   最初，num1 和 num2 具有相同的值和相同的 id，这意味着 num1 和 num2 指向相同的内存位置。
*   突变后(递增 num1 和 num2 的值)，num1 和 num2 各自的 id 也发生了变化。这意味着 num1 和 num2 不再指向以前的内存位置。 ***原来变异用全新的内存位置创建了新的 num1 和 num2 对象。***

***这就是为什么在示例 2 中，当我们更新 param1 时，param2 没有变化。尽管 param1 和 param2 最初指的是同一个对象，但经过变异后，param1 是一个全新的对象。***

***对参数 1 的任何更改都不再与参数 2 相关。这就是 param2 保持不变的原因。***

感谢您阅读本文，希望您喜欢。