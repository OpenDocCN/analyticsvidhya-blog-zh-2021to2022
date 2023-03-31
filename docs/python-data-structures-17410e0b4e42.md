# Python —数据结构简介

> 原文：<https://medium.com/analytics-vidhya/python-data-structures-17410e0b4e42?source=collection_archive---------13----------------------->

![](img/0d4cea0922ebf508f418b0994ef13e0f.png)

数据结构是任何语言的一个非常重要的方面。通过数据结构，我们可以解决许多复杂的问题。我们还可以以最小的时间和空间复杂度更快地执行我们的程序。

Python 有 **4 种基本类型**的内置数据结构。

1.  **列表**
2.  **字典**
3.  **元组**
4.  **设定**

# 列表和元组:

列表和元组是由整数索引的有序值序列。此外，列表和元组彼此相似，它们之间只有一个区别。列表是可变的，元组是不可变的(一旦创建，就不能操纵元组中的任何元素)。

**创建列表/元组:**

```
list = []
tuple = ()
```

**元组/列表长度:**

```
l=[12,13,14,15]/l=(12,13,14,15)
len(l)4
```

**列表/元组的元素“X”的索引:**

```
l=[12,13,14,15]/l=(12,13,14,15)
l.i­nd­ex(­13)1
If not found, throws a Value­Error exception
```

**列表/元组中 X 的出现次数:**

```
l=[12,12,12,13,14,15]/l=(12,12,12,13,14,15)
l.count(12)3
```

**更新列表/元组项:**

```
Lists:
l=['a','b','c','d'] 
l[3] = 'x'
print(l)['a','b','c','x']Tuples: tuples are immutable!
```

**删除列表/元组中位置 X 的元素:**

```
Lists:
l=['a','b','c','d']
del l[1] #removing the element in mentioned index
l.pop()  #removing the last element
print(l)['a','c']Tuples: tuples are immutable!
```

**连接两个列表/元组:**

```
Lists: myList1 + myList2
Tuples: myTuple1 + myTuple2
Concatenating a List and a Tuple will produce a TypeE­rror exception
```

**在列表/元组的位置 x 插入元素:**

```
Lists: myLis­t.i­nse­rt(x, "value")
Tuples: tuples are immutable!
```

**将“x”附加到列表/元组:**

```
Syntax: Lists: myList.append("x")
Tuples: tuples are immutable!
```

**将列表/元组转换为元组/列表:**

```
Syntax: List to Tuple: tuple(myList)
Tuple to List: list(­myT­uple)
```

**一些额外的内置函数:**

```
min(a): Gives minimum value in amax(a): Gives minimum value in asum(a): Adds up items of an iterable and returns sumsorted(a): Sorted list copy of a
```

# 词典:

它是一组无序的键值对。在 python 中，你只能**访问字典的键**，字典的主要特点之一是它是一种快速有效的存储数据的方式。

**初始化空字典:**

```
d = {}
```

**添加一个带有关键字“name”的元素到字典:**

```
d["name­"] = 'jon'{'name':'jon'}
```

**用关键字“name”更新元素:**

语法:

```
d["name"] = 'tom'
d['age'] = 25{'name':'tom','age':25}
```

**获取带有关键字“k”的元素:**

```
d["name­"]'tom' -- If the key is not present, a KeyError is raised
```

**检查字典中是否有关键字“k”:**

```
"name" in dTrue
```

**获取按键列表:**

```
d.k­eys()['name','age']
```

**获取字典的大小:**

```
len(d)2
```

**从字典中删除带有关键字“k”的元素:**

```
del d­["name"]{'age':25}
```

**删除字典中的所有元素:**

```
d.c­lear(){}
```

# 集合:

集合是一系列无序且未索引的对象。它们的值也是不变的，我们将在后面讨论。

**创建器械包:**

在 Python 中，集合由花括号表示。要创建一个集合，您只需编写逗号分隔的值，并将它们存储在一个变量中。

```
mySet = {“Pakistan”, “Berlin”, “Iran”, “China”, “Palestine”}
print(mySet)
```

当执行上述代码时，它会产生以下结果:

```
{'Pakistan', 'Berlin', 'Iran', 'China', 'Palestine'}
```

**注:**无论何时打印集合，元素的顺序都会不同，因为它们是无序的**。**

**访问集合中的元素:**

不能使用索引来访问集合中的元素，因为集合中的元素是无序的，并且没有附加索引。但是，您可以在 for 循环中使用 set 来使用它的值。

```
for i in mySet:
  print(i)
```

当执行上述代码时，它会产生以下结果:

```
‘Pakistan’
‘Berlin’
‘Iran’
‘China’
‘Palestine’
```

您还可以使用关键字中的**来检查集合中是否存在某个值。**

```
print(“America” in mySet)False
```

**修改集合:**

集合中的元素是不可变的，但是您可以向集合中添加更多的值。要向集合添加值，使用 **add** 方法。

```
mySet.add(“Moscow”)
print(mySet)
```

当执行上述代码时，它会产生以下结果:

```
{'Pakistan', 'Iran', 'Berlin', ‘Moscow’,'Palestine', 'China'}
```

要将多个值或另一个集合添加到一个集合中，添加集合时间，使用**更新**方法。

```
mySet.update({“America”, “Australia”, “Africa”}) #you can also    
                                                 provide a list here
print(mySet)
```

当执行上述代码时，它会产生以下结果:

```
{‘Moscow’, 'Pakistan', ‘America’, ‘Berlin’, 'Iran', ‘Australia’, 'China', 'Palestine', ‘Africa’}
```

**删除集合和集合元素:**

删除器械包和所有其他序列一样简单，只需使用 **del** 关键字和器械包名称，器械包就会被删除。

```
del mySet
```

但是，要删除一个集合中的元素，可以使用方法 **remove** 。

```
mySet.remove(“Moscow”)
print(mySet)
```

当执行上述代码时，它会产生以下结果:

```
{'Pakistan', ‘America’, ‘Berlin’, 'Iran', ‘Australia’, 'China', 'Palestine', ‘Africa’}
```

如果元素不在集合中，Remove 将引发错误。如果您不确定某个项目是否在列表中，您可以使用**丢弃**。如果存在，它将删除该值，如果没有找到该值，则不会引发错误。

```
mySet.discard(“America”)
print(mySet)
```

当执行上述代码时，它会产生以下结果:

```
{'Pakistan', ‘Berlin’, 'Iran', ‘Australia’, 'China', 'Palestine', ‘Africa’}
```

就是这样！希望您发现这是对 Python 数据结构的有用介绍。