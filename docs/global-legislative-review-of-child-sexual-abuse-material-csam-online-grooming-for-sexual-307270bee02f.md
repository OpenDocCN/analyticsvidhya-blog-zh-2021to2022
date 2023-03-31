# 儿童性虐待材料的全球立法审查(CSAM)和为性目的的网上美容

> 原文：<https://medium.com/analytics-vidhya/global-legislative-review-of-child-sexual-abuse-material-csam-online-grooming-for-sexual-307270bee02f?source=collection_archive---------11----------------------->

用 d3.js 可视化全球立法

# 介绍

不久前，我与[救助儿童会](https://www.savethechildren.net/)合作参加了 [Omdena AI 挑战赛](https://omdena.com/projects/children-violence/)，救助儿童会是一家领先的儿童人道主义组织，有着一个大胆的抱负——创造一个让每个孩子都能接受优质教育、免受忽视、剥削和虐待的世界。

![](img/8baa9f1a7bd196628c7144dfe55566dc.png)

照片由[玛格丽特·韦尔](https://unsplash.com/@margotd1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/s/photos/children-playing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

在两个月的时间里，我们与救助儿童会的技术促进发展和儿童保护团队密切合作，从多个来源收集和分析数据，包括在线论坛、科学和报纸文章，以获取真知灼见。

# 互联网——一个危险的地方

截至 2021 年 1 月，约有 46 亿互联网用户，约占全球人口的 60%。随着互联网可用性的提高和可访问性的改善，这一数字将逐年增加。

世界上大多数人现在都可以访问互联网。据估计，每天大约有 875，000 名新用户，年轻人是互联网最活跃的用户。在美国，联邦调查局(FBI)估计任何时候都有近 750，000 名罪犯在线。

虽然对儿童的剥削也发生在现实世界中，但网络空间最重要的特征之一是它为用户提供了相对匿名性，这使得跟踪和追踪罪犯尤其具有挑战性。

虽然一些儿童(大多数来自贫困家庭)无法访问互联网，但对许多儿童来说，互联网的使用占据了越来越多的时间，在更多的地方，在更个性化的设备(手机和平板电脑)上。

在这个现代科技时代，在线虐待和修饰是一个越来越受关注的领域。

> 一项研究显示，至少有 13%的儿童报告在互联网上受到了负面的不必要的性关注。

# CSAM 和在线美容

**CSAM** 可能以各种形式出现，包括文字、图像、视频和/或图画。

> 2017 年，国际互联网热线协会 INHOPE 报告称，在收到的 120 万份报告中，18%是描述 14-17 岁儿童的性虐待图像，79%是 3-13 岁的儿童，3%是 0-2 岁的儿童。

**网上引诱**是指犯罪分子通过互联网与儿童交朋友，意图对他们进行性剥削的一系列策略。罪犯与儿童建立了信任关系(有时，他们自己也是如此)，他们利用这种关系操纵和/或迫使儿童进行不适当的活动，如性对话和/或制造和传播 CSAM。

在线诱导的最终目的是通过身体接触或虚拟接触对儿童进行性虐待。

![](img/f04990c9b7510e938b38c4df9d37786e.png)

由[马可·奥里奥西](https://unsplash.com/@marcooriolesi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](/collections/4941868/parliament%2Flegislature?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

# 问题

遏制这一增长趋势的一个非常重要的第一步是制定打击这一趋势的立法。与大多数类型的立法一样，世界各国的反虐待儿童立法往往不一致。许多国家有法律，但没有强制执行。

值得一提的是，一个国家缺乏关于网上诱导或 CSAM 的立法，并不意味着其他形式的虐待儿童行为不被定罪。

失踪和被剥削儿童国际中心制定了一项示范立法，以提高认识和获得更好的理解。发表了一些定期更新的报告来揭示这个问题。

为了更好地理解这个问题，ICMEC 探讨了许多关键标准，包括定义、犯罪、制裁和判刑(CSAM 还有额外的强制性报告和行业责任)。这些标准着眼于国家立法是否:

**CSAM**

> 存在于特定的 CSAM；
> 
> 提供了 CSAM 的定义；
> 
> 将借助技术的 CSAM 犯罪定为刑事犯罪；
> 
> 将明知故犯地拥有 CSAM 教定为犯罪，无论其传播的意图如何；
> 
> 要求互联网服务提供商(ISP)向执法部门或其他授权机构报告可疑的 CSAM 病毒。

**在线梳理**

> 存在以性为目的的网上引诱儿童的现象；
> 
> 提供修饰的定义(或描述),包括在线修饰，并使用计算机和互联网专用术语；
> 
> 将网上引诱定为犯罪，目的是在网下与儿童见面；
> 
> 将网上诱导定为犯罪，无论是否有意在网下与儿童见面；
> 
> 将向儿童展示色情制品定为犯罪。

# 数据和可视化

从报告中，我们通过收集可视化所需的信息生成了两个数据集。每行代表一个国家，每列代表以上详述的标准。这是通过对每个国家的立法进行文本分析而获得的。

数据的来源是[梳理](https://www.icmec.org/online-grooming-of-children-for-sexual-purposes-model-legislation-global-review/)和 [CSAM](https://www.icmec.org/csam-model-legislation/) 。单元格的值为二进制格式，如果符合该标准，则值为 1，否则为 0。

## CSAM

CSAM 数据集

## 在线美容

在线整理数据集

使用 d3.js，一个强大的基于 javascript 的交互式可视化工具，我们使用上面生成的数据集创建了一个地图可视化。可视化使用 Netlify 的免费 GitHub 托管(如果第一次没有正确加载，刷新页面)，可以在这里找到。

可视化是交互式的，包含列出的国家。页面顶部有一个开关，可以选择在线梳理或 CSAM 可视化。

选择一个国家将在地图下方显示与每个类别相关的信息。此外，请注意，国家/地区根据 0-5 分(通过计算该国家/地区的立法类别数计算得出)被标记为红色到绿色。对 CSAM 来说，一个全红色的国家意味着他们没有任何 CSAM 类别的立法，而一个全绿色的国家意味着所有的立法都在书本上。

来看看可视化是如何在 d3 中构建的—

d3.js 可视化代码

# 参考

詹姆斯 2020 在线疏导:它是什么，它是如何发生的，以及如何保护儿童[https://www . thorn . org/blog/Online-grooming-What-it-is-how-it-that-happen-and-how-defend-children/](https://www.thorn.org/blog/online-grooming-what-it-is-how-it-happens-and-how-to-defend-children/)

2018 事实与统计:*美国司法部 NSOPW* 。美国司法部[https://www.nsopw.gov/en-US/Education/FactsStatistics](https://www.nsopw.gov/en-US/Education/FactsStatistics?AspxAutoDetectCookieSupport=1)

ICMEC 2018 年儿童性虐待材料:示范立法和全球审查[https://www.icmec.org/csam-model-legislation/](https://www.icmec.org/csam-model-legislation/)

ICMEC 2017 网上为性目的培养儿童:示范立法&全球审查[https://www . IC mec . org/Online-Grooming-of-Children-for-Sexual-purpose-purpose-Model-Legislation-Global-Review/](https://www.icmec.org/online-grooming-of-children-for-sexual-purposes-model-legislation-global-review/)

Lim W，Guerra-Arias M，Oguntayo S 2020 使用自然语言处理探索关于针对儿童的在线暴力的科学文献【https://omdena.com/blog/online-violence-children/? FB clid = iwar 2 ewtp l5 xbqpmyfsl 9 zjmiyzqw 8g 51 HC 6 xk-0 ngy 3 jibfxk q 8 lfhgupnsw

美国司法部 2018 年针对儿童/网络大鳄的暴力犯罪【https://www.fbi.gov/investigate/violent-crime/cac