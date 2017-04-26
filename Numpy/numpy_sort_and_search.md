
## 内容索引
1. 按字典序排序 --- lexsort函数
2. 复数排序 --- sort_complex函数
3. 搜索 --- argmax、nanargmax、argwhere、searchsorted函数
4. 数据元素抽取 --- extract、nonzero函数


```python
import numpy as np
```

## 排序
- sort函数返回排序后的数组
- lexsort函数根据键值的字典序进行排序
- argsort函数返回输入数组排序后的下标
- ndarray类sort方法可对数组进行原地排序
- msort函数沿着第一个周排序
- sort_complex函数对复数按照先实数后虚数的顺序进行排序

## 1. 按字典序排序
NumPy中的lexsort函数返回输入数组按字典序排序的下标。我们需要给lexsort函数提供排序所依据的键值数组或元组。


```python
# 我们使用转换函数，载入收盘价和日期数据
import datetime
def datestr2num(s):
    return datetime.datetime.strptime(s.decode('utf-8'), "%d-%m-%Y").toordinal()

dates, cp = np.loadtxt('AAPL.csv', delimiter=',', usecols=(1,6), converters={1:datestr2num}, unpack=True)
dates = dates.astype(int)
```


```python
# 数据本身已经按照日期顺序排好，不过我们现在优先按照收盘价排序
# lexsort返回排序后的数组下标
indices = np.lexsort((dates, cp))
print("Indices:\n", indices) 
```

    Indices:
     [ 0 16  1 17 18  4  3  2  5 28 19 21 15  6 29 22 27 20  9  7 25 26 10  8 14
     11 23 12 24 13]
    


```python
print(["%s %s" % (datetime.date.fromordinal(dates[i]), cp[i]) for i in indices]) 
```

    ['2011-01-28 336.1', '2011-02-22 338.61', '2011-01-31 339.32', '2011-02-23 342.62', '2011-02-24 342.88', '2011-02-03 343.44', '2011-02-02 344.32', '2011-02-01 345.03', '2011-02-04 346.5', '2011-03-10 346.67', '2011-02-25 348.16', '2011-03-01 349.31', '2011-02-18 350.56', '2011-02-07 351.88', '2011-03-11 351.99', '2011-03-02 352.12', '2011-03-09 352.47', '2011-02-28 353.21', '2011-02-10 354.54', '2011-02-08 355.2', '2011-03-07 355.36', '2011-03-08 355.76', '2011-02-11 356.85', '2011-02-09 358.16', '2011-02-17 358.3', '2011-02-14 359.18', '2011-03-03 359.56', '2011-02-15 359.9', '2011-03-04 360.0', '2011-02-16 363.13']
    

## 2. 对复数进行排序
复数包含实数部分和虚数部分。NumPy中有专门的复数类型，使用两个浮点数表示复数。这些复数可以使用NumPy的sort_complex函数进行排序。该函数按照**先实部后虚部的顺序排序**。


```python
# 生成5个随机数作为实部，5个随机数作为虚部
# 设置随机数种子为42
np.random.seed(42)
complex_numbers = np.random.random(5) + 1j * np.random.random(5)
print("Complex numbers:\n", complex_numbers) 
```

    Complex numbers:
     [ 0.37454012+0.15599452j  0.95071431+0.05808361j  0.73199394+0.86617615j
      0.59865848+0.60111501j  0.15601864+0.70807258j]
    


```python
print("Sorted:\n", np.sort_complex(complex_numbers)) 
```

    Sorted:
     [ 0.15601864+0.70807258j  0.37454012+0.15599452j  0.59865848+0.60111501j
      0.73199394+0.86617615j  0.95071431+0.05808361j]
    

## 3. 搜索
NumPy有多个函数可以在数据中进行搜索。

* argmax函数返回数组中最大值对应的下标


```python
a = np.array([2,4,8])
np.argmax(a)
```




    2



* nanargmax函数提供相同的功能，但忽略NaN值


```python
b = np.array([np.nan, 2, 4])
np.nanargmax(b)
```




    2



argmin和nanargmin函数功能类似，只是换成最小值

* argwhere函数根据天骄搜索非零的元素，并分组返回对应的下标


```python
np.argwhere(a <= 4)
```




    array([[0],
           [1]], dtype=int64)



### 使用searchsorted函数
searchsorted函数可以为指定的插入值寻找维持数组排序的索引位置。该函数使用二分搜索算法，计算复杂度为O(log(n))。

searchsorted函数为指定的插入值返回一个在有序数组中的索引位置，从这个位置插入可以保持数组的有序性。


```python
# 创建升序排列的数组
a = np.arange(5)

# 调用searchsorted函数
indices = np.searchsorted(a, [-2,7])
print("Indices", indices) 
```

    Indices [0 5]
    


```python
# 使用insert函数构建完整的数组
print("The full array", np.insert(a, indices, [-2,7])) 
```

    The full array [-2  0  1  2  3  4  7]
    

searchsorted函数为7和-2返回索引5和0.用这些索引作为插入位置，我们生成了数组[-2  0  1  2  3  4  7]，这样就维持了数组的顺序。

## 4. 数组元素抽取
NumPy的extract函数可以根据某个条件从数组中抽取元素。这与where函数相似，nonzero函数专门用来抽取非零的数组元素。


```python
a = np.arange(7)

# 生成选择偶数元素的条件变量
condition = (a%2) == 0

# 使用extract函数基于生成的条件从数组中抽取元素
print("Even numbers:\n", np.extract(condition, a)) 
```

    Even numbers:
     [0 2 4 6]
    


```python
print("Non zero:\n", np.nonzero(a)) 
```

    Non zero:
     (array([1, 2, 3, 4, 5, 6], dtype=int64),)
    


```python

```
