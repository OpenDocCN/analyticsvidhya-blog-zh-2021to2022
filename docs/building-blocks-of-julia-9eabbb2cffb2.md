# 在 Julia 中编程

> 原文：<https://medium.com/analytics-vidhya/building-blocks-of-julia-9eabbb2cffb2?source=collection_archive---------18----------------------->

## 像 Python 一样的能力，像 C 一样的速度

![](img/723618e5ef6b1544e50d3c318d2c75f2.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[给](https://unsplash.com/@laurentmedia?utm_source=medium&utm_medium=referral)拍照

Julia 是一种高级、高性能的编程语言，由 Jeff Bezanson、Stefan Karpinski 和 Viral Shah 领导的计算机科学家团队在 2012 年创建。Julia 旨在解决传统科学计算语言(如 MATLAB、Python 和 R)的局限性，同时仍然保留它们的易用性和灵活性。

朱莉娅的一个主要特点是它的性能。Julia 被设计得很快，执行速度堪比 C 和 Fortran 等编译语言。这是通过结合使用实时(JIT)编译和类型推断来实现的，前者在代码执行时动态编译代码，后者允许 Julia 在运行时确定变量的数据类型。

Julia 的另一个重要特性是支持多种调度。多重分派允许 Julia 根据传递给函数的参数类型选择合适的方法。这使得 Julia 成为一种灵活且富于表现力的语言，可以很容易地扩展和定制，以适应广泛的编程任务。

Julia 还包括许多内置的数据结构和库，使得使用数组、矩阵和其他科学计算工具变得容易。这些工具包括线性代数、统计、优化和机器学习工具，以及对分布式计算和并行性的支持。

除了其科学计算功能，Julia 还包括对通用编程任务的支持，如 web 开发、数据库访问和文件 I/o。Julia 不断增长的包生态系统为这些任务提供了广泛的库和工具，使其成为可用于各种编程任务的通用语言。

朱莉娅的主要优势之一是它的社区。Julia 拥有一个快速增长的开发者和用户社区，他们积极地为该语言及其生态系统做出贡献。这个社区已经创建了大量高质量的软件包，以及许多学习和讨论该语言的在线资源和论坛。

大多数现代编程语言都有一套类似的构建模块，例如

1.  接收来自用户的输入并向用户显示输出
2.  在变量中存储值的能力(通常是不同种类的，如整数、浮点或字符)
3.  可以存储姓名、地址或任何其他类型文本的字符串
4.  一些高级数据类型，比如可以存储一系列常规变量(比如一系列整数)的数组
5.  循环代码的能力从某种意义上说，你想从一个用户那里接收 10 个名字，你将为此写 10 次代码，但只写一次，并告诉计算机循环 10 次
6.  有条件地执行代码语句的能力，例如，如果分数超过 40，则学生通过，否则失败
7.  将您的代码放在函数中
8.  通过一种或多种基本数据类型(如结构或类)的组合形成的高级数据类型
9.  从磁盘读取文件并将文件保存到磁盘
10.  能够对你的代码进行注释，这样当你过一段时间再次访问它的时候就能理解它

让我们开始吧，看看我们如何在 Julia 中做这些事情。

0。如何在你的桌面上安装 Julia？

在我们开始用 Julia 写程序之前，我们需要安装 [Julia](https://julialang.org/downloads/) 。接下来你可以安装 [VSCode](https://code.visualstudio.com/Download) 。现在启动 VSCode 并安装 Julia(Julia lang)扩展。现在，您可以创建一个新的 test.jl 文件，并添加以下代码，看看是否运行。

```
4+2; # If you don't want to see the result of the expression printed, use a semicolon at the end of the expressionans; # the value of the last expression you typed on the REPL, it's stored within the variable ans
```

在我们开始之前，链接函数在 Julia 中是可能的，就像这样:

```
1:10 |> collect
```

## 1.接收来自用户的输入并向用户显示输出

有几种方法可以向用户显示输出。让我们看看显示输出的一些方法:

```
# receiving input from user
name = readline(stdin) # showing output to user
println("you name is ", name)
```

## 2.在变量中存储值的能力(通常是不同种类的，如整数、浮点或字符)

变量的名字是小写的。单词分隔可以用下划线来表示。朱莉娅有几种类型的变量，大致分为具体和抽象类型。可以有子类型的类型(例如 Any、Number)称为抽象类型。可以有实例的类型称为具体类型。这些类型不能有任何子类型。

具体类型可以进一步分为基本类型和复合类型。让我们深入了解一下:

```
# Primitive types
## the basic integer and float types (signed and unsigned): Int8, UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Int128, UInt128, Float16, Float32, and Float64
a = 10 ## more advanced numeric types: BigFloat, BigInt
a = BigInt(2)^200 ## Boolean and character types: Bool and Char
selected = true## Text string types: String
name = "vivek"# Composite type
## Rational, used to represent fractions. It is composed of two pieces, a numerator and a denominator, both integers (of type Int)
666//444 # To make rational numbers, use two slashes (//)
```

一些高级数据类型包括字典和集合。集合类似于数组，不同之处在于它们不允许元素重复。

```
dict = Dict("a" => 1, "b" => 2, "c" => 3)
dict = Dict{String,Integer}("a"=>1, "b" => 2) # If you know the types of the keys and values in advance, you can specify them after the Dict keyword, in curly braces# looking things up
dict["a"]
values(dict) # to retrieve all values
keys(dict) # to retrieve all keys# these can be useful for iterating
for k in keys(dict)
for (key, value) in dictmerge(d1, d2) # merge() function which can merge two dictionaries
findmin(d1) # find the minimum value in a dictionary, and return the value, and its key
filter((k, v) -> k == 1, d1)# sort dict - you can use the SortedDict data type from the DataStructures.jl package
Pkg.add("DataStructures")
import DataStructures
dict = DataStructures.SortedDict("b" => 2, "c" => 3, "d" => 4, "e" => 5, "f" => 6)# Sets - A set is a collection of elements, just like an array or dictionary, with no duplicated elements. 
colors = Set{String}(["red","green","blue","yellow"])
push!(colors, "black")  # You can use push!() to add elements to a setunion(colors, rainbow) # The union of two sets is the set of everything that is in one or the other sets
intersect(colors, rainbow) # The intersection of two sets is the set that contains every element that belongs to both sets
setdiff(colors, rainbow) # The difference between two sets is the set of elements that are in the first set, but not in the second
```

我们将在下面的第 8 节讨论抽象数据类型。

## 3.可以存储姓名、地址或任何其他类型文本的字符串

在 Julia 中，写在一对双引号内的任何值都被视为一个字符串。

```
"this is a string"# double quotes and dollar signs need to be preceded (escaped) with a backslash
"""this is "a" string with double quotes""" # triple double quotes can be used to store strings with double quotes in them
```

Julia 还允许用户指定特殊字符串。

```
# special strings
r" " indicates a regular expression
v" " indicates a version string
b" " indicates a byte literal
raw" " indicates a raw string that doesn't do interpolation
```

这里的关键思想是学习如何操作字符串变量

我们将关注一些常见的操作:a .连接字符串

```
# Concatenate strings
join(split(s, r"a|e|i|o|u", false), "aiou") # You can join the elements of a split string in array form using join()
```

b.计算字符串中的字符数

```
# Counting number of characters in a string
length(str) # to find the length of a string
lastindex(str) # to find index of last char of string
```

c.更改大小写— toupper() & tolower()函数

```
uppercase(s)
```

d.拆分字符串

```
split("You know my methods, Watson.") # by default splits on space
split("You know my methods, Watson.", 'W') # splits on the char W
# If you want to split a string into separate single-character strings, use the empty string ("") split("You know my methods, Watson.", r"a|e|i|o|u", false) # splits string on the char that matches any of the vowels
# false makes sure that empty strings are not returned
```

e.字符串插值

```
# string interpolation - use the results of Julia expressions inside strings.
x = 42
"The value of x is $(x)." # "The value of x is 42."
```

f.迭代一个字符串

```
for char in s  # iterate through a string
    print(char, "_")
end
```

g.获取字符串中所有字符的索引

```
for i in eachindex(str)
    @show su[i]
end
```

h.数字和字符串之间的转换

```
a = BigInt(2)^200 
a=string(a) # convert number to string
parse(BigInt, a) # convert strings to numbers
```

I .查找和替换字符串中的内容

```
s = "My dear Frodo";
in('M', s) # true
occursin("Fro", s) # true
findfirst("My", s) # 1:2
replace(s, "Frodo" => "Frodo Baggins")
```

还有许多其他功能:

```
length(str) - - length of string
sizeof(str) - length/size
startswith(strA, strB) - does strA start with strB?
endswith(strA, strB) - does strA end with strB?
occursin(strA, strB) - does strA occur in strB?
all(isletter, str) - is str entirely letters?
all(isnumeric, str) - is str entirely number characters?
isascii(str) - is str ASCII?
all(iscntrl, str) - is str entirely control characters?
all(isdigit, str) - is str 0-9?
all(ispunct, str) - does str consist of punctuation?
all(isspace, str) - is str whitespace characters?
all(isuppercase, str) - is str uppercase?
all(islowercase, str) - is str entirely lowercase?
all(isxdigit, str) - is str entirely hexadecimal digits?
uppercase(str) - return a copy of str converted to uppercase
lowercase(str) - return a copy of str converted to lowercase
titlecase(str) - return copy of str with the first character of each word converted to uppercase
uppercasefirst(str) - return copy of str with first character converted to uppercase
lowercasefirst(str) - return copy of str with first character converted to lowercase
chop(str) - return a copy with the last character removed
chomp(str) - return a copy with the last character removed only if it's a newline
```

## 4.一些高级数据类型，比如可以存储一系列常规变量(比如一系列整数)的数组

数组可以是一维的，也可以是多维的。使用方括号、数组构造函数或其他几种方法创建数组。数组支持 Julia 中的许多功能，所以我在这篇特定于数组的文章中有更详细的介绍。现在让我们来看看关键的功能。

```
# Defining
# Creating arrays by initializing
arr_Int64 = [1, 2, 3, 4, 5]# Creating empty arrays
b = Int64[]# Creating 2-d arrays
arr_2d = [1 2 3 4] # If you leave out the commas when defining an array, you can create 2D arrays quickly. Here's a single row, multi-column array: 
arr_2d = [1 2 3 4 ; 5 6 7 8] # you can add another row using ;# Creating arrays using range objects
a = 1:10 # creates a range variable with 10 elements from 1 to 10
collect(a) # collect displays a range variable 
[a...] # instead of collect, you could use the ellipsis (...) operator (three periods) after the last element
range(1, length=12, stop=100) # Julia calculates the missing pieces for you by combining the values for the keywords step(), length(), and stop()# Using comprehensions and generators to create arrays
[n^2 for n in 1:5] # a 1-d array
[r * c for r in 1:5, c in 1:5] # a 2-d array# Reshape an array to create a multi-dimentional array
reshape([1, 2, 3, 4, 5, 6, 7, 8], 2, 4) # create a simple array and then change its shape# Supports indexing and slicing
# 1-d
a[5] # 5th element
a[end] # last element
a[end-1] # second last element
# 2-d
a = [[1, 2] [3,4]]
a[2,2] # element at row-2 x col-2
a[:,2] # all elements of col-2
getindex(a, 2,2) # same as a[2,2]# Elements can be added
a = Array[[1, 2], [3,4]]
push!(a, [5,6]) # The push!() function pushes another item onto the back of an array
pushfirst!(a, 0) # To add an item at the front
splice() # To insert an element into an array at a given index
splice!(a, 4:5, 4:6) # insert, at position 4:5, the range of numbers 4:6
L = ['a','b','f']; splice!(L, 3:2, ['c','d','e']) # insert c, d, e between b and f# Elements can be removed
splice!(a,5); # If you don't supply a replacement, you can also use splice!() can remove elements and move the rest of them along
pop!(a) # To remove the last item
popfirst!(a)# Elementwise and vectorized operations
a / 100 # every element of the new array is the original divided by 100\. These operations operate elementwisen1 = 1:6;
n2 = 2:7;
n1 .* n2; # if two arrays are to be multiplied then we just add a . before the mathematical operator to signify elementwise
# the first element of the result is what you get by multiplying the first elements of the two arrays, and so on# How function works on individual variables
f(a, b) = a * b
a=10;b=20;print(f(a,b))# How function can be applied elementwise to arrays
n1 = 1:6;
n2 = 2:7;
print(f.(n1, n2))
```

## 5.循环代码的能力从某种意义上说，你想从一个用户那里接收 10 个名字，你将为此写 10 次代码，但只写一次，并告诉计算机循环 10 次

Julia 有几个循环选项，如“for”和“while”。还有嵌套选项(单个、两个、三个，..)循环。a . While 循环反复执行相同的代码，直到满足停止条件:

```
# while end - iterative conditional evaluation
x=0
while x < 4
    println(x)
    global x += 1
end
```

b.for 循环:在 Julia 中充当迭代器；它遍历序列中的项或任何其他可迭代的项。我们已经知道可以迭代的对象包括字符串、列表、元组，甚至字典的内置可迭代对象，比如键或值。

```
# for end - iterative evaluation
# use the global keyword to define a variable that outlasts the loop
for i in 1:10
    z = i
    println("z is $z")
end# Some sample for loop statements for different data types
for color in ["red", "green", "blue"] # an array
for letter in "julia" # a string
for element in (1, 2, 4, 8, 16, 32) # a tuple
for i in Dict("A"=>1, "B"=>2) # a dictionary
for i in Set(["a", "e", "a", "e", "i", "o", "i", "o", "u"])
```

Julia 还提供了 break 和 continue 语句，允许我们进一步改变循环。以下是它们的用法:break:脱离当前最近的封闭循环。继续:转到最近的封闭循环的顶部。

```
# Example with break statement
x=0
while true
    println(x)
    x += 1
    x >= 4 && break # breaks out of the loop
end
```

break 和 continue 语句可以出现在循环体中的任何地方，但是我们通常会将它们与 if 语句进一步嵌套，以便根据某种条件执行某个操作。

以下是循环选项的一些其他选项:

```
# list comprehensions
[i^2 for i in 1:10]
[(r,c) for r in 1:5, c in 1:2] # two iterators in a comprehension# Generator expressions - generator expressions can be used to produce values from iterating a variable
sum(x^2 for x in 1:10)# Enumerating arrays
m = rand(0:9, 3, 3)
[i for i in enumerate(m)]# Zipping arrays
for i in zip(0:10, 100:110, 200:210)
    println(i)
end# Iterable objects
ro = 0:2:100
[i for i in ro] 
```

## 6.有条件地执行代码语句的能力，例如，如果分数超过 40，则学生通过，否则失败

Julia 提供了几个应用条件逻辑的选项。让我们来看看它们:a .三元和复合表达式:

```
x = 1
x > 3 ? "yes" : "no"
```

b.布尔开关表达式:

```
isodd(1000003) && @warn("That's odd!")
isodd(1000004) || @warn("That's odd!")
```

c.if elseif else end —条件求值:

```
name = "Julia"
if name == "Julia"
   println("I like Julia")
elseif name == "Python"
   println("I like Python.")
   println("But I prefer Julia.")
else
   println("I don't know what I like")
end
```

c.使用 try 处理错误..接住。这使得代码即使在错误发生时仍能继续执行，而错误通常会导致程序暂停。

```
# try catch error throw exception handling
try
    <statement-that-might-cause-an-error>;
catch e # error gets caught if it happens
    println("caught an error: $e") # show the error if you want to
end
println("but we can continue with execution...")# Example 1 - error doesnt occur
try
    a=10 # no error 
catch e
    print(e)
end# Example 2 - error occurs
try
    la-la-la # undefined variable error
catch e
    print(e)
end
```

## 7.将您的代码放在函数中

函数允许我们创建一个可以多次执行的代码块，而不需要重新编写。朱莉娅有一个叫做单一表达式函数的东西。这些通常在一行中定义，如下所示:

```
# Single expression functions
f(x) = x * x
g(x, y) = sqrt(x^2 + y^2)
```

也支持具有多个表达式的函数，并且可以使用 function 关键字进行定义:

```
# Syntax
# Functions with multiple expressions
function say_hello(name) 
    println("hello ", name)
end
say_hello("vivek")
```

此外，可以对函数进行编程，使用 return 关键字返回单个或多个值。

```
# define function which returns a value
function add_numbers(a,b)
    return a+b
end
# call the function
add_numbers(2,3)# define function which returns multiple values
function add_multiply_numbers(a, b=10) # we can supply default values as well
    return(a+b, a*b)
end
# call the function
add_multiply_numbers(2,3)
add_multiply_numbers(2)
```

args…让函数接受任意数量的参数。可以使用 for 循环来迭代这些参数。

```
function show_args(args...)
    for arg in args
        println(arg," ")
    end
end
show_args(10,20,25,35,50)
```

Julia 也支持匿名函数，没有名字。

```
map((x,y,z) -> x + y + z, [1,2,3], [4, 5, 6], [7, 8, 9])
```

Map 和 reduce 也可用于将函数应用于数组。Map —如果已经有了一个函数和一个数组，可以使用 Map()为数组的每个元素调用函数

```
a=1:10;
map(sin, a) # map() returns a new array but if you call map!() , you modify the contents of the original array
```

map()函数收集某个函数处理 iterable 对象的每个元素的结果，比如一个数字数组。

```
map(+, 1:10)
```

reduce()函数做类似的工作，但是在函数看到并处理了每个元素之后，只剩下一个元素。该函数应该接受两个参数并返回一个。

```
reduce(+, 1:10)
```

## 8.通过一种或多种基本数据类型(如结构或类)的组合形成的高级数据类型

Julia 允许用户使用抽象类型(抽象的)或可变结构(具体的)创建用户定义的变量。让我们一起来看看。抽象类型

```
abstract type MyAbstractType end # By default, the type you create is a direct subtype of Any
abstract type MyAbstractType2 <: Number end # the new abstract type is a subtype of Number
```

使用可变结构的具体类型

```
# define the data type
mutable struct student <: Any
   name
   age::Int
end# initialize a variable of that data type
x=student("vivek", 30)# use the variable
x.name
x.age
```

## 9.从磁盘读取文件并将文件保存到磁盘

让我们看看如何有条理地阅读。

```
f = open("dirk-gently.txt") # To read text from a file, first obtain a file handle: 
close(f) # When you've finished with the file, you should close the connection
```

如果你使用下面的技巧，那么你就不需要关闭。当这个程序块完成时，打开的文件会自动关闭。

```
open("dirk-gently.txt") do file
    # do stuff with the open file
end
```

## 10.能够对你的代码进行注释，这样当你过一段时间再次访问它的时候就能理解它

我们可以告诉 Julia，以#开头的一行代码是一个注释。

```
# this is a comment
```

总的来说，Julia 是一种强大而灵活的编程语言，非常适合科学计算和其他高性能任务。Julia 强调性能、多种调度以及不断增长的软件包和工具生态系统，对于研究人员、数据科学家和其他需要快速、灵活和富于表现力的语言来开展工作的专业人员来说，它是一个有价值的工具。

最后，我将强调在学习任何新东西时练习的重要性。坚持不懈，尝试这些积木的不同组合，先解决简单的问题，再解决复杂的问题，是变得流利的唯一途径。

欢迎评论！