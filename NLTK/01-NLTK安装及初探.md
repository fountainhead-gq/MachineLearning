# NLTK安装及初探

### Getting Started with NLTK

运行Python的IDE环境，进入NLTK数据源下载界面：
```python
>>> import nltk
>>> nltk.download()
```
或者：
`conda install nltk`

接着跳出对话框，设置下载目录后，根据提示选择选择Collections/all,即下载所有的文件。


## Operation 操作

下载完成以后再输入:
```python
>>> from nltk.book import *
```
可以看到加载的书籍：
```python
*** Introductory Examples for the NLTK Book ***
Loading text1, ..., text9 and sent1, ..., sent9
Type the name of the text or sentence to view it.
Type: 'texts()' or 'sents()' to list the materials.
text1: Moby Dick by Herman Melville 1851
text2: Sense and Sensibility by Jane Austen 1811
text3: The Book of Genesis
text4: Inaugural Address Corpus
text5: Chat Corpus
text6: Monty Python and the Holy Grail
text7: Wall Street Journal
text8: Personals Corpus
text9: The Man Who Was Thursday by G . K . Chesterton 1908
```

### Searching Text 搜索文本

```python
>>> text1.concordance("air")
```
会显示25个包含air的语句上下文

搜索相关词的近义词
```python
>>> text1.similar("air")
whale sea ship deck world boat other captain whales water pequod crew
fishery time forecastle head sun matter room wind
```

判断在文本单词的位置,或检测在文本出现特定的词，并显示在同样的背景下一些词:
```python
>>> text1.dispersion_plot(["air","freedom","wind","crew"])
```
![air](https://github.com/fountainhead-gq/MachineLearning/blob/master/NLTK/image/air.jpg)

检查词的共享环境
```python
>>> text1.common_contexts(["air","wind"])
the_but the_i the_he the_to the_the the_is the_that the_which the_and
```

### Counting Vocabulary 词统计

显示文本总字数（包括标点）
```python
>>> len(text1)
260819
```

显示文本总词数
```python
>>> len(set(text1))
19317
```

词排序,顺序依次是特殊符号，其次是按字母顺序
```python
>>> sorted(set(text1))
```

检测文本的词汇丰富性
```python
>>> len(set(text1)) / len(text1)
0.07406285585022564
```
说明不同词的数量是总词量的7.4%

词出现的总次数
```python
>>> text1.count("air")
141
```


### Frequency Distributions 频率分布

统计词频，50个最常用的词：

```python
>>> fdist1 = FreqDist(text1)
>>> print(fdist1)
<FreqDist with 19317 samples and 260819 outcomes>
>>> fdist1.most_common(50)
[(',', 18713), ('the', 13721), ('.', 6862), ('of', 6536), ('and', 6024),
('a', 4569), ('to', 4542), (';', 4072), ('in', 3916), ('that', 2982),
("'", 2684), ('-', 2552), ('his', 2459), ('it', 2209), ('I', 2124),
('s', 1739), ('is', 1695), ('he', 1661), ('with', 1659), ('was', 1632),
('as', 1620), ('"', 1478), ('all', 1462), ('for', 1414), ('this', 1280),
('!', 1269), ('at', 1231), ('by', 1137), ('but', 1113), ('not', 1103),
('--', 1070), ('him', 1058), ('from', 1052), ('be', 1030), ('on', 1005),
('so', 918), ('whale', 906), ('one', 889), ('you', 841), ('had', 767),
('have', 760), ('there', 715), ('But', 705), ('or', 697), ('were', 680),
('now', 646), ('which', 640), ('?', 637), ('me', 627), ('like', 624)]
>>> fdist1["air"]
141
```

统计词频，并输出累计图像
```python
>>> fdist1.plot(50, cumulative=True)
```
![fdist1](https://github.com/fountainhead-gq/MachineLearning/blob/master/NLTK/image/fdist1.jpg)

返回出现一次的词
```python
>>> len(fdist1.hapaxes())
9002
>>> fdist1.hapaxes()
```


### Fine-grained Selection of Words 细粒度选择词

数学集表达式
```
{w | w ∈ V & P(w)}

[w for w in V if p(w)]
```

查询单词长度超过15的单词：
```python
>>> V = set(text1)
>>> long_words = [w for w in V if len(w) > 15]
>>> sorted(long_words)
['CIRCUMNAVIGATION', 'Physiognomically', 'apprehensiveness', 'cannibalistically',
'characteristically', 'circumnavigating', 'circumnavigation', 'circumnavigations',
'comprehensiveness', 'hermaphroditical', 'indiscriminately', 'indispensableness',
'irresistibleness', 'physiognomically', 'preternaturalness', 'responsibilities',
'simultaneousness', 'subterraneousness', 'supernaturalness', 'superstitiousness',
'uncomfortableness', 'uncompromisedness', 'undiscriminating', 'uninterpenetratingly']
```

单词长度超过12,并且出现超过10次的词：
```python
>>> long_words = [w for w in V if len(w) > 12 and fdist1[w]>10]
>>> sorted(long_words)
['circumstances', 'indispensable', 'involuntarily', 'peculiarities', 'perpendicular', 'simultaneously', 'unaccountable']
```


### Collocations and Bigrams 搭配与双词

查询常用的词组(双联词)搭配：
```python
>>> text4.collocations()
United States; fellow citizens; four years; years ago; Federal
Government; General Government; American people; Vice President; Old
World; Almighty God; Fellow citizens; Chief Magistrate; Chief Justice;
God bless; every citizen; Indian tribes; public debt; one another;
foreign nations; political parties
```


### Counting Other Things 计数

统计不同长度单词出现的频率：
```python
>>> fdist = FreqDist(len(w) for w in text1)
>>> print(fdist)
<FreqDist with 19 samples and 260819 outcomes>
>>> fdist
FreqDist({3: 50223, 1: 47933, 4: 42345, 2: 38513, 5: 26597, 6: 17111, 7: 14399, 8: 9966, 9: 6428, 10: 3528, ...})
# 数字1到20，因为不存在长度超过20的单词
```

同样显示不用单词长度的出现频率
```python
>>> fdist.most_common()
[(3, 50223), (1, 47933), (4, 42345), (2, 38513), (5, 26597), (6, 17111), (7, 14399),
(8, 9966), (9, 6428), (10, 3528), (11, 1873), (12, 1053), (13, 567), (14, 177),
(15, 70), (16, 22), (17, 12), (18, 1), (20, 1)]
>>> fdist.max()
3
>>> fdist[3]
50223
>>> fdist.freq(3)
0.19255882431878046
```

NLTK的频率分布函数：

|示例      |描述      |
|------------------------  |--------------------------- |
|fdist = FreqDist(samples) |创建一个包含给定的样本频率分布|
|fdist['monstrous']        |统计给定词（如monstrous）的出现次数 |
|fdist.freq('monstrous')   |统计定样品（如monstrous）的频率 |
|fdist.N()                 |样本总数   等价于len(samples) |
|fdist.most_common(n)      |n个最常用的样本频率 |
|for sample in fdist:	   |遍历样本 |
|fdist.max()               |样本(长度)的最大计数 |
|fdist.tabulate()          |制表的频率分布  |
|fdist.plot()              |样本(长度)频率的曲线图分布   |
|fdist.plot(cumulative=True) |样本(长度)频率的累计曲线图分布 |


比较操作符：

|函数           |描述          |
|------------   |------------  |
|s.startswith(t) |测试s是否以t开头 |
|s.endswith(t)	|测试s是否以t结尾 |
|t in s	        |t是否是s的子串 |
|s.islower()	|s是否是小写 |
|s.isupper()	|s是否是大写 |
|s.isalpha()	|s非空，并且所有的字符都是字母 |
|s.isalnum()	|s非空，并且所有的字符都是字母数字 |
|s.isdigit()	|s非空，并且所有的字符都是数字 |
|s.istitle()	|s中的全部词是首字母大写 |

例子：
```python
>>> sorted(w for w in set(text1) if w.endswith('ableness'))
['comfortableness', 'honourableness', 'immutableness', 'indispensableness', ...]
>>> sorted(term for term in set(text4) if 'gnt' in term)
['Sovereignty', 'sovereignties', 'sovereignty']
>>> sorted(item for item in set(text6) if item.istitle())
['A', 'Aaaaaaaaah', 'Aaaaaaaah', 'Aaaaaah', 'Aaaah', 'Aaaaugh', 'Aaagh', ...]
>>> sorted(item for item in set(sent7) if item.isdigit())
['29', '61']
```

### 参考

[nltk about book](http://www.nltk.org/book/ch01.html)
