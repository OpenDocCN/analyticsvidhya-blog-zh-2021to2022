# 学习 R-第 2 部分

> 原文：<https://medium.com/analytics-vidhya/learning-r-part-2-fb9ff130c51f?source=collection_archive---------20----------------------->

![](img/8e9973bede3976d6176ad82132d22a23.png)

在第一部分中，我介绍了 R 语言的一些背景知识，如何安装 R 和 RStudio，以及如何安装软件包或加载库()。在这一部分，我将涵盖以下主题

1.  r 基础知识
2.  功能
3.  数据类型

# **R 基础**

1.  **变量** —变量是给定一个存储位置的名称，用于存储程序中的值。R 编程中的变量可以用来存储数字(实数和复数)、单词、矩阵甚至表格。使用赋值符号`**<-**`定义一个变量。

```
# Assigning values to variables 
a <- 1 
b <- 1 
c <- -1
```

要查看存储在变量中的值，在控制台中键入变量名并点击返回，或者通过键入`print(variable_name)`并点击返回来使用`print()`功能。

2.**宾语**——在 **R** 中的一切都是**宾语**。对象是具有一些属性和作用于其属性的方法的数据结构。类是对象的蓝图。我们可以把班级想象成一个房子的草图(原型)。它包含所有关于地板、门、窗等的细节。

# **功能**

**功能**是一组避免**重复**同一任务或降低**复杂度的指令。**

函数应该执行指定的任务，并且可以包含也可以不包含参数。它有一个主体，可以返回也可以不返回一个或多个值。

**函数语法**

```
function (arglist)  {
  #Function body
}
```

1.  **多参数功能**

```
**# 1\. Function with two parameters**multiply <- function (value1, value2)  {
  value1 * value2
}
multiply(2,3)**# 2\. Function with variable number of arguments**calculateAverage <- function (...) {
    input <- list(...)
    do.call(sum, input)/length(input)
}
calculateAverage(2,2,4,2)**# 3\. Function with default values for arguments** multiplyByThree <- function (value1, value2 = 3)  {
  value1 * value2
}
multiplyByThree(2)
multiplyByThree(5,3)
```

# **数据类型**

r 有各种各样的数据类型，包括标量、向量(数字、字符、逻辑)、矩阵、数据帧和列表。

## 一、病媒生物

这些数据类型中最简单的是**向量**，并且有六个这样的原子向量。

1.  逻辑—真、假
2.  数字-12.3，5，999
3.  整数-2L，34L，0L
4.  复合体-3+2i
5.  字符-“a”，“好”，“真”，“23.4”
6.  raw-“Hello”存储为 48 65 6c 6c

使用`class()`功能查找对象类型。

1.  **逻辑**

```
v <- TRUE 
print(class(v))# Output 
"logical"
```

2.**数字**

```
v <- 23.5
print(class(v))# Output
"numeric"
```

3.**整数**

```
v <- 2L
print(class(v))# Output
"integer"
```

4.**复合体**

```
v <- 2+5i
print(class(v))# Output
"complex"
```

5.**人物**

```
v <- "TRUE"
print(class(v))# Output
"character"
```

6.**生**

```
v <- charToRaw("Hello")
print(class(v))# Output
"raw"
```

当你想要创建一个包含多个元素的向量时，你应该使用 **c()** 函数，这意味着将元素组合成一个向量。

```
# Create a vector.
apple <- c("red","green","yellow")print(apple)# Output
"red" "green" "yellow"# Get the class of the vector.
print(class(apple))# Output
"character"
```

## 二、议案列表

列表是一个 R 对象，它里面可以包含不同类型的元素，比如向量、函数，甚至它里面的另一个列表。

```
# Create a list.
list1 <- list(c(2,5,3),21.3,sin)

# Print the list.
print(list1)#Output
[[1]]
2 5 3

[[2]]
21.3

[[3]]
function (x)  .Primitive("sin")
```

**ⅲ级。基质**

矩阵是一个二维数据集。它可以使用矩阵函数的矢量输入来创建。

```
# Create a matrix.
M = matrix( c('a','a','b','c','b','a'), nrow = 2, ncol = 3, byrow = TRUE)
print(M)#Output
    [,1] [,2] [,3]
[1,] "a"  "a"  "b" 
[2,] "c"  "b"  "a"
```

**四。阵列**

与只能是二维的矩阵不同，数组可以是任意维的。要创建一个数组，我们需要传递维度作为输入。

```
# Create an array.
a <- array(c('green','yellow'),dim = c(3,3,2))
print(a)#Output
, , 1

     [,1]     [,2]     [,3]    
[1,] "green"  "yellow" "green" 
[2,] "yellow" "green"  "yellow"
[3,] "green"  "yellow" "green" 

, , 2

     [,1]     [,2]     [,3]    
[1,] "yellow" "green"  "yellow"
[2,] "green"  "yellow" "green" 
[3,] "yellow" "green"  "yellow"
```

**五、因素**

因子是使用向量创建的 r 对象。它将向量以及向量中元素的不同值存储为标签。标签总是字符，不管它是数字、字符还是布尔等等。在输入向量中。它们在统计建模中很有用。使用 **factor()** 函数创建因子。 **nlevels** 函数给出了级别的计数。

```
# Create a vector.
apple_colors <- c('green','green','yellow','red','red','red','green')

# Create a factor object.
factor_apple <- factor(apple_colors)

# Print the factor.
print(factor_apple)
print(nlevels(factor_apple))# Output
[1] green  green  yellow red    red    red    green 
Levels: green red yellow
[1] 3
```

六。数据帧

数据框可视为表格，其中行代表观察值，列代表不同的变量。数据框是表格数据对象。与数据框中的矩阵不同，每列可以包含不同模式的数据。第一列可以是数字，第二列可以是字符，第三列可以是逻辑。这是一个长度相等的向量列表。

使用 **data.frame()** 函数创建数据框。

```
# Create the data frame.
BMI <- 	data.frame(
   gender = c("Male", "Male","Female"), 
   height = c(152, 171.5, 165), 
   weight = c(81,93, 78),
   Age = c(42,38,26)
)
print(BMI)#Output
gender height weight Age
1   Male  152.0     81  42
2   Male  171.5     93  38
3 Female  165.0     78  26
```

为了从数据框的列中访问数据，我们使用美元符号符号`$`，它被称为访问器。

```
# Use the accessor operator to obtain the values in particular columnBMI$height# Output152.0 171.5 165.0
```

使用 str()可以访问对象的结构，通过函数 head()可以看到数据集的前 6 行

```
# Finding out more about the structure of the objectstr(BMI)# Output
'data.frame': 3 obs. of  4 variables:
 $ gender: chr  "Male" "Male" "Female"
 $ height: num  152 172 165
 $ weight: num  81 93 78
 $ Age   : num  42 38 26# To show the first 6 lines of the datasethead(murders)
# Outputstate abb region population total
1    Alabama  AL  South    4779736   135
2     Alaska  AK   West     710231    19
3    Arizona  AZ   West    6392017   232
4   Arkansas  AR  South    2915918    93
5 California  CA   West   37253956  1257
6   Colorado  CO   West    5029196    65
```

要确定一个变量中的条目数，请使用函数 length()

```
# To determine how many entries are in a vector 
pop <- BMI$height
length(pop)# Output
3
```