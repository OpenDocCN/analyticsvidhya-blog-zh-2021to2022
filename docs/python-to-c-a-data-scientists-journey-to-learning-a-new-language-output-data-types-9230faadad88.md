# Python 到 C++，数据科学家学习新语言的旅程——输出、数据类型、变量和用户输入

> 原文：<https://medium.com/analytics-vidhya/python-to-c-a-data-scientists-journey-to-learning-a-new-language-output-data-types-9230faadad88?source=collection_archive---------5----------------------->

![](img/890736ea088de4cf94031ae66343a512.png)

# **简介**

读者，欢迎回到我的 Python 到 C++系列的下一期！上周，我简要介绍了这个系列，并回顾了 Python 和 C++语言之间的一些基本异同。最后，我用每种语言创建了一个 Hello World 程序，并逐行解释了代码中的不同之处。

在本周的文章中，我将讨论这两种语言在一般输出、数据类型、变量和收集用户输入方面的异同。让我们的旅程继续！

# **打印输出**

将输出打印给用户是编程中最重要的一个方面。无论是给用户下达指令，给用户讲故事，还是表扬用户的出色表现，无论程序员使用什么语言，几乎每个程序都需要某种形式的输出。

在 Python 中，这个方法内置于基本的 Python 库中，只需要调用它就可以成功运行。我们在上周的 Hello World 节目中看到了这一点。要接收输出，只需运行以下代码:

```
print(“This is a test…”)print(“Printing is simple in Python…”)
```

在终端中键入以下内容:

```
$ python3 output.py
```

返回以下输出:

```
>> This is a test...
>> Printing is simple in Python...
```

我们通过键入 print()来调用 print 方法，确保包含左括号和右括号。在这些括号中，我们传入一个参数，在这个例子中，我们希望看到输出到终端的内容——因为我们要输出一个短语或字符串，所以我们必须确保用引号将短语括起来。稍后我们会学到更多关于弦的知识。

这在 C++语言中稍微复杂一些，因为 print 或 output 方法不是内置在基本 c++库中，而是一个单独的库，称为 iostream 库，是输入输出流的缩写。正如我们上周看到的，我们首先需要使用#include 调用库。然后，当调用 iostream 库中的特定方法时，我们必须调用该库，在这种情况下，通过键入 std 进行调用。接下来是调用我们想要使用的方法。因为我们要打印输出，所以我们将使用 cout 方法。后面跟着两个左箭头符号“<

```
#include <iostream>int main(){std::cout << “This is a test…”;std::cout << “Printing in C++ is a bit more complicated…”;std::cout << “\nThis is a test…\n”;std::cout << “Printing in C++ is a bit more complicated…\n”;return 0;}
```

在终端中键入以下内容:

```
$ g++ -o output output.cpp
$ ./output
```

返回以下输出:

```
>> This is a test...Printing in C++ is a bit more complicated...
>> This is a test...
>> Printing in C++ is a bit more complicated...
```

您将看到我在这段代码中使用了两种不同的方法。一个在输出短语结尾带有“\n”，另一个在输出短语结尾没有“\n”。您会发现，当打印不带“\n”的两个短语时，两个短语都打印到同一行，而以“\n”结尾的短语各打印一行。您可能会从 Python 语言中认出这一点，它在这里的用法完全相同。“\n”告诉编译器，我们希望在换行符之前的内容之后创建一个新行。在 C++中还有另外一种方法，我们稍后会讲到。

# **数据类型**

程序员用于输入、输出、计算等的一切。具有特定的数据类型。几乎每一种编程语言都是如此。数据类型用于帮助计算机理解它到底在处理什么。在不同的编程语言中，许多数据类型都非常相似，在很大程度上，Python 和 C++就是这种情况。以下是我们将在两种语言中使用的完整代码。我们将一节一节地分解数据类型，首先用 python:

```
# This is an inline comment in python# strprint(“Hello World!”)print(type(“Hello World!”))# intprint(1)print(type(1))# floatprint(1.5)print(type(1.5))# boolprint(True)print(type(True))
```

然后在 C++中:

```
//This is an inline comment in C++#include <iostream>#include <typeinfo>#include <string>int main(){// stringstd::string a = “Hello World!”;std::cout << a << std::endl;std::cout << typeid(a).name() << std::endl;// intstd::cout << 4 << std::endl;std::cout << typeid(4).name() << std::endl;// floatfloat c = 4.5;std::cout << c << std::endl;std::cout << typeid(c).name() << std::endl;// doubledouble d = 4.555555555555555555;std::cout << d << std::endl;std::cout << typeid(d).name() << std::endl;// boolstd::cout << true << std::endl;std::cout << typeid(true).name() << std::endl;// charstd::cout << ‘a’ << std::endl;std::cout << typeid(‘a’).name() << std::endl;return 0;}
```

**字符串**:在 Python 和 C++中，字符串都是一个短语或一组字母的串。我们上周的输出，“你好，世界！”是一个字符串。“asdflkj”也是一个字符串。字符串由一组字母周围的引号表示。在 Python 中，这些引用可以是单引号或双引号，只要字符串的两边使用相同的标记。例如，在 Python 中:“你好，世界！”或者“你好，世界！”可以用，但是‘你好世界！和“你好，世界！”不能。在 C++中，当输入字符串时，必须使用双引号，因为单引号是用来表示不同的数据类型的，我们稍后会学到。以下是 Python 中的字符串输出示例，后面是 type()方法，该方法将输出传入括号中的任何参数的数据类型:

```
print(“Hello Readers!”)print(type(“Hello Readers!”))
```

终端中的以下内容:

```
python3 data_types.py
```

从 str 部分产生以下输出:

```
>> Hello Readers!>> <class ‘str’>
```

下面是用 C++输出的同一个字符串。

```
std::string a = “Hello Readers!”;std::cout << a << std::endl;std::cout << typeid(a).name() << std::endl;
```

终端中的以下内容:

```
$ g++ -o data_types data_types.cpp$ ./data_types
```

导致输出:

```
>> Hello Readers!>> NSt3__112basic_stringIcNS_11char_traitsIcEENS_9allocatorIcEEEE
```

**Integers (int)** : int 是一种数据类型，在 Python 和 C++中象征着同样的东西:整数。每当我们做数学、计数等时。使用整数我们将使用整数，用单词 int 表示。以下是 Python 中输出整数的代码示例:

```
print(1)print(type(1))
```

int 部分有以下端子输入和输出:

```
$ python3 data_types.py>> 1>> <class ‘int’>
```

以下是 C++中相同的整数输出:

```
std::cout << 4 << std::endl;std::cout << typeid(4).name() << std::endl;
```

利用 int 部分的端子输入和输出:

```
$ g++ -o data_types data_types.cpp$ ./data_types>> 4>> i
```

注意我们在输出语句的末尾使用了 std::endl。这是我们可以使用的第二种方法，在之前的输出之后创建一个新行，而不是使用“\n”。当我们的 output 语句不需要包含引号时，这是很有用的，整数就是这种情况。

**Float** :浮点数是两种编程语言中都有的另一种数据类型。这些是包含小数点的分数，例如 3.14 或 32.68。在 C++中，浮点由特定的最大精度定义，在 7 到 9 位数之间。以下代码在 Python 中输出一个浮点数:

```
print(1.5)print(type(1.5))
```

使用终端:

```
$ python3 data_types.py>> 1.5>> <class ‘float’>
```

下面的代码在 C++中输出相同的浮点数:

```
float c = 4.5;std::cout << c << std::endl;std::cout << typeid(c).name() << std::endl;
```

使用终端:

```
$ g++ -o data_types data_types.cpp$ ./data_types>> 4.5>> f
```

**Double:** 在这里，我们遇到了第一个在 C++中发现但在 Python 中没有的数据类型。这种数据类型与 float 非常相似，除了一个主要区别:它是一个浮点数，精度高于 float 数据类型。实际上，float 具有 32 位精度，而 double 具有 64 位精度。程序员希望达到的精度水平将是使用哪种数据类型的决定因素。在 C++中，使用浮点时默认且最常见的类型是 double。下面的代码在 C++中输出一个双精度值:

```
double d = 4.555555555555555555;std::cout << d << std::endl;std::cout << typeid(d).name() << std::endl;
```

使用终端:

```
$ g++ -o data_types data_types.cpp$ ./data_types>> 4.55556>> d
```

**Characters(char):**Characters 是 C++中的第二种主要数据类型，但在 Python 中没有，尽管术语 character 在 Python 中用于描述类似的事情。字符是字符串中的一项，可以是任何单个字母、数字或用单引号括起来的特殊字符。一些例子包括:' a '，' 1 '，' # '等。在 Python 中，我们说一个字符串，比如“Hello World！”由一组字符组成，“H”，“e”，“l”，“l”，“o”等。然而，在 Python 中，这些字符中的每一个仍然被认为是单个字符串。C++更进了一步，将它们归为一个单独的数据类型，char。如上所述，字符串的特征是双引号，而字符的特征是单引号。下面的代码在 C++中输出一个字符:

```
std::cout << ‘a’ << std::endl;std::cout << typeid(‘a’).name() << std::endl;
```

使用终端:

```
$ g++ -o data_types data_types.cpp$ ./data_types>> a>> c
```

**布尔(bool):** 布尔数据类型，在 C++和 Python 中都有，用于回答是或否的问题。布尔数据类型只包含两种可能性，Python 中的 True 或 False(大写)，C++中的 true (1)或 false (0)。这种数据类型通常在程序员希望检查特定条件时使用，例如“如果 x 是 5”:我们将在下一卷中了解更多关于条件的内容。

以下代码在 Python 中输出一个布尔值:

```
print(True)print(type(True))
```

使用终端:

```
$ python3 data_types.py>> True>> <class ‘bool’>
```

以下代码在 C++中输出相同的布尔值:

```
std::cout << true << std::endl;std::cout << typeid(true).name() << std::endl;
```

使用终端:

```
$ g++ -o data_types data_types.cpp$ ./data_types>> 1>> b
```

# **变量**

程序员经常需要一种方法来存储一些数据类型以备后用；不管使用哪种编程语言，都是如此。这些存储容器被称为变量。变量是保持代码整洁和减少程序中出现错误的可能性的一种方式，尤其是当程序变得更大更复杂的时候。如果你来自 Python 语言，你会非常理解这一点。要使用一个变量，程序员必须首先声明这个变量；这实际上是告诉计算机我们想要使用内存中的一个特定的存储位置，并且我们想要给这个存储空间指定一个特定的名称。例如，在 Python 中，我们会声明一个名为 a 的变量，并给它分配字符串“Hello Readers！”使用以下代码:

```
a = “Hello Readers!”
```

其中 a 是计算机内存中某个空间的标题，字符串是我们分配给该存储空间的内容。在 python 中，当声明一个变量时，我们不需要指定我们想要在那个变量中存储什么数据类型，因为在后台运行的软件会自动推断出来。在 C++中，我们没有得到同样的简化(记住，速度比易用性更重要！).在下面的代码中可以看到 C++中相同的变量声明和内容赋值:

```
#include <iostream>#include <string>int main(){std::string a = “Hello Readers!”;
return 0
}
```

从这段代码中可以看出，在声明变量时，程序员还必须包括他/她打算在该变量中存储什么数据类型(存储空间)；C++编程语言不能推导出数据类型。在 C++中，可以在不立即给变量分配任何内容的情况下进行变量声明——这给存储空间分配了一个名称，但暂时让它为空，以便在程序的稍后阶段可以给变量分配所声明数据类型的一些内容。下面的代码就是一个例子:

```
int b;
```

以下代码显示了变量在 Python 中的用法，使用不同数据类型的内容，并输出每个变量的赋值内容(存储空间):

```
a = “Hello Readers!”b = 1c = 1.5d = 1.67426855349e = ‘a’f = Trueg = Falseprint(a,”\n”,b,”\n”,c,”\n”,d,”\n”,e,”\n”,f,”\n”,g)
```

以下 C++代码使用了相同或相似的内容:

```
#include <iostream> // Used for cin and cout#include <string> // Used to allow us to declare stringsint main(){std::string a = “Hello Readers!”;int b = 1;float c = 1.5;double d = 1.67426855349;char e = ‘a’;bool f = true;bool g = false;std::cout << b << std::endl;std::cout << c << std::endl;std::cout << d << std::endl;std::cout << e << std::endl;std::cout << f << std::endl;std::cout << g << std::endl;return 0;}
```

# **收集用户输入**

在大多数程序的某一点上，开发人员需要从用户那里收集某种输入或数据；此外，从你自己作为不同程序的用户的经验中你会知道，在大多数情况下，如果你正在使用的程序在某种程度上不是交互式的，它会很快失去用户的兴趣或者变得不那么容易记住。既然如此，理解如何从用户那里收集某种输入就变得至关重要；今天我们将触及最基本的问题。

在 Python 中，这是通过直接内置于编程语言的后台软件中的另一种方法来实现的。这是通过简单地调用方法 input()并在括号内传递一个参数来完成的，该参数包括某种提示，通知用户应该输入什么。如果我们只想打印出用户输入的任何内容，我们可以将 print()方法与 input()方法结合使用，例如下面的代码:

```
print(input(“What is your name? \n”))
```

我们还可以将用户的输入保存到某个变量中，供以后使用。在下面的代码中可以看到一些这样的例子:

```
a = input(“Type in a phrase: \n”)b = input(“Type in a whole number: \n”)c = input(“Type in a decimal number: \n”)d = input(“Type in ‘True’ or ‘False’: \n”)
```

但是，请记住，用户的所有输入都以字符串格式返回给程序。在上面的代码中，当我们检查每个变量的数据类型时，我们得到的是字符串输出，即使在我们需要整数、浮点数或布尔值的情况下也是如此。使用上述变量:

```
print(a, “\n”, type(a))print(b, “\n”, type(b))print(c, “\n”, type(c))print(d, “\n”, type(d))
```

通过终端:

```
$ python3 collecting_input.py>> Type in a phrase
>> I like bacon
>> Type in a whole number:
>> 5
>> Type in a decimal number:
>> 2.3
>> Type in ‘True’ or ‘False’:
>> True
>> I like bacon
>> <class ‘str’>
>> 5
>> <class ‘str’>
>> 2.3
>> <class ‘str’>
>> True
>> <class ‘str’>
```

这个问题可以使用 Python 的内置方法来解决:int()、float()和 bool()，假设满足适当的条件，这些方法只是将传入的参数转换为所需的数据类型。这种转换和新的数据类型输出可以在下面的代码中看到，使用了上面相同的变量:

```
b = int(b)c = float(c)d = bool(d)print(a, “\n”, type(a))print(b, “\n”, type(b))print(c, “\n”, type(c))print(d, “\n”, type(d))
```

使用终端:

```
$ python3 collecting_input.py>> Type in a phrase
>> I like bacon
>> Type in a whole number:
>> 5
>> Type in a decimal number:
>> 2.3
>> Type in ‘True’ or ‘False’:
>> True
>> I like bacon
>> <class ‘str’>
>> 5
>> <class ‘int’>
>> 2.3
>> <class ‘float’>
>> True
>> <class ‘bool’>
```

在 C++中，使用了有点类似的方法。这个方法在 iostream 库中使用 std 关键字找到(与 cout 方法相同的库)，称为 cin。当我们想从用户那里检索某种数据时，我们首先使用 cout 方法向用户发送一个提示。在下一行中，我们指出我们希望将用户的输入保存到某个变量中(这个变量在程序的前面已经声明了)。因为变量声明包括预期的数据类型，所以我们不需要像在 Python 中那样使用转换方法。然而，当处理字符串时，C++中的用户输入是以一个字符接一个字符的方式收集的。简单地使用 std::cin 方法，将用户的输入赋给变量 a，如果用户键入“Hello Reader！”该程序将只捕捉和存储' H '字符。要获得完整的字符串，首先需要调用 getline()方法，然后传入参数 std::cin 来指定我们希望从用户那里获得完整的输入行，后面是用来存储该行的变量名 we。下面的代码就是一个例子:

```
#include <iostream>#include <string>#include <typeinfo>int main(){std::string a;int b;double c;bool d;std::cout << “Type in a phrase: \n”;getline(std::cin, a);std::cout << a << endl;return 0;
}
```

然而，当处理其他数据类型时，这不是必需的；我们只需要声明一个变量，通过 std::cout 方法给用户一些提示，然后使用 std::cin 方法将用户的输入存储到声明的变量中。请记住使用 std::cout 方法和 std::cin 方法之间的区别，即我们想要输出的内容位于双左箭头之后，<>。这可以从下面的代码示例中看出:

```
#include <iostream>#include <string>#include <typeinfo>int main(){std::string a;int b;double c;bool d;std::cout << “Type in a phrase: \n”;getline(std::cin, a);std::cout << “Type in a whole number: \n”;std::cin >> b;std::cout << “Type in a decimal number: \n”;std::cin >> c;std::cout << “Type in ‘True’ or ‘False’: \n”;std::cin >> d;std::cout << a << std::endl;std::cout << typeid(a).name() << std::endl;std::cout << b << std::endl;std::cout << typeid(b).name() << std::endl;std::cout << c << std::endl;std::cout << typeid(c).name() << std::endl;std::cout << d << std::endl;std::cout << typeid(d).name() << std::endl;return 0;}
```

感谢您加入我本周的 Python 到 C++系列文章！我希望你和我学得一样多，我希望下周能再见到你，届时我们将学习如何用两种语言处理数字和字符串，包括基本的数学运算和将字符串分解成基本部分。一如既往，神奇的读者，重复它，快乐编码！