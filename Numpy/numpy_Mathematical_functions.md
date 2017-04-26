
# Mathematical functions


```python
import numpy as np
```


```python
np.__version__
```




    '1.11.3'



## Trigonometric functions

Q1. Calculate sine, cosine, and tangent of x, element-wise.


```python
x = np.array([0., 1., 30, 90])
print ("sine:", np.sin(x))
print ("cosine:", np.cos(x))
print ("tangent:", np.tan(x))
```

    sine: [ 0.          0.84147098 -0.98803162  0.89399666]
    cosine: [ 1.          0.54030231  0.15425145 -0.44807362]
    tangent: [ 0.          1.55740772 -6.4053312  -1.99520041]
    

Q2. Calculate inverse sine, inverse cosine, and inverse tangent of x, element-wise.


```python
x2 = np.array([-1., 0, 1.])
print ("inverse sine:", np.arcsin(x2))
print ("inverse cosine:", np.arccos(x2))
print ("inverse tangent:", np.arctan(x2))
```

    inverse sine: [-1.57079633  0.          1.57079633]
    inverse cosine: [ 3.14159265  1.57079633  0.        ]
    inverse tangent: [-0.78539816  0.          0.78539816]
    

Q3. Convert angles from radians to degrees.


```python
x = np.array([-np.pi, -np.pi/2, np.pi/2, np.pi])

out1 = np.degrees(x)
out2 = np.rad2deg(x)
assert np.array_equiv(out1, out2)
print (out1)
print(out2)
```

    [-180.  -90.   90.  180.]
    [-180.  -90.   90.  180.]
    

Q4. Convert angles from degrees to radians.


```python
x = np.array([-180.,  -90.,   90.,  180.])

out1 = np.radians(x)
out2 = np.deg2rad(x)
assert np.array_equiv(out1, out2)
print (out1)
```

    [-3.14159265 -1.57079633  1.57079633  3.14159265]
    

## Hyperbolic functions

Q5. Calculate hyperbolic sine, hyperbolic cosine, and hyperbolic tangent of x, element-wise.


```python
x = np.array([-1., 0, 1.])
print (np.sinh(x))
print (np.cosh(x))
print (np.tanh(x))
```

    [-1.17520119  0.          1.17520119]
    [ 1.54308063  1.          1.54308063]
    [-0.76159416  0.          0.76159416]
    

## Rounding

Q6. Predict the results of these, paying attention to the difference among the family functions.


```python
x = np.array([2.1, 1.5, 2.5, 2.9, -2.1, -2.5, -2.9])

out1 = np.around(x)
out2 = np.floor(x)
out3 = np.ceil(x)
out4 = np.trunc(x)
out5 = [round(elem) for elem in x]

print (out1)
print (out2)
print (out3)
print (out4)
print (out5)

```

    [ 2.  2.  2.  3. -2. -2. -3.]
    [ 2.  1.  2.  2. -3. -3. -3.]
    [ 3.  2.  3.  3. -2. -2. -2.]
    [ 2.  1.  2.  2. -2. -2. -2.]
    [2.0, 2.0, 2.0, 3.0, -2.0, -2.0, -3.0]
    

Q7. Implement out5 in the above question using numpy.


```python
print (np.floor(np.abs(x) + 0.5) * np.sign(x)) 
```

    [ 2.  2.  3.  3. -2. -3. -3.]
    

## Sums, products, differences

Q8. Predict the results of these.


```python
x = np.array(
    [[1, 2, 3, 4],
     [5, 6, 7, 8]])

outs = [np.sum(x),
        np.sum(x, axis=0),
        np.sum(x, axis=1, keepdims=True),
        "",
        np.prod(x),
        np.prod(x, axis=0),
        np.prod(x, axis=1, keepdims=True),
        "",
        np.cumsum(x),
        np.cumsum(x, axis=0),
        np.cumsum(x, axis=1),
        "",
        np.cumprod(x),
        np.cumprod(x, axis=0),
        np.cumprod(x, axis=1),
        "",
        np.min(x),
        np.min(x, axis=0),
        np.min(x, axis=1, keepdims=True),
        "",
        np.max(x),
        np.max(x, axis=0),
        np.max(x, axis=1, keepdims=True),
        "",
        np.mean(x),
        np.mean(x, axis=0),
        np.mean(x, axis=1, keepdims=True)]
           
for out in outs:
    if out == "":
        print()
    else:
        print("->", out)

```

    -> 36
    -> [ 6  8 10 12]
    -> [[10]
     [26]]
    
    -> 40320
    -> [ 5 12 21 32]
    -> [[  24]
     [1680]]
    
    -> [ 1  3  6 10 15 21 28 36]
    -> [[ 1  2  3  4]
     [ 6  8 10 12]]
    -> [[ 1  3  6 10]
     [ 5 11 18 26]]
    
    -> [    1     2     6    24   120   720  5040 40320]
    -> [[ 1  2  3  4]
     [ 5 12 21 32]]
    -> [[   1    2    6   24]
     [   5   30  210 1680]]
    
    -> 1
    -> [1 2 3 4]
    -> [[1]
     [5]]
    
    -> 8
    -> [5 6 7 8]
    -> [[4]
     [8]]
    
    -> 4.5
    -> [ 3.  4.  5.  6.]
    -> [[ 2.5]
     [ 6.5]]
    

    D:\Anaconda3\lib\site-packages\ipykernel\__main__.py:34: FutureWarning: elementwise comparison failed; returning scalar instead, but in the future will perform elementwise comparison
    

Q9. Calculate the difference between neighboring elements, element-wise.


```python
x = np.array([1, 2, 4, 7, 0])
print (np.diff(x))
```

    [ 1  2  3 -7]
    

Q10. Calculate the difference between neighboring elements, element-wise, and
prepend [0, 0] and append[100] to it.


```python
x = np.array([1, 2, 4, 7, 0])

out1 = np.ediff1d(x, to_begin=[0, 0], to_end=[100])
out2 = np.insert(np.append(np.diff(x), 100), 0, [0, 0])
assert np.array_equiv(out1, out2)
print (out2)
```

    [  0   0   1   2   3  -7 100]
    

Q11. Return the cross product of x and y.


```python
x = np.array([1, 2, 3])
y = np.array([4, 5, 6])

print (np.cross(x, y))
```

    [-3  6 -3]
    

## Exponents and logarithms

Q12. Compute $e^x$, element-wise.


```python
x = np.array([1., 2., 3.], np.float32)
out = np.exp(x)
print (out)
```

    [  2.71828175   7.38905621  20.08553696]
    

Q13. Calculate exp(x) - 1 for all elements in x.


```python
x = np.array([1., 2., 3.], np.float32)
out1 = np.expm1(x)
out2 = np.exp(x) - 1.
assert np.allclose(out1, out2)
print (out1)
print (out2)
```

    [  1.71828175   6.38905621  19.08553696]
    [  1.71828175   6.38905621  19.08553696]
    

Q14. Calculate $2^p$ for all p in x.


```python
x = np.array([1., 2., 3.], np.float32)
out1 = np.exp2(x)
out2 = 2 ** x
print (out1)
print (out2)
```

    [ 2.  4.  8.]
    [ 2.  4.  8.]
    

Q15. Compute natural, base 10, and base 2 logarithms of x element-wise.


```python
x = np.array([1, np.e, np.e**2])
print ("natural log =", np.log(x))
print ("common log =", np.log10(x))
print ("base 2 log =", np.log2(x))
```

    natural log = [ 0.  1.  2.]
    common log = [ 0.          0.43429448  0.86858896]
    base 2 log = [ 0.          1.44269504  2.88539008]
    

Q16. Compute the natural logarithm of one plus each element in x in floating-point accuracy.


```python
x = np.array([1e-99, 1e-100])
print (np.log1p(x))
# Compare it with np.log(1 +x)
```

    [  1.00000000e-099   1.00000000e-100]
    

## Floating point routines

Q17. Return element-wise True where signbit is set.


```python
x = np.array([-3, -2, -1, 0, 1, 2, 3])

out1 = np.signbit(x)
out2 = x < 0
assert np.array_equiv(out1, out2)
print (out1)

```

    [ True  True  True False False False False]
    

Q18. Change the sign of x to that of y, element-wise.


```python
x = np.array([-1, 0, 1])
y = -1.1
print (np.copysign(x, y))
```

    [-1. -0. -1.]
    

## Arithmetic operations

Q19. Add x and y element-wise.


```python
x = np.array([1, 2, 3])
y = np.array([-1, -2, -3])
out1 = np.add(x, y)
out2 = x + y
print (out1)
print (out2)
```

    [0 0 0]
    [0 0 0]
    

Q20. Subtract y from x element-wise.


```python
x = np.array([3, 4, 5])
y = np.array(3)

out1 = np.subtract(x, y)
out2 = x - y 

print (out1)
print (out2)
```

    [0 1 2]
    [0 1 2]
    

Q21. Multiply x by y element-wise.


```python
x = np.array([3, 4, 5])
y = np.array([1, 0, -1])

out1 = np.multiply(x, y)
out2 = x * y 

print (out1)
print (out2)
```

    [ 3  0 -5]
    [ 3  0 -5]
    

Q22. Divide x by y element-wise in two different ways.


```python
x = np.array([3., 4., 5.])
y = np.array([1., 2., 3.])

out1 = np.true_divide(x, y)
out2 = x / y 
print (out1)

out3 = np.floor_divide(x, y)
out4 = x // y
print (out3)

```

    [ 3.          2.          1.66666667]
    [ 3.  2.  1.]
    

Q23. Compute numerical negative value of x, element-wise.


```python
x = np.array([1, -1])

out1 = np.negative(x)
out2 = -x
print (out1)

```

    [-1  1]
    

Q24. Compute the reciprocal of x, element-wise.


```python
x = np.array([1., 2., .2])

out1 = np.reciprocal(x)
out2 = 1/x
print (out1)
```

    [ 1.   0.5  5. ]
    

Q25. Compute $x^y$, element-wise.


```python
x = np.array([[1, 2], [3, 4]])
y = np.array([[1, 2], [1, 2]])

out = np.power(x, y)
print (out)

```

    [[ 1  4]
     [ 3 16]]
    

Q26. Compute the remainder of x / y element-wise in two different ways.


```python
x = np.array([-3, -2, -1, 1, 2, 3])
y = 2

out1 = np.mod(x, y)
out2 = x % y
print (out1)

out3 = np.fmod(x, y)
print (out3)
```

    [1 0 1 1 0 1]
    [-1  0 -1  1  0  1]
    

## Miscellaneous

Q27. If an element of x is smaller than 3, replace it with 3.
And if an element of x is bigger than 7, replace it with 7.


```python
x = np.arange(10)
out1 = np.clip(x, 2, 7)
out2 = np.copy(x)
print (out1)
print (out2)
out2[out2 < 3] = 3
out2[out2 > 7] = 7
print (out2)
```

    [2 2 2 3 4 5 6 7 7 7]
    [0 1 2 3 4 5 6 7 8 9]
    [3 3 3 3 4 5 6 7 7 7]
    

Q28. Compute the square of x, element-wise.


```python
x = np.array([1, 2, -1])

out1 = np.square(x)
out2 = x * x

print (out1)

```

    [1 4 1]
    

Q29. Compute square root of x element-wise.



```python
x = np.array([1., 4., 9.])

out = np.sqrt(x)
print (out)

```

    [ 1.  2.  3.]
    

Q30. Compute the absolute value of x.


```python
x = np.array([[1, -1], [3, -3]])

out = np.abs(x)
print (out)

```

    [[1 1]
     [3 3]]
    

Q31. Compute an element-wise indication of the sign of x, element-wise.


```python
x = np.array([1, 3, 0, -1, -3])

out1 = np.sign(x)
out2 = np.copy(x)
print (out1)
print (out2)

out2[out2 > 0] = 1
out2[out2 < 0] = -1
print (out2)
```

    [ 1  1  0 -1 -1]
    [ 1  3  0 -1 -3]
    [ 1  1  0 -1 -1]
    


```python

```
