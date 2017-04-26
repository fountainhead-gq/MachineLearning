
## 内容索引
1. 矩阵 --- mat函数
2. 线性代数 --- 
numpy.linalg中的逆矩阵函数inv函数、行列式det函数、求解线性方程组的solve函数、内积dot函数、特征分解eigvals函数、eig函数、奇异值分解svd函数、广义逆矩阵的pinv函数


```python
import numpy as np
```

## 1. 矩阵
在NumP中，矩阵是ndarray的子类，可以由专用的字符串格式来创建。我们可以使用mat、matrix、以及bmat函数来创建矩阵。

### 1.1 创建矩阵
mat函数创建矩阵时，若输入已经为matrix或ndarray对象，则不会为它们创建副本。因此，调用mat函数和调用matrix(data, copy=False)等价。

**在创建矩阵的专用字符串中，矩阵的行与行之间用分号隔开，行内的元素之间用空格隔开。**


```python
A = np.mat('1 2 3; 4 5 6; 7 8 9')
print("Creation from string:\n", A) 
```

    Creation from string:
     [[1 2 3]
     [4 5 6]
     [7 8 9]]
    


```python
# 转置
print("Transpose A :\n", A.T) 

# 逆矩阵
print("Inverse A :\n", A.I) 
```

    Transpose A :
     [[1 4 7]
     [2 5 8]
     [3 6 9]]
    Inverse A :
     [[  3.15251974e+15  -6.30503948e+15   3.15251974e+15]
     [ -6.30503948e+15   1.26100790e+16  -6.30503948e+15]
     [  3.15251974e+15  -6.30503948e+15   3.15251974e+15]]
    


```python
# 通过NumPy数组创建矩阵
print("Creation from array: \n", np.mat(np.arange(9).reshape(3,3))) 
```

    Creation from array: 
     [[0 1 2]
     [3 4 5]
     [6 7 8]]
    

### 1.2 从已有矩阵创建新矩阵
我们可以利用一些已有的较小的矩阵来创建一个新的大矩阵。用bmat函数来实现。


```python
A = np.eye(2)
print("A:\n", A) 

B = 2 * A
print("B:\n", B) 

# 使用字符串创建复合矩阵
print("Compound matrix:\n", np.bmat("A B")) 
print("Compound matrix:\n", np.bmat("A B; B A")) 
```

    A:
     [[ 1.  0.]
     [ 0.  1.]]
    B:
     [[ 2.  0.]
     [ 0.  2.]]
    Compound matrix:
     [[ 1.  0.  2.  0.]
     [ 0.  1.  0.  2.]]
    Compound matrix:
     [[ 1.  0.  2.  0.]
     [ 0.  1.  0.  2.]
     [ 2.  0.  1.  0.]
     [ 0.  2.  0.  1.]]
    

## 2. 线性代数
线性代数是数学的一个重要分支。numpy.linalg模块包含线性代数的函数。使用这个模块，我们可以计算逆矩阵、求特征值、解线性方程组以及求解行列式。

### 2.1 计算逆矩阵
使用inv函数计算逆矩阵。


```python
A = np.mat("0 1 2; 1 0 3; 4 -3 8")
print("A:\n", A) 
inverse = np.linalg.inv(A)
print("inverse of A:\n", inverse) 
print("check inverse:\n", inverse * A) 
```

    A:
     [[ 0  1  2]
     [ 1  0  3]
     [ 4 -3  8]]
    inverse of A:
     [[-4.5  7.  -1.5]
     [-2.   4.  -1. ]
     [ 1.5 -2.   0.5]]
    check inverse:
     [[ 1.  0.  0.]
     [ 0.  1.  0.]
     [ 0.  0.  1.]]
    

### 2.2 行列式
行列式是与方阵相关的一个标量值。对于一个n*n的实数矩阵，**行列式描述的是一个线性变换对“有向体积”所造成的影响。行列式的值为正，表示保持了空间的定向(顺时针或逆时针)，为负表示颠倒空间的定向。**numpy.linalg模块中的det函数可以计算矩阵的行列式。


```python
A = np.mat("3 4; 5 6")
print("A:\n", A) 
print("Determinant:\n", np.linalg.det(A)) 
```

    A:
     [[3 4]
     [5 6]]
    Determinant:
     -2.0
    

### 2.3 求解线性方程组
矩阵可以对向量进行线性变换，这对应于数学中的线性方程组。solve函数可以求解形如Ax = b的线性方程组，其中A是矩阵，b是一维或二维的数组，x是未知变量。


```python
A = np.mat("1 -2 1; 0 2 -8; -4 5 9")
print("A:\n", A) 
b = np.array([0,8,-9])
print("b:\n", b) 
```

    A:
     [[ 1 -2  1]
     [ 0  2 -8]
     [-4  5  9]]
    b:
     [ 0  8 -9]
    


```python
x = np.linalg.solve(A, b)
print("Solution:\n", x) 

# check
print("Check:\n",b == np.dot(A, x)) 
print(np.dot(A, x)) 
```

    Solution:
     [ 29.  16.   3.]
    Check:
     [[ True  True  True]]
    [[ 0.  8. -9.]]
    

### 2.4 特征值和特征向量
特征值（eigenvalue）即方程Ax = ax的根，是一个标量。其中，A是一个二维矩阵，x是一个一维向量。特征向量（eigenvector）是关于特征值的向量。在numpy.linalg模块中，eigvals函数可以计算矩阵的特征值，而eig函数可以返回一个包含特征值和对应特征向量的元组。


```python
A = np.mat("3 -2; 1 0")
print("A:\n", A) 

print("Eigenvalues:\n", np.linalg.eigvals(A)) 

eigenvalues, eigenvectors = np.linalg.eig(A)
print("Eigenvalues:\n", eigenvalues) 
print("Eigenvectors:\n", eigenvectors) 
```

    A:
     [[ 3 -2]
     [ 1  0]]
    Eigenvalues:
     [ 2.  1.]
    Eigenvalues:
     [ 2.  1.]
    Eigenvectors:
     [[ 0.89442719  0.70710678]
     [ 0.4472136   0.70710678]]
    


```python
# check
# 计算 Ax = ax的左右两部分的值
for i in range(len(eigenvalues)):
    print("Left:\n", np.dot(A, eigenvectors[:,i])) 
    print("Right:\n", np.dot(eigenvalues[i], eigenvectors[:,i])) 
    print()
```

    Left:
     [[ 1.78885438]
     [ 0.89442719]]
    Right:
     [[ 1.78885438]
     [ 0.89442719]]
    
    Left:
     [[ 0.70710678]
     [ 0.70710678]]
    Right:
     [[ 0.70710678]
     [ 0.70710678]]
    
    

### 2.5 奇异值分解
SVD(Singular Value Decomposition，奇异值分解)是一种因子分解运算，将一个矩阵分解为3个矩阵的乘积。奇异值分解是特征值分解的一种推广。

在numpy.linalg模块中的svd函数可以对矩阵进行奇异值分解。该函数返回3个矩阵——U、Sigma和V，其中U和V是正交矩阵，Sigma包含输入矩阵的奇异值。


```python
from IPython.display import Latex
Latex(r"$M=U \Sigma V^*$")
```




$M=U \Sigma V^*$



*号表示共轭转置


```python
A = np.mat("4 11 14;8 7 -2")
print("A:\n", A) 

U, Sigma, V = np.linalg.svd(A, full_matrices=False)
print("U:\n", U) 
print("Sigma:\n", Sigma)
print("V:\n", V) 
```

    A:
     [[ 4 11 14]
     [ 8  7 -2]]
    U:
     [[-0.9486833  -0.31622777]
     [-0.31622777  0.9486833 ]]
    Sigma:
     [ 18.97366596   9.48683298]
    V:
     [[-0.33333333 -0.66666667 -0.66666667]
     [ 0.66666667  0.33333333 -0.66666667]]
    


```python
# Sigma矩阵是奇异值矩阵对角线上的值
np.diag(Sigma)
```




    array([[ 18.97366596,   0.        ],
           [  0.        ,   9.48683298]])




```python
# check
M = U*np.diag(Sigma)*V
print("Product:\n", M) 
```

    Product:
     [[  4.  11.  14.]
     [  8.   7.  -2.]]
    

### 2.6 广义逆矩阵
广义逆矩阵可以使用numpy.linalg模块中的pinv函数进行求解。inv函数只接受方阵作为输入矩阵，而pinv函数没有这个限制。


```python
A = np.mat("4 11 14; 8 7 -2")
print("A:\n", A) 
```

    A:
     [[ 4 11 14]
     [ 8  7 -2]]
    


```python
pseudoinv = np.linalg.pinv(A)
print("Pseudo inverse:\n", pseudoinv) 
```

    Pseudo inverse:
     [[-0.00555556  0.07222222]
     [ 0.02222222  0.04444444]
     [ 0.05555556 -0.05555556]]
    


```python
# check
print("Check pseudo inverse:\n", A*pseudoinv) 
```

    Check pseudo inverse:
     [[  1.00000000e+00  -4.44089210e-16]
     [ -1.38777878e-16   1.00000000e+00]]
    

得到的结果并非严格意义上的单位矩阵，但是非常近似。


```python
A = np.mat("0 1 2; 1 0 3; 4 -3 8")
print("A:\n", A) 
inverse = np.linalg.inv(A)
print("inverse of A:\n", inverse) 
print("check inverse:\n", inverse * A) 

pseudoinv = np.linalg.pinv(A)
print("Pseudo inverse:\n", pseudoinv) 
print("Check pseudo inverse:\n", A*pseudoinv) 
```

    A:
     [[ 0  1  2]
     [ 1  0  3]
     [ 4 -3  8]]
    inverse of A:
     [[-4.5  7.  -1.5]
     [-2.   4.  -1. ]
     [ 1.5 -2.   0.5]]
    check inverse:
     [[ 1.  0.  0.]
     [ 0.  1.  0.]
     [ 0.  0.  1.]]
    Pseudo inverse:
     [[-4.5  7.  -1.5]
     [-2.   4.  -1. ]
     [ 1.5 -2.   0.5]]
    Check pseudo inverse:
     [[  1.00000000e+00  -1.77635684e-15   4.44089210e-16]
     [  1.33226763e-15   1.00000000e+00   7.77156117e-16]
     [  5.32907052e-15  -7.10542736e-15   1.00000000e+00]]
    


```python

```
