# Analyzing Sentence Structure 分析句子结构


### Context Free Grammar 上下文无关的文法

```python
>>> grammar1 = nltk.CFG.fromstring("""
...   S -> NP VP
...   VP -> V NP | V NP PP
...   PP -> P NP
...   V -> "saw" | "ate" | "walked"
...   NP -> "John" | "Mary" | "Bob" | Det N | Det N PP
...   Det -> "a" | "an" | "the" | "my"
...   N -> "man" | "dog" | "cat" | "telescope" | "park"
...   P -> "in" | "on" | "by" | "with"
...   """)
>>> sent = "Mary saw Bob".split()
>>> rd_parser = nltk.RecursiveDescentParser(grammar1)
>>> for tree in rd_parser.parse(sent):
...     print(tree)
(S (NP Mary) (VP (V saw) (NP Bob)))
```

|Symbol	|Meaning	|Example
|----   |-------    |-------  
|S	|sentence	    |the man walked
|NP	|noun phrase	|a dog
|VP	|verb phrase	|saw a park
|PP	|prepositional phrase	|with a telescope
|Det |determiner	|the
|N	|noun	|dog
|V	|verb	|walked
|P	|preposition	|in



### Writing Your Own Grammars
```python
>>> grammar1 = nltk.data.load('file:mygrammar.cfg')
>>> sent = "Mary saw Bob".split()
>>> rd_parser = nltk.RecursiveDescentParser(grammar1)
>>> for tree in rd_parser.parse(sent):
...      print(tree)
```





### 参考
[nltk book ch08](http://www.nltk.org/book/ch08.html)
