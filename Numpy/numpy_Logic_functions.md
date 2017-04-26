
# Logic functions


```python
import numpy as np
```


```python
np.__version__
```




    '1.11.3'



## Truth value testing


Q1. Let x be an arbitrary array. Return True if none of the elements of x is zero. Remind that 0 evaluates to False in python.



```python
x = np.array([1,2,3])
print(np.all(x)) 

x = np.array([1,0,3])
print(np.all(x))
```

    True
    False
    

Q2. Let x be an arbitrary array. Return True if any of the elements of x is non-zero.


```python
x = np.array([1,0,0])
print (np.any(x))

x = np.array([0,0,0])
print (np.any(x))
```

    True
    False
    

## Array contents


Q3. Predict the result of the following code.


```python
x = np.array([1, 0, np.nan, np.inf])
print (np.isfinite(x))
```

    [ True  True False False]
    

Q4. Predict the result of the following code.


```python
x = np.array([1, 0, np.nan, np.inf])
print (np.isinf(x))
```

    [False False False  True]
    

Q5. Predict the result of the following code.


```python
x = np.array([1, 0, np.nan, np.inf])
print (np.isnan(x))
```

    [False False  True False]
    

## Array type testing

Q6. Predict the result of the following code.


```python
x = np.array([1+1j, 1+0j, 4.5, 3, 2, 2j])
print (np.iscomplex(x))
```

    [ True False False False False  True]
    

Q7. Predict the result of the following code.


```python
x = np.array([1+1j, 1+0j, 4.5, 3, 2, 2j])
print (np.isreal(x))
```

    [False  True  True  True  True False]
    

Q8. Predict the result of the following code.


```python
print (np.isscalar(3))
print (np.isscalar([3]))
print (np.isscalar(True))
```

    True
    False
    True
    

## Logical operations

Q9. Predict the result of the following code.


```python
print (np.logical_and([True, False], [False, False]))
print (np.logical_or([True, False, True], [True, False, False]))
print (np.logical_xor([True, False, True], [True, False, False]))
print (np.logical_not([True, False, 0, 1]))

```

    [False False]
    [ True False  True]
    [False False  True]
    [False  True  True False]
    

## Comparison

Q10. Predict the result of the following code.


```python
print (np.allclose([3], [2.999999]))
print (np.array_equal([3], [2.999999]))
```

    True
    False
    

Q11. Write numpy comparison functions such that they return the results as you see.


```python
x = np.array([4, 5])
y = np.array([2, 5])
print (np.greater(x, y))
print (np.greater_equal(x, y))
print (np.less(x, y))
print (np.less_equal(x, y))

```

    [ True False]
    [ True  True]
    [False False]
    [False  True]
    

Q12. Predict the result of the following code.


```python
print (np.equal([1, 2], [1, 2.000001]))
print (np.isclose([1, 2], [1, 2.000001]))
```

    [ True False]
    [ True  True]
    


```python

```
