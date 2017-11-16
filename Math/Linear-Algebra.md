# 1.1 标量，向量，矩阵，张量

线性代数的内容主要关于这四种对象：

### 标量

标量表示单独的一个数，通常用斜体小写字母表示，如 $$ s\in \mathbb R, n \in \mathbb N$$。

### 向量

向量是一个一维数组，这些数字有一定的顺序，可以通过下标获取对应的值，通常用粗体小写字母表示：$$\mathbf{x} \in \mathbb R^n$$，表示元素取实数，且有 $$n$$个元素，它的元素用对应的斜体小写字母加下标表示：$$x_1$$。一般我们将向量写成列向量的形式：

有时候我们需要得到一个向量的子集，例如得到第 1,3,6 个元素，那么我们可以令集合$$s=\{1,3,6\}$$，然后用 $$\mathbf x_{S}$$ 来表示这个子集。

另外，我们用 $$−$$ 的下标表示去集合的补：$$\mathbf x_{-1}$$ 表示除了 $$x_1$$ 之外 xx 中的所有元素，$$\mathbf x_{-S}$$ 表示除了 $$x_1, x_3, x_6$$ 之外 xx 中的所有元素。



### 矩阵

矩阵是一个二维数组，每个元素的下标有两个，通常用大写粗体字母表示：$$\mathbf A \in \mathbb R^{m\times n}$$ 表示取实值的 $$m$$行 $$n$$列矩阵，其元素用对应的小写斜体字母加下标表示：$$A_{1,1}, A_{m,n}$$。

另外，我们用 :: 下标表示矩阵的一行或者一列：$$\mathbf A_{i,:}$$ 第$$i$$ 行，$$\mathbf A_{:,j}$$ 第 $$j$$ 列。

矩阵可以写成这样的形式：

$$\begin{bmatrix}A_{1,1} & A_{1,2} \\A_{2,1} & A_{2,2} \\\end{bmatrix}$$

有时候我们需要对矩阵进行逐元素操作，例如将函数 $$f$$ 应用到$$A$$的所有元素上，此时我们用$$f({\bf A})_{i,j} $$表示。



### 张量

张量是超过二维的数组，我们用 $$A$$ 表示张量，$${\sf A}_{i,j,k}$$ 表示其元素（三维张量情况下）。



### 矩阵转置

矩阵的转置操作相当于沿着对角线翻转，定义为：

$$\mathbf A^\top_{i,j} = \mathbf A_{i,j}$$

矩阵转置的转置等于矩阵本身：

$$\bf \left(A^\top\right)^\top=A$$

转置将矩阵的形状从 $$m\times n$$ 变成了 $$n\times m$$。

向量可以看成是只有一列的矩阵，因此也可以进行转置操作，有时候为了方便，我们可以使用行向量加转置的操作，如：$$\mathbf x=[x_1,x_2,x_3]^\top$$

标量也可以看出是一行一列的矩阵，其转置等于自己：$$a^\top=a$$。



### 矩阵加法和数乘

加法即逐元素相加，要求两个矩阵的形状一样：

$$\mathbf{C = A+B}, C_{i,j}=A_{i,j}+B_{i,j}$$

数乘即一个标量与矩阵相乘：

$$\mathbf D = a \cdot \mathbf B + c, D_{i,j}=a \cdot B_{i,j}+c$$

有时候我们允许使用一个矩阵和向量相加的形式，得到一个矩阵，把 $$b$$加到了 $$C$$ 的每一行上，本质上是构造了一个将$$b$$ 按行复制的一个新矩阵，这种机制叫做 `broadcasting`：

$$\mathbf {C = A + b}, C_{i,j} = A_{i,j} + b_{j}$$



# 1.2 矩阵乘法

两个矩阵相乘得到第三个矩阵：

$$\bf C=AB$$

为了使得定义合法，我们需要 A 的形状为 $$m\times n$$, B 的形状为 $$n\times p$$，得到的矩阵为 C 的形状为 $$m\times p$$。

定义为

$$C_{i,j} = \sum_{k} A_{i,k}B_{k,j}$$

注意矩阵乘法不是逐元素相乘，逐元素相乘又叫 `Hadamard` 乘积，记作 $$\bf A\odot B$$。

向量可以看出是列为 11 的矩阵，两个相同大小的向量 $$\bf x, y$$ 的点乘（`dot product`）或者内积，可以使用矩阵乘法表示为$$\bf x^\top y$$。

我们也可以把矩阵乘法理解为： $$C_{i,j}$$ 表示 A 的第$$i$$行与 B 的第 $$j$$列的点乘。



### 矩阵乘法的性质

矩阵乘法满足结合律（`associative`）和分配律（`distributive`）：

$$\begin{align}\bf A(B+C)&=\bf AB+AC \\\bf A(BC)&=\bf (AB)C\end{align}$$



矩阵乘法通常是不可交换的：

$$\bf AB \neq BA$$

但是向量内积是可交换的：

$$\bf x^\top y = y^\top x$$

矩阵乘法的转置形式如下：

$$\bf (AB)^\top = B^\top A^\top$$

利用这个式子和标量转置等于其本身，我们马上得到内积是可交换的结论：

$$\bf x^\top y = (x^\top y)^\top = y^\top x$$



### 线性方程组

线性方程组可以表示为矩阵和向量乘法的形式：

$$\bf Ax = b$$

其中 $$\mathbf A\in\mathbb R^{m\times n}, \mathbf b\in\mathbb R^{m}$$ 是已知的，$$\mathbf x\in\mathbb R^{n}$$ 是我们要求的未知量。

它是线性方程组的一种紧凑表示：

$$\begin{align}\mathbf A_{1,:} \mathbf x&=b_1 \\\mathbf A_{2,:} \mathbf x&=b_2 \\\dots &\\\mathbf A_{m,:} \mathbf x&=b_m \\\end{align}$$



# 1.3 单位矩阵和逆

### 单位矩阵

为了引入矩阵的逆，我们需要先定义单位矩阵：单位矩阵乘以任意一个向量等于这个向量本身。记 $$\mathbf I_n $$ 为保持 $$n$$ 维向量不变的单位矩阵，即：

$$\mathbf I_n \in \mathbb R^{n\times n}, \forall \mathbf x \in \mathbb R^{n}, \mathbf I_n \mathbf{x=x}$$

单位矩阵的形式十分简单，所有的对角元素都为 1，其他元素都为 0，如：

$$\mathbf I_3\begin{bmatrix}1 & 0 & 0 \\0 & 1 & 0 \\0 & 0 & 1 \\\end{bmatrix}$$



### 矩阵的逆

矩阵 A 的逆记作 $$\mathbf A^{-1}$$，定义为一个矩阵使得

$$\mathbf A^{-1} \mathbf A = \mathbf I_n$$

如果 $$\mathbf A^{-1}$$ 存在，那么线性方程组 $$\bf Ax=b$$ 的解为：

$$\mathbf A^{-1}\mathbf{Ax} = \mathbf I_n \mathbf x = \mathbf x = \mathbf A^{-1}\mathbf b$$



# 1.4 线性无关和生成空间

如果 $$\mathbf A^{-1}$$存在，那么方程 $$\bf Ax=b$$对于任意 b 必须只有一个解$$ \mathbf A^{-1}\mathbf b$$。

如果对于某个 b 存在多解，那么一定是无穷多的解。 因为如果 x,y 是方程的解，那么 $$\forall \alpha \in \mathbb R, \mathbf z = \alpha \mathbf x + (1-\alpha)\mathbf y$$ 都是方程的解。



### 线性组合和生成空间

对于一组向量的集合 $$\{\mathbf v^{(1)}, \dots, \mathbf v^{(n)}\}$$，其线性组合为

$$\sum_{i} c_i \mathbf v^{(i)}$$

集合 $$\{\mathbf v^{(1)}, \dots, \mathbf v^{(n)}\}$$ 的生成空间（`span`）就是所有这些线性组合得到的向量组成的集合。

我们将 $$\mathbf{Ax}$$ 改写为

$$\mathbf{Ax}=\sum_{i}x_i \mathbf A_{:,i}$$

因此 b 可以看成是 A 的列向量组的线性组合（`linear combination`）。

为了使得所有的$$b\in \mathbb R^{m}$$ 都有解，那么 A 的列向量组的生成空间应当是全空间 $$\mathbb R^{m}$$。

为此，这些列向量组的个数 n 应当至少有 m 个，即 $$n \geq m$$。



### 线性无关

$$n \geq m$$ 只是必要条件而不是充分条件，因为这些列向量可以存在冗余。

这种冗余叫做线性相关性。

一组线性无关的向量组定义为：其中任何一个向量，不能使用其它向量的线性组合表示。

对于$$\mathbb R^{m}$$ 的向量组，其中线性无关的向量最多只有 m 个。

向一组向量中加入一个这组向量的线性组合，并不会向其生成空间中加入新的向量。

因此，为了使得所有的 $$b\in \mathbb R^{m}$$ 都有解，A 的列向量组应当有 m 个线性无关解。

为了使得方程只有一个解，我们需要限制列的个数，所以 $$n \geq m$$。

综合上面的结果，为了使得 $$\mathbf A^{-1}$$ 存在，我们需要有 A 满足 m=n 即方阵，以及所有的列向量都是线性无关的。

有线性相关的列向量的方阵叫做奇异的。

当然，如果  $$\mathbf A^{-1}$$ 不存在，线性方程组 $$\bf Ax=b$$ 还是可能有解的。

### 左逆和右逆

之前我们讨论的逆是矩阵的左逆，事实上，矩阵可以定义右逆为：

$$\mathbf{AA}^{-1} = \mathbf I$$

对于方阵，左逆和右逆是相等的。





# 1.5 范数

## 向量范数

### $$L^p$$范数

通常我们用范数（`norm`）来衡量向量，向量的 $$L_p$$ 范数定义为：

$$\|\mathbf x\|_{p} = \left(\sum_{i} |x_i|^p \right)^{\frac{1}{p}}, p \in \mathbb R, p \geq 1$$

除了$$L_p$$ 范数，范数还可以有很多定义方式，事实上，任何将向量映射为实数的函数 f(x)只要满足以下条件，都是合法的范数：

- 非负性：$$f(\mathbf x) \geq 0$$ ，且 $$f(\mathbf x) = 0 \Rightarrow \mathbf{x = 0}$$
- 三角不等式：$$f(\mathbf{x+y}) \leq f(\mathbf x) + f(\mathbf y)$$
- 数乘：$$\forall \alpha \in \mathbb R, f(\alpha\mathbf x) = |\alpha|f(\mathbf x)$$



### $$L^2$$范数

$$L^2$$ 范数，即通常所说的欧几里得范数（`Euclidean norm`），是向量 x 到坐标原点的欧几里得距离。因为它用的太广泛，所以我们通常省略其下标 2，将其记为 $$\|\mathbf x\|$$。

有时候也用 $$L^2$$ 范数的平方来衡量向量：$$\bf x^\top x$$。事实上，$$L^2$$范数的平方在计算上更为便利，例如它的对 x 梯度的各个分量只依赖于 x 的对应的各个分量，而$$L^2$$ 范数对 x 梯度的各个分量要依赖于整个 x 向量。

### 其他范数

$$L^2$$ 范数并不一定适用于所有的情况，它在原点附近的增长就十分缓慢，因此不适用于需要区别 0 和非常小但是非 0 值的情况。在这种情况下，$$L_1$$ 范数就是一个比较好的选择，它在所有方向上的增长速率都是一样的，定义为：

$$\|\mathbf x\|_1 = \sum_{i} |x_i|$$

它经常使用在需要区分 0 和非零元素的情形中。

有时候我们需要衡量向量中非零元素的个数，有些人将其称为“ $$L^0$$范数”，但是这并不严谨，因为**它并不是一个范数**（不满足三角不等式和数乘）。$$L_1$$ 范数可以作为它的一个替代。

另一个常使用的范数是 $$L^{\infty}  $$范数，它在数学上是向量元素绝对值的最大值，因此也被叫做 `max norm`：

$$\|\mathbf x\|_{\infty} = \max_i |x_i|$$

### 内积的范数形式

内积可以写成范数的形式，其中 $$\theta$$ 是两个向量的夹角：

$$\mathbf{x^\top y} = \mathbf{\|x\|\|y\|} \cos \theta$$

## 矩阵范数

有时候我们想衡量一个矩阵，机器学习中通常使用的是 `F` 范数（`Frobenius norm`），其定义为：

$$\|\mathbf A\|_F = \sqrt{\sum_{i,j} A_{i,j}^2}$$

或者叫做矩阵的 $$L^2$$ 范数。





# 1.6 特殊矩阵和向量

### 对角矩阵

对角阵（`diagonal matrix`）是一个主对角线之外的元素皆为 0 的矩阵。对角线上的元素可以为 0 或其他值。

如果矩阵 $$D$$ 是对角阵，则 $$D_{i,j} = 0 \forall i\neq j$$。

单位矩阵是对角阵。

对于 $$n\times n$$ 对角方阵，我们可以用主对角线上的元素将其表示为：$$\mathbf D = \text{diag}(\bf v)$$，表示 $$D_{i,i} = v_i$$。

对角方阵有很多好的性质：

- 与向量的乘法：$$\text{diag}\bf(v) x = v \odot x$$
- 逆：$$\text{diag}(\mathbf v)^{-1} = \text{diag}([1/v_1,\dots,1/v_n]^\top)$$

注意对角矩阵不一定是对角方阵。

### 对称矩阵

对称矩阵（`symmetric matrix`）满足转置操作下的不变性：

$$\bf A^\top=A$$

即 $$\forall i, j, A_{i,j} = A_{j, i}$$。

### 单位向量

模（$$L^2$$ 范数）为 1 的向量叫做单位向量（`unit vector`）：

$$\|\mathbf x\|_2 = 1$$

### 向量正交

两个向量的内积为 0 时，我们称这两个向量正交（`orthogonal`）：

$$\mathbf{x^\top y} = 0$$

### 单位正交

如果一组单位向量两两正交，称这组向量是单位正交的（`orthonormal`）。

### 正交矩阵

如果一个方阵的逆矩阵就是它的转置，那么我们称这个矩阵是正交矩阵（`orthogonal matrix`）：

$$\bf A^\top A=AA^\top = I$$