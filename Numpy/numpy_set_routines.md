
# Set routines


```python
import numpy as np
```


```python
np.__version__
```




    '1.11.3'



## Making proper sets

Q1. Get unique elements and reconstruction indices from x. And reconstruct x.


```python
x = np.array([1, 2, 6, 4, 2, 3, 2])
out, indices = np.unique(x, return_inverse=True)
print ("unique elements =", out)
print ("reconstruction indices =", indices)
print ("reconstructed =", out[indices])
```

    unique elements = [1 2 3 4 6]
    reconstruction indices = [0 1 4 3 1 2 1]
    reconstructed = [1 2 6 4 2 3 2]
    

## Boolean operations

Q2. Create a boolean array of the same shape as x. If each element of x is present in y, the result will be True, otherwise False.


```python
x = np.array([0, 1, 2, 5, 0])
y = np.array([0, 1])
print (np.in1d(x, y))
```

    [ True  True False False  True]
    

Q3. Find the unique intersection of x and y.


```python
x = np.array([0, 1, 2, 5, 0])
y = np.array([0, 1, 4])
print (np.intersect1d(x, y))
```

    [0 1]
    

Q4. Find the unique elements of x that are not present in y.


```python
x = np.array([0, 1, 2, 5, 0])
y = np.array([0, 1, 4])
print (np.setdiff1d(x, y))
```

    [2 5]
    

Q5. Find the xor elements of x and y.


```python
x = np.array([0, 1, 2, 5, 0])
y = np.array([0, 1, 4])
out1 = np.setxor1d(x, y)
out2 = np.sort(np.concatenate((np.setdiff1d(x, y), np.setdiff1d(y, x))))

print (out1)
print (out2)

```

    [2 4 5]
    [2 4 5]
    

Q6. Find the union of x and y.


```python
x = np.array([0, 1, 2, 5, 0])
y = np.array([0, 1, 4])
out1 = np.union1d(x, y)
out2 = np.sort(np.unique(np.concatenate((x, y))))
print (out1)
print (out2)
```

    [0 1 2 4 5]
    [0 1 2 4 5]
    


```python

```
