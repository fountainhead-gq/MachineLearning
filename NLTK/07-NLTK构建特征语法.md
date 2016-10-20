# Building Feature Based Grammars 构建基于特征的语法


### Processing Feature Structures 特征结构的处理

```python
>>> fs1 = nltk.FeatStruct(TENSE='past', NUM='sg')
>>> print(fs1)
[ NUM   = 'sg'   ]
[ TENSE = 'past' ]
>>> fs1 = nltk.FeatStruct(PER=3, NUM='pl', GND='fem')
>>> print(fs1['GND'])
fem
>>> print(nltk.FeatStruct(NAME='Lee', TELNO='01 27 86 42 96', AGE=33))
[ AGE   = 33               ]
[ NAME  = 'Lee'            ]
[ TELNO = '01 27 86 42 96' ]
>>> fs1 = nltk.FeatStruct(NUMBER=74, STREET='rue Pascal')
>>> fs2 = nltk.FeatStruct(CITY='Paris')
>>> print(fs1.unify(fs2))
[ CITY   = 'Paris'      ]
[ NUMBER = 74           ]
[ STREET = 'rue Pascal' ]
```

在nltk的库里已经有了一些产生式文法描述可以直接使用,位置在`/nltk_data/grammars/book_grammars`,其中最简单的一个sql0.fcfg，这是一个查找国家城市的sql语句的文法
```sql
## Natural Language Toolkit: sql.fcfg
##
## Deliberately naive string-based grammar for
## deriving SQL queries from English
##
## Author: Ewan Klein <ewan@inf.ed.ac.uk>
## URL: <http://nltk.sourceforge.net>
## For license information, see LICENSE.TXT

% start S

S[SEM=(?np + WHERE + ?vp)] -> NP[SEM=?np] VP[SEM=?vp]

VP[SEM=(?v + ?pp)] -> IV[SEM=?v] PP[SEM=?pp]
VP[SEM=(?v + ?ap)] -> IV[SEM=?v] AP[SEM=?ap]
NP[SEM=(?det + ?n)] -> Det[SEM=?det] N[SEM=?n]
PP[SEM=(?p + ?np)] -> P[SEM=?p] NP[SEM=?np]
AP[SEM=?pp] -> A[SEM=?a] PP[SEM=?pp]

NP[SEM='Country="greece"'] -> 'Greece'
NP[SEM='Country="china"'] -> 'China'

Det[SEM='SELECT'] -> 'Which' | 'What'

N[SEM='City FROM city_table'] -> 'cities'

IV[SEM=''] -> 'are'
A[SEM=''] -> 'located'
P[SEM=''] -> 'in'
```
从上到下是从最大范围到最小范围一个个的解释，S是句子。

加载这个文件，执行：
```python
>>> from nltk import load_parser
>>> cp = load_parser('grammars/book_grammars/sql0.fcfg')
>>> query = 'What cities are located in China'
>>> tokens = query.split()
>>> for tree in cp.parse(tokens):
...     print(tree)
...
(S[SEM=(SELECT, City FROM city_table, WHERE, , , Country="china")]
  (NP[SEM=(SELECT, City FROM city_table)]
    (Det[SEM='SELECT'] What)
    (N[SEM='City FROM city_table'] cities))
  (VP[SEM=(, , Country="china")]
    (IV[SEM=''] are)
    (AP[SEM=(, Country="china")]
      (A[SEM=''] located)
      (PP[SEM=(, Country="china")]
        (P[SEM=''] in)
        (NP[SEM='Country="china"'] China)))))
```

同样，也可以解析成SQL
```python
>>> from nltk import load_parser
>>> cp = load_parser('grammars/book_grammars/sql0.fcfg')
>>> query = 'What cities are located in China'
>>> trees = list(cp.parse(query.split()))
>>> answer = trees[0].label()['SEM']
>>> answer = [s for s in answer if s]
>>> q = ' '.join(answer)
>>> print(q)
SELECT City FROM city_table WHERE Country="china"
```

接着执行，查看结果：
```python
>>> from nltk.sem import chat80
>>> rows = chat80.sql_query('corpora/city_database/city.db', q)
>>> for r in rows: print(r[0], end=" ")
canton chungking dairen harbin kowloon mukden peking shanghai sian tientsin
```

### 参考
[nltk book 09](http://www.nltk.org/book/ch09.html)        
