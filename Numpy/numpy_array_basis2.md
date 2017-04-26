
## 内容索引
1. 数组组合
使用**vstack、dstack、hstack、column_stack、row_stack以及concatenate函数**完成数组的组合

2. 数组分割 
使用**hsplit、vsplit、dsplit和split函数**

3. 数组属性

4. 数组转换
**tolist、astype函数**


```python
%pylab
```

    Using matplotlib backend: Qt5Agg
    Populating the interactive namespace from numpy and matplotlib
    

## 1. 数组的组合

Numpy数组有水平组合、垂直组合和深度组合等多种组合方式，我们将使用**vstack、dstack、hstack、column_stack、row_stack以及concatenate函数**完成数组的组合


```python
a = arange(9).reshape(3,3)
b = 2*a
print('a: \n',a) 
print('b: \n',b) 
```

    a: 
     [[0 1 2]
     [3 4 5]
     [6 7 8]]
    b: 
     [[ 0  2  4]
     [ 6  8 10]
     [12 14 16]]
    

### （1）水平组合
将ndarray对象构成的元组作为参数，传给hstack函数
或者使用concatenate函数实现该功能


```python
hstack((a, b))
```




    array([[ 0,  1,  2,  0,  2,  4],
           [ 3,  4,  5,  6,  8, 10],
           [ 6,  7,  8, 12, 14, 16]])




```python
concatenate((a,b), axis=1)
```




    array([[ 0,  1,  2,  0,  2,  4],
           [ 3,  4,  5,  6,  8, 10],
           [ 6,  7,  8, 12, 14, 16]])



### （2）垂直组合
vstack函数
concatenate函数，axis参数设置为0


```python
vstack((a,b))
```




    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 0,  2,  4],
           [ 6,  8, 10],
           [12, 14, 16]])




```python
concatenate((a,b), axis=0)
```




    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 0,  2,  4],
           [ 6,  8, 10],
           [12, 14, 16]])



### （3）深度组合
深度组合，就是将一系列数组沿着纵轴(深度)方向进行层叠组合。


```python
dstack((a,b))
```




    array([[[ 0,  0],
            [ 1,  2],
            [ 2,  4]],
    
           [[ 3,  6],
            [ 4,  8],
            [ 5, 10]],
    
           [[ 6, 12],
            [ 7, 14],
            [ 8, 16]]])



### （4）列组合
column_stack函数。对于一维数组将按列方向进行组合。对于二维数组，效果和hstack一样。


```python
oned = arange(2)
twice_oned = 2 * oned

column_stack((oned, twice_oned))
```




    array([[0, 0],
           [1, 2]])




```python
column_stack((a,b))
```




    array([[ 0,  1,  2,  0,  2,  4],
           [ 3,  4,  5,  6,  8, 10],
           [ 6,  7,  8, 12, 14, 16]])




```python
column_stack((a,b)) == hstack((a,b))
```




    array([[ True,  True,  True,  True,  True,  True],
           [ True,  True,  True,  True,  True,  True],
           [ True,  True,  True,  True,  True,  True]], dtype=bool)



### （5）行组合
按行方向进行组合。对于两个一维数组，将直接层叠起来合成一个二维数组；对于二维数组，row_stack和vstack的效果一样。


```python
oned = arange(2)
twice_oned = 2 * oned

row_stack((oned, twice_oned))
```




    array([[0, 1],
           [0, 2]])




```python
row_stack((a,b))
```




    array([[ 0,  1,  2],
           [ 3,  4,  5],
           [ 6,  7,  8],
           [ 0,  2,  4],
           [ 6,  8, 10],
           [12, 14, 16]])



## 2. 数组的分割
Numpy数组可以进行水平、垂直和深度分割，相关的函数有hsplit、vsplit、dsplit和split。我们可以将数组分割成相同大小的子数组，也可以指定原数组中需要分割的位置。

### （1）水平分割


```python
a
```




    array([[0, 1, 2],
           [3, 4, 5],
           [6, 7, 8]])




```python
#将数组沿水平方向分割为3个相同大小的子数组
hsplit(a, 3)
```




    [array([[0],
            [3],
            [6]]), array([[1],
            [4],
            [7]]), array([[2],
            [5],
            [8]])]




```python
#调用split函数，指定参数axis=1
split(a, 3, axis=1)
```




    [array([[0],
            [3],
            [6]]), array([[1],
            [4],
            [7]]), array([[2],
            [5],
            [8]])]



### （2）垂直分割


```python
vsplit(a, 3)
```




    [array([[0, 1, 2]]), array([[3, 4, 5]]), array([[6, 7, 8]])]




```python
split(a, 3, axis=0)
```




    [array([[0, 1, 2]]), array([[3, 4, 5]]), array([[6, 7, 8]])]



### （3）深度分割


```python
c = arange(27).reshape(3,3,3)
```


```python
c
```




    array([[[ 0,  1,  2],
            [ 3,  4,  5],
            [ 6,  7,  8]],
    
           [[ 9, 10, 11],
            [12, 13, 14],
            [15, 16, 17]],
    
           [[18, 19, 20],
            [21, 22, 23],
            [24, 25, 26]]])




```python
dsplit(c, 3)
```




    [array([[[ 0],
             [ 3],
             [ 6]],
     
            [[ 9],
             [12],
             [15]],
     
            [[18],
             [21],
             [24]]]), array([[[ 1],
             [ 4],
             [ 7]],
     
            [[10],
             [13],
             [16]],
     
            [[19],
             [22],
             [25]]]), array([[[ 2],
             [ 5],
             [ 8]],
     
            [[11],
             [14],
             [17]],
     
            [[20],
             [23],
             [26]]])]



## 3. 数组的属性

* ndim属性，给出数组的维度
* size属性，给出数组元素的总个数
* itemsize属性，给出数组元素在内存中所占的字节数
* nbytes属性，给出整个数组所占存储空间，即itemsize和size属性的乘积
* T属性，和transpose函数一样
* real属性给出复数数组的实部，imag属性给出复数数组的虚部

flat属性将返回numpy.flatiter对象，这是获得flatiter对象的唯一方式。
这个所谓的“扁平迭代器”可以让我们像遍历一维数组一样遍历任意的多维数组。


```python
b = arange(4).reshape(2,2)
```


```python
b
```




    array([[0, 1],
           [2, 3]])




```python
f = b.flat
f
```




    <numpy.flatiter at 0x2b4a1c0>




```python
for item in f:
    print(item) 
```

    0
    1
    2
    3
    


```python
#使用flatiter对象直接获取一个数组的元素
b.flat[2]
```




    2




```python
b.flat[[1,3]]
```




    array([1, 3])




```python
#对flat属性赋值将导致整个数组的元素被覆盖
b.flat = 7
```


```python
b
```




    array([[7, 7],
           [7, 7]])




```python
b.flat[[1,3]] = 1
b
```




    array([[7, 1],
           [7, 1]])



## 4. 数组的转换
**tolist函数将numpy数组转换成python列表**


```python
b
```




    array([[7, 1],
           [7, 1]])




```python
b.tolist()
```




    [[7, 1], [7, 1]]



**astype函数可以在转换数组时指定数据类型**


```python
b.astype(float)
```




    array([[ 7.,  1.],
           [ 7.,  1.]])




```python

```
