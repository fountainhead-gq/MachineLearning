

```python
import numpy as np
```

## 断言函数
**单元测试通常使用断言函数作为测试的组成部分。**在进行数值计算时，我们经常遇到比较两个近似相等的浮点数这样的基本问题，由于计算机对浮点数的表示本身就不精确，所以浮点数的比较并不是那么简单。

numpy.testing包中有很多实用的工具函数考虑了浮点数的比较，可以测试前提是否成立。

[NumPy的testing模块文档](http://docs.scipy.org/doc/numpy/reference/routines.testing.html)

## 1. assert_almost_equal断言近似相等
assert_almost_equal函数的作用是，如果两个数字的近似程度没有达到指定精度，就抛出异常。


```python
#使用assert_almost_equal函数来检查它们是否近似相等

#指定精度，小数点后7位
print("Decimal 6", np.testing.assert_almost_equal(0.123456789, 0.123456780, decimal=7)) 
```

    Decimal 6 None
    


```python
#指定精度，小数点后8位
#抛出异常
print("Decimal 7", np.testing.assert_almost_equal(0.123456789, 0.123456780, decimal=8)) 
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-3-9c9c516a4fbf> in <module>()
          1 #指定精度，小数点后8位
          2 #抛出异常
    ----> 3 print("Decimal 7", np.testing.assert_almost_equal(0.123456789, 0.123456780, decimal=8))
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_almost_equal(actual, desired, decimal, err_msg, verbose)
        537         pass
        538     if round(abs(desired - actual), decimal) != 0:
    --> 539         raise AssertionError(_build_err_msg())
        540 
        541 
    

    AssertionError: 
    Arrays are not almost equal to 8 decimals
     ACTUAL: 0.123456789
     DESIRED: 0.12345678


## 2. assert_approx_equal断言近似相等
如果两个数字的近似程度没有达到指定的有效数字要求，assert_approx_equal函数就抛出异常。

触发条件为：abs(actual-expected) >= 10**(significant-1)


```python
# 指定8位有效数字
print("Significance 8", np.testing.assert_approx_equal(0.123456789,0.123456780, significant=8)) 
```

    Significance 8 None
    


```python
# 指定9位有效数字
# 抛出异常
print("Significance 9", np.testing.assert_approx_equal(0.123456789,0.123456780, significant=9)) 
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-5-6d9b38187af9> in <module>()
          1 # 指定9位有效数字
          2 # 抛出异常
    ----> 3 print("Significance 9", np.testing.assert_approx_equal(0.123456789,0.123456780, significant=9))
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_approx_equal(actual, desired, significant, err_msg, verbose)
        635         pass
        636     if np.abs(sc_desired - sc_actual) >= np.power(10., -(significant-1)):
    --> 637         raise AssertionError(msg)
        638 
        639 def assert_array_compare(comparison, x, y, err_msg='', verbose=True,
    

    AssertionError: 
    Items are not equal to 9 significant digits:
     ACTUAL: 0.123456789
     DESIRED: 0.12345678


## 3. assert_array_almost_equal断言数组近似相等
如果两个数组中元素的近似程度没有达到指定的精度要求，assert_array_almost_equal函数将抛出异常。

触发条件为： |expected - actual| < 0.5 * 10^(-decimal)


```python
print("Decimal 8", np.testing.assert_array_almost_equal([0, 0.123456789], [0, 0.123456780], decimal=8)) 
```

    Decimal 8 None
    


```python
print("Decimal 9", np.testing.assert_array_almost_equal([0, 0.123456789], [0, 0.123456780], decimal=9)) 
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-7-a574ef65c014> in <module>()
    ----> 1 print("Decimal 9", np.testing.assert_array_almost_equal([0, 0.123456789], [0, 0.123456780], decimal=9))
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_array_almost_equal(x, y, decimal, err_msg, verbose)
        916     assert_array_compare(compare, x, y, err_msg=err_msg, verbose=verbose,
        917              header=('Arrays are not almost equal to %d decimals' % decimal),
    --> 918              precision=decimal)
        919 
        920 
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_array_compare(comparison, x, y, err_msg, verbose, header, precision)
        737                                 names=('x', 'y'), precision=precision)
        738             if not cond:
    --> 739                 raise AssertionError(msg)
        740     except ValueError:
        741         import traceback
    

    AssertionError: 
    Arrays are not almost equal to 9 decimals
    
    (mismatch 50.0%)
     x: array([ 0.         ,  0.123456789])
     y: array([ 0.        ,  0.12345678])


## 4. assert_array_equal断言数组相等
如果两个数组对象不相同，assert_array_equal函数将抛出异常。两个数组相等必须形状一致且元素也严格相等，允许数组存在NaN元素。

比较数组也可以使用assert_allclose函数。该函数有参数atol(absolute tolerance,绝对容差限)和rtol(relative tolerance，相对容差限)。对于两个数组a和b，将测试是否满足以下条件：| a - b | <= (atol+rtol*b)


```python
print("Pass", np.testing.assert_allclose([0,0.123456789,np.nan],[0,0.123456780,np.nan], rtol=1e-7,atol=0)) 
```

    Pass None
    


```python
print("Fail", np.testing.assert_array_equal([0,0.123456789,np.nan],[0,0.123456780,np.nan])) 
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-9-34dffe0a737e> in <module>()
    ----> 1 print("Fail", np.testing.assert_array_equal([0,0.123456789,np.nan],[0,0.123456780,np.nan]))
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_array_equal(x, y, err_msg, verbose)
        811     """
        812     assert_array_compare(operator.__eq__, x, y, err_msg=err_msg,
    --> 813                          verbose=verbose, header='Arrays are not equal')
        814 
        815 def assert_array_almost_equal(x, y, decimal=6, err_msg='', verbose=True):
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_array_compare(comparison, x, y, err_msg, verbose, header, precision)
        737                                 names=('x', 'y'), precision=precision)
        738             if not cond:
    --> 739                 raise AssertionError(msg)
        740     except ValueError:
        741         import traceback
    

    AssertionError: 
    Arrays are not equal
    
    (mismatch 50.0%)
     x: array([ 0.      ,  0.123457,       nan])
     y: array([ 0.      ,  0.123457,       nan])


## 5. 数组排序
两个数组必须形状一致并且第一个数组元素严格小于第二个数组元素，否则assert_array_less函数将抛出异常。


```python
# assert_array_less函数比较两个有严格顺序的数组
print("Pass", np.testing.assert_array_less([0,0.1,np.nan], [1,0.2,np.nan])) 
```

    Pass None
    

## 6. assert_equal比较对象
这里的对象不一定是NumPy数组对象，也可以是Python中的列表、元组或字典。

## 7. assert_string_equal字符串比较
assert_string_equal函数断言两个字符串变量完全相同。如果测试不通过，将会抛出异常并显示两个字符串之间的差别。该函数区分大小写，


```python
print("Fail", np.testing.assert_string_equal("NumPy", "NumPy")) 
```

    Fail None
    

## 8. 浮点数比较
浮点数在计算机中是以不精确的方式表示的，这给浮点数的比较带来了问题。NumPy中的assert_array_almost_equal_nulp和assert_array_max_ulp函数可以提供可靠的浮点数比较功能。ULP是Unit of Least Precision的缩写，即浮点数的最小精度单位。根据IEEE 754标准，四则运算的误差必须保持在半个ULP之内。

机器精度(machine epsilon)是指浮点运算中的相对舍入误差上界。机器精度等于ULP相对于1的值。NumPy的finfo函数可以获取机器精度。


```python
# 使用finfo函数确定机器精度
eps = np.finfo(float).eps
print("Eps", eps) 
```

    Eps 2.22044604925e-16
    

使用assert_array_almost_equal_nulp函数比较两个近似相等的浮点数1.0和1.0+eps


```python
print("one eps", np.testing.assert_array_almost_equal_nulp(1.0, 1.0+eps)) 
```

    one eps None
    


```python
print("two eps", np.testing.assert_array_almost_equal_nulp(1.0, 1.0+eps*2)) 
```


    ---------------------------------------------------------------------------

    AssertionError                            Traceback (most recent call last)

    <ipython-input-15-22236b226aa5> in <module>()
    ----> 1 print("two eps", np.testing.assert_array_almost_equal_nulp(1.0, 1.0+eps*2))
    

    D:\Anaconda3\lib\site-packages\numpy\testing\utils.py in assert_array_almost_equal_nulp(x, y, nulp)
       1452             max_nulp = np.max(nulp_diff(x, y))
       1453             msg = "X and Y are not equal to %d ULP (max is %g)" % (nulp, max_nulp)
    -> 1454         raise AssertionError(msg)
       1455 
       1456 def assert_array_max_ulp(a, b, maxulp=1, dtype=None):
    

    AssertionError: X and Y are not equal to 1 ULP (max is 2)


**多ULP浮点数的比较**

assert_array_max_ulp函数可以指定ULP的数量作为允许的误差上界。参数maxulp接受整数作为ULP数量的上限，默认值为1。


```python
print("one eps", np.testing.assert_array_max_ulp(1.0, 1.0+eps)) 
```

    one eps 1.0
    


```python
print("two eps", np.testing.assert_array_max_ulp(1.0, 1.0+eps*2,maxulp=2)) 
```

    two eps 2.0
    


```python

```
