# 文本数据的数据清理

> 原文：<https://medium.com/analytics-vidhya/data-cleaning-for-textual-data-256b4bbffd?source=collection_archive---------3----------------------->

## 疯狂之旅。。。

![](img/a423a65a86d30c7c125518824ca7dac4.png)

[图为](https://unsplash.com/@towfiqu999999?utm_source=medium&utm_medium=referral) [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的

数据是执行的任何分析或创建的任何模型的基础。然而，数据可能会出错:格式、排列、额外的空格等。此外，随着数据每天呈指数级增长，处理和管理这些数据变得更具挑战性。每个行业——零售、银行、酒店、教育——都在巨大的数据海洋中航行。数据清理是一项极其重要的任务。

文本数据是非结构化数据的一种形式。因此，人们更容易筛选原始文本。但是，对于机器来说就不一样了。机器不能使用原始文本来训练或创建它们的模型，而且分析它们也很复杂。

在这篇博客中，我将介绍一些对我有帮助的文本数据清理任务。为了节省时间，我在大多数项目中重用这些代码片段。此外，您也可以将它包含在您的项目中。

# 规范化非 Ascii 字符

ASCII 代表**美国信息交换标准码，**它代表小写(a-z)、大写(A-Z)和数字(0-9)，这是大多数计算机使用的。ASCII 不包括特殊字符，如英国英镑符号或德国乌尔马特，称为 Unicode 字符。Unicode 代表阿拉伯语、希腊语等的字母。、数学符号、历史脚本以及比 ASCII 覆盖更广泛字符的表情符号。实质上，Unicode 是 ASCII 字符集的超集。

因此，如果数据集中存在这些特殊字符，就会妨碍文本数据的读取和分析。有两种方法可以解决这个问题:用对应的 Unicode 字符规范化这些字符，或者将它们完全删除。

```
import unicodedata# function to normalize non ascii
def normalize_non_ascii(text):
    text = unicodedata.normalize("NFKD", text)
    return textsample_text = "Australia©ª«Germany"clean_text = normalize_non_ascii(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** Australia©ª«Germany
# **After:** Australia©a«Germany
```

我们可以看到，`a`之前和之后的字符没有被规范化，因为它们不属于`unicode`集合。所以还是除掉他们比较好。

```
import re# function to remove non ascii
def remove_non_ascii(text):
    text = re.sub(r'[^\x00-\x7F]+', '', text)
    return textsample_text = "Australia©ª«Germany"clean_text = remove_non_ascii(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** Australia©ª«Germany
# **After:** AustraliaGermany
```

# 删除不可打印的字符

不可打印字符是指不占用任何空间并且在打印时不可见的字符。一些不可打印的字符是`\t, \r, \n, \x16, \xlf`。这些字符通常填充 web 抓取的数据集中，使信息变得杂乱，并以给分析师带来问题的方式使它们变得非结构化。所以移除它们通常是很重要的。

```
import sys, unicodedatadef remove_non_printable(text):
    # Get all unicode characters
    all_chars = (chr(i) for i in range(sys.maxunicode))
    # Get all non printable characters
    control_chars = ''.join(c for c in all_chars if unicodedata.category(c) == 'Cc')
    # Create regex of above characters
    control_char_re = re.compile('[%s]' % re.escape(control_chars))
    # remove non-printable characters
    text = control_char_re.sub('', text) return textsample_text = "\x00\x11Hello"clean_text = remove_non_printable(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** \x00\x11Hello
# **After:** Hello
```

# 删除重音符号

各种基于拉丁语的句子含有西班牙语、德语和许多其他语言的口音。所以我们也可以去掉那些使我们的数据集不一致的重音符号。

```
import unicodedatadef remove_accents(text):
    text = unicodedata.normalize('NFD', text).encode('ascii', 'ignore').decode("utf-8")
    return textsample_text = "JoÂÃÄÅhn i×Øψs a goωϖℵod b¡¢oy"clean_text = remove_accents(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** JoÂÃÄÅhn i×Øψs a goωϖℵod b¡¢oy
# **After:** JoAAAAhn is a good boy
```

# 删除多余的空格、制表符和换行符

很有可能使用 web 抓取数据中的转义序列来读取空格、制表符和换行符。因此，在读取 CSV 文件时，这些转义序列打乱了数据集，并错放了数据，造成了许多麻烦。所以去掉这些字符是必不可少的。除此之外，中间可以有多个空格，所以可以删除。方法非常简单，只需用空白替换这些字符序列就可以实现。

```
import redef remove_spaces(text):
    text = text.replace("\t", "").replace("\r", "").replace("\n", "")      # remove \t, \n, \r
    text = re.sub('\s{2,}', '', text)    # remove 2 or more than 2 spaces
    return textsample_text = "   \tUnnecessary \n\n\tspaces in the \rtext  "clean_text = remove_spaces(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:**    \tUnnecessary \n\n\tspaces in the \rtext
# **After:** Unnecessary spaces in the text
```

# 删除标点符号

对于数据分析，大多数时候不需要标点符号，它只会把分析搞得一团糟。所以通常要将它们移除。可以使用`string`和`re`库删除它。

```
import re
from string import punctuationdef remove_punctuation(text):
    text = re.sub('[%s]' % re.escape(punctuation), '', text)
    return textsample_text = "A' lot of, unnecessary punctuations!!!"clean_text = remove_punctuation(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** A' lot of, unnecessary punctuations!!!
# **After:** A lot of unnecessary punctuations
```

# 删除超链接

如果你有网络抓取的数据，你会经常处理超链接。因此，您可能需要在分析中去掉超链接，使用正则表达式可以轻松实现这一点。

```
import redef remove_hyperlinks(text):
    text = re.sub(r'https?://\S+', '', text)
    return textsample_text = "Some random URLs: [https://sample.com](https://google.com) [http://sample.edu/random](http://sample.edu/random)"clean_text = remove_hyperlinks(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** Some random URLs: [https://sample.com](https://google.com) [http://sample.edu/random](http://sample.edu/random)
# **After:** Some random URLs:
```

# 删除 HTML

与前面的情况类似，从 web 抓取的数据中，有时必须去掉 HTML 元素及其标签，并保留这些标签中的内容。对于这个任务，也可以使用正则表达式。

```
import redef remove_html(text):
    html_re = re.compile(r'<.*?>')      # create regex for html tag
    text = re.sub(html_re, '', text)    # remove html tags
    return textsample_text = """
<body>
<h1>This is a Heading</h1>
<p>This is a paragraph.</p>
</body>
"""clean_text = remove_html(sample_text)
print(clean_text)# ======== Expected Output =========
# **Before:** 
# <body>
# <h1>This is a Heading</h1>
# <p>This is a paragraph.</p>
# </body># **After:** # This is a Heading
# This is a paragraph.
```

我希望这些对你有用。我处理过大量文本数据，这些是我在分析数据集或创建仪表板之前执行的常见数据清理任务。您可以遵循这些步骤，或者根据您的数据集要求对它们进行修改。

请在评论区分享您的评论/观点；我很乐意回答所有问题。