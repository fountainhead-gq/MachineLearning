

```python
import numpy as np
print(np.version.full_version)  # NumPy版本

a = np.arange(20)  # 一维数组a，从0开始，步长为1，长度为20
b = a.reshape(4, 5) # 构造4*5的二维数组，其中reshape的参数表示各维度的大小，且按各维顺序排列

print(a)
print(b)

a = a.reshape(2, 2, 5)
print(a)
```

    1.11.3
    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
    [[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11 12 13 14]
     [15 16 17 18 19]]
    [[[ 0  1  2  3  4]
      [ 5  6  7  8  9]]
    
     [[10 11 12 13 14]
      [15 16 17 18 19]]]
    


```python
# ndim查看维度；shape查看各维度的大小；size查看全部的元素个数，等于各维度大小的乘积；dtype可查看元素类型
print(a.ndim)
print(a.shape)
print(a.size)

```

    3
    (2, 2, 5)
    20
    


```python
# 数组的创建可通过转换列表实现
raw = [0,1,2,3,4]
a = np.array(raw)
print(a)

raw = [[0,1,2,3,4], [5,6,7,8,9]]
b = np.array(raw)
print(b)
```

    [0 1 2 3 4]
    [[0 1 2 3 4]
     [5 6 7 8 9]]
    


```python
# 4*5的全零矩阵
d = (4, 5)
np.zeros(d)
```




    array([[ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.],
           [ 0.,  0.,  0.,  0.,  0.]])




```python
# 指定类型改为整型
d = (4, 5)
np.ones(d, dtype=int)
```




    array([[1, 1, 1, 1, 1],
           [1, 1, 1, 1, 1],
           [1, 1, 1, 1, 1],
           [1, 1, 1, 1, 1]])




```python
# [0, 1)区间的随机数数组：
np.random.rand(5)
```




    array([ 0.13746355,  0.68537943,  0.89973015,  0.97209753,  0.33914642])




```python
# 四则运算
a = np.array([[1.0, 2], [2, 4]])
print(a)
b = np.array([[3.2, 1.5], [2.5, 4]])
print(b)
print(a + b)
print(a * 2)
print(2 + b)
```

    [[ 1.  2.]
     [ 2.  4.]]
    [[ 3.2  1.5]
     [ 2.5  4. ]]
    [[ 4.2  3.5]
     [ 4.5  8. ]]
    [[ 2.  4.]
     [ 4.  8.]]
    [[ 5.2  3.5]
     [ 4.5  6. ]]
    


```python
# 开根号平方指数
print(np.exp(a))
print(np.sqrt(a))
print(np.square(a))
print(np.power(a, 3))

```

    [[  2.71828183   7.3890561 ]
     [  7.3890561   54.59815003]]
    [[ 1.          1.41421356]
     [ 1.41421356  2.        ]]
    [[  1.   4.]
     [  4.  16.]]
    [[  1.   8.]
     [  8.  64.]]
    


```python
# 全部元素的和、按行求和、按列求和
a = np.arange(20).reshape(4,5)
print(a)
print(a.sum())
print(a.max())
print(a.min())
print(a.max(axis=1))
print(a.min(axis=0))
```

    [[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11 12 13 14]
     [15 16 17 18 19]]
    190
    19
    0
    [ 4  9 14 19]
    [0 1 2 3 4]
    


```python
# 矩阵对象（matrix）,数组可以通过asmatrix或者mat转换为矩阵
a = np.arange(20).reshape(4, 5)
a = np.asmatrix(a)
print(type(a))
print(a)

b = np.matrix('1.0 2.0; 3.0 4.0')
print(type(b)) 

# range函数还可以通过arange(起始，终止，步长)的方式调用生成等差数列，注意含头不含尾
b = np.arange(2, 45, 3).reshape(5, 3)
b = np.mat(b)
print(b)

```

    <class 'numpy.matrixlib.defmatrix.matrix'>
    [[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11 12 13 14]
     [15 16 17 18 19]]
    <class 'numpy.matrixlib.defmatrix.matrix'>
    [[ 2  5  8]
     [11 14 17]
     [20 23 26]
     [29 32 35]
     [38 41 44]]
    


```python
# 指定生成的一维数组的长度
np.linspace(0, 2, 9)
```




    array([ 0.  ,  0.25,  0.5 ,  0.75,  1.  ,  1.25,  1.5 ,  1.75,  2.  ])




```python
# 数组和矩阵元素的访问可通过下标进行
a = np.array([[3.2, 1.5], [2.5, 4]])
print(a[0][1]) 
print(a[0, 1]) 
```

    1.5
    1.5
    


```python
# 取矩阵的指定列
a = np.arange(20).reshape(4, 5)
print(a) 
print(a[:,[1,3]]) 
```

    [[ 0  1  2  3  4]
     [ 5  6  7  8  9]
     [10 11 12 13 14]
     [15 16 17 18 19]]
    [[ 1  3]
     [ 6  8]
     [11 13]
     [16 18]]
    


```python
a[:, 2][a[:, 0] > 5]  # 第一列大于5的第三列元素
loc = np.where(a==11)  # where函数查找特定值在数组中的位置
print(loc) 
print(a[loc[0][0], loc[1][0]]) 
```

    (array([2], dtype=int64), array([1], dtype=int64))
    11
    


```python
# 矩阵转置
a = np.random.rand(2,4)
print(a)
a = np.transpose(a)
print(a)
b = np.random.rand(2,4)
b = np.mat(b)
print(b)
print(b.T)
```

    [[ 0.87550081  0.51270126  0.79776509  0.00481561]
     [ 0.17278032  0.81914799  0.7429944   0.28837629]]
    [[ 0.87550081  0.17278032]
     [ 0.51270126  0.81914799]
     [ 0.79776509  0.7429944 ]
     [ 0.00481561  0.28837629]]
    [[ 0.62861515  0.48764909  0.96119497  0.75360651]
     [ 0.71265929  0.77148968  0.30722035  0.29610931]]
    [[ 0.62861515  0.71265929]
     [ 0.48764909  0.77148968]
     [ 0.96119497  0.30722035]
     [ 0.75360651  0.29610931]]
    


```python
# 矩阵求逆
import numpy.linalg as nlg
a = np.random.rand(2,2)
a = np.mat(a)
print(a) 
ia = nlg.inv(a)
print(ia)
print(a * ia) 
```

    [[ 0.45765423  0.45036018]
     [ 0.48244648  0.38106113]]
    [[ -8.88659462  10.50269374]
     [ 11.250967   -10.67279553]]
    [[  1.00000000e+00   8.88178420e-16]
     [  0.00000000e+00   1.00000000e+00]]
    


```python
# 求特征值和特征向量
a = np.random.rand(3,3)
eig_value, eig_vector = nlg.eig(a)
print(eig_value) 
print(eig_vector) 
```

    [ 1.4779076  -0.43255416  0.38649881]
    [[-0.34791249 -0.76381666 -0.03541517]
     [-0.62094318  0.63307519 -0.40863787]
     [-0.70241474  0.12569771  0.91200924]]
    


```python
# 按列拼接两个向量成一个矩阵：
a = np.array((1,2,3))
b = np.array((3,4,5))
print(np.column_stack((a,b))) 
```

    [[1 3]
     [2 4]
     [3 5]]
    


```python
# nan作为缺失值的记录，通过isnan判定
a = np.random.rand(2,2)
a[0, 1] = np.nan
print(np.isnan(a)) 

print(np.nan_to_num(a)) 
```

    [[False  True]
     [False False]]
    [[ 0.47441798  0.        ]
     [ 0.31226535  0.36162077]]
    


```python

```
