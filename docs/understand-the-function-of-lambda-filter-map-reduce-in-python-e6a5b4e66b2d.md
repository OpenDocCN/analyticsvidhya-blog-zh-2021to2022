# 通过例子理解 Python 中 Lambda，Filter，Map，Reduce 的功能

> 原文：<https://medium.com/analytics-vidhya/understand-the-function-of-lambda-filter-map-reduce-in-python-e6a5b4e66b2d?source=collection_archive---------21----------------------->

您是否曾经因为在 python 中使用常规循环和函数而感到沮丧，并且想知道是否有简洁高效的编码方式？那么，这个博客是给你的。这篇博客让你对 Python 中的 lambda、filter、map 和 reduce 函数有一个简单的了解。我们将通过例子来介绍这些主题，以使您熟悉这些概念。所以，让我们开始吧！！！

# λ()

Lambda 是一个小型匿名微定义函数，可以接受任意数量的参数，但只执行一个表达式。它也可以用于返回对象函数。

> **例子**

```
lamb = lambda x , y , z: x * y + zprint("For x=5, y=4 and z=7 : "+str(lamb(5,4,15)))
```

*输出*

```
For x=5, y=4 and z=7 : 35
```

在上面的例子中，“lamb”是一个具有 lambda 函数的标识符，其中 x、y 和 z 是它们的变量。lambda 函数可以不使用 return 参数返回对象函数。其被分配给标识符“lamb”。

# 地图()

Map 是一个包含用户定义函数和可迭代迭代器的函数。当调用 map 函数时，迭代器中的每个元素都在用户定义的函数中单独传递，并将返回的元素存储为迭代器。

> **举例**

```
def cube(x): return x*3lst=[1,2,3,4,5]result=map(cube,lst)print("Result = "+str(list(result)))
```

*输出*

```
Result = [3, 6, 9, 12, 15]
```

这里，函数映射调用了一个用户定义的函数“cube ”,它只接受 1 个变量，并将迭代器“lst”中的每个元素逐个传递给用户定义的函数 cube。当用户定义的函数' cube '返回一个对象时，那么' result '将它们存储为一个 iterable 对象。

# **Reduce()**

函数“reduce”需要从“itertools”导入，并且包含一个高阶函数。Reduce 包含一个用户定义的函数和一个可迭代序列，但与 map 不同，它最终只获得一个输出。

> **示例**

```
from functools import reducedef sum(x,y):return x+y#sum=lambda x,y:x+y#This also gives same output as def sum function but by using lambdalst=[1,2,3,4,5]result = reduce(sum,lst) #1+2=3#3+3=6#6+4=1010+5=15#print("Result = "str(result))
```

*输出*

```
Result = 15
```

# **过滤器()**

过滤函数类似于 for 循环，但速度更快。它在用户定义的函数中传递一个 iterable 对象，并返回满足条件的值。

> **示例**

```
check=lambda n:n%2==0lst=[1,2,3,4,5]result=filter(check,lst)map_result=map(check,lst)print(‘Map function of’+str(lst)+’: ‘+str(list(map_result)))print(“After using filter function: “+str(list(result)))
```

*输出*

```
Map function of[1, 2, 3, 4, 5]: [False, True, False, True, False]After using filter function: [2, 4]
```

> *如果你有兴趣了解机器学习的基础知识，可以看看我的另一个博客'* [什么是机器学习？——机器学习导论”。](https://tanujcbe.medium.com/what-is-machine-learning-an-introduction-to-machine-learning-2162a26b7356)
> 
> 如果你想讨论什么，随时联系我，然后通过[***Linkedin***](https://www.linkedin.com/in/tanuj-m/)***，***[***Github***](https://github.com/Tanujcbe)**和*[*face boo*](https://www.facebook.com/tanuj.murali.1)***k****