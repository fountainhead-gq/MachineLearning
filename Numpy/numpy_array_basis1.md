

```python
%pylab inline
```

    Populating the interactive namespace from numpy and matplotlib
    

ndarray是一个多维数组对象，该对象由**实际的数据、描述这些数据的元数据**组成，大部分数组操作仅仅修改元数据部分，而不改变底层的实际数据。

### 用arange函数创建数组


```python
a = arange(5)
a.dtype
```




    dtype('int32')




```python
a
```




    array([0, 1, 2, 3, 4])




```python
a.shape
```




    (5,)



数组的shape属性返回一个元组(tuple)，元组中的元素即NumPy数组每一个维度的大小。

### 1. 创建多维数组
array函数可以依据给定的对象生成数组。
给定的对象应是类数组，如python的列表、numpy的arange函数


```python
m = array([arange(2), arange(2)])
```


```python
print(m)
print(m.shape) 
print(type(m)) 
print(type(m.shape)) 
```

    [[0 1]
     [0 1]]
    (2, 2)
    <class 'numpy.ndarray'>
    <class 'tuple'>
    

### 选取元素


```python
a = array([[1,2],[3,4]])
print(a[0,0]) 
print(a[0,1])
```

    1
    2
    

### NumPy数据类型
Numpy除了Python支持的整型、浮点型、复数型之外，还添加了很多其他的数据类型。

Type	Remarks	   Character code
bool_	compatible: Python bool	'?'
bool8	8 bits	 
Integers:

byte	compatible: C char	'b'
short	compatible: C short	'h'
intc	compatible: C int	'i'
int_	compatible: Python int	'l'
longlong	compatible: C long long	'q'
intp	large enough to fit a pointer	'p'
int8	8 bits	 
int16	16 bits	 
int32	32 bits	 
int64	64 bits	 
Unsigned integers:

ubyte	compatible: C unsigned char	'B'
ushort	compatible: C unsigned short	'H'
uintc	compatible: C unsigned int	'I'
uint	compatible: Python int	'L'
ulonglong	compatible: C long long	'Q'
uintp	large enough to fit a pointer	'P'
uint8	8 bits	 
uint16	16 bits	 
uint32	32 bits	 
uint64	64 bits	 
Floating-point numbers:

half	 	'e'
single	compatible: C float	'f'
double	compatible: C double	 
float_	compatible: Python float	'd'
longfloat	compatible: C long float	'g'
float16	16 bits	 
float32	32 bits	 
float64	64 bits	 
float96	96 bits, platform?	 
float128	128 bits, platform?	 
Complex floating-point numbers:

csingle	 	'F'
complex_	compatible: Python complex	'D'
clongfloat	 	'G'
complex64	two 32-bit floats	 
complex128	two 64-bit floats	 
complex192	two 96-bit floats, platform?	 
complex256	two 128-bit floats, platform?	 
Any Python object:

object_	any Python object	'O'

**每一种数据类型均有对应的类型转换函数**


```python
print(float64(42)) 
print(int8(42.0)) 
print(bool(42)) 
print(float(True)) 
```

    42.0
    42
    True
    1.0
    


```python
arange(8, dtype=uint16)
```




    array([0, 1, 2, 3, 4, 5, 6, 7], dtype=uint16)



**复数不能转换成整数和浮点数**

Numpy数组中每一个元素均为相同的数据类型，现在给出单个元素所占字节


```python
a.dtype
```




    dtype('int32')




```python
a.dtype.itemsize
```




    4



**dtype类的属性**


```python
t = dtype('float64')
print(t.char) 
print(t.type) 
print(t.str) 
```

    d
    <class 'numpy.float64'>
    <f8
    

`str` 属性可以给出数据类型的字符串表示，该字符串的首个字符表示字节序，然后是字符编码，然后是所占字节数
字节序是指位长为32和64的字（word）存储的顺序，包括大端序(big-endian)和小端序(little-endian)。
大端序是将最高位字节存储在最低的内存地址处，用>表示；与之相反，小端序是将最低位字节存储在最低的内存地址处，用<表示。

**创建自定义数据类型**

自定义数据类型是一种异构数据类型，可以当做用来记录电子表格或数据库中一行数据的结构。

下面我们创建一种自定义的异构数据类型，该数据类型包括一个用字符串记录的名字、一个用整数记录的数字以及一个用浮点数记录的价格。


```python
t = dtype([('name', str_, 40), ('numitems', int32), ('price', float32)])
```


```python
t
```




    dtype([('name', '<U40'), ('numitems', '<i4'), ('price', '<f4')])




```python
t['name']
```




    dtype('<U40')




```python
itemz = array([('Meaning of life DVD', 32, 3.14), ('Butter', 13, 2.72)], dtype=t)
```


```python
print(itemz[0])
print(itemz[1])
```

    ('Meaning of life DVD', 32, 3.140000104904175)
    ('Butter', 13, 2.7200000286102295)
    

### 2. 数组的索引和切片


```python
a = arange(9)
#下标0-7， 以2为步长
print(a[:7:2]) 

print(a[1::5]) 
print(a[1::]) 
#以负数下标翻转数组
print(a[::-1]) 
print(a[::-2]) 

```

    [0 2 4 6]
    [1 6]
    [1 2 3 4 5 6 7 8]
    [8 7 6 5 4 3 2 1 0]
    [8 6 4 2 0]
    

### 多维数组的切片和索引


```python
b = arange(24).reshape(2,3,4)
print(b.shape) 
print(b)
```

    (2, 3, 4)
    [[[ 0  1  2  3]
      [ 4  5  6  7]
      [ 8  9 10 11]]
    
     [[12 13 14 15]
      [16 17 18 19]
      [20 21 22 23]]]
    

用三维坐标选定任意一个房间，即楼层、行号、列号


```python
#选取第一层楼所有房间
print(b[0]) 
print()
print(b[0, :, :]) 
```

    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    
    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    


```python
#多个冒号用一个省略号代替
b[0, ...]
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])




```python
#间隔选元素
b[0,1,::2]
```




    array([4, 6])




```python
#多维数组执行翻转一维数组的命令，将在最前面的维度上翻转元素的顺序
b[::-1]
```




    array([[[12, 13, 14, 15],
            [16, 17, 18, 19],
            [20, 21, 22, 23]],
    
           [[ 0,  1,  2,  3],
            [ 4,  5,  6,  7],
            [ 8,  9, 10, 11]]])




```python
b[::-1,::-1,::-1]
```




    array([[[23, 22, 21, 20],
            [19, 18, 17, 16],
            [15, 14, 13, 12]],
    
           [[11, 10,  9,  8],
            [ 7,  6,  5,  4],
            [ 3,  2,  1,  0]]])



## 3. 改变数组的维度

**ravel 完成展平操作**


```python
b.ravel()
```




    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
           17, 18, 19, 20, 21, 22, 23])



**flatten 也是展平**

flatten函数会请求分配内存来保存结果，而ravel函数只是返回数组的一个视图（view）


```python
b.flatten()
```




    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
           17, 18, 19, 20, 21, 22, 23])



**用元组设置维度**


```python
b.shape = (6, 4)
```


```python
b
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11],
           [12, 13, 14, 15],
           [16, 17, 18, 19],
           [20, 21, 22, 23]])



**transpose转置矩阵**


```python
b.transpose()
```




    array([[ 0,  4,  8, 12, 16, 20],
           [ 1,  5,  9, 13, 17, 21],
           [ 2,  6, 10, 14, 18, 22],
           [ 3,  7, 11, 15, 19, 23]])



**resize和reshape函数功能一样**
但resize会直接改变所操作的数组


```python
b.reshape(2,3,4)
```




    array([[[ 0,  1,  2,  3],
            [ 4,  5,  6,  7],
            [ 8,  9, 10, 11]],
    
           [[12, 13, 14, 15],
            [16, 17, 18, 19],
            [20, 21, 22, 23]]])




```python
b
```




    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11],
           [12, 13, 14, 15],
           [16, 17, 18, 19],
           [20, 21, 22, 23]])




```python
b.resize(2,12)
```


```python
b
```




    array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11],
           [12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]])




```python

```
