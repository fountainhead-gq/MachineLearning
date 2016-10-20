# LTP语言云

首先注册一个帐号[ltp-cloud](http://www.ltp-cloud.com/),得到api_key


### 简介

**分词**  
中文分词 (Word Segmentation, WS) 指的是将汉字序列切分成词序列。 因为在汉语中，词是承载语义的最基本的单元。分词是信息检索、文本分类、情感分析等多项中文自然语言处理任务的基础。

**词性标注**  
词性标注(Part-of-speech Tagging, POS)是给句子中每个词一个词性类别的任务。 这里的词性类别可能是名词、动词、形容词或其他。 下面的句子是一个词性标注的例子。 其中，v代表动词、n代表名词、c代表连词、d代表副词、wp代表标点符号。

**词性标记集**   

|Tag	|Description	|Example	|Tag	|Description	|Example
|----   |----         |-------   |------  |----    |----
|a	|adjective	|美丽	|ni	|organization name	|保险公司
|b	|other noun-modifier	|大型, 西式	|nl	|location noun	|城郊
|c	|conjunction	|和, 虽然	|ns	|geographical name	|北京
|d	|adverb	|很	|nt	|temporal noun	|近日, 明代
|e	|exclamation	|哎	|nz	|other proper noun	|诺贝尔奖
|g	|morpheme	|茨, 甥	|o	|onomatopoeia	|哗啦
|h	|prefix	|阿, 伪	|p	|preposition	|在, 把
|i	|idiom	|百花齐放	|q	|quantity	|个
|j	|abbreviation	|公检法	|r	|pronoun	|我们
|k	|suffix	|界, 率	|u	|auxiliary	|的, 地
|m	|number	|一, 第一	|v	|verb	|跑, 学习
|n	|general noun	|苹果	|wp	|punctuation	|，。！
|nd	|direction noun	|右侧	|ws	|foreign words	|CPU
|nh	|person name	|杜甫, 汤姆	|x	|non-lexeme	|萄, 翱

**命名实体识别**  
命名实体识别 (Named Entity Recognition, NER) 是在句子的词序列中定位并识别人名、地名、机构名等实体的任务。 如之前的例子，命名实体识别的结果是：  
`国务院 (机构名) 总理李克强 (人名) 调研上海外高桥 (地名) 时提出，支持上海 (地名) 积极探索新机制。`  
命名实体识别对于挖掘文本中的实体进而对其进行分析有很重要的作用。

**依存语法**  
依存语法 (Dependency Parsing, DP) 通过分析语言单位内成分之间的依存关系揭示其句法结构。 直观来讲，依存句法分析识别句子中的“主谓宾”、“定状补”这些语法成分，并分析各成分之间的关系。

依存句法分析标注关系 (共15种) 及含义如下：

|关系类型	|Tag	|Description	|Example
|-------   |-----  |----------  |------
|主谓关系	|SBV	|subject-verb	|我送她一束花 (我 <-- 送)
|动宾关系	|VOB	|直接宾语，verb-object	|我送她一束花 (送 --> 花)
|间宾关系	|IOB	|间接宾语，indirect-object	|我送她一束花 (送 --> 她)
|前置宾语	|FOB	|前置宾语，fronting-object	|他什么书都读 (书 <-- 读)
|兼语	|DBL	|double	|他请我吃饭 (请 --> 我)
|定中关系	|ATT	|attribute	|红苹果 (红 <-- 苹果)
|状中结构	|ADV	|adverbial	|非常美丽 (非常 <-- 美丽)
|动补结构	|CMP	|complement	|做完了作业 (做 --> 完)
|并列关系	|COO	|coordinate	|大山和大海 (大山 --> 大海)
|介宾关系	|POB	|preposition-object	|在贸易区内 (在 --> 内)
|左附加关系	|LAD	|left adjunct	|大山和大海 (和 <-- 大海)
|右附加关系	|RAD	|right adjunct	|孩子们 (孩子 --> 们)
|独立结构	|IS	|independent structure	|两个单句在结构上彼此独立
|标点	|WP	|punctuation	|。
|核心关系|	|HED	head	|指整个句子的核心

**语义角色标注**  
语义角色标注 (Semantic Role Labeling, SRL) 是一种浅层的语义分析技术，标注句子中某些短语为给定谓词的论元 (语义角色) ，如施事、受事、时间和地点等。其能够对问答系统、信息抽取和机器翻译等应用产生推动作用。  
核心的语义角色为 A0-A5 六种，A0 通常表示动作的施事，A1通常表示动作的影响等，A2-A5 根据谓语动词不同会有不同的语义含义。其余的15个语义角色为附加语义角色，如LOC 表示地点，TMP 表示时间等。

|标记	|说明 |
|---  |--- |---
|ADV	|adverbial, default tag ( 附加的，默认标记 )
|BNE	|beneﬁciary ( 受益人 )
|CND	|condition ( 条件 )
|DIR	|direction ( 方向 )
|DGR	|degree ( 程度 )
|EXT	|extent ( 扩展 )
|FRQ	|frequency ( 频率 )
|LOC	|locative ( 地点 )
|MNR	|manner ( 方式 )
|PRP	|purpose or reason ( 目的或原因 )
|TMP	|temporal ( 时间 )
|TPC	|topic ( 主题 )
|CRD	|coordinated arguments ( 并列参数 )
|PRD	|predicate ( 谓语动词 )
|PSR	|possessor ( 持有者 )
|PSE	|possessee ( 被持有 )

**语义依存分析**  
语义依存分析 (Semantic Dependency Parsing, SDP)，分析句子各个语言单位之间的语义关联，并将语义关联以依存结构呈现。 使用语义依存刻画句子语义，好处在于不需要去抽象词汇本身，而是通过词汇所承受的语义框架来描述该词汇，而论元的数目相对词汇来说数量总是少了很多的。语义依存分析目标是跨越句子表层句法结构的束缚，直接获取深层的语义信息。  
语义依存分析不受句法结构的影响，将具有直接语义关联的语言单元直接连接依存弧并标记上相应的语义关系。这也是语义依存分析与句法依存分析的重要区别。


### 代码示例

```python
import requests

url = 'http://api.ltp-cloud.com/analysis/?api_key=YourApiKey&text=我是中国人。&pattern=dp&format=plain'
rq = requests.get(url)
print(rq.text)
```
以下是不同参数得到的结果：

分词（pattern=ws）：  
`http://api.ltp-cloud.com/analysis/?api_key=YourApiKey&text=我是中国人。&pattern=ws&format=plain`  

```python
# output: 我 是 中国 人 。
```
词性标注(pattern=pos)：  
`http://api.ltp-cloud.com/analysis/?api_key=YourApiKey&text=我是中国人。&pattern=pos&format=plain`  
```python
# output: 我_r 是_v 中国_ns 人_n 。_wp
```

命名实体识别(pattern=ner):   
`http://api.ltp-cloud.com/analysis/?api_key=YourApiKey&text=我是中国人。&pattern=ner&format=plain`  
```python
# output: 我 是 [中国]Ns 人 。
```

语义依存分析(pattern=sdp):    
`http://api.ltp-cloud.com/analysis/?api_key=YourApiKey&text=我是中国人。&pattern=sdp&format=plain`  
```python
output:
我_0 是_1 Exp
是_1 -1 Root
中国_2 人_3 Nmod
人_3 是_1 Clas
。_4 是_1 mPunc
```

### API参数集
，语言云服务的API参数集如下表所示：

|参数名	|含义	|说明
|----   |----   |----
|api_key	|用户注册语言云服务后获得的认证标识 |  |
|text	|待分析的文本。	|请以UTF-8格式编码，GET方式最大10K，POST方式最大20K
|pattern	|用以指定分析模式，可选值包括ws(分词)，pos(词性标注)，ner(命名实体识别)，dp(依存句法分析)，sdp(语义依存分析)，srl(语义角色标注),all(全部任务)	|plain格式中不允许指定全部任务
|format	|用以指定结果格式类型，可选值包括xml(XML格式)，json(JSON格式)，conll(CONLL格式)，plain(简洁文本格式)	|在指定pattern为all条件下，指定format为xml或json，返回结果将包含sdp结果，但conll格式不会包含sdp结果；
|xml_input	|用以指定输入text是否是xml格式，可选值为false(默认值),true	|仅限POST方式
|has_key	|用以指定json结果中是否含有键值，可选值包括true(含有键值，默认)，false(不含有键值)	|配合format=json使用
|only_ner	|用以指定plain格式中是否只需要ner列表，可选值包括false(默认值)和true	|配合pattern=ner&format=plain使用
|callback	|用以指定JavaScript调用中所使用的回调函数名称	|配合format=json使用

参数名及对应的参数值区分大小写且不支持缩略形式。以上参数同时支持且只支持GET和POST两种方式的提交。

**语言模型**  
目前比较认可而且有效的语言模型是n元语法模型(n-gram model)，它本质上是马尔可夫模型，简单来描述就是：一句话中下一个词的出现和最近n个词有关(包括它自身)。详细解释一下：  
如果这里的n=1时，那么最新一个词只和它自己有关，也就是它是独立的，和前面的词没关系，这叫做一元文法  
如果这里的n=2时，那么最新一个词和它前面一个词有关，比如前面的词是“我”，那么最新的这个词是“是”的概率比较高，这叫做二元文法，也叫作一阶马尔科夫链  
依次类推，工程上n=3用的是最多的，因为n越大约束信息越多，n越小可靠性更高  
n元语法模型实际上是一个概率模型，也就是出现一个词的概率是多少，或者一个句子长这个样子的概率是多少。
自然语言处理研究的两大方向：基于规则、基于统计。n元语法模型显然是基于统计的方向


### 参考
[语言云简介](http://www.ltp-cloud.com/intro/)
