# NLTK安装及使用操作

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
>>> text1.dispersion_plot(["air"])
```
![air]()
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
![fdist1]()

返回出现一次的词
```python
>>> len(fdist1.hapaxes())
9002
>>> fdist1.hapaxes()
```
