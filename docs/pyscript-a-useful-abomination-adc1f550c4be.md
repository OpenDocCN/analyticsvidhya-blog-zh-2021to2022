# PyScript:一个有用的憎恶？

> 原文：<https://medium.com/analytics-vidhya/pyscript-a-useful-abomination-adc1f550c4be?source=collection_archive---------1----------------------->

![](img/affdfd75c27c042e010e33552f3701ca.png)

Python 正在入侵 HTML。Artturi Jalli 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

*我是乔治，斯坦福大学毕业的科技企业家和人工智能工程师。关注我这里的 medium*[*George Davila Durendal*](https://medium.com/u/6a9a11dd71c5?source=post_page-----adc1f550c4be--------------------------------)*或以上的*[*LinkedIn*](https://www.linkedin.com/in/george-durendal)*获取更多的技术&代码。*

上周，Anaconda 的数据科学团队发布了一个名为 PyScript 的库，该公司开发了著名的 python 包管理系统[*【conda*](https://en.wikipedia.org/wiki/Conda_(package_manager))。PyScript 荒谬地声称将 python 代码直接粘贴到 HTML 中

```
<html> <head>
     <title>PyScript Test</title>
     <script defer src="[https://pyscript.net/alpha/pyscript.js](https://pyscript.net/alpha/pyscript.js)">
     </script>
   </head> <py-script> 
   print('Hello world welcome to PyScript') 
   </py-script></html>
```

是的，这是一个真正的功能网页。

# 为什么？

看到上面的例子，您可能会认为 Anaconda 数据科学团队已经疯了。但是实际上，PyScript 提供了非常有用的创新。

最近一直在发生一场鲜为人知的战争。不同的框架一直在争夺机器学习和数据科学的前沿。

大多数数据科学家和机器学习工程师都是后端工程师。大多数建模 ML 工程师用 python 完成他们的日常工作，一小部分使用 C++。大多数数据科学家用 python、R、Julia 甚至 Excel 来做日常工作。相对而言，很少有人精通前端技术。即使是那些有经验的人也会发现为他们的模型构建一个合适的前端是破坏性的和耗时的。

像 [Streamlit](https://streamlit.io/) 、 [Gradio](https://gradio.app/) 和 [PyWebIO](https://pywebio.readthedocs.io/en/latest/) 这样的框架一直在试图弥合这个差距。所有这些框架都是 python 前端，允许我们直接用 python 编写 web 界面(非常重要的是，以 python 友好的方式)。您可以使用这些框架编写任何东西，从简单的计算器到[图像风格转换演示](/analytics-vidhya/create-your-own-deepart-clone-pytorch-neural-style-transfer-web-app-using-streamlit-with-code-b12c0e41d4c7)，而无需离开 python。这些都有它的好处。Streamlit 非常 pythonic 化，设计精美，易于交互。Gradio 自动通过隧道连接到网络，使得从笔记本电脑部署应用程序变得非常容易。但是这些框架有一个尖锐的问题，不容易部署和/或集成到 JS 前端。也就是说，我不能只是在我的网站上贴一个按钮，以一种一劳永逸的方式说“点击我来运行这个 python 脚本”。至少您需要在某个地方托管一个 python 脚本(Pythonanywhere 对于这个用例来说是一个非常好且简单的云主机，但是它仍然需要相当多的设置和/或成本)。

另一方面，虽然神奇的 [Tensorflow.js](https://www.tensorflow.org/js) 框架在技术上是一个后端框架，但它使构建通过 WASM 和 WebGL 在用户设备上运行的 ML 前端变得非常容易。 [ML5.js](https://github.com/ml5js/ml5-library) 是这种前端简单性的特别精彩的升华，因为你可以在[仅仅 70 行代码](https://editor.p5js.org/ml5/sketches/BodyPix_Webcam)中在浏览器中部署像人体分割模型一样复杂的 ML 模型。唉，这仍然存在 1)用 javascript 编写和 2)比 python tensorflow(后端使用 C 而不是 JS)慢的问题。因此，如果一个 python 工程师学习 Tensorflow.js 只是为了编写一个 web 演示，他们仍然必须用 python 部署最终的模型，浪费时间和资源。而且很多 ML 模型反正是用 sklearn 写的，所以既有语言又有框架不兼容的问题。

PyScript 解决了其中的一些问题。它只从一个基本服务器在浏览器中运行 python。所以你可以在你的免费 github.io 网站上运行你的 ML/DS 演示，如下所示。不仅如此，您还可以无缝地将输入和输出从 python 传递到 javascript 或 html，反之亦然。在实践中，这意味着你可以在你的网站上嵌入一个 python 模型，而不必太担心托管或维护。

# 用 PyScript 编码

[](https://github.com/pyscript/pyscript) [## GitHub - pyscript/pyscript

### PyScript 是 Scratch、JSFiddle 或其他“易于使用”的编程框架的 Pythonic 替代品，使 web 成为一个…

github.com](https://github.com/pyscript/pyscript) 

PyScript 声称能够在你的 html 中运行 python。所以下面的 JavaScript

```
<script> 
document.write('Hello World');
</script>
```

和下面的 PyScript

```
<py-script> 
print('Hello World') 
</py-script>
```

产生相同的结果。如果您和我一样，您可能会认为 pyscript print()等同于 JS console.log()，事实上这也是我第一次编写上面的例子时所做的假设。但显然目前情况并非如此(这可能会改变，正如我在下面展示的，我们可以将 python 输出分配给 html 元素，因此 print=console.log 可能是更有用的关系)。

下面是上面 hello world 示例的工作演示:

 [## PyScript 测试

georgedavila.github.io](https://georgedavila.github.io/helloWorldPyScript.html) 

对于更复杂的演示，我们可以看一个由 [Tirthajyoti Sarkar](https://github.com/tirthajyoti) 编写的例子，它使用像 numpy 和 matplotlib 这样的 python 库来生成随机分布和绘图:

 [## PyScript 随机数生成

### 为了在您的浏览器上提供 Python 的强大功能，这项技术需要加载一个特殊的虚拟机和所有…

tirthajyoti.github.io](https://tirthajyoti.github.io/pyscript_random_gen.html) 

我们应该注意代码片段

```
<py-env>      
- matplotlib 
</py-env>
```

这是设置环境，实际上就像通常的“pip install matplotlib”一样。Numpy 是一个标准的 python 库，所以 pyscript 带有默认的 python 配置，我们通过将外部库包含在<py-env>中来导入它们。</py-env>

我们可以进一步观察到，python 脚本正在从 html 元素中获取值，并使用 PyScript 自己的元素函数将数字作为输出传递回来

```
<py-script>
import numpy as np
import matplotlib.pyplot as plt out = Element("outputDiv")
input_num = Element("how-many")
dist_type = Element("dist").value
out2 = Element("outputDiv2")...out.write(f"Here are {n} random variates from Normal distribution: {r}") ...out2.write(fig1)
</py-script>
```

为了了解它如何处理更复杂的 python 库，我尝试在我的 github.io 页面上部署了 [PyScript 团队的 scikit-learn k-means 集群示例](https://github.com/pyscript/pyscript/blob/main/pyscriptjs/examples/panel_kmeans.html):

 [## Pyscript/Panel KMeans 演示

### 导入 asyncio 导入 altair 作为 alt 导入 panel 作为 pn 导入 panel 作为 pd 从 panel.io.pyodide 导入 show 从…

georgedavila.github.io](https://georgedavila.github.io/panel_kmeans.html) 

加载上面的页面可能需要一点时间，但是当它加载时，你可以看到 PyScript 库能够承载更复杂的代码。我只需要上传一个 HTML 文件到 github。它的简单性很好地缓解了通常与机器学习相关的繁琐的环境管理。

所以我们有它！一种将 python 脚本轻松集成到前端的方法。特别是 scikit-learn 库，它是现代机器学习和数据科学的重要支柱。因此，轻松托管和共享 sklearn 模型的能力很可能使 PyScript 成为数据科学家的最爱，尤其是那些想要托管项目或开发概念证明的人。整个网站甚至可能围绕这一点而建立。例如，假设你建立了一个 sklearn 模型，它根据某人的身高和体重输入来告诉他做多少个俯卧撑。现在，您可以使用 PyScript，而不是将 python 模型部署到云中，并通过 JQuery 作为 API 访问它——这是一周前的做法。在可预见的未来，PyScript 可能仍然是两个选项中较慢的一个；因此，企业级模型应该得到适当的部署和协调。但是它很好地填补了向世界推出一个最小可行模型的甜蜜点。

# 笔记

Brython 已经存在了相当一段时间。我大约一年前看到它，但很快发现它不适合现代 ML/DS 用例。相反，PyScript 似乎是考虑到这些字段而从头开始构建的。它问世不到一周，已经可以和 sklearn 一起使用了。

从我目前的测试来看，Tensorflow.js 在浏览器内推理方面似乎比 PyScript 快得多。