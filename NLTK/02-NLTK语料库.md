# NLTK文本语料库


### Gutenberg Corpus  语料库Gutenberg 

NLTK包含来自[Gutenberg](http://www.gutenberg.org/)的部分电子文本文档，Gutenberg大约有25,000免费的电子图书。

```python
>>> import nltk
>>> nltk.corpus.gutenberg.fileids()
['austen-emma.txt', 'austen-persuasion.txt', 'austen-sense.txt', 'bible-kjv.txt',
'blake-poems.txt', 'bryant-stories.txt', 'burgess-busterbrown.txt',
'carroll-alice.txt', 'chesterton-ball.txt', 'chesterton-brown.txt',
'chesterton-thursday.txt', 'edgeworth-parents.txt', 'melville-moby_dick.txt',
'milton-paradise.txt', 'shakespeare-caesar.txt', 'shakespeare-hamlet.txt',
'shakespeare-macbeth.txt', 'whitman-leaves.txt']
```

#### nltk.corpus.gutenberg

`nltk.corpus.gutenberg`就是gutenberg语料库的阅读器，它有很多实用的方法，比如：
- `nltk.corpus.gutenberg.raw('chesterton-brown.txt')`：输出chesterton-brown.txt文章的原始内容
- `nltk.corpus.gutenberg.words('chesterton-brown.txt')`：输出chesterton-brown.txt文章的单词列表
- `nltk.corpus.gutenberg.sents('chesterton-brown.txt')`：输出chesterton-brown.txt文章的句子列表


**类似的语料库还有**：
- `from nltk.corpus import webtext`：网络文本语料库，网络和聊天文本
- `from nltk.corpus import brown`：布朗语料库，按照文本分类好的500个不同来源的文本
- `from nltk.corpus import reuters`：路透社语料库，1万多个新闻文档
- `from nltk.corpus import inaugural`：就职演说语料库，55个总统的演说


### Web and Chat Text 网络聊天文本
NLTK的Webtext集包括论坛内容，加勒比海盗台词，个人广告，对话，和葡萄酒评论等。

```python
>>> from nltk.corpus import webtext,nps_chat
>>> webtext.fileids()
['firefox.txt', 'grail.txt', 'overheard.txt', 'pirates.txt', 'singles.txt', 'wine.txt']
>>> nps_chat.fileids()
['10-19-20s_706posts.xml', '10-19-30s_705posts.xml', '10-19-40s_686posts.xml', '10-19-adults_706posts.xml', '10-24-40s_706posts.xml', '10-26-teens_706posts.xml', '11-06-adults_706posts.xml', '11-08-20s_705posts.xml', '11-08-40s_706posts.xml', '11-08-adults_705posts.xml', '11-08-teens_706posts.xml', '11-09-20s_706posts.xml', '11-09-40s_706posts.xml', '11-09-adults_706posts.xml', '11-09-teens_706posts.xml']
>>> nps_chat.posts('10-19-20s_706posts.xml')
[['now', 'im', 'left', 'with', 'this', 'gay', 'name'], [':P'], ...]
```

### Brown Corpus 布朗语料
布朗语料库是英国的第一个一百万字的语料电子，布朗大学在1961年创建的。包含新闻、社论等资源。
```python
>>> from nltk.corpus import brown
>>> brown.categories()
['adventure', 'belles_lettres', 'editorial', 'fiction', 'government', 'hobbies',
'humor', 'learned', 'lore', 'mystery', 'news', 'religion', 'reviews', 'romance',
'science_fiction']
```

### Reuters Corpus 路透社语料库
路透社语料库包含10788新闻文档共计130万字。分类成90个主题，并分成两组，称为“训练”和“测试”。
```python
>>> from nltk.corpus import reuters
>>> reuters.fileids()
['test/14826', 'test/14828', 'test/14829', 'test/14832', ...]
>>> reuters.categories('training/9865')
['barley', 'corn', 'grain', 'wheat']
```

### Inaugural Address Corpus 就职演说语料库
包含55个总统的就职演说
```python
>>> inaugural.fileids()
['1789-Washington.txt', '1793-Washington.txt', '1797-Adams.txt', '1801-Jefferson.txt', '1805-Jefferson.txt', '1809-Madison.txt', '1813-Madison.txt', '1817-Monroe.txt', '1821-Monroe.txt', '1825-Adams.txt', '1829-Jackson.txt', '1833-Jackson.txt', '1837-VanBuren.txt', '1841-Harrison.txt', '1845-Polk.txt', '1849-Taylor.txt', '1853-Pierce.txt', '1857-Buchanan.txt','1861-Lincoln.txt', '1865-Lincoln.txt','1869-Grant.txt', '1873-Grant.txt', '1877-Hayes.txt', '1881-Garfield.txt', '1885-Cleveland.txt', '1889-Harrison.txt', '1893-Cleveland.txt','1897-McKinley.txt', '1901-McKinley.txt', '1905-Roosevelt.txt', '1909-Taft.txt', '1913-Wilson.txt', '1917-Wilson.txt', '1921-Harding.txt', '1925-Coolidge.txt', '1929-Hoover.txt', '1933-Roosevelt.txt', '1937-Roosevelt.txt', '1941-Roosevelt.txt', '1945-Roosevelt.txt', '1949-Truman.txt', '1953-Eisenhower.txt', '1957-Eisenhower.txt', '1961-Kennedy.txt', '1965-Johnson.txt', '1969-Nixon.txt', '1973-Nixon.txt', '1977-Carter.txt', '1981-Reagan.txt', '1985-Reagan.txt', '1989-Bush.txt', '1993-Clinton.txt', '1997-Clinton.txt', '2001-Bush.txt', '2005-Bush.txt', '2009-Obama.txt']
```

### Text Corpus Structure 语料库结构
以上各种语料库都是分别建立的，因此会稍有一些区别，但是不外乎以下几种组织结构：孤立式  isolated（孤立的多篇文章）、分类式 categorized（按照类别组织，相互之间没有交集）、交叉式 overlapping（一篇文章可能属于多个类）、渐变式 temporal（语法随着时间发生变化）
[text-corpus-structure](https://github.com/fountainhead-gq/MachineLearning/blob/master/NLTK/image/text-corpus-structure.png)



### 语料库的通用接口

- `fileids()`：返回语料库中的文件
- `categories()`：返回语料库中的分类
- `raw()`：返回语料库的原始内容
- `words()`：返回语料库中的词汇
- `sents()`：返回语料库句子
- `abspath()`：指定文件在磁盘上的位置
- `open()`：打开语料库的文件流

|示例                    |说明                  |
|------------------      |-------------------   |
|fileids()	             |语料库文件   |
|fileids([categories])	 |语料库的相应的文件  |
|categories()	         |语料库类别 |
|categories([fileids])	 |语料库对应文件的类别 |
|raw()	                 |语料库的原始内容 |
|raw(fileids=[f1,f2,f3]) |指定文件的原始内容 |
|raw(categories=[c1,c2]) |指定类别的原始内容 |
|words()	             |整个语料库的词 |
|words(fileids=[f1,f2,f3]) |指定文件的词 |
|words(categories=[c1,c2]) |指定类别的词 |
|sents()	             |整个语料的句子 |
|sents(fileids=[f1,f2,f3])	|指定文件的句子 |
|sents(categories=[c1,c2])	|指定类型的句子 |
|abspath(fileid)	     |在磁盘上给定的文件的位置 |
|encoding(fileid)	     |文件的编码 |
|open(fileid)	         |打开一个流用于读取给定的语料库文件 |
|root	                 |语料库安装根路径 |
|readme()	             |语料库的README文件的内容 |


### Loading your own Corpus 加载自己的语料库
收集自己的语料文件到指定路径下，然后执行
```python
>>> from nltk.corpus import PlaintextCorpusReader
>>> corpus_root = r"C:\Users\144405\Downloads\corpus"
>>> wordlists = PlaintextCorpusReader(corpus_root, '.*')
>>> wordlists.fileids()
['Friends - 1x01 - It All Began.txt']
>>> wordlists.words()
['The', 'One', 'Where', 'Monica', 'Gets', 'a', 'New', ...]
>>> wordlists.sents()
[['The', 'One', 'Where', 'Monica', 'Gets', 'a', 'New', 'Roommate', '(', 'The','Pilot', '-', 'The', 'Uncut', 'Version', ')'],  ...]
>>> len(wordlists.sents())
581
```


### Conditional Frequency Distributions 条件频率分布
`FreqDist()`将计算列表中的每个项的出现次数。

```python
# coding:utf-8

import nltk
from nltk.corpus import brown

# 链表推导式，genre是brown语料库里的所有类别列表，word是这个类别中的词汇列表
# (genre, word)就是类别加词汇对
genre_word = [(genre, word)
        for genre in brown.categories()
        for word in brown.words(categories=genre)
        ]

# 创建条件频率分布
cfd = nltk.ConditionalFreqDist(genre_word)

# 曲线图 或表格cfd.tabulate
cfd.plot(conditions=['news', 'romance'], samples=[u'stock', u'sunbonnet', u'Elevated', u'narcotic', u'four', u'woods', u'railing', u'Until', u'aggression', u'marching', u'looking', u'eligible', u'electricity',
u'consulate', u'Casey', u'all-county', u'Belgians', u'Western',  u'Duhagon', u'sinking',  u'co-operation', u'Famed', u'regional', u'Charitable', u'appropriation',u'yellow', u'uncertain', u'Heights', u'bringing', u'prize',
u'Loen', u'Publique', u'wooden', u'Loeb', u'specialties', u'Sands', u'succession', u'Paul', u'Phyfe'])
```


### Generating Random Text with Bigrams 生成双联词文本
bigrams()函数生成连续的字对列表
```python
>>> sent = ['In', 'the', 'beginning', 'God', 'created', 'the', 'heaven', 'and', 'the', 'earth', '.']
>>> list(nltk.bigrams(sent))
[('In', 'the'), ('the', 'beginning'), ('beginning', 'God'), ('God', 'created'),
('created', 'the'), ('the', 'heaven'), ('heaven', 'and'), ('and', 'the'),
('the', 'earth'), ('earth', '.')]
 ```


 ```python
# 循环15次，从cfdist中取当前单词最大概率的连词,并打印出来  
>>> def generate_model(cfdist, word, num=15):
...     for i in range(num):
...         print(word, end=' ')
...         word = cfdist[word].max()
...
>>> text = nltk.corpus.genesis.words('english-kjv.txt') # 加载语料库
>>> bigrams = nltk.bigrams(text)  # 生成双连词
>>> cfd = nltk.ConditionalFreqDist(bigrams) # 生成条件频率分布
>>> cfd['living']
FreqDist({'creature': 7, 'thing': 4, 'substance': 2, 'soul': 1, ',': 1, '.': 1}) # living后面最可能出现的是creature
>>> generate_model(cfd, 'living')
living creature that he said , and the land of the land of the land
# living最大概率的双连词是creature,依次类推。the land of的最大概率双联词依次循环，共15个
```

|示例   |说明  |
|----------------------------------- |-------------------------- |
|cfdist = ConditionalFreqDist(pairs) |创建对应列表的条件频率分布 |
|cfdist.conditions()	|条件 |
|cfdist[condition]	|对应条件下的频率分布 |
|cfdist[condition][sample]	|频率条件的给定样品 |
|cfdist.tabulate()	|条件频率分布的图表 |
|cfdist.tabulate(samples, conditions)	|限定于指定的采样和条件的条件频率分布图标 |
|cfdist.plot()	|条件频率分布的图表曲线图 |
|cfdist.plot(samples, conditions)	|限定于指定的采样和条件的条件频率分布曲线图 |


### Wordlist Corpora 单词表语料库
我们可以用它来寻找语料库中异常或错误拼写的词。
```python
# 查询不常见或出错的单词
import nltk

def unusual_words(text):
    text_vocab = set(w.lower() for w in text if w.isalpha())
    english_vocab = set(w.lower() for w in nltk.corpus.words.words())
    unusual = text_vocab - english_vocab
    return sorted(unusual)

>>> unusual_words(nltk.corpus.nps_chat.words())
['aaaaaaaaaaaaaaaaa', 'aaahhhh', 'abortions', 'abou', 'abourted', 'abs', 'ack',
'acros', 'actualy', 'adams', 'adds', 'adduser', 'adjusts', 'adoted', 'adreniline',
'ads', 'adults', 'afe', 'affairs', 'affari', 'affects', 'afk', 'agaibn', 'ages', ...]
```

#### corpus of stopwords 停用词语库
过滤掉一些高频词，如 the、to、 at 等
```python
>>> from nltk.corpus import stopwords
>>> stopwords.words('english')
['i', 'me', 'my', 'myself', 'we', 'our', 'ours', 'ourselves', 'you', 'your', 'yours',
'yourself', 'yourselves', 'he', 'him', 'his', 'himself', 'she', 'her', 'hers',
'herself', 'it', 'its', 'itself', 'they', 'them', 'their', 'theirs', 'themselves',
'what', 'which', 'who', 'whom', 'this', 'that', 'these', 'those', 'am', 'is', 'are',
'was', 'were', 'be', 'been', 'being', 'have', 'has', 'had', 'having', 'do', 'does',
'did', 'doing', 'a', 'an', 'the', 'and', 'but', 'if', 'or', 'because', 'as', 'until',
'while', 'of', 'at', 'by', 'for', 'with', 'about', 'against', 'between', 'into',
'through', 'during', 'before', 'after', 'above', 'below', 'to', 'from', 'up', 'down',
'in', 'out', 'on', 'off', 'over', 'under', 'again', 'further', 'then', 'once', 'here',
'there', 'when', 'where', 'why', 'how', 'all', 'any', 'both', 'each', 'few', 'more',
'most', 'other', 'some', 'such', 'no', 'nor', 'not', 'only', 'own', 'same', 'so',
'than', 'too', 'very', 's', 't', 'can', 'will', 'just', 'don', 'should', 'now']
```

*A Word Puzzle:查找出有多少个单词是由egivrvonl中的字母构成，长度大于5并且必包含字母r*
```python
>>> puzzle_letters = nltk.FreqDist('egivrvonl')
>>> obligatory = 'r'
>>> wordlist = nltk.corpus.words.words()
>>> [w for w in wordlist if len(w) >= 6
...                      and obligatory in w
...                      and nltk.FreqDist(w) <= puzzle_letters]
['glover', 'gorlin', 'govern', 'grovel', 'ignore', 'involver', 'lienor',
'linger', 'longer', 'lovering', 'noiler', 'overling', 'region', 'renvoi',
'revolving', 'ringle', 'roving', 'violer', 'virole']
```

### Comparative Wordlists 比较词表
查看区分多语言词列表
```python
>>> fr2en = swadesh.entries(['fr', 'en']) # french-english
>>> fr2en
[('je', 'I'), ('tu, vous', 'you (singular), thou'), ('il', 'he'), ...]
>>> translate['jouer'] # translate to english
'play'

>>> de2en = swadesh.entries(['de', 'en'])    # German-English
>>> es2en = swadesh.entries(['es', 'en'])    # Spanish-English
>>> translate.update(dict(de2en))
>>> translate.update(dict(es2en))
>>> translate['Hund']
'dog'
>>> translate['perro']
'dog'
```

### WordNet 同义词集

```python
>>> from nltk.corpus import wordnet as wn
>>> wn.synsets('automobile')
[Synset('car.n.01'), Synset('automobile.v.01')]
>>> wn.synsets('motorcar')
[Synset('car.n.01')]
>>> wn.synset('car.n.01').lemma_names()
['car', 'auto', 'automobile', 'machine', 'motorcar']
>>> wn.synset('car.n.01').definition()
'a motor vehicle with four wheels; usually propelled by an internal combustion engine'
>>> wn.synset('car.n.01').examples()
['he needs a car to get to work']
```

### 参考
[nltk book](http://www.nltk.org/book/ch02.html#raw-access)
