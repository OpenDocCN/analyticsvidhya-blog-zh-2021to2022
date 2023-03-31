# 了解 Diffie-Hellman 密钥交换

> 原文：<https://medium.com/analytics-vidhya/understanding-the-diffie-hellman-key-exchange-7eb594e3f736?source=collection_archive---------5----------------------->

![](img/172396d43846d30c37c0f92bbc25ac25.png)

Diffie Hellman 是一种看起来不可能的数学把戏，直到完成，它才变得显而易见。

这里的问题是在没有实际交换的情况下交换一个密钥(把它想象成一个密码)。因为如果你做了交换，有可能会有一个正在听的第三个人知道你的秘密。

但这怎么可能呢？两个从未谋面的人如何能就一个秘密密钥(数字)达成一致，并保证没有第三个人能猜出或发现它？

# 出路？Diffie-Hellman 密钥交换技术

下面是外行人对一个最美的密码学概念的解释。Diffie-Hellman 现在几乎在任何地方每次交换密钥时都会用到。当然，我们在这里不涉及数学细节，我们的主要焦点是理解它是如何工作的(这对我们来说已经足够了)。

**现在开始，让我们假设爱丽丝和鲍勃很天真，对密码学一无所知。**

![](img/ab6fba62a709894a4831b1cc339759ff.png)![](img/0f64baac283adfc930c95bcbaddfe07f.png)![](img/a58e5d76ed3dda689a658e4701765307.png)![](img/db3675b5b9fca9c01764fccd6e9d98da.png)![](img/9babf5817c7b7b5b637a45c325cadcee.png)

**现在，这看起来是一个聪明的举动，但是当然，也有不利的一面。**

![](img/433c7b49ca03290fb349daf749be5fd8.png)![](img/903a5c1571a66b2e05ca5a9797c15e4f.png)![](img/e0eaca816653717f69541d3145d95ca0.png)![](img/d4ae0b7c86738f86e860b5045b07c6cf.png)![](img/65632685a2b021a32c0b348de21da8ac.png)

**密码学所有问题的根源。如何把第一个秘密发出去？**

![](img/8b43fa36c310c36d65975154b9347839.png)![](img/38f7e790ee4d60913ea508653e2e7c82.png)

Diffie 和 Hellman 的天才来了。黄金法则:

![](img/9c4cb72fbc13201902c4c86151f01dc4.png)

**最容易理解的方法是使用颜色**

![](img/aef934afb3abc2882de6a8dca33d999b.png)![](img/0c48f5eb743a1fea5cce16e984ef9645.png)![](img/1b962a51b998cad673c0079670f47ee6.png)![](img/f7f5706527990855448537f2f69bd6e7.png)

**这就是第一条原则发挥作用的地方:做起来容易，做起来难**

![](img/ec66238cf595e27d7a38dc76a9d96a47.png)

**伊芙永远也不会仅仅通过观察混合物来判断出哪两种颜色是混合在一起的**

![](img/d597da25d6a645e9eacbed72e1a6f4bf.png)![](img/2d32153dee83346b8adae794fe0dfd56.png)

第二个原则是:同一件事可以用多种方式完成。

所以下面的两个将给出相同的输出:

> 将红色和蓝色混合，然后加入绿色。
> 
> 将绿色和红色混合，然后加入蓝色

![](img/c42f3a1246ad92e37fd35e494b31b4d1.png)

现在 Alice 使用秘密密钥加密她的文本。Bob 可以使用相同的密钥来解密它。

![](img/9730fa41e8d48f5d46bd9e8354c20efa.png)![](img/97e839200d0899a351a919c0785adb8d.png)![](img/39a7ce3c473a6913511ef6b33b179b24.png)![](img/618623bf2fc203fe15cc11d60a74a023.png)

**如你所见，私色与终秘色永不越界。**

![](img/19df3fa8e8c61fc63b7d10a3b8d56de4.png)![](img/578e6bbd9e2ec71baafa4c649095a9cd.png)

我会用实际的数学公式和计算做另一个博客，但这里是摘要。只要用数字代替颜色，用公式代替混合物，你就能得到同样的东西。

![](img/dd996454a2c722d6bda70024fcd220dc.png)![](img/31e061962fd1b64344bdeb5b487b3f4d.png)

[参考文献](https://www.youtube.com/watch?v=YEBfamv-_do&ab_channel=ArtoftheProblem):

图片提供:

[https://pixabay.com/users/mastertux-470906/?UTM _ source = link-attribute&amp；UTM _ medium = referral&amp；UTM _ campaign = image&amp；utm_content=3348307](https://pixabay.com/users/mastertux-470906/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=3348307)