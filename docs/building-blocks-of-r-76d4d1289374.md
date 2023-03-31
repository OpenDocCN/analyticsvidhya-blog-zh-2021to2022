# R 的构建模块

> 原文：<https://medium.com/analytics-vidhya/building-blocks-of-r-76d4d1289374?source=collection_archive---------30----------------------->

用于统计分析、图形表示和报告的程序设计语言。

![](img/b7817c384a0d241cbc715224bde2dd70.png)

克里斯·迪诺托在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

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

让我们开始吧，看看我们如何在 r 中做这些事情。

在开始用 R 写程序之前，我们需要安装 [R](https://cran.r-project.org/mirrors.html) 和 [R studio](https://rstudio.com/products/rstudio/download/) 。

```
myString <- "Hello, World!"
print (myString)
```

## 1.接收来自用户的输入并向用户显示输出

有几种方法可以向用户显示输出。让我们看看显示输出的一些方法:

```
var.1 = c(0,1,2,3)
'Method 1: values of the variables can be printed using print()
print(var.1)
# Output: 0 1 2 3'Method 2: cat() function combines multiple items into a continuous print output
cat ("var.1 is ", var.1 ,"\n")
# Output: var.1 is  0 1 2 3
```

## 2.在变量中存储值的能力(通常是不同种类的，如整数、浮点或字符)

基本数据类型:在 R 中我们称变量为对象。有几种类型的对象，让我们来看看重要的对象:

```
# Logical
v <- TRUE 
print(class(v)) # class funciton can be used to see the data type of the variable# Numeric
v <- 23.5
print(class(v))# Integer
v <- 2L
print(class(v))# Complex
v <- 2+5i
print(class(v))# Character
v <- "TRUE"
print(class(v))# Raw
v <- charToRaw("Hello")
print(class(v))
```

高级数据类型:R 的强大之处在于它允许我们访问一些高级对象，而不是前面提到的基本对象。让我们来看看一些高级变量:

```
# Vectors - When you want to create vector with more than one element, you should use c() function which means to combine the elements into a vector.
# Create a vector.
apple <- c('red','green',"yellow")
print(apple)
# Get the class of the vector.
print(class(apple))# Lists - A list is an R-object which can contain many different types of elements inside it like vectors, functions and even another list inside it.
# Create a list.
list1 <- list(c(2,5,3),21.3,sin)
# Print the list.
print(list1)# Matrices - A matrix is a two-dimensional rectangular data set. It can be created using a vector input to the matrix function.
# Create a matrix.
M = matrix( c('a','a','b','c','b','a'), nrow = 2, ncol = 3, byrow = TRUE)
print(M)# Arrays - While matrices are confined to two dimensions, arrays can be of any number of dimensions. The array function takes a dim attribute which creates the required number of dimension. In the below example we create an array with two elements which are 3x3 matrices each.
# Create an array.
a <- array(c('green','yellow'),dim = c(3,3,2))
print(a)# Factors - Factors are the r-objects which are created using a vector. It stores the vector along with the distinct values of the elements in the vector as labels. The labels are always character irrespective of whether it is numeric or character or Boolean etc. in the input vector. They are useful in statistical modeling. Factors are created using the factor() function. The nlevels functions gives the count of levels.
# Create a vector.
apple_colors <- c('green','green','yellow','red','red','red','green')
# Create a factor object.
factor_apple <- factor(apple_colors)
# Print the factor.
print(factor_apple)
print(nlevels(factor_apple))# Data frames are tabular data objects. Unlike a matrix in data frame each column can contain different modes of data. The first column can be numeric while the second column can be character and third column can be logical. It is a list of vectors of equal length. Data Frames are created using the data.frame() function.
# Create the data frame.
BMI <-  data.frame(
   gender = c("Male", "Male","Female"), 
   height = c(152, 171.5, 165), 
   weight = c(81,93, 78),
   Age = c(42,38,26)
)
print(BMI)
```

## 3.可以存储姓名、地址或任何其他类型文本的字符串

写在 R 中一对单引号或双引号内的任何值都被视为一个字符串。

这里的关键思想是学习如何操作字符串变量

我们将关注一些常见的操作:a .连接字符串

```
# Concatenate strings
paste(str1, str2, str3, ... , sep = " ", collapse = NULL)
```

b.计算字符串中的字符数

```
# Counting number of characters in a string - nchar() function
nchar(test_str)
```

c.更改大小写— toupper() & tolower()函数

```
str = 'apPlE'
toupper(str) # APPLE
tolower(str) # apple
```

d.提取部分字符串— substring()函数

```
# Syntax
substring(x,first,last)
# Example - Extract characters from 5th to 7th position.
result <- substring("Extract", 5, 7)
print(result)
```

e.格式化—可以使用 format()函数将数字和字符串格式化为特定的样式。

```
# Syntax
format(x, digits, nsmall, scientific, width, justify = c("left", "right", "centre", "none")) # Example
# Total number of digits displayed. Last digit rounded off.
result <- format(23.123456789, digits = 9)
print(result)# Display numbers in scientific notation.
result <- format(c(6, 13.14521), scientific = TRUE)
print(result)# The minimum number of digits to the right of the decimal point.
result <- format(23.47, nsmall = 5)
print(result)# Format treats everything as a string.
result <- format(6)
print(result)# Numbers are padded with blank in the beginning for width.
result <- format(13.7, width = 6)
print(result)# Left justify strings.
result <- format("Hello", width = 8, justify = "l")
print(result)# Justfy string with center.
result <- format("Hello", width = 8, justify = "c")
print(result)
```

## 4.一些高级数据类型，比如可以存储一系列常规变量(比如一系列整数)的数组

数组是存储在一个变量中的一系列相似类型的数据。数组可以是一维的，也可以是多维的。使用 array()函数创建一个数组。它将向量作为输入，并使用 dim 参数中的值来创建一个数组。

例如，如果我们创建一个维数为(2，3，4)的数组，那么它会创建 4 个矩形矩阵，每个矩阵有 2 行 3 列。

```
# dim=c(rows, columns, matrices)
array2 = array(1:12, dim=c(2, 3, 2))# Naming Columns and Rows
column.names <- c("COL1","COL2","COL3")
row.names <- c("ROW1","ROW2")
matrix.names <- c("Matrix1","Matrix2")
array2 = array(1:12, dim=c(2, 3, 2), dimnames = list(row.names, column.names, matrix.names))
```

让我们看看如何访问数组元素:

```
# dim=c(rows, columns, matrices)
print(array2[2,,2]) # Print the second row of the second matrix of the array.
print(array2[1,3,1]) # Print the element in the 1st row and 3rd column of the 1st matrix.
print(array2[,,2]) # Print the 2nd Matrix.
```

因为这里的返回值是矩阵，所以我们可以对它们执行矩阵操作

跨数组元素的计算(我们也可以使用用户定义的函数)

*   应用()
*   拉普利()
*   萨普利()
*   塔普利()

```
# apply(X, MARGIN, FUN) - apply to r or c or both - input to this funciton is a df - output is a vector, list or array 
m1 <- matrix(C<-(1:10),nrow=5, ncol=2) 
apply(m1, 2, sum) # lapply(X, FUN) - apply to all elements - input to this function is list, vector or df - output is a list 
# sapply(X, FUN) - apply to all elements - input to this function is list, vector or df - output is a vector or a matrix 
movies <- c("BRAVEHEART","BATMAN","VERTIGO","GANDHI") 
lapply(movies, tolower) 
sapply(movies, tolower) # tapply(X, INDEX, FUN = NULL) - apply to each factor variable in a vector - input to this function is a vector - output it an array data(iris) 
tapply(iris$Sepal.Width, iris$Species, median)
```

## 5.循环代码的能力从某种意义上说，你想从一个用户那里接收 10 个名字，你将为此写 10 次代码，但只写一次，并告诉计算机循环 10 次

r 有几个循环选项(重复、while 和 for)。还有嵌套选项(单个、两个、三个，..)循环。a .重复循环反复执行相同的代码，直到满足停止条件:

```
# Syntax
repeat { 
   commands 
   if(condition) {
      break
   }
}# Example
v <- c("Hello","loop")
cnt <- 2
repeat {
   print(v)
   cnt <- cnt+1   
   if(cnt > 5) {
      break
   }
}
```

b.While 循环反复执行相同的代码，直到满足停止条件:

```
# Syntax
while (test_expression) {
   statement
}# Example
v <- c("Hello","while loop")
cnt <- 2
while (cnt < 7) {
   print(v)
   cnt = cnt + 1
}
```

c.for 循环:

```
# Syntax
for (value in vector) {
   statements
}# Example
v <- LETTERS[1:4]
for ( i in v) {
   print(i)
}
```

r 还提供了 break 和 next 语句，允许我们进一步改变循环。以下是它们的用途:

*   当在循环中遇到 break 语句时，循环立即终止，程序控制在循环后的下一条语句处继续。
*   遇到 next 时，R 解析器跳过进一步的计算，开始循环的下一次迭代。

## 6.有条件地执行代码语句的能力，例如，如果分数超过 40，则学生通过，否则失败

r 提供了如果..，如果..其他..，如果..其他..如果..，并切换选项以应用条件逻辑。让我们来看看它们:a .在 R 中创建 if 语句的基本语法是:

```
# Syntax
if (test_expression) {
statement
}# Example
x <- 5
if(x > 0){
print("Positive number")
}
```

b.在 R 中创建 if…else 语句的基本语法是:

```
if (test_expression) {
statement1
} else {
statement2
}# Example
x <- -5
if(x > 0){
print("Non-negative number")
} else {
print("Negative number")
}
```

c.在 R 中创建 if…else if…else 语句的基本语法是:

```
if (test_expression1) {
statement1
} else if (test_expression2) {
statement2
} else if (test_expression3) {
statement3
} else {
statement4
}# Example
x <- 0
if (x < 0) {
print("Negative number")
} else if (x > 0) {
print("Positive number")
} else
print("Zero")
```

d.switch 语句允许根据值列表测试变量是否相等。每个值称为一个案例，每个案例都要检查被打开的变量。

```
x <- switch(
   2,
   "first",
   "second",
   "third",
   "fourth"
)
print(x)
```

## 7.将您的代码放在函数中

在 R 中，用户定义的函数是通过使用关键字函数创建的。

```
# Syntax
function_name <- function(arg_1, arg_2, ...) {
   Function body 
}
# Example
# Create a function to print squares of numbers in sequence.
new.function <- function(a) {
   for(i in 1:a) {
      b <- i^2
      print(b)
   }
}
```

我们可以调用函数 new.function，提供 6 作为参数。

```
new.function(6)
```

我们也可以创建可以传递参数的函数。如果用户没有提供值，这些函数也可以定义为使用这些参数的默认值。让我们看看这是如何做到的:

```
new.function <- function(a = 3, b = 6) {
   result <- a * b
   print(result)
}
```

现在，我们可以在传递或不传递任何值的情况下调用它:

```
# Call the function without giving any argument.
new.function()# Call the function with giving new values of the argument.
new.function(9,5)
```

## 8.通过一种或多种基本数据类型(如结构或类)的组合形成的高级数据类型

类是帮助创建对象的蓝图，包含其成员变量和属性。r 允许你创建两种类型的类，S3 和 S4。S3 类:这些让你重载函数。S4 类:这些让你限制数据，因为调试程序是相当困难的

我们将在此介绍 s4 课程。S4 类是由 setClass()方法定义的。

```
# Defining a class
setClass("emp_info", slots=list(name="character", age="numeric", contact="character"))
emp1 <- new("emp_info",name="vivek", age=30, contact="somehwere on the internet")# Access elements of a class
emp1@name
```

## 9.从磁盘读取文件并将文件保存到磁盘

让我们看看如何有条理地读写 csv。CSV 是数据科学中最常用的文件类型，但是 R 也可以读取其他几种文件类型。

```
# read a csv file
data <- read.csv('file.csv')# write a csv file
write.csv(df, 'file.csv', row.names = FALSE)
```

## 10.能够对你的代码进行注释，这样当你过一段时间再次访问它的时候就能理解它

我们可以告诉 R，一行代码是一个注释，以#开头。

```
# this is a comment
```

最后，我将强调在学习任何新东西时练习的重要性。坚持不懈，尝试这些积木的不同组合，先解决简单的问题，再解决复杂的问题，是变得流利的唯一途径。

欢迎评论！