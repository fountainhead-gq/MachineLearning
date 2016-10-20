# NLTK词性分类和标注

### Stemmers 词干

英文词干提取器
```python
>>> raw = """DENNIS: Listen, strange women lying in ponds distributing swords
... ... is no basis for a system of government.  Supreme executive power derives from
... ... a mandate from the masses, not from some farcical aquatic ceremony."""
>>> tokens = word_tokenize(raw)
>>> porter = nltk.PorterStemmer()
>>> [porter.stem(t) for t in tokens]
['DENNI', ':', 'Listen', ',', 'strang', 'women', 'lie', 'in', 'pond', 'distribut', 'sword', '...', 'is', 'no', 'basi', 'for', 'a', 'system', 'of', 'govern', '.', 'Suprem', 'execut', 'power', 'deriv', 'from', '...', 'a', 'mandat', 'from', 'the', 'mass', ',', 'not', 'from', 'some', 'farcic', 'aquat', 'ceremoni', '.']
```

### Lemmatization 词形还原
去除掉词缀，速度比词干提取慢

```python
>>> wnl = nltk.WordNetLemmatizer()
>>> [wnl.lemmatize(t) for t in tokens]
['DENNIS', ':', 'Listen', ',', 'strange', 'woman', 'lying', 'in', 'pond',
'distributing', 'sword', 'is', 'no', 'basis', 'for', 'a', 'system', 'of',
'government', '.', 'Supreme', 'executive', 'power', 'derives', 'from', 'a',
'mandate', 'from', 'the', 'mass', ',', 'not', 'from', 'some', 'farcical',
'aquatic', 'ceremony', '.']
```


### Using a Tagger  标注器
```python
>>> from nltk import word_tokenize
>>> tokens = word_tokenize(raw)
>>> text = word_tokenize("And now for something completely different")
>>> nltk.pos_tag(text)
[('And', 'CC'), ('now', 'RB'), ('for', 'IN'), ('something', 'NN'),
('completely', 'RB'), ('different', 'JJ')]
```
`CC`是并列连词(coordinating conjunction),`RB`是副词(adverbs),`IN`是介词(preposition),`NN`是名词(noun),`JJ`是形容词(adjective)


### Tagged Corpora 标记语料库
tagged_words（）标记语料库中包含的文本
```python
>>> nltk.corpus.brown.tagged_words()
[('The', 'AT'), ('Fulton', 'NP-TL'), ...]
>>> nltk.corpus.brown.tagged_words(tagset='universal')
[('The', 'DET'), ('Fulton', 'NOUN'), ...]
```

|标签     |说明     |
|-------  |-------- |
|ADJ	|形容词 adjective	|
|ADP	|介词 adposition	|
|ADV	|副词 adverb	|
|CONJ	|连词 conjunction	|
|DET	|冠词 determiner, article	|
|NOUN	|名词 noun	|
|NUM	|数字 numeral	|
|PRT	|助词 particle	|
|PRON	|代词 pronoun	|
|VERB	|动词 verb	|
|.	    |标点 punctuation marks	|
|X	    |其他 other	|


### The Default Tagger 默认标注器
```python
>>> raw = 'I do not like green eggs and ham, I do not like them Sam I am!'
>>> tokens = word_tokenize(raw)
>>> default_tagger = nltk.DefaultTagger('NN')
>>> default_tagger.tag(tokens)
[('I', 'NN'), ('do', 'NN'), ('not', 'NN'), ('like', 'NN'), ('green', 'NN'),
('eggs', 'NN'), ('and', 'NN'), ('ham', 'NN'), (',', 'NN'), ('I', 'NN'),
('do', 'NN'), ('not', 'NN'), ('like', 'NN'), ('them', 'NN'), ('Sam', 'NN'),
('I', 'NN'), ('am', 'NN'), ('!', 'NN')]
```

### Tagged Tokens 标记令牌

```python
>>> tagged_token = nltk.tag.str2tuple('fly/NN')
>>> tagged_token
('fly', 'NN')
>>> tagged_token[0]
'fly'
>>> tagged_token[1]
'NN'
```
nltk.tag.str2tuple可以把fly/NN这种字符串转成一个二元组，事实上nltk的语料库中都是这种字符串形式的标注

### The Lookup Tagger 查询标注器
找出最频繁的词汇以及他们的词性，然后用这个信息去查找语料库，匹配的就标记上，剩余的标记为None。

`UnigramTagger`一元标注是基于已经标注的语料库做训练，然后用训练好的模型来标注新的语料
```python
>>> tagged_sents = [[('I', 'NN'), ('love', 'VERB')]]
>>> unigram_tagger = nltk.UnigramTagger(tagged_sents)
>>> sents = brown.sents(categories='news')
>>> tags = unigram_tagger.tag(sents[0])
>>> tags
[('i', None), ('love', 'VERB'), ('I', 'NN')]

```
`agged_sents`是用于训练的语料库，也可以直接用已有的标注好的语料库。

一元标注指的是只考虑当前这个词，不考虑上下文。二元标注器指的是考虑它前面的词的标注，用法只需要把上面的`UnigramTagger`换成`BigramTagger`,三元标注换成`TrigramTagger`


### Combining Taggers 组合标注器
组合标注是为了解决准确性和覆盖面之间的一种方法。
```python
>>> t0 = nltk.DefaultTagger('NN')
>>> t1 = nltk.UnigramTagger(train_sents, backoff=t0)
>>> t2 = nltk.BigramTagger(train_sents, backoff=t1)
>>> t2.evaluate(test_sents)
0.844513...
```

### Storing Taggers 存储标注器
训练需要大量的时间，为了简便和持久化，可以存储到文件硬盘等。
```python
>>> from pickle import dump
>>> output = open('t2.pkl', 'wb')
>>> dump(t2, output, -1)
>>> output.close()
```

同样，也可以加载文件
```python
>>> from pickle import load
>>> input = open('t2.pkl', 'rb')
>>> tagger = load(input)
>>> input.close()
```

检验能否用于标记
```python
>>> text = """The board's action shows what free enterprise
...     is up against in our complex maze of regulatory laws ."""
>>> tokens = text.split()
>>> tagger.tag(tokens)
[('The', 'AT'), ("board's", 'NN$'), ('action', 'NN'), ('shows', 'NNS'),
('what', 'WDT'), ('free', 'JJ'), ('enterprise', 'NN'), ('is', 'BEZ'),
('up', 'RP'), ('against', 'IN'), ('in', 'IN'), ('our', 'PP$'), ('complex', 'JJ'),
('maze', 'NN'), ('of', 'IN'), ('regulatory', 'NN'), ('laws', 'NNS'), ('.', '.')]
```


### 参考
[nltk book](http://www.nltk.org/book/ch05.html)
