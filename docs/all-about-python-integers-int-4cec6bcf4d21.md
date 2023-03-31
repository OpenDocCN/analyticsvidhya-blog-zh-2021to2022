# 关于 Python 整数(int)的所有内容

> 原文：<https://medium.com/analytics-vidhya/all-about-python-integers-int-4cec6bcf4d21?source=collection_archive---------14----------------------->

在 Python 编程语言中，一切都是对象。甚至文字也是一个对象。如果一切都是一个对象，那么肯定每个对象都会有自己的一套方法。根据文字的类型，每个文字都有自己的方法集。在这篇短文中，我们只关注整数对象。

让我们先来看看我们可以在整数上执行的不同操作:

```
>>> 5+6
11
>>> 5-6
-1
>>> 5&6
4
>>> 5==6
False

>>> 5//6
0
>>> 5/6
0.8333333333333334
>>> 5>=6
False
>>> 6>5
True
>>> 5<6
True
>>> 5<=6
True
>>> ~6
-7
>>> 5<<6
320
>>> 5>>6
0
>>> 6%5
1
>>> 5*6
30
>>> -5
-5
>>> 5|6
7 
```

## **方法:**

1.  *divmod() :*

返回元组，其中第一个元素是给定两个整数的除法结果，第二个元素是整数的模。

```
>>> divmod(5,6)
(0, 5)
```

2. *int():* 将浮点值和数字字符串转换为整数。

```
>>> int(5.6)
5
>>> int('6')
6
```

3. *float():* 将给定的整数值转换成浮点数。

```
>>> float(5) #converts given integer to float 
5.0
```

4. *pow():* 返回 x 的 y 次方值。

```
>>> pow(2,4) #returns value of 4 to the power of 2
16
```

5. *round():* 返回给定浮点数的舍入值。

```
>>> round(5.667) # round off 
6
```

6. *str():* 将给定整数转换为字符串。

```
>>> str(5)
'5'
```

7. *bin():* 返回给定整数的二进制值。

```
>>> bin(5)
'0b101'
```

8. *bit_length():* 返回用二进制表示所需的位数。

```
>>> (5).bit_length()
3
```

9. *int.to_bytes():* 将整数转换成给定长度的字节

**语法:**

(int_value)。to_bytes(length，byteorder，*，signed=False)

> 在哪里，
> 
> ***int_value* :** 要转换成字节的整数值
> 
> ***长度*** *:* 整数用长度字节表示。如果整数不能用长度参数的给定值来表示，则会引发 overflow error
> 。
> 
> ***byte order***:byte order 参数决定了用来表示整数的字节顺序。如果 byteorder 为“big”，则最高有效字节位于字节数组的开头。如果 byteorder 为“little”，则最高有效字节位于字节数组的末尾。若要请求主机系统的本机字节顺序，请使用“sys.byteorder”作为字节顺序值
> 
> **有符号**:该参数决定是否用二进制补码来表示整数。如果 signed 为 False 并且给定了负整数，则会引发 OverflowError。

**举例:**

```
>>> (45).to_bytes(2, byteorder = 'big')
b'\x00-'
>>> (45).to_bytes(4, byteorder = 'little')
b'-\x00\x00\x00'
```

10. *int.from_bytes():* 返回给定字节数组表示的整数。

**语法:**

int.from_bytes(bytes，byteorder，*，signed=False)

> 在哪里，
> 
> **bytes** :该参数必须是类 bytes 对象(如 bytes 或 bytearray)。
> **byte order:**byte order 参数决定了用来表示整数的字节顺序。如果 byteorder 为“big”，则最高有效字节位于字节数组的开头。如果 byteorder 为“little”，则最高有效字节位于字节数组的末尾。若要请求主机系统的本机字节顺序，请使用“sys.byteorder”作为字节顺序值。
> **有符号:**该参数表示是否用二进制补码来表示整数。

**举例:**

```
>>> (45).from_bytes(b'\x00-', byteorder = 'big')
45
```

在这里，你一定在想，我们使用整数对象调用这个函数，我们需要它作为输出。from_bytes()函数是 integer 对象的方法，所以我们只能用 integer 对象来调用它。您可以使用任何整数对象调用函数，如下所示:

```
>>> (56).from_bytes(b'\x00-', byteorder = 'big')
45
```

但是，这可能有点令人困惑，以下是调用此函数的最佳方法:

```
>>> int.from_bytes(b'\x00-', byteorder = 'big')
45
```

到目前为止，我们已经看到了可以对 integer 对象执行的不同操作以及 integer 对象的总共十种方法。如果你想了解更多关于 python 整数的知识，请在解释器中输入“help(int)”。