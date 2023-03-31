# Python 文字

> 原文：<https://medium.com/analytics-vidhya/python-literals-e68cc2d4de41?source=collection_archive---------8----------------------->

![](img/a3449ccd0f2819103c5c65edeec78789.png)

文字是在[变量或常量](https://nirushi-wijesiri.medium.com/python-variables-and-constants-307cc7834178)中给出的原始数据。基本上，我们赋给变量或常数的数据，叫做文字。

在 Python 中，有几种类型的文字。

1.  数字文字
2.  字符串文字
3.  布尔文字
4.  特殊文字
5.  文字收藏

我将逐一解释这 5 个字面值。放心吧！当你看到例子时，你会非常清楚。

# 数字文字

数字文字是 [**不可变**](https://en.wikipedia.org/wiki/Immutable_object) **，**意思是，它们是不可改变的。

数值可以属于 3 种不同的数值类型:

*   整数
*   浮动
*   复杂的

现在让我们举个例子，看看这些类型是什么。

## **Python 中如何使用数值文字？**

```
# 1.Integer Literal
int_1 = 450 #Decimal Literal
int_2 = 0b1011 #Binary Literal
int_3 = 0o310 #Octal Literal
int_4 = 0x12c #Hexadecimal Literal# 2.Float Literal
float_1 = 42.3
float_2 = 3.5e2# 3.Complex Literal
complex_num = 3.14jprint(int_1, int_2, int_3, int_4)
print(float_1, float_2)
print(complex_num, complex_num.imag, complex_num.real)
```

我来解释一下上面的例子。

*   **整数文字量**

有 4 种不同类型的整数文字。十进制文字，二进制文字，八进制文字和十六进制文字。

我给 int_1 赋值 450。这是一个十进制数。因此根据这一点，十进制文字是正常的十进制数。

我已经把 0b1011 赋给 int_2 了。这里，前缀“0b”让编译器知道这个值是一个二进制数。因此，编译器将 1011 作为二进制数。所以，二进制文字就是二进制数。他们的表现方式是这里的重要部分。

我把 0o310 赋给了 int_3。就像在二进制文字中一样，也有一个前缀“0o ”,它让编译器知道这是一个八进制值。因此，编译器将 310 作为一个八进制数。所以，八进制文字就是八进制数。

我已经把 0x12c 赋给 int_4 了。在这里，“0x”也是一个前缀，它让编译器知道 12c 是一个十六进制值。所以，十六进制文字就是十六进制数字。

*等到最后！有个惊喜给你！*

![](img/7acff92da6577716f55fed55ec8bbbb8.png)

*   **浮点文字**

在 float 文字下，我声明了两个 float 变量，分别为 float_1 和 float_2。还有，我给它们赋了两个浮点值，分别是 42.3 和 3.5e2。

在这里，42.3 是我们正常使用的正常浮点值。但是，3.5e2 是不同的东西，对不对？

3.5e2 用指数表示，相当于 3.5*10。所以，浮点文字是浮点数，在编码时，有几种方法可以表示这些浮点值。

*   **复杂文字**

在复杂文字下，我声明了一个名为 complex_num 的变量，并给它赋值 3.14j。这里，3.14j 是一个[复数](https://en.wikipedia.org/wiki/Complex_number)。因此，它有虚值和实值。通过使用“complex_num.imag ”,您可以获得虚构的文字，通过使用“complex_num.real ”,您可以获得真实的文字。

现在，是惊喜的时候了！您对这个示例代码的输出有什么看法？让我们看看！

**输出**

```
450 11 200 300
42.3 350.0
3.14j 3.14 0.0
```

![](img/f6bdf28efde6cf1a4933460a4d6590b8.png)

惊喜！！！

看第一行。4 个数都是十进制打印！现在，看第二行。float_2 打印了 350.0，相当于 3.5*10。很有趣，不是吗？

这就是关于数字文字的内容。

# 字符串文字

字符串是用引号括起来的一系列字符。当我们说引号时，它们可以是单引号、双引号或三引号。

此外，可以有一个由单引号或双引号包围的单个字符，它被称为字符文字。

## Python 中如何使用字符串文字？

```
normal_string = "Python is easy"
character_literal = "B"
multiline_string = """This type can be used to declare strings
                   that you have to use multiple code lines"""
unicode_string = u"\u00dcnic\u00f6de"
raw_string = r"See the output and \n you will know what this is!"print(normal_string)
print(character_literal)
print(multiline_string)
print(unicode_string)
print(raw_string)
```

在上面的示例代码中，您可能会看到一些您不太熟悉的符号。相信我！只要继续前进，你就会知道你应该知道的一切。

*   首先，我声明了一个名为“normal_string”的变量，并给它赋了一个字符串。所以，“Python 很简单”是一个普通的字符串文字。
*   作为第二个变量，我创建了“character_literal ”,并给它分配了一个字符。因此，“B”是一个字符字面量。
*   我创建的第三个变量是“multiline_string”。我用三重引号给它赋值。这些用于分配需要在代码中编写多行代码的字符串。它们被称为多行字符串文字。
*   我的第四个变量是“unicode_string”。在这个特定的字符串文字中，您可以看到我使用了包含几个字母和数字的“\u”短语。嗯，它们是用来提及 [Unicode](https://wiki.python.org/moin/Unicode) 值的。这里我使用了 2 个 Unicodes。第一个是“\u00dc”，代表ü。第二个是“\u00f6”，代表“奥”。看到输出，你就会明白它是如何工作的。这种类型的文字称为 Unicode 文字。文字前面提到的字母“u”让编译器知道这是一个 Unicode 文字。
*   最后，我创建了“raw_string”变量，并给它分配了一个前面带有字母“r”的字符串。字母“r”让编译器知道这是一个原始字符串。它主要做的是，省略字符串中的[转义字符](https://en.wikipedia.org/wiki/Escape_character)。

现在让我们看看上面示例代码的输出！

![](img/e805c9dffa3386c0be8d9c8732f35aa9.png)

**输出**

```
Python is easy
B
This type can be used to declare strings that you have to use multiple code lines
Ünicöde
See the output and \n you will know what this is!
```

看吧！我告诉过你这最终会有意义的！

这就是你现在需要知道的关于字符串的所有内容。

# 布尔文字

布尔文字可以有两个值中的任何一个:**真**或**假**。

## Python 中如何使用布尔文字？

```
choice_1 = (1 == True)
choice_2 = (1 == False)
number_1 = True + 4
number_2 = False + 10

print("choice_1 is", choice_1)
print("choice_2 is", choice_2)
print("number_1:", number_1)
print("number_2:", number_2)
```

**T5 在 Python 中，True 表示值为 1，False 表示值为 0。**

据此，“choice_1”的值为真，因为 1 等于真。我知道一开始会很困惑，但是让我告诉你如何抓住重点。

如果值“1”等于真，则“选择 _1”的值为真，否则“选择 _1”为假。现在，看“选择 _2”。像这样读。如果值“1”等于假，“choice_2”为真。但是，值“1”不等于假。False 的值为“0”。所以“choice_2”的值是假的。”如果还是不明白，就再读一遍前面的短语。你会明白重点的！

同样，您可以在数值表达式中使用 True 和 False 作为值。

在示例中，我将“True + 4”赋给了第三个变量“number_1”。在那里，你可以把 True 作为“1”，得到的答案是“1+4 = 5”。同理，“number_2”就要变成“0+10 = 10”。

因此，输出会是这样的。

**输出**

```
choice_1 is Tue
choice_2 is False
number_1: 5
number_2: 10
```

这真的很简单，现在，你知道布尔文字。

# 特殊文字

Python 有一个特殊的字面意思，那就是“无”。它用于指定该字段尚未创建。

让我们用这个例子来理解这一点。

![](img/991ace31f283cdaaaf6955eb8d5a9460.png)

## Python 中如何使用特殊文字？

```
deadpool_1 = "Available"
deadpool_2 = None

def movie(x):
    if x == deadpool_1:
        print(deadpool_1)
    else:
        print(deadpool_2)

movie(deadpool_1)
movie(deadpool_2)
```

如果你对 Python 的功能没有任何概念，这可能会有点混乱。为了理解特殊文字，我将在这个例子中解释您应该知道的基础知识。

在上面的程序中，我们定义了一个“电影”函数。在影片中，当我们将参数设置为“deadpool_1”时，它显示“Available”。并且，当参数为“deadpool_2”时，它显示“None”。

**输出**

```
Available
None
```

因为这是您在 Python 中遇到的唯一特殊文字，所以使用它非常容易。

# 文字收藏

在 Python 中，有四种不同的文字集合。它们是，列表字面值、元组字面值、字典字面值和集合字面值。

让我用一个例子来解释每一种类型。

## Python 中如何使用文字集合？

```
flowers = ["lilly", "rose", "tulip"] #list
numbers = (1, 3, 5) #tuple
alphabets = {'a':'avengers', 'b':'black_panther', 'c':'captain_america'} #dictionary
vowels = {'a', 'e', 'i' , 'o', 'u'} #set

print(flowers)
print(numbers)
print(alphabets)
print(vowels)
```

在上面的程序中，我创建了一个花的列表、一个数字元组、一个包含值的字典，每个值都有指定的键和一组元音。

您将在 **Python 数据类型**中了解更多关于文字集合的知识。现在，你们可以开始学习 Python 的下一个篇章了！

![](img/e44a0d0d2924442236041dc42202ad69.png)

这就是关于 Python 文字的内容。希望这篇文章能帮助你学到一些新的东西。永远相信自己，你已经成功了一半！祝你好运！