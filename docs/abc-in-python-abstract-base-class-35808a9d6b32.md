# Python 中的 ABC(抽象基类)

> 原文：<https://medium.com/analytics-vidhya/abc-in-python-abstract-base-class-35808a9d6b32?source=collection_archive---------5----------------------->

![](img/7e212f0ab4954d43945dc5a582a1709f.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**简介**

抽象类是面向对象编程中的一个重要概念。它就像是其他职业的蓝图。

**我们在哪里使用抽象？**

对于较大的项目，不可能记住类的细节，而且代码的可重用性也会增加 bug。因此，它在我们的项目中起着至关重要的作用。

默认情况下，Python 不提供抽象类。Python 库中的' **abc** '模块提供了定义自定义抽象基类的基础设施。

抽象**类**不能在 python 中实例化。抽象的**方法**可以被它的子类调用。

上述模块为我们提供了以下内容:

1.  ABCMeta
2.  ABC(助手类)

这些是用于创建抽象类的关键字。

**ABCMeta** :
是定义抽象类的元类。

**ABC:**
它是一个 helper 类。我们可以在希望避免元类用法混乱的地方使用它。

这两个步骤是相似的。

**注:**[ABC](https://docs.python.org/3/library/abc.html#abc.ABC)(type(ABC))的类型仍然是 [ABCMeta](https://docs.python.org/3/library/abc.html#abc.ABCMeta) 。因此从 [ABC](https://docs.python.org/3/library/abc.html#abc.ABC) 继承需要关于元类使用的通常的预防措施，因为多重继承可能导致元类冲突。

**register():**
我们可以用 register 把一个具体的子类或者内置的子类做成一个虚拟的子类。但是在方法解析顺序中不可见(MRO)。使用 super()甚至不能调用它的方法。

**@抽象方法:**

它用于使抽象类的方法成为抽象方法。

可以使用任何普通的“超级”调用机制来调用抽象方法。abstractmethod()可用于声明属性和描述符的抽象方法。

不支持向类中动态添加抽象方法，或者在方法或类创建后试图修改其抽象状态。abstractmethod()只影响使用常规继承派生的子类；用 ABC 的 register()方法注册的“虚拟子类别”不受影响。

还有其他装饰器(@abstractclassmethod，@abstractstaticmethod)和一些可用的魔法方法。我们可以进一步讨论这个问题。

这些是 ABC 的基础和最需要的部分。

谢谢你，🧡…..