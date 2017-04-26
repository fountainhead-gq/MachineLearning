
## 内容索引
1. 通用函数
    - 创建通用函数 --- frompyfunc工厂函数
    - 通用函数的方法 --- reduce函数、accumulate函数、reduceat函数、outer函数
2. 数组的除法运算 --- divide函数、true_divide函数、floor_divide函数
3. 数组的模运算 --- mod函数、remainder函数、fmod函数
4. 位操作函数和比较函数 --- 


```python
import numpy as np
```

## 1. 通用函数
通用函数的输入时一组标量、输出也是一组标量，他们通常可以对应于基本数学运算。如加减乘除。

[通用函数的文档](http://docs.scipy.org/doc/numpy/reference/ufuncs.html#ufunc)

通用函数是对普通的python函数进行矢量化，它是对ndarray对象的逐个元素的操作。

### 1.1 创建通用函数
使用NumPy中的frompyfunc函数，通过一个Python函数来创建通用函数。


```python
# 定义一个Python函数
def pyFunc(a):
    result = np.zeros_like(a)
    # 这里可以看出来对逐个元素的操作
    result = 42
    return result
```

使用zeros_like函数创建一个和a形状相同的、元素全部为0的数组result。flat属性提供了一个扁平迭代器，可以逐个设置数组元素的值。

使用frompyfunc创建通用函数，制定输入参数为1，输出参数为1。


```python
ufunc1 = np.frompyfunc(pyFunc, 1, 1)
ret = ufunc1(np.arange(4))
print("The answer:\n", ret) 
```

    The answer:
     [42 42 42 42]
    


```python
ret = ufunc1(np.arange(4).reshape(2,2))
print("The answer:\n", ret) 
```

    The answer:
     [[42 42]
     [42 42]]
    

**使用在第五节介绍的vectorize函数**


```python
func2 = np.vectorize(pyFunc)
ret = func2(np.arange(4))
print("The answer:\n", ret) 
```

    The answer:
     [42 42 42 42]
    

### 1.2 通用函数的方法
通用函数并不是真正的函数，而是能够表示函数的numpy.ufunc的对象。frompyfunc是一个构造ufunc类对象的工厂函数。

通用函数类有4个方法：**reduce、accumulate、reduceat、outer。这些方法只对输入两个参数、输出一个参数的ufunc对象有效**。

### 1.3 在add函数上分别调用4个方法

#### (1) reduce方法：
沿着指定的轴，在连续的数组元素之间递归调用通用函数，即可得到输入数组的规约(reduce)计算结果。对于add函数，其对数组的reduce计算结果等价于对数组元素求和。


```python
a = np.arange(9)
print("a:\n", a) 
print("Reduce:\n", np.add.reduce(a)) 
```

    a:
     [0 1 2 3 4 5 6 7 8]
    Reduce:
     36
    

#### (2) accumulate方法：
可以递归作用于输入数组，与reduce不同的是，它将存储运算的中间结果并返回。add函数上调用accumulate方法，等价于直接调用cumsum函数。


```python
print("Accumulate:\n", np.add.accumulate(a)) 
```

    Accumulate:
     [ 0  1  3  6 10 15 21 28 36]
    


```python
print("cumsum:\n", np.cumsum(a)) 
```

    cumsum:
     [ 0  1  3  6 10 15 21 28 36]
    

#### (3) reduceat方法有点复杂，它需要输入一个数组以及一个索引值列表作为参数


```python
print("Reduceat:\n", np.add.reduceat(a, [0,5,2,7])) 
```

    Reduceat:
     [10  5 20 15]
    

- 第一步：用到索引值列表中的0和5，实际上就是对数组中索引值在0到5之间的元素进行reduce操作
- 第二步：用到索引值5和2.由于2比5小，所以直接返回索引值为5的元素
- 第三步：用到索引值2和7，计算2到7的数组的reduce操作
- 第四步：用到索引值7，对索引值7开始直到数组末尾的元素进行reduce操作


```python
print("Reduceat step 1:", np.add.reduce(a[0:5])) 
print("Reduceat step 2:", a[5]) 
print("Reduceat step 3:", np.add.reduce(a[2:7])) 
print("Reduceat step 4:", np.add.reduce(a[7:])) 
```

    Reduceat step 1: 10
    Reduceat step 2: 5
    Reduceat step 3: 20
    Reduceat step 4: 15
    

#### (4) outer方法：返回一个数组，它的秩(rank)等于两个输入数组的秩的和。它会作用于两个输入数组之间存在的所有元素对。


```python
print("Outer:\n", np.add.outer(np.arange(3), a)) 
```

    Outer:
     [[ 0  1  2  3  4  5  6  7  8]
     [ 1  2  3  4  5  6  7  8  9]
     [ 2  3  4  5  6  7  8  9 10]]
    

## 2. 数组的除法运算

在NumPy中，计算算术运算符+、-、* 隐式关联着通用函数add、subtrack和multiply。也就是说，当你对NumPy数组使用这些运算符时，对应的通用函数将自动被调用。

除法包含的过程比较复杂，在数组的除法运算中射击三个通用函数divide、true_divide和floor_division，以及两个对应的运算符/和//。

### (1) divide函数在整数除法中均只保留整数部分


```python
a = np.array([2, 6, 5])
b = np.array([1, 2, 3])
print("Divide:\n", np.divide(a, b), np.divide(b, a)) 
```

    Divide:
     [ 2.          3.          1.66666667] [ 0.5         0.33333333  0.6       ]
    

运算结果的小数部分被截断了


```python
c = np.array([2.1, 6.2, 5.0])
d = np.array([1, 2, 1.9])
print("Divide:\n", np.divide(c, d), np.divide(d, c)) 
```

    Divide:
     [ 2.1         3.1         2.63157895] [ 0.47619048  0.32258065  0.38      ]
    

divide函数如果有一方是浮点数，那么结果也是浮点数结果

### (2) true_divide函数与数学中的除法定义更为接近，返回除法的浮点数结果不截断


```python
print("True Divide:\n", np.true_divide(a, b), np.true_divide(b, a)) 
```

    True Divide:
     [ 2.          3.          1.66666667] [ 0.5         0.33333333  0.6       ]
    

### (3) floor_divide函数总是返回整数结果，相当于先调用divide函数再调用floor函数。
floor函数对浮点数进行**向下取整**并返回整数。


```python
print("Floor Divide:\n", np.floor_divide(a, b), np.floor_divide(b, a)) 
```

    Floor Divide:
     [2 3 1] [0 0 0]
    

**默认情况下，使用/运算符相当于调用divide函数，使用//运算符对应于floor_divide函数**

## 3. 数组的模运算
计算模数或者余数，可以使用NumPy中的mod、remainder和fmod函数。也可以用%运算符。

### (1) remainder函数逐个返回两个数组中元素相除后的余数，如果第二个数字为0，则直接返回0


```python
a = np.arange(-4,4)
print("a:\n", a) 
print("Remainder:\n", np.remainder(a, 2)) 
```

    a:
     [-4 -3 -2 -1  0  1  2  3]
    Remainder:
     [0 1 0 1 0 1 0 1]
    

**mod函数与remainder函数的功能完全一致，%操作符仅仅是remainder函数的简写**

### (2) fmod函数处理负数的方式和remainder不同。所得余数的正负由被除数决定，与除数的正负无关


```python
print("Fmod:\n", np.fmod(a, 2)) 
```

    Fmod:
     [ 0 -1  0 -1  0  1  0  1]
    


```python
print(np.fmod(a, -2)) 
```

    [ 0 -1  0 -1  0  1  0  1]
    

## 4. 位操作函数和比较函数
位操作函数可以在整数或整数数组的位上进行操作，它们都是通用函数。

位操作符：^、&、|、<<、>>等。

比较操作符：<、>、==等。

### 4.1 检查两个整数的符号是否一致
这里要用到XOR或者^操作符。XOR操作符又称为**不等运算符**，因此当两个操作数的符号不一致时，XOR操作的结果为负数。

在NumPy中，^操作符对应于bitwise_xor函数，<操作符对应于less函数。


```python
x = np.arange(-9, 9)
y = -x
print("Sign different? ", (x^y) < 0) 
print("Sign different? ", np.less(np.bitwise_xor(x, y), 0)) 
```

    Sign different?  [ True  True  True  True  True  True  True  True  True False  True  True
      True  True  True  True  True  True]
    Sign different?  [ True  True  True  True  True  True  True  True  True False  True  True
      True  True  True  True  True  True]
    

除了等于0的情况，所有整数对的符号都不一样。

### 4.2 检查一个数是否为2的幂数
在二进制数中，2的幂数表示为一个1后面跟着一串0的形式。**如果在2的幂数以及比它小1的数之间进行位与操作AND，那么应该等于0。**

在NumPy中，&操作符对应于bitwise_and函数，==操作符对应于equal函数。


```python
b = np.arange(20)
print(b) 
print("Power of 2 :\n", (b & (b-1)) == 0) 
print("Power of 2 :\n", np.equal(np.bitwise_and(b, (b-1)), 0)) 
```

    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19]
    Power of 2 :
     [ True  True  True False  True False False False  True False False False
     False False False False  True False False False]
    Power of 2 :
     [ True  True  True False  True False False False  True False False False
     False False False False  True False False False]
    

### 4.3 计算一个数被2的幂数整除后的余数
计算余数的技巧只在模为2的幂数时有效。二进制的位左移一位，数值翻倍。

**上一个例子看到，将2的幂数减去1，得到一串1组成的二进制数，这为我们提供了掩码，与这样的掩码做位与操作，即可得到以2的幂数作为模的余数。**

在NumPy中，<<操作符对应于left_shift函数。


```python
print("Modulus 4:\n", x & ((1<<2) - 1)) 
```

    Modulus 4:
     [3 0 1 2 3 0 1 2 3 0 1 2 3 0 1 2 3 0]
    


```python
def mod_2_pow(x, n):
    mod = x & ((1<<n) - 1)
    return mod
```


```python
mod_2_pow(x,2)
```




    array([3, 0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3, 0], dtype=int32)




```python
mod_2_pow(x, 3)
```




    array([7, 0, 1, 2, 3, 4, 5, 6, 7, 0, 1, 2, 3, 4, 5, 6, 7, 0], dtype=int32)




```python

```
