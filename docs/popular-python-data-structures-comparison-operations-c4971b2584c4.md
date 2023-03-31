# 流行的 Python 数据结构:比较和操作

> 原文：<https://medium.com/analytics-vidhya/popular-python-data-structures-comparison-operations-c4971b2584c4?source=collection_archive---------7----------------------->

![](img/d1ef5fa4da06b17a0250a4d70865c0f8.png)

费迪南·斯托尔在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

Python 是数据科学中最流行的语言之一，对于初学者和有经验的专业人员来说，对其数据结构有深刻的理解是必须的。尽管有许多可用的数据结构，但其中只有四种在大约 80%的时间里被使用。

**列表、元组、字典**和**集合**是 Python 中数据结构的*。*

*让我们看看它们的基本特征、操作、用例，并了解它们之间的比较。*

# *列表*

*列表是有序的、可变的数据集合，用方括号[ ]括起来，并用逗号分隔它们的值。它们可以包含不同的数据类型、重复值，并且是可迭代的。*

****用例*** *:列表一般用于存储属于同一类别的相似数据。比如水果清单，B 班学生清单……等等。**

*让我们创建一个鲜花列表:*

```
*flowers = ['Rose', 'Iris', 'Hyacinth', 'Lily', 'Tulip'] #Output: ['Rose', 'Iris', 'Hyacinth', 'Lily', 'Tulip']*
```

## *列表操作*

*在我们的花列表的帮助下，诸如*访问、添加、修改、移除*和*删除*列表元素的操作如下所示。*

***访问列表中的项目***

```
*# Items can be accessed using Indexing and Slicing
print(flowers[1])    #output: Iris 
print (flowers[2:4]) #output: ['Hyacinth', 'Lily']*
```

***向列表添加项目***

```
*# **.append()** adds an item to the end of the listflowers.append('Lotus') # **.insert(index,value)** adds an item to the index of your choiceflowers.insert(3,'Magnolia')   

# **.extend()** adds multiple items to the listflowers.extend(['Jasmine','Carnation'])  

*#output:* ['Rose', 'Iris', 'Hyacinth', 'Magnolia', 'Lily','Tulip', 'Lotus','Jasmine','Carnation']*
```

***移除/删除列表中的项目***

```
*# **del** function deletes items based on Indexdel flowers[1]    

# **.pop()** too deletes items based on Indexflowers.pop(0)        

# **.remove()** takes the value to be deleted as the argument flowers.remove('Lotus')   

#output: ['Hyacinth', 'Magnolia', 'Lily', 'Tulip', 'Jasmine', 'Carnation']*
```

***替换列表中的项目***

```
*flowers[3]='Orchid' 

 #output: ['Hyacinth', 'Magnolia', 'Lily', 'Orchid', 'Jasmine', 'Carnation']

# You can also use **List Comprehension** to replace values

flowers=['Daffodil' if i=='Lily' else i for i in flowers]

#output: ['Hyacinth', 'Magnolia', 'Daffodil', 'Orchid', 'Jasmine', 'Carnation']*
```

***对列表中的项目进行排序***

```
*flowers.sort() #sorts in ascending order ,for descending use **list.sort(reverse = True)**

#output:['Carnation', 'Daffodil', 'Hyacinth', 'Jasmine', 'Magnolia', 'Orchid']*
```

***清除&删除列表***

```
*#Removing all items from a list

flowers.clear()   #output: []

# Deleting a list

del flowers   #if you enter the list name, an error is returned:
               "NameError: name 'flowers' is not defined"*
```

# *元组*

*元组是有序的、不可变的数据集合，是 Python 中的默认数据类型。不可变意味着它们不能被修改，项目不能被添加或删除。数据用括号()括起来。*

****用例:*** *元组是理想的数据结构，其中的数据不应该发生变化，属于单一的信息单元。例如，坐标，即地点的纬度和经度，可以存储在元组中。**

## *元组操作*

*对元组中的*元素进行访问、排序和赋值*等操作如下所示:*

*让我们创建一组 pin 码:*

```
*pincodes = (500010, 500045, 500022, 500068, 500034)*
```

***访问元组的元素***

```
*# Items can be accessed using Indexing and Slicing

print(pincodes[3])          #output: 500068print(pincodes[0:4])    #output: (500010, 500045, 500022, 500068)*
```

***排序值***

```
*sorted(pincodes)  #sorted returns a new list with sorted values,
                   instead of changing the original tuple#output: [500010, 500022, 500034, 500045, 500068]*
```

***元组赋值***

```
*#You can assign multiple values to variables at once using tuples

a,b,c=(1,2,3)

print(a)  #output: 1
print(b)  #output: 2
print(c)  #output: 3*
```

***串联***

```
*#Since you can't modify a tuple, simply create a new one

tuple1=(10,20,30)

tuple2= tuple1 + (40,50,60) #concatenation

#output: (10, 20, 30, 40, 50, 60)*
```

# *字典*

*字典是无序的、可变的数据集合，其中数据以**键:值**对的形式存储。如果这些值的键是已知的，则可以直接访问这些值，而不必迭代。数据用花括号括起来。值可以是可变的和重复的，但是键必须是唯一的和不可变的。*

****用例:*** *当你需要即时访问数据，而不必迭代所有的值时，字典很有用。例如，员工数据可以存储在字典中。**

*让我们创建一个字典:*

```
*Employee={'Id':1,'Name':'Tom','Age':30,'Education':'Masters','Department':'IT','Salary':100000}*
```

## *字典操作*

*在我们的员工字典的帮助下，下面举例说明了诸如*访问、添加、修改、删除*和*删除*字典元素的操作。*

***访问数据***

```
*#Accessing Elements Employee['Name'] #output: Tom #Finding keys using 'IN' Operator 'Education' in Employee #output: True #Viewing Keys Employee.keys()
 #output: dict_keys(['Id', 'Name', 'Age', 'Education', 'Department', 'Salary']) #Viewing Values Employee.values()
 #output: dict_values([1, 'Tom', 30, 'Masters', 'IT', 100000])*
```

***添加元素***

```
*Employee['Bonus']='Yes'    # Syntax: dictionary['newkey']='value'*
```

***删除元素***

```
*#del functiondel Employee['Bonus'] # .pop()Employee.pop('Salary') # .popitem() deletes the last element of the dictionaryEmployee.popitem()      

#output: {'Id': 1, 'Name': 'Tom', 'Age': 30, 'Education': 'Masters'}*
```

***清除&删除字典***

```
*Employee.clear()    #output: {}

del Employee       #deletes the dictionary*
```

# *设置*

*集合是无序的、可变的数据集合，不能包含重复的值。它们可以采用不同的数据类型，并用大括号{}括起来。*

****用例:*** *集合有助于对数据进行数学运算，以及需要存储相异值时。**

*让我们创建一组类型:*

```
*Genres= {"Fiction", "NonFiction", "Drama", "Poetry"} #output: {'Drama', 'Fiction', 'NonFiction', 'Poetry'} #notice how sets are sorted automatically*
```

## *集合操作*

**添加、删除*和*集合*的数学运算如下图所示:*

***向器械包添加物品***

```
*Genres.add("Folktale") #Adding Multiple Items Genres.update(['Horror','Distopian','SciFi']) #output: {'Distopian','Drama','Fiction','Folktale','Horror','NonFiction','Poetry','SciFi'}*
```

***从器械包中移除物品***

```
*Genres.remove("Poetry") # .remove() deletes an item, and raises an Error if it doesn't already exist in the setGenres.discard("Thriller") # .discard() deletes an item in a set, but in case the item doesn't exist, it doesn't return an error*
```

***集合上的数学运算***

```
*#Let's create two sets

Set1={1,2,3,4,5}
Set2={4,5,6,7,8}

**#Intersection**

Set1 & Set2              #Use **'&'** for **intersection**
#output:{4, 5}

Set1.intersection(Set2)  # or use **.intersection()** to find common elements

**#Union**

Set1.union(Set2)  
#output: {1, 2, 3, 4, 5, 6, 7, 8}

#Let's create another Set

Set3={1,2,3}

**#Subset**

Set3.issubset(Set1)              #check if subset
#output: True         

**#Superset**

Set1.issuperset(Set3)    #check if superset
#output: True

**#Difference**

Set1.difference(Set2)     # finds how different Set1 is from Set2 
#output: {1, 2, 3}*
```

# *结论:*

*上面讨论的基本数据结构知识对于执行数据科学和分析操作至关重要。每种数据结构都有自己的优点和缺点。数据结构的选择取决于要处理的数据的类型和性质，以及要对数据执行的操作。*

> *我是一名数据科学爱好者和 MBA 毕业生，对研究和统计充满热情。您可以在以下网址找到我:*
> 
> ***领英**:[https://www.linkedin.com/in/ruhii/](https://www.linkedin.com/in/ruhii/)*

**原载于 2021 年 3 月 20 日 https://www.analyticsvidhya.com*[](https://www.analyticsvidhya.com/blog/2021/03/popular-python-data-structures-comparison-operations/)**。***