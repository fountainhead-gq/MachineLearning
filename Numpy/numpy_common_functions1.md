
## 内容索引
这一小节介绍NumPy的常用函数。

1. 读入csv
loadtxt函数

2. 计算平均值
average、mean函数

3. 求最大最小值
max、min函数

4. 计算中位数
median、msort函数

5. 计算方差
var函数

## 1. 读写CSV文件
CSV（Comma-Separated Value，逗号分隔值）格式是一种常见的文件格式。通常，数据库的转存文件就是csv格式的，文件中的各个字段对应于数据库中的列。

**Numpy中的loadtxt函数可以方便地读取csv文件，自动切分字段，并将数据载入Numpy数组。**


```python
import numpy as np
```


```python
c, v = np.loadtxt('data.csv', delimiter=',', usecols=(6,7), unpack=True)
```

data.csv文件是苹果公司的历史股价数据。第一列为股票代码，第二列为dd-mm-yyyy格式的日期，第三列为空，随后各列依次是开盘价(4)、最高价(5)、最低价(6)和收盘价(7)，最后一列为当日的成交量(8)。

loadtxt函数中，*usecols参数*为一个元组，以获得*第7字段至第8字段*的数据，也就是股票的收盘价和成交量数据。

*unpack参数*设置为True，意思是分拆存储不同列的数据，即分别将收盘价和成交量的数组赋值给变量c和v。

用usecols中的参数指定我们感兴趣的数据列，并将unpack参数设置为True使得不同列的数据分别存储。


```python
c
```




    array([ 336.1 ,  339.32,  345.03,  344.32,  343.44,  346.5 ,  351.88,
            355.2 ,  358.16,  354.54,  356.85,  359.18,  359.9 ,  363.13,
            358.3 ,  350.56,  338.61,  342.62,  342.88,  348.16,  353.21,
            349.31,  352.12,  359.56,  360.  ,  355.36,  355.76,  352.47,
            346.67,  351.99])




```python
v
```




    array([ 21144800.,  13473000.,  15236800.,   9242600.,  14064100.,
            11494200.,  17322100.,  13608500.,  17240800.,  33162400.,
            13127500.,  11086200.,  10149000.,  17184100.,  18949000.,
            29144500.,  31162200.,  23994700.,  17853500.,  13572000.,
            14395400.,  16290300.,  21521000.,  17885200.,  16188000.,
            19504300.,  12718000.,  16192700.,  18138800.,  16824200.])




```python
#选择第4列，开盘价
opening_price = np.loadtxt('data.csv', delimiter=',', usecols=(3,), unpack=True)
```


```python
print(opening_price) 
```

    [ 344.17  335.8   341.3   344.45  343.8   343.61  347.89  353.68  355.19
      357.39  354.75  356.79  359.19  360.8   357.1   358.21  342.05  338.77
      344.02  345.29  351.21  355.47  349.96  357.2   360.07  361.11  354.91
      354.69  349.69  345.4 ]
    

## 2. 计算平均值

### 2.1 计算加权平均
VWAP是Volume-Weighted Average Price，成交量加权平均价格，某个价格的成交量越高，该价格所占的权重就越大。**VWAP就是以成交量为权重计算出来的加权平均值。**


```python
vwap = np.average(c, weights=v)
print("VWAP =", vwap) 
```

    VWAP = 350.589549353
    

TWAP是Time Weighted Average Price，时间加权平均价格，其基本思想是最近的价格重要性大一些，所以我们应该对近期的价格给以较高的权重。
我们使用arange函数创建从0递增的自然数序列，自然数的个数即为收盘价的个数。


```python
t = np.arange(len(c))
print("twap = ",np.average(c, weights=t)) 
```

    twap =  352.428321839
    

### 2.2 算术平均
使用mean函数计算算术平均


```python
mean = np.mean(c)
print("mean = ",mean) 
```

    mean =  351.037666667
    


```python
print("mean = ", c.mean()) 
```

    mean =  351.037666667
    

## 3. 求最大最小值和取值范围

步骤：读入最高价和最低价，使用max和min函数得到最大最小值。


```python
h,l = np.loadtxt('data.csv', delimiter=',', usecols=(4,5), unpack=True)
print('hightest = ', np.max(h)) 
print('lowest = ', np.min(l)) 
```

    hightest =  364.9
    lowest =  333.53
    

numpy中ptp函数可以计算数组的取值范围。该函数返回的是数组元素最大值和最小值的差值，即max(array)-min(array)。


```python
print('Spread high price : ', np.ptp(h)) 
print('Spread low price : ', np.ptp(l)) 
```

    Spread high price :  24.86
    Spread low price :  26.97
    

## 4. 计算中位数

计算收盘价的中位数


```python
closing_price = np.loadtxt('data.csv', delimiter=',', usecols=(6,), unpack=True)
print('median = ', np.median(closing_price)) 
```

    median =  352.055
    

对数组进行排序，之后再去中位数


```python
sorted_closing = np.msort(closing_price)
print("sorted_closing_price = ", sorted_closing) 
```

    sorted_closing_price =  [ 336.1   338.61  339.32  342.62  342.88  343.44  344.32  345.03  346.5
      346.67  348.16  349.31  350.56  351.88  351.99  352.12  352.47  353.21
      354.54  355.2   355.36  355.76  356.85  358.16  358.3   359.18  359.56
      359.9   360.    363.13]
    


```python
#先判断数组的个数是奇数还是偶数
N = len(closing_price)
median_ind = (N-1)/2
if N & 0x1 :
    print("median = ", sorted_closing[median_ind]) 
else:
    print("median = ", (sorted_closing[median_ind]+sorted_closing[median_ind+1])/2) 
```

    median =  352.055
    

    D:\Anaconda3\lib\site-packages\ipykernel\__main__.py:7: VisibleDeprecationWarning: using a non-integer number instead of an integer will result in an error in the future
    

## 5. 计算方差
方差体现变量变化的程度，股价变动过于剧烈的股票一定会给持有者制造麻烦。


```python
print("variance = ", np.var(closing_price)) 
```

    variance =  50.1265178889
    


```python
#手动求方差
print('variance from definition = ', np.mean( (closing_price-c.mean())**2 )) 
```

    variance from definition =  50.1265178889
    


```python

```
