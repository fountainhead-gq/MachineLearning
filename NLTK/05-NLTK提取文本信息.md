# Extracting Information from Text 提取文本信息

结构化数据，如获取公司和地理位置之间的关系。
```python
>>> locs = [('Omnicom', 'IN', 'New York'),
... ('DDB Needham', 'IN', 'New York'),
... ('Kaplan Thaler Group', 'IN', 'New York'),
... ('BBDO South', 'IN', 'Atlanta'),
... ('Georgia-Pacific', 'IN', 'Atlanta')]
>>> query = [e1 for (e1, rel, e2) in locs if e2=='Atlanta']
>>> print(query)
['BBDO South', 'Georgia-Pacific']
```


## Chunking 分块

开始探索名词短语分块(Noun Phrase Chunking)

```python
>>> sentence = [("the", "DT"), ("little", "JJ"), ("yellow", "JJ"),
... ("dog", "NN"), ("barked", "VBD"), ("at", "IN"),  ("the", "DT"), ("cat", "NN")]
>>> grammar = "NP: {<DT>?<JJ>*<NN>}"
>>> cp = nltk.RegexpParser(grammar)
>>> result = cp.parse(sentence)
>>> print(result)
(S
  (NP the/DT little/JJ yellow/JJ dog/NN)
  barked/VBD
  at/IN
  (NP the/DT cat/NN))
>>> result.draw()
```
![np-chunking](https://github.com/fountainhead-gq/MachineLearning/blob/master/NLTK/image/np-chunking.jpg)



## Developing and Evaluating Chunkers 开发和评估分块

conll2000语料包含270K的华尔街日报文字，也有标注好的分块信息。
```python
>>> from nltk.corpus import conll2000
>>> print(conll2000.chunked_sents('train.txt')[200])
(S
  (NP The/DT notification/NN policy/NN)
  (VP was/VBD)
  (NP part/NN)
  (PP of/IN)
  (NP a/DT set/NN)
  (PP of/IN)
  (NP guidelines/NNS)
  (PP on/IN)
  (VP handling/NN)
  (NP coups/NNS)
  (VP outlined/VBN)
  (PP in/IN)
  (NP a/DT secret/JJ 1988/CD exchange/NN)
  (PP of/IN)
  (NP letters/NNS)
  (PP between/IN)
  (NP the/DT Reagan/NNP administration/NN)
  and/CC
  (NP the/DT Senate/NNP Intelligence/NNP Committee/NNP)
  ./.)
  ```

conll2000包含三种类型：`NP`,`VP`,`PP`,可以根据类型选择。
  ```python
  >>> print(conll2000.chunked_sents('train.txt',chunk_types=['NP'])[200])
(S
  (NP The/DT notification/NN policy/NN)
  was/VBD
  (NP part/NN)
  of/IN
  (NP a/DT set/NN)
  of/IN
  (NP guidelines/NNS)
  on/IN
  handling/NN
  (NP coups/NNS)
  outlined/VBN
  in/IN
  (NP a/DT secret/JJ 1988/CD exchange/NN)
  of/IN
  (NP letters/NNS)
  between/IN
  (NP the/DT Reagan/NNP administration/NN)
  and/CC
  (NP the/DT Senate/NNP Intelligence/NNP Committee/NNP)
  ./.)
  ```

简易的评估分块
```python
>>> test_sents = conll2000.chunked_sents('test.txt', chunk_types=['NP'])
>>> print(cp.evaluate(test_sents))
ChunkParse score:
    IOB Accuracy:  59.7%
    Precision:     45.3%
    Recall:        24.2%
    F-Measure:     31.6%
```    

### Trees

```python
>>> tree1 = nltk.Tree('NP', ['Alice'])
>>> print(tree1)
(NP Alice)
>>> tree2 = nltk.Tree('NP', ['the', 'rabbit'])
>>> print(tree2)
(NP the rabbit)
>>> tree3 = nltk.Tree('VP', ['chased', tree2])
>>> tree4 = nltk.Tree('S', [tree1, tree3])
>>> print(tree4)
(S (NP Alice) (VP chased (NP the rabbit)))
>>> print(tree4[1])
(VP chased (NP the rabbit))
>>> print(tree4[1].label())
VP
>>> tree4[1].leaves()
['chased', 'the', 'rabbit']
>>> tree4.leaves()
['Alice', 'chased', 'the', 'rabbit']
```

### 参考
[nltk book ](http://www.nltk.org/book/ch07.html)
