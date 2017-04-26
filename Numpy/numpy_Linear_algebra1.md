
# Linear algebra


```python
import numpy as np
```


```python
np.__version__
```




    '1.11.3'



## Matrix and vector products

Q1. Predict the results of the following code.


```python
x = [1,2]
y = [[4, 1], [2, 2]]
print(np.dot(x, y)) 
print(np.dot(y, x)) 
print(np.matmul(x, y)) 
print(np.inner(x, y)) 
print(np.inner(y, x)) 
```

    [8 5]
    [6 6]
    [8 5]
    [6 6]
    [6 6]
    

Q2. Predict the results of the following code.


```python
x = [[1, 0], [0, 1]]
y = [[4, 1], [2, 2], [1, 1]]
print(np.dot(y, x)) 
print(np.matmul(y, x)) 
```

    [[4 1]
     [2 2]
     [1 1]]
    [[4 1]
     [2 2]
     [1 1]]
    

Q3. Predict the results of the following code.


```python
x = np.array([[1, 4], [5, 6]])
y = np.array([[4, 1], [2, 2]])
print(np.vdot(x, y)) 
print(np.vdot(y, x)) 
print(np.dot(x.flatten(), y.flatten())) 
print(np.inner(x.flatten(), y.flatten())) 
print((x*y).sum()) 
```

    30
    30
    30
    30
    30
    

Q4. Predict the results of the following code.


```python
x = np.array(['a', 'b'], dtype=object)
y = np.array([1, 2])
print(np.inner(x, y)) 
print(np.inner(y, x)) 
print(np.outer(x, y)) 
print(np.outer(y, x)) 
```

    abb
    abb
    [['a' 'aa']
     ['b' 'bb']]
    [['a' 'b']
     ['aa' 'bb']]
    

## Decompositions

Q5. Get the lower-trianglular `L` in the Cholesky decomposition of x and verify it.


```python
x = np.array([[4, 12, -16], [12, 37, -43], [-16, -43, 98]], dtype=np.int32)
L = np.linalg.cholesky(x)
print(L)
assert np.array_equal(np.dot(L, L.T.conjugate()), x)
```

    [[ 2.  0.  0.]
     [ 6.  1.  0.]
     [-8.  5.  3.]]
    True
    

Q6. Compute the qr factorization of x and verify it.


```python
x = np.array([[12, -51, 4], [6, 167, -68], [-4, 24, -41]], dtype=np.float32)
q, r = np.linalg.qr(x)
print("q=\n", q, "\nr=\n", r) 
assert np.allclose(np.dot(q, r), x)
```

    q=
     [[-0.85714287  0.39428571  0.33142856]
     [-0.42857143 -0.90285712 -0.03428571]
     [ 0.2857143  -0.17142858  0.94285715]] 
    r=
     [[ -14.  -21.   14.]
     [   0. -175.   70.]
     [   0.    0.  -35.]]
    

Q7. Factor x by Singular Value Decomposition and verify it.


```python
x = np.array([[1, 0, 0, 0, 2], [0, 0, 3, 0, 0], [0, 0, 0, 0, 0], [0, 2, 0, 0, 0]], dtype=np.float32)
U, s, V = np.linalg.svd(x, full_matrices=False)
print("U=\n", U, "\ns=\n", s, "\nV=\n", V) 
assert np.allclose(np.dot(U, np.dot(np.diag(s), V)), x)

```

    U=
     [[ 0.  1.  0.  0.]
     [ 1.  0.  0.  0.]
     [ 0.  0.  0. -1.]
     [ 0.  0.  1.  0.]] 
    s=
     [ 3.          2.23606801  2.          0.        ] 
    V=
     [[-0.          0.          1.          0.          0.        ]
     [ 0.44721359  0.          0.          0.          0.89442718]
     [-0.          1.          0.          0.          0.        ]
     [ 0.          0.          0.          1.          0.        ]]
    

## Matrix eigenvalues

Q8. Compute the eigenvalues and right eigenvectors of x. (Name them eigenvals and eigenvecs, respectively)


```python
x = np.diag((1, 2, 3))
eigenvals = np.linalg.eig(x)[0]
eigenvals_ = np.linalg.eigvals(x)
assert np.array_equal(eigenvals, eigenvals_)
print("eigenvalues are\n", eigenvals) 
eigenvecs = np.linalg.eig(x)[1]
print("eigenvectors are\n", eigenvecs) 
```

    eigenvalues are
     [ 1.  2.  3.]
    eigenvectors are
     [[ 1.  0.  0.]
     [ 0.  1.  0.]
     [ 0.  0.  1.]]
    

Q9. Predict the results of the following code.


```python
print(np.array_equal(np.dot(x, eigenvecs), eigenvals * eigenvecs)) 
```

    True
    

## Norms and other numbers

Q10. Calculate the Frobenius norm and the condition number of x.


```python
x = np.arange(1, 10).reshape((3, 3))
print(np.linalg.norm(x, 'fro')) 
print(np.linalg.cond(x, 'fro')) 
```

    16.8819430161
    3.19323951562e+17
    

Q11. Calculate the determinant of x.


```python
x = np.arange(1, 5).reshape((2, 2))
out1 = np.linalg.det(x)
out2 = x[0, 0] * x[1, 1] - x[0, 1] * x[1, 0]
assert np.allclose(out1, out2)
print(out1) 
```

    -2.0
    

Q12. Calculate the rank of x.


```python
x = np.eye(4)
out1 = np.linalg.matrix_rank(x)
out2 = np.linalg.svd(x)[1].size
assert out1 == out2
print(out1) 

```

    4
    

Q13. Compute the sign and natural logarithm of the determinant of x.


```python
x = np.arange(1, 5).reshape((2, 2))
sign, logdet = np.linalg.slogdet(x)
det = np.linalg.det(x)
assert sign == np.sign(det)
assert logdet == np.log(np.abs(det))
print(sign, logdet) 

```

    -1.0 0.69314718056
    

Q14. Return the sum along the diagonal of x.


```python
x = np.eye(4)
out1 = np.trace(x)
out2 = x.diagonal().sum()
print(out1) 
print(out2)
```

    4.0
    4.0
    

## Solving equations and inverting matrices

Q15. Compute the inverse of x.


```python
x = np.array([[1., 2.], [3., 4.]])
out1 = np.linalg.inv(x)
assert np.allclose(np.dot(x, out1), np.eye(2))
print(out1) 
```

    [[-2.   1. ]
     [ 1.5 -0.5]]
    


```python

```
