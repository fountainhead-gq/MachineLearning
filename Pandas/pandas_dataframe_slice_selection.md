

```python
import pandas as pd
from pandas import Series, DataFrame
import numpy as np
```


```python
df = DataFrame(np.random.randn(6,4), columns=list('ABCD'))
df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.309768</td>
      <td>0.392892</td>
      <td>1.258556</td>
      <td>0.385553</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.823937</td>
      <td>-0.061938</td>
      <td>0.163814</td>
      <td>-1.024202</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.263913</td>
      <td>-0.725503</td>
      <td>0.199839</td>
      <td>0.331503</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.000231</td>
      <td>-1.502259</td>
      <td>-0.670218</td>
      <td>0.884615</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.225345</td>
      <td>-1.463684</td>
      <td>1.396229</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-0.372747</td>
      <td>-2.178139</td>
      <td>0.450547</td>
      <td>1.021365</td>
    </tr>
  </tbody>
</table>
</div>



## 1. DataFrame选择数据

**选择A列的数据**


```python
df['A']
```




    0   -0.309768
    1   -1.823937
    2    0.263913
    3    0.000231
    4    1.281549
    5   -0.372747
    Name: A, dtype: float64



**切片得到行数据**


```python
df[1:3]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>-1.823937</td>
      <td>-0.061938</td>
      <td>0.163814</td>
      <td>-1.024202</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.263913</td>
      <td>-0.725503</td>
      <td>0.199839</td>
      <td>0.331503</td>
    </tr>
  </tbody>
</table>
</div>



**DataFrame的loc方法帮助选择数据**


```python
# 选择第0行数据
df.loc[0]
```




    A   -0.309768
    B    0.392892
    C    1.258556
    D    0.385553
    Name: 0, dtype: float64




```python
# 选择多列数据
df.loc[:, ['A', 'B']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.309768</td>
      <td>0.392892</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.823937</td>
      <td>-0.061938</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.263913</td>
      <td>-0.725503</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.000231</td>
      <td>-1.502259</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.225345</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-0.372747</td>
      <td>-2.178139</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 选择局部数据，行列交叉区域的数据
df.loc[0:2, ['A', 'B']]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.309768</td>
      <td>0.392892</td>
    </tr>
    <tr>
      <th>1</th>
      <td>-1.823937</td>
      <td>-0.061938</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.263913</td>
      <td>-0.725503</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 只选择一个数据
df.loc[0, 'A']
```




    -0.30976817308500076



**at方法用于专门获取某个值**


```python
df.at[0, 'A']
```




    -0.30976817308500076



## 2. DataFrame切片操作

**iloc方法提取第四行数据**


```python
df.iloc[3]
```




    A    0.000231
    B   -1.502259
    C   -0.670218
    D    0.884615
    Name: 3, dtype: float64




```python
# 返回series数据类型
type(df.iloc[3])
```




    pandas.core.series.Series




```python
# 返回4-5行，1-2列
df.iloc[3:5, 0:2]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>0.000231</td>
      <td>-1.502259</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.225345</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 提取不连续行和列的数
df.iloc[[1,2,4], [0,2]]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>-1.823937</td>
      <td>0.163814</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.263913</td>
      <td>0.199839</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.463684</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 提取某一个值
df.iloc[1,1]
```




    -0.061938026695062411



**iat是专门提取某个数的方法，效率更高**


```python
df.iat[1,1]
```




    -0.061938026695062411



## 3. DataFrame筛选数据


```python
# 筛选D列数据中大于0的行
df[df.D > 0]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.309768</td>
      <td>0.392892</td>
      <td>1.258556</td>
      <td>0.385553</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.263913</td>
      <td>-0.725503</td>
      <td>0.199839</td>
      <td>0.331503</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.000231</td>
      <td>-1.502259</td>
      <td>-0.670218</td>
      <td>0.884615</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.225345</td>
      <td>-1.463684</td>
      <td>1.396229</td>
    </tr>
    <tr>
      <th>5</th>
      <td>-0.372747</td>
      <td>-2.178139</td>
      <td>0.450547</td>
      <td>1.021365</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 使用&符号实现多条件筛选
df[(df.D > 0) & (df.C < 0)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>0.000231</td>
      <td>-1.502259</td>
      <td>-0.670218</td>
      <td>0.884615</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.225345</td>
      <td>-1.463684</td>
      <td>1.396229</td>
    </tr>
  </tbody>
</table>
</div>



**加入我们只需要A和B列的数据，而D和C列数据都是用于筛选的，可如此写**


```python
df[['A', 'B']][(df.D > 0) & (df.C < 0)]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>0.000231</td>
      <td>-1.502259</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.281549</td>
      <td>-1.225345</td>
    </tr>
  </tbody>
</table>
</div>



**通过insin方法来筛选特定的值**


```python
# 
alist = [1, 0.054497, 0.36]
```


```python

```
