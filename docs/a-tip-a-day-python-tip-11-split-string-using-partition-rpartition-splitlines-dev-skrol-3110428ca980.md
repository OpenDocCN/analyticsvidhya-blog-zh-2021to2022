# 一天一个技巧— Python 技巧#11:使用 partition、rpartition & splitlines | Dev Skrol 拆分字符串

> 原文：<https://medium.com/analytics-vidhya/a-tip-a-day-python-tip-11-split-string-using-partition-rpartition-splitlines-dev-skrol-3110428ca980?source=collection_archive---------1----------------------->

![](img/e6d2d6aef8ab9194c1d3a60ceb0a7a11.png)

塔妮娅·梅尔尼克祖克在 [Unsplash](https://unsplash.com/s/photos/split?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

拆分字符串有多种方法。

我们经常使用的流行的 split 函数是 split()，r split()

除了这 3 个函数，还有一些函数在字符串操作中非常有用。

分区函数将整个字符串分成 3 部分，并将其作为一个包含 3 个元素的元组返回。

1.  分隔符前的字符串
2.  分离器
3.  分隔符后的字符串

该函数只拆分一次，不会迭代拆分。所以它在分隔符第一次出现时就分裂了。

# 语法:

## string.partition(分隔符)

```
fullstring = "partition function is a type of split function in python." 
result = fullstring.partition("function") 
print(result) 
print(type(result))
```

**输出:**

```
('partition ', 'function', ' is a type of split function in python.') <class 'tuple'>
```

# 2.rpartition()

这个函数的工作方式与 partition()类似，只是它考虑分隔符的最后一次出现。

```
fullstring = "partition function is a type of split function in python." 
result = fullstring.rpartition("function") 
print(result)
```

**输出:**

```
('partition function is a type of split ', 'function', ' in python.')
```

# 3.分割线()

该函数仅通过换行符进行拆分。

这将不接受任何分隔符，而是接受另一个可选的布尔参数“keepends”。

如果 keepends 为 True，那么它将返回换行符以及换行符的左边部分。

默认情况下，keepends 设置为 False。

```
fullstring = "partition function splits a string.\n This splits by separator.\n Returns tuple of 3 elements.\n Lets learn it." 
result = fullstring.splitlines() 
print(result)
```

**输出:**

```
['partition function splits a string.', ' This splits by separator.', ' Returns tuple of 3 elements.', ' Lets learn it.']
```

**另一个带有** *keepends* **参数的例子:**

```
result = fullstring.splitlines(keepends=True) 
print(result) 
type(result)
```

**输出:**

```
['partition function splits a string.\n', ' This splits by separator.\n', ' Returns tuple of 3 elements.\n', ' Lets learn it.'] list
```

我希望您喜欢学习 python 中各种类型的 split 函数。

让我们在未来的技巧中探索更多关于 Python 的内容。

# 更多有趣的 Python 技巧:

[一天一个技巧——Python 技巧 9:关于格式{}的 3 件趣事](https://devskrol.com/index.php/2021/09/03/a-tip-a-day-python-tip-9-3-interesting-things-about-format/)

[一天一个提示——Python 提示# 6——熊猫合并](https://devskrol.com/index.php/2020/10/25/a-tip-a-day-python-tip-6-pandas-merge/)

[一天一个提示— Python 提示#5 —熊猫串联&追加](https://devskrol.com/index.php/2020/10/20/a-tip-a-day-python-tip-5-pandas-concat-append/) [所有其他提示](https://devskrol.com/index.php/category/python-tips/)

# 其他有趣的概念:

[SpaCy Vs NLTK —基本 NLP 操作代码和结果比较](https://devskrol.com/index.php/2021/04/17/spacy-vs-nltk-basic-nlp-operations-code-and-result-comparison/)

[在组内估算 NAN 的最佳方式—均值&模式](https://devskrol.com/index.php/2020/08/09/best-way-to-impute-nan-within-groups-mean-mode/)

*原载于 2021 年 10 月 16 日*[*【https://devskrol.com】*](https://devskrol.com/index.php/2021/10/16/a-tip-a-day-python-tip-11-split-string-using-partition-rpartition-splitlines/)*。*