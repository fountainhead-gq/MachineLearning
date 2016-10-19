# Learning to Classify Text

机器学习的过程是训练模型和使用模型的过程，训练就是基于已知数据做统计学习，使用就是用统计学习好的模型来计算未知的数据。

机器学习按照方式不同主要分为三大类，`有监督学习`(Supervised learning)、`无监督学习`(Unsupervised learning)以及`半监督学习`(Semi-supervised learning)。

- 监督学习：通过已有的一部分输入数据与输出数据之间的对应关系，生成一个函数，将输入映射到合适的输出。
- 非监督学习：直接对输入数据集进行建模，寻找关联。只需要寻找关联性，并不需要什么明确的目标值输出。
- 半监督学习：综合利用有输入输出的数据，和只有输入的数据来进行训练。可以简单理解成监督学习和非监督学习的综合。

文本分类也分为有监督的分类和无监督的分类。我们将研究一些重要的机器学习技术，包括决策树，朴素贝叶斯分类，最大熵分类。


## Supervised Classification 监督学习
监督是基于包含每个输入正确的标签的训练语料库构建的分类器。


贝叶斯是概率论的鼻祖，贝叶斯定理是关于随机事件的条件概率的一则定理，贝叶斯公式是：   
p(C|D)=p(C)p(D|C)/p(D)

>[wikipedia 朴素贝叶斯分类器](https://zh.wikipedia.org/wiki/%E6%9C%B4%E7%B4%A0%E8%B4%9D%E5%8F%B6%E6%96%AF%E5%88%86%E7%B1%BB%E5%99%A8)朴素贝叶斯自20世纪50年代已广泛研究。在20世纪60年代初就以另外一个名称引入到文本信息检索界中，[1]:488 并仍然是文本分类的一种热门（基准）方法，文本分类是以词频为特征判断文件所属类别或其他（如垃圾邮件、合法性、体育或政治等等）的问题。通过适当的预处理，它可以与这个领域更先进的方法（包括支持向量机）相竞争。[2] 它在自动医疗诊断中也有应用。[3]
朴素贝叶斯分类器是高度可扩展的，因此需要数量与学习问题中的变量（特征/预测器）成线性关系的参数。最大似然训练可以通过评估一个封闭形式的表达式来完成，[1]:718 只需花费线性时间，而不需要其他很多类型的分类器所使用的费时的迭代逼近。
在统计学和计算机科学文献中，朴素贝叶斯模型有各种名称，包括简单贝叶斯和独立贝叶斯。[4] 所有这些名称都参考了贝叶斯定理在该分类器的决策规则中的使用，但朴素贝叶斯不（一定）用到贝叶斯方法；[4] Russell和Norvig提到“[朴素贝叶斯]有时被称为贝叶斯分类器，这个马虎的使用促使真正的贝叶斯论者称之为傻瓜贝叶斯模型。”

理论上，概率模型分类器是一个条件概率模型。贝叶斯分类器就是基于贝叶斯概率理论设计的分类器算法，nltk库中已有`nltk.NaiveBayesClassifier`：

我们通过给定名字的最后一个字母提取功能来判断性别、根据最后的字母来统计性别概率。
```python
>>> import nltk
>>> def gender_features(word):
...     return {'last_letter': word[-1]}
...
>>> gender_features('Shrek')
{'last_letter': 'k'}
>>> from nltk.corpus import names
>>> labeled_names = ([(name, 'male') for name in names.words('male.txt')] +
... [(name, 'female') for name in names.words('female.txt')])
>>> import random
>>> random.shuffle(labeled_names)
>>> featuresets = [(gender_features(n), gender) for (n, gender) in labeled_names]
>>> train_set, test_set = featuresets[500:], featuresets[:500]
>>> classifier = nltk.NaiveBayesClassifier.train(train_set)
>>> classifier.classify(gender_features('Neo'))
'male'
>>> classifier.classify(gender_features('lily'))
'female'
>>> print(nltk.classify.accuracy(classifier, test_set)) # 评估精确度
0.774
>>> classifier.show_most_informative_features(5) # 找最优信息量的特征
Most Informative Features
             last_letter = 'a'            female : male   =     34.4 : 1.0
             last_letter = 'k'              male : female =     32.6 : 1.0
             last_letter = 'f'              male : female =     16.6 : 1.0
             last_letter = 'p'              male : female =     10.5 : 1.0
             last_letter = 'd'              male : female =      9.7 : 1.0
>>> from nltk.classify import apply_features
>>> train_set = apply_features(gender_features, labeled_names[500:])
>>> train_set
[({'last_letter': 'y'}, 'female'), ({'last_letter': 'a'}, 'female'), ...]
```

## Document Classification 文档分类
我们可以定义一个特征提取，简单地检查给定的词是否存在于词库的常用列表中。

```python
import nltk
from nltk.corpus import movie_reviews

documents = [(list(movie_reviews.words(fileid)), category)
             for category in movie_reviews.categories()
             for fileid in movie_reviews.fileids(category)]

all_words = nltk.FreqDist(w.lower() for w in movie_reviews.words())
word_features = list(all_words)[:2000]

def document_features(document):
    document_words = set(document)
    features = {}
    for word in word_features:
        features['contains({})'.format(word)] = (word in document_words)
    return features

print(document_features(movie_reviews.words(r'F:\VENV\test\secret.txt')))
{'contains(topnotch)': False, 'contains(steamy)': False, 'contains(fantastic)': False, 'contains(eases)': False,...}
```


### 参考
[nltk book 07](http://www.nltk.org/book/ch07.html)
