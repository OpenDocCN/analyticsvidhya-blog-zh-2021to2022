# TextBlob 入门。

> 原文：<https://medium.com/analytics-vidhya/getting-started-with-textblob-156823634aab?source=collection_archive---------9----------------------->

TextBlob 是一个 python 库，用于处理基于文本的信息。它提供了一个基本的 API 来投入到正常的特征语言准备(NLP)任务中。例如语法特征标记、名词短语提取、情感分析、分类、翻译等等。

# **text blob 的特点:**

*   名词短语抽取
*   词性标注
*   情感分析
*   分类(朴素贝叶斯、决策树)
*   标记化(将文本拆分成单词和句子)
*   单词和短语频率
*   从语法上分析
*   `**n**`-克
*   单词变形(复数和单数)和词条变形
*   拼写纠正
*   通过扩展添加新的模型或语言
*   WordNet 集成

# **安装**

$ pip install-U text blob
$ python-m text blob . download _ corpora

它将安装 TextBlob 并下载 NLTK 语料库。

# 使用 TextBlob

**创建文本块:**

第一次导入:从文本块**导入**文本块

从 textblob 导入 TextBlob
b = TextBlob("我拼写不错！")

**拼写检查:**检查任何拼写错误。b = TextBlob("我有很好的演讲！"正确的

*O/P: TextBlob("我拼写很好！")*

**字数统计:**统计给定句子中单词或名词短语的出现频率。

b3 = TextBlob("我们不再是说 Ni ekki 的骑士了。"
“我们现在是说 Ekki ekki ekki PTANG 的骑士。”b3.word_counts['ekki']

*O/P: 4*

**标记:**可以使用 tags 属性标记词性。它返回 n 个元组列表，格式为(word，POS tag)。

b4 = TextBlob("Python 是一种高级通用编程语言。")
b4 .标签

*O/P: [('Python '，' NNP ')，
('is '，' VBZ ')，
('a '，' DT ')，
('高级'，' JJ ')，
('and '，' CC ')，
('通用'，' JJ ')，
('编程'，' NN '，
('语言'，' NN ')*

*标签列表:*
DT-限定词
EX-existive there(比如:“有”…就当“有存在”)
FW-外来词
IN-介词/从属连词
JJ-形容词'大'
JJR-形容词，比格'
JJS-形容词，最高级'最大'
LS-列表标记 1)
MD-情态 could，will
NN-名词，单数'书桌'【T12 复数' Americans '
PDT-预先决定' all the kids '
POS-所有格结尾 parent's
PRP-人称代词我，他，她
PRP $-所有格代词我的，他的，她的
RB-副词 very，silently，
RP-particle give
TO-go ' TO ' the store。
UH-感叹词
VB-动词，基础形式取
VBD-动词，过去式，取
CC-并列连词
CD-基数
VBG-动词，动名词/现在分词取
VBN-动词，过去分词取
VBP-动词，唱。现在时，非 3d take
VBZ——动词，第三人称唱法。present 带
WDT-wh-限定词 which
WP-wh-代词 who，wh
WP $-所有格 wh-代词 who
WRB-wh-副词 where，when

**记号化:**可以把 TextBlobs 分解成单词和句子。

b5 = TextBlob(' ' '美和丑比较好。
显性比隐性好。简单比复杂好')
打印("句子:"，b5 .句子)
#单词表
打印("单词:"，b5 .单词)

*O/P:句子:【句子(“美和丑比较好。”)，句子(“明的比暗的好。”)，句子(“简单的比复杂的好。”)]
词语:['美'，'和'，'丑'，'是'，'更好'，'外显'，'是'，'比'，'含蓄'，'简单'，'是'，'更好'，'比'，'复杂']*

**定义:**它为我们提供了给定单词的定义。

从 textblob 导入 Word
a = Word(" Explicit ")
print(a . definitions)

*O/P:【‘精确、清晰的表达或者容易观察到的；没有留下任何暗示'，'与事实或术语的主要意思一致']*

情感分析:情感分析让我们分析给定文本背后的情感。情感分析将在元组中返回 2 个值:

极性:取-1 和+1 之间的值。-1 表示非常消极的语言，+1 表示非常积极的语言。

主观性:取 0 到+1 之间的值。0 表示尽可能客观，+1 表示非常主观的文本。

感知= b5 .感悟
打印(感知)

sub = b5 .感情.主观
打印(sub)

pol = b5 .情操.极性
打印(pol)

*O/P:情绪(极性=0.19285714285714284，主观性= 0.6081632653061224)
0.60816326 530616*

词形变化:一个单词形式的变化或附加，表明它在句子中使用方式的变化。
如果我们在“dog”后面加上复数词形变化“-s ”,就得到“dogs”

Elon Musk 是特斯拉和许多其他公司的首席执行官。)
打印("单词:"，句子.单词)
打印("单数:"，句子.单词【4】。singularize())
打印("复数:"，句子.单词[-1]。复数())

*O/P:Words: ['Elon '，' Musk '，' is '，' the '，' CEO '，' of '，' Tesla '，' and '，' many '，' other '，' company']
单数:CEO
复数:公司*

**引理化:**将一个词的不同形式简化为一个单一形式，
例如，将“builds”、“building”或“build”简化为引理“build”
Playing、Played、Plays 有共同的词根形式 Play。

从 textblob 导入 Word
w1 = Word(" octopi ")
print(w1 . lemma tize())

w2 = Word(" went ")
print(w2 . lemma tize(" v "))

w3 = Word(" worse ")
print(w3 . lemma tize(" a "))

*O/P:章鱼
走
坏*

WordNet 是一个专门为自然语言处理而设计的英语词典。

Synset 是 NLTK 中一种特殊的简单接口，用于在 WordNet 中查找单词。同义词集实例是表达相同概念的同义词的组合。有些单词只有一个同义词集，有些有几个。

从 nltk.corpus 导入 wordnet
sys = wordnet . synsets(' hello ')[0]

print(" Synset name:"，sys . name())
# Defining
print(" \ nSynset meaning:"，sys.definition())
#在上下文中使用该词的短语列表
print ("\nSynset example:"，sys.examples())

*O/P: Synset 名称:hello.n.01*

*同义词含义:问候的表达方式*

*同义词示例:[‘每天早上他们都礼貌地互打招呼’]*

sys _ 1 = wordnet . synsets(' hello ')[0]
print(" Syn tag:"，sys _ 1 . pos())

sys _ 2 = wordnet . synsets(' doing ')【0】
print(" Syn tag:"，sys _ 2 . pos())

sys _ 3 = wordnet . synsets(' modification ')【0】
print(" Syn tag:"，sys_3.pos())

sys_4 = .

*O/P:Syn tag:n
Syn tag:v
Syn tag:n
Syn tag:r* **解析:**根据语法成分分析(一个句子)，识别词性、句法关系。

b6= TextBlob("将一个单词的不同形式简化为一种形式。")
print(b6.parse())

*O/P:Reduce/VB/B-VP/O the/DT/B-NP/O different/JJ/I-NP/O forms/NNS/I-NP/O of/IN/B-PP/B-PNP a/DT/B-NP/I-PNP word/NN/I-NP/I-PNP TO/TO/B-PP/B-PNP one/CD/B-NP/I-PNP single/JJ/I-NP/I-PNP/./O/O*

n 元语法: n 元语法是来自给定文本或语音样本的 n 个项目的连续序列。该方法返回 n 个连续单词的元组列表。

b7 = TextBlob("有总比没有好。")
b7.ngrams(n=3)

*O/P: [WordList(['Somethings '，' is '，' better'])，
WordList(['is '，' better '，' than'])，
WordList(['better '，' than '，' nothing'])]*

GitHub:[https://github.com/rahulsharma-rks/TextBlob](https://github.com/rahulsharma-rks/TextBlob)