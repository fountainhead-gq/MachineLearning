
pandas含有使数据分析工作变得更快更简单的高级数据结构和操作工具。pandas基于NumPy构建，让以NumPy为中心的应用变得更加简单。


```python
import pandas as pd
from pandas import Series, DataFrame
import numpy as np
```

## 1. Pandas的数据结构
pandas的两个主要数据结构是：Series和DataFrame。

### 1.1 Series
Series是一种类似于一维数组的对象，它由一组**数据**（各种NumPy数据类型）以及一组与之相关的**数据索引**组成。

#### 1. Series的构建


```python
# Series的字符串表现形式为：索引在左边，值在右边。
# 由于我们没有为数据指定索引，于是会自动创建一个0到N-1的整数索引
obj = Series([4, 7, -5, 3])
obj
```




    0    4
    1    7
    2   -5
    3    3
    dtype: int64




```python
# 获取Series的values和index属性
obj.values
```




    array([ 4,  7, -5,  3], dtype=int64)




```python
obj.index
```




    RangeIndex(start=0, stop=4, step=1)




```python
# 创建Series带有可以对各个数据点进行标记的索引
obj2 = Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2
```




    d    4
    b    7
    a   -5
    c    3
    dtype: int64




```python
obj2.index
```




    Index(['d', 'b', 'a', 'c'], dtype='object')




```python
obj2['a']
```




    -5



#### 2. NumPy数组运算


```python
obj2
```




    d    4
    b    7
    a   -5
    c    3
    dtype: int64




```python
# 布尔表达式过滤
obj2[obj2 > 0]
```




    d    4
    b    7
    c    3
    dtype: int64




```python
# 标量乘法
obj2 * 2
```




    d     8
    b    14
    a   -10
    c     6
    dtype: int64




```python
# 应用数学函数
np.exp(obj2)
```




    d      54.598150
    b    1096.633158
    a       0.006738
    c      20.085537
    dtype: float64



将Series看成是一个定长的有序字典，因为它是索引值到数据值的一个映射。


```python
'b' in obj2
```




    True




```python
'e' in obj2
```




    False



#### 3. 通过Python字典创建Series


```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
# 传入Python字典，原字典的键成为Series的索引
obj3 = Series(sdata)
obj3
```




    Ohio      35000
    Oregon    16000
    Texas     71000
    Utah       5000
    dtype: int64




```python
sindex = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = Series(sdata, index=sindex)
# sdata中跟states索引项匹配的值会被找出来并放到相应的位置上
obj4
```




    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    dtype: float64




```python
obj4.isnull()
```




    California     True
    Ohio          False
    Oregon        False
    Texas         False
    dtype: bool



#### 4. Series自动对齐


```python
obj3 + obj4
```




    California         NaN
    Ohio           70000.0
    Oregon         32000.0
    Texas         142000.0
    Utah               NaN
    dtype: float64



#### 5. Series的name属性


```python
obj4.name = 'population'
obj4.index.name = 'state'
obj4
```




    state
    California        NaN
    Ohio          35000.0
    Oregon        16000.0
    Texas         71000.0
    Name: population, dtype: float64



#### 6. 修改Series的索引


```python
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj
```




    Bob      4
    Steve    7
    Jeff    -5
    Ryan     3
    dtype: int64



### 1.2 DataFrame
DataFrame是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型。DataFrame既有行索引也有列索引，它可以被看做由Series组成的字典（功用同一个索引）。

#### 1. 构建DataFrame
最常用是直接传入一个由等长列表或NumPy数组组成的字典


```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9]}
frame = DataFrame(data)
frame
# DataFrame会自动加上索引，且全部被有序排列
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>pop</th>
      <th>state</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.5</td>
      <td>Ohio</td>
      <td>2000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.7</td>
      <td>Ohio</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.6</td>
      <td>Ohio</td>
      <td>2002</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.4</td>
      <td>Nevada</td>
      <td>2001</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.9</td>
      <td>Nevada</td>
      <td>2002</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 如果指定列序列，则DataFrame的列就会按照指定顺序进行排列
DataFrame(data, columns=['year', 'state', 'pop'])
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 如果传入的列在数据中找不到，就会产生NA值
frame2 = DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
         index=['one', 'two', 'three', 'four', 'five'])
frame2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



#### 2. 对DataFrame的行和列的操作

通过类似字典标记的方式或属性的方式，可以将DataFrame的列获取为一个Series


```python
frame2['state']
```




    one        Ohio
    two        Ohio
    three      Ohio
    four     Nevada
    five     Nevada
    Name: state, dtype: object




```python
frame2.year
```




    one      2000
    two      2001
    three    2002
    four     2001
    five     2002
    Name: year, dtype: int64



返回的Series拥有原DataFrame相同的索引，且其name属性已经被设置好了

用索引字段ix可以获得DataFrame的一行


```python
frame2.ix['three']
```




    year     2002
    state    Ohio
    pop       3.6
    debt      NaN
    Name: three, dtype: object



列可以通过赋值的方式进行修改


```python
frame2['debt'] = 16.5
```


```python
frame2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>16.5</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>16.5</td>
    </tr>
  </tbody>
</table>
</div>



将列表或数组赋值给某个列时，其长度必须跟DataFrame的长度相匹配。如果赋值的是一个Series，就会精确匹配DataFrame的索引


```python
frame2['debt'] = [1, 2, 3, 4, 5]
frame2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>1</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>2</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>3</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>




```python
val = Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
frame2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>-1.2</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>-1.5</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>-1.7</td>
    </tr>
  </tbody>
</table>
</div>



为不存在的列赋值会创建出一个新列


```python
frame2['eastern'] = frame2.state == 'Ohio'
```


```python
frame2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
      <th>eastern</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>-1.2</td>
      <td>True</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>-1.5</td>
      <td>False</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>-1.7</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



关键字del用于删除列


```python
del frame2['eastern']
```


```python
frame2.columns
```




    Index(['year', 'state', 'pop', 'debt'], dtype='object')




```python
frame2
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>-1.2</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>-1.5</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>-1.7</td>
    </tr>
  </tbody>
</table>
</div>



通过索引方式返回的列只是相应数据的视图而已，并不是副本。对返回的Series所做的任何修改都会反映到原DataFrame上。通过Series的copy方法即可显式地复制列。

#### 3. 传给DataFrame嵌套字典
如果数据形式是嵌套字典（字典的字典），将它传给DataFrame，它会被解释为：外层的键作为列，内层的键则作为行索引。


```python
pop = {'Nevada': {2001: 2.4, 2002: 2.9},
      'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame3 = DataFrame(pop)
frame3
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Nevada</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000</th>
      <td>NaN</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>2.4</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>2.9</td>
      <td>3.6</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 对结果进行转置
frame3.T
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2000</th>
      <th>2001</th>
      <th>2002</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Nevada</th>
      <td>NaN</td>
      <td>2.4</td>
      <td>2.9</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>1.5</td>
      <td>1.7</td>
      <td>3.6</td>
    </tr>
  </tbody>
</table>
</div>



可以输入给DataFrame构造器的数据：
- 二维ndarray： 数据矩阵
- 由数组、列表或元组组成的字典： 每个序列会变成DataFrame的一列。所有序列的长度必须相同
- NumPy的结构化/记录数组： 类似于 有数组组成的字典
- 由Series组成的字典
- 由字典组成的字典
- 字典或Series的列表
- 由列表或元组组成的列表
- 另一个DataFrame
- NumPy的MaskedArray

#### 4. DataFrame的属性


```python
# 设置DataFrame的index和columns的name属性，并显示出来
frame3.index.name = 'year'
frame3.columns.name = 'state'
frame3
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>state</th>
      <th>Nevada</th>
      <th>Ohio</th>
    </tr>
    <tr>
      <th>year</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000</th>
      <td>NaN</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>2.4</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>2.9</td>
      <td>3.6</td>
    </tr>
  </tbody>
</table>
</div>



DataFrame的values属性会以二维ndarray的形式返回DataFrame中的数据


```python
frame3.values
```




    array([[ nan,  1.5],
           [ 2.4,  1.7],
           [ 2.9,  3.6]])



## 2. 索引对象
pandas的索引对象负责轴标签和其他元数据(比如轴名称等)，构建Series或DataFrame时， 所用到的任何数组或其他序列的标签都会被转换成一个Index。Index对象是


```python
obj = Series(range(3), index=['a','b','c'])
index = obj.index
index
```




    Index(['a', 'b', 'c'], dtype='object')




```python
index[1:]
```




    Index(['b', 'c'], dtype='object')




```python

```
