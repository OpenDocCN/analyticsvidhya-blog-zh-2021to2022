# 恐怖主义数据分析:印度的纳萨尔恐怖主义

> 原文：<https://medium.com/analytics-vidhya/terrorism-data-analysis-red-terror-in-india-e026fd0e3a41?source=collection_archive---------17----------------------->

![](img/bc80121b51f50f7e24942de1f8bb68e8.png)

在查蒂斯格尔邦的苏克马区，22 名印度士兵遭到 PLGA 的伏击。据报道，毛派分子试图破坏 Sukma 区 Silger-Jagargunda 附近的一项道路建设。这条路危及他们的生存。这次袭击是由 PLGA 酋长马德维·希德马领导的。

我想分析印度过去 50 年的恐怖主义趋势。所以我从[](https://www.kaggle.com/START-UMD/gtd)**下载了全球恐怖主义数据(1972–2017)。**

# **数据分析**

**让我们从将 CSV 数据导入数据框并显示前 5 条记录开始。**

```
import pyforest
terror_data=pd.read_csv("terror.csv")
terror_data.head()
```

**![](img/9fd06d80e5705f304e03a72a84813088.png)**

**我们创建了一个数据框。现在我将搜索印度的数据，只保留相关的列用于我的分析。CSV 文件中共有 135 列。**

```
india=terror_data[terror_data["country_txt"]== "India"]india_terrorism=india[["iyear", "imonth","provstate", "city", "attacktype1_txt","target1", "gname","gsubname","motive",  "weapdetail", "nkill","location"]]
```

**![](img/ab26bdb44667b9a487ce08da8150a026.png)**

**我们将统计从 1972 年到 2017 年间印度发生的恐怖袭击次数。**

```
india_terrorism["iyear"].value_counts()
```

**![](img/b5697c5a99782e576ad0b63ac5952c7f.png)**

**就恐怖袭击而言，2016 年是印度最致命的一年，而 1981 年是最和平的一年。**

## **70 年代的恐怖袭击(1972 -1980)**

**![](img/6e3da26da70dd2597a1f1bdea87f64fa.png)**

**70 年代是印度处理米佐拉姆致命叛乱的时期。我在我的缅甸文章中提到，米佐拉姆刚刚走出暴力，这就是为什么缅甸问题应该非常谨慎地处理。**

```
terror_70 = india_terrorism[(india_terrorism['iyear'] == 1972) | (india_terrorism['iyear'] == 1973) | (india_terrorism['iyear'] == 1974)|(india_terrorism['iyear'] == 1975)|(india_terrorism['iyear'] == 1976)|(india_terrorism['iyear'] == 1977)|(india_terrorism['iyear'] == 1978)|(india_terrorism['iyear'] == 1979) ]
terror_70
```

**![](img/8b085448a2cae502328f269e2300aeaf.png)****![](img/632d979a06cd98247b0ec2d42550fbd2.png)****![](img/6a444093b8c2261636299d3f1eaf4b60.png)**

****被恐怖组织杀害的人(gname)****

```
terror_70["nkill"].sum()
```

**![](img/6986727e4b369ec826954e0567fc23fb.png)**

```
terror_70=terror_70.groupby("gname").sum()
terror_70["nkill"]
```

**![](img/254754f16aee139ea352e880b21afd52.png)****![](img/d8fe71a873121db9d07b12a90f816123.png)**

> **纳萨尔派和毛派在 70 世纪杀害了 35 人中的 7 人。占所有恐怖袭击的 20%。**

## **80 年代(1980-89)的恐怖袭击**

**![](img/321ae2ea2ea228e4e69ade6c06fdd5c3.png)**

**这十年旁遮普发生了致命的叛乱。在这十年里发生了许多不幸的事件。**

```
terror_80 = india_terrorism[(india_terrorism['iyear'] == 1981) | (india_terrorism['iyear'] == 1982) | (india_terrorism['iyear'] == 1983)|(india_terrorism['iyear'] == 1984)|(india_terrorism['iyear'] == 1985)|(india_terrorism['iyear'] == 1986)|(india_terrorism['iyear'] == 1987)|(india_terrorism['iyear'] == 1988)|(india_terrorism['iyear'] == 1989) ]terror_80
```

**![](img/361be3e6c516a49495ed2e1e2a0ebeda.png)****![](img/5334699e3b068d9f113353e5967f1947.png)**

****被恐怖组织杀害的人(gname)****

```
Total people killed by Terror attacks.terror_80=terror_80.groupby("gname").sum().sort_values(by="nkill", ascending=False)
terror_80["nkill"]
```

**![](img/cd6a2db70e98b600468eddb8de79b3f8.png)****![](img/a46676c516f78175db1f3fd4c08da47c.png)**

```
terror_80["nkill"].sum()
```

**![](img/5ae915abad799fed4104687e3d54bf82.png)**

> **80 世纪共有 3079 人死于恐怖袭击。纳萨尔派杀死 41 人，解放军杀死 20 人，毛派杀死 11 人。PWG 杀死了 9 人，部落分裂分子杀死了 5 人，共产党杀死了 1 人。共有 87 人被左翼分子杀害，占总伤亡人数的 2%。**

## **90 年代的恐怖袭击(1990 年至 1999 年)**

**![](img/5b6afeb7468f2c5d6a061c966a0018ab.png)**

**这是克什米尔交战的十年。大多数袭击发生在克什米尔和旁遮普。旁遮普的叛乱在 90 年代末结束。**

```
terror_90 = india_terrorism[(india_terrorism['iyear'] == 1990) | (india_terrorism['iyear'] == 1991) | (india_terrorism['iyear'] == 1992)|(india_terrorism['iyear'] == 1993)|(india_terrorism['iyear'] == 1994)|(india_terrorism['iyear'] == 1995)|(india_terrorism['iyear'] == 1996)|(india_terrorism['iyear'] == 1997)|(india_terrorism['iyear'] == 1998) |(india_terrorism['iyear'] == 1999)]
terror_90
```

**![](img/0948de5df8697144abd57f29c80da1d8.png)**

```
terror_90_count=terror_90["gname"].value_counts()
terror_90_count.head(60)
```

**![](img/e9df4fee83fcb335b0b43bb8f4e8792e.png)**

****被恐怖组织杀害的人(gname)****

```
terror_90=terror_90.groupby("gname").sum().sort_values(by="nkill", ascending=False)
terror_90["nkill"].head(50)
```

**![](img/85f85793be5a7a71cb670e8e44913066.png)****![](img/d236f45f5f79184c1711071b4053bb09.png)**

> **我想在这里提到的重要一点是， **Ranbir Sena** 我们可以在上面的列表中看到，是一个**反纳萨尔派**激进组织，主要由上层种姓男子创建，旨在结束纳萨尔派恐怖主义。**

```
terror_90["nkill"].sum()
```

**![](img/f68b2ae74bc94053945d036b088b4f53.png)**

> **PWG、MCC、纳萨尔派、解放军、毛派、部落游击队、毛派农场工人都是左翼极端主义的形式。他们杀死了 6211 人中的 500 人，占总数的 8%。**

## **20 世纪的恐怖袭击(2000-09)**

**![](img/c57316a9dc1f34eaba985fa9771c469c.png)**

**这十年见证了 2004 年 ***CPI-毛派*** 组织的形成，该组织开始了针对印度政府的致命叛乱，导致数千名毛派分子、警察、平民被杀。**

```
terror_20 = india_terrorism[(india_terrorism['iyear'] == 2000) | (india_terrorism['iyear'] == 2001) | (india_terrorism['iyear'] == 2002)|(india_terrorism['iyear'] == 2003)|(india_terrorism['iyear'] == 2004)|(india_terrorism['iyear'] == 2005)|(india_terrorism['iyear'] == 2006)|(india_terrorism['iyear'] == 2007)|(india_terrorism['iyear'] == 2008) |(india_terrorism['iyear'] == 2009)]terror_20
```

**![](img/b6b5cee1e12ec47620a123fbcdbe315e.png)**

```
terror_20_count=terror_20["gname"].value_counts()
terror_20_count.head(60)
```

**![](img/cdd86272743ee2d3e89e9d4c861bcaea.png)**

****被恐怖组织杀害的人(gname)****

```
terror_20=terror_20.groupby("gname").sum().sort_values(by="nkill", ascending=False)
terror_20["nkill"].head(50)
```

**![](img/f90e508d3f10f08f7a7d258d3607aa52.png)****![](img/04f73e40dc8e86ca8c7eada08f8c7e43.png)**

```
terror_20["nkill"].sum()
```

**![](img/36ff8f9088af25bd0e7d15d00061386e.png)**

> **总共 6148 人中有 1451 人被左翼极端组织杀害。占所有恐怖相关死亡的 23 %。它甚至可能更高，因为“未知”的攻击者也有很多左翼极端组织，但为了简单起见，我不做任何假设。**

## **21 世纪的恐怖袭击(2010-17)**

```
terror_21 = india_terrorism[(india_terrorism['iyear'] == 2010) | (india_terrorism['iyear'] == 2011) | (india_terrorism['iyear'] == 2012)|(india_terrorism['iyear'] == 2013)|(india_terrorism['iyear'] == 2014)|(india_terrorism['iyear'] == 2015)|(india_terrorism['iyear'] == 2016)|(india_terrorism['iyear'] == 2017)]terror_21
```

**![](img/ddf85992346a22316b1af7b952fe4196.png)**

```
terror_21_count=terror_21["gname"].value_counts()
terror_21_count.head(60)
```

**![](img/905eb47cfa9389b489f187b0026de9bc.png)**

****被恐怖组织杀害的人(gname)****

```
terror_21=terror_21.groupby("gname").sum().sort_values(by="nkill", ascending=False)
terror_21["nkill"].head(50)
```

**![](img/52ab10cc6a68a2af4aa91ee73cb9ba5e.png)****![](img/a35178ade02d16ba6197c0c4b9bab454.png)**

```
terror_21["nkill"].sum()
```

**![](img/347fc5b331ddf82c8bfa8952e0b98f42.png)**

> **印度共产党-毛派(CPI-Maoist)，毛派，印度人民解放阵线，人民解放军，CPI 马克思主义者，贾坎德邦解放猛虎组织，贾坎德邦普拉斯图蒂委员会(JPC)是具有**左派意识形态的恐怖组织，**他们杀害了总数为 3851 人(2010 年至 2017 年)中的 2349 人，占压倒性的 60%。**

# **受毛派影响的国家**

**尽管有太多的毛派组织，但是，为了简单起见，我只考虑其母组织(CPI-Maoist)。**

```
naxal=india_terrorism[india_terrorism["gname"]=="Communist Party of India - Maoist (CPI-Maoist)"]
naxal["provstate"].value_counts()
```

**![](img/5f6125d8d63b27973344776d4cfba4b8.png)**

# **毛派攻击的动机**

```
naxal["motive"].value_counts()
```

**![](img/3fa55780f8d8455825b7b77f4c67ea81.png)**

**大多数时候，村民因为向警方告密而被毛派杀害。**