####  tf.reduce_XXX()

`reduce_XXX()`这些操作的本质就是降维，以xxx的手段降维。
在所有`reduce_xxx`系列操作中，都有`reduction_indices`这个参数，即沿某个方向，使用xxx方法，对`input_tensor`进行降维。
`reduction_indices`的默认值时None，即把`input_tensor`降到 0维，也就是一个数。
对于2维`input_tensor`，`reduction_indices=0`时，按列；`reduction_indices=1`时，按行。

- reduce_mean:
```python
For example:
# 'x' is [[1., 1.]
#         [2., 2.]]
tf.reduce_mean(x) ==> 1.5
tf.reduce_mean(x, 0) ==> [1.5, 1.5]
tf.reduce_mean(x, 1) ==> [1.,  2.]
```
- reduce_sum:
```python
# 'x' is [[1, 1, 1]
#         [1, 1, 1]]
tf.reduce_sum(x) ==> 6
tf.reduce_sum(x, 0) ==> [2, 2, 2]
tf.reduce_sum(x, 1) ==> [3, 3]
tf.reduce_sum(x, 1, keep_dims=True) ==> [[3], [3]]
tf.reduce_sum(x, [0, 1]) ==> 6
```



在神经网络中，我们有很多的非线性函数来作为激活函数，比如连续的平滑非线性函数（sigmoid，tanh和softplus），连续但不平滑的非线性函数（relu，relu6和relu_x）和随机正则化函数（dropout）。所有的激活函数都是单独应用在每个元素上面的，并且输出张量的维度和输入张量的维度一样。  

`tf.nn.relu(features, name = None)`  解释：这个函数的作用是计算激活函数`relu`，即`max(features, 0)`。   
`tf.nn.softplus(features, name = None)` 解释：这个函数的作用是计算激活函数`softplus`，即`log( exp( features ) + 1)`。  
`tf.nn.dropout(x, keep_prob, noise_shape = None, seed = None, name = None)`  解释：这个函数的作用是计算神经网络层的`dropout`。  
`tf.nn.bias_add(value, bias, name = None)` 解释：这个函数的作用是将偏差项 bias 加到 value 上面。   
`tf.sigmoid(x, name = None)`  解释：这个函数的作用是计算 x 的 sigmoid 函数。具体计算公式为 `y = 1 / (1 + exp(-x))`。  

#### 卷积层

卷积操作是使用一个二维的卷积核在一个批处理的图片上进行不断扫描。具体操作是将一个卷积核在每张图片上按照一个合适的尺寸在每个通道上面进行扫描。为了达到好的卷积效率，需要在不同的通道和不同的卷积核之间进行权衡。

- `conv2d`: 任意的卷积核，能同时在不同的通道上面进行卷积操作。
- `depthwise_conv2d`: 卷积核能相互独立的在自己的通道上面进行卷积操作。
- `separable_conv2d`: 在纵深卷积 `depthwise filter` 之后进行逐点卷积` separable filter` 。  

卷积核的卷积过程是按照 strides 参数来确定的，比如 `strides = [1, 1, 1, 1]` 表示卷积核对每个像素点进行卷积，即在二维屏幕上面，两个轴方向的步长都是1。`strides = [1, 2, 2, 1]` 表示卷积核对每隔一个像素点进行卷积，即在二维屏幕上面，两个轴方向的步长都是2。  

对于输出数据的维度` shape(output)`，这取决于填充参数 `padding` 的设置：

- `padding = 'SAME'`: 向下取舍，仅适用于全尺寸操作，即输入数据维度和输出数据维度相同。
- `padding = 'VALID'`: 向上取舍，适用于部分窗口，即输入数据维度和输出数据维度不同。  

`tf.nn.conv2d(input, filter, strides, padding, use_cudnn_on_gpu=None, name=None)` 解释：这个函数的作用是对一个四维的输入数据 input 和四维的卷积核 filter 进行操作，然后对输入数据进行一个二维的卷积操作，最后得到卷积之后的结果。  
输入参数：
- `input`: 一个`Tensor`。数据类型必须是`float32`或者`float64`。
-` filter`: 一个Tensor。数据类型必须是input相同。
- `strides`: 一个长度是4的一维整数类型数组，每一维度对应的是 input 中每一维的对应移动步数，比如，`strides[1]` 对应 `input[1]` 的移动步数。
-` padding`: 一个字符串，取值为 `SAME` 或者 `VALID` 。
- `use_cudnn_on_gpu`: 一个可选布尔值，默认情况下是 `True` 。
- `name`: （可选）为这个操作取一个名字。  

输出参数：
一个`Tensor`，数据类型是` input` 相同。

```python
input_data = tf.Variable( np.random.rand(10,6,6,3), dtype = np.float32 )
filter_data = tf.Variable( np.random.rand(2, 2, 3, 1), dtype = np.float32)
y = tf.nn.conv2d(input_data, filter_data, strides = [1, 1, 1, 1], padding = 'SAME')
```

#### 池化层

池化操作是利用一个矩阵窗口在输入张量上进行扫描，并且将每个矩阵窗口中的值通过取最大值，平均值或者`XXXX`来减少元素个数。每个池化操作的矩阵窗口大小是由 `ksize` 来指定的，并且根据步长参数 `strides` 来决定移动步长。比如，如果 `strides` 中的值都是1，那么每个矩阵窗口都将被使用。如果 `strides` 中的值都是2，那么每一维度上的矩阵窗口都是每隔一个被使用。以此类推

`tf.nn.avg_pool(value, ksize, strides, padding, name=None)` 解释：这个函数的作用是计算池化区域中元素的平均值。  
`tf.nn.max_pool(value, ksize, strides, padding, name=None)` 解释：这个函数的作用是计算池化区域中元素的最大值。
`tf.nn.max_pool_with_argmax(input, ksize, strides, padding, Targmax = None, name=None)` 解释：这个函数的作用是计算池化区域中元素的最大值和该最大值所在的位置。

#### 规则化

规则化是能防止模型过拟合的好方法。特别是在大数据的情况下。

`tf.nn.l2_normalize(x, dim, epsilon=1e-12, name=None)` 解释：这个函数的作用是利用 L2 范数对指定维度 `dim` 进行标准化。

#### 误差值

度量两个张量或者一个张量和零之间的损失误差，这个可用于在一个回归任务或者用于正则的目的（权重衰减）。
`tf.nn.l2_loss(t, name=None)` 解释：这个函数的作用是利用 L2 范数来计算张量的误差值，但是开方并且只取 L2 范数的值的一半，具体如下：`output = sum(t ** 2) / 2`

#### 分类操作

`Tensorflow`提供了操作，能帮助你更好的进行分类操作。

`tf.nn.sigmoid_cross_entropy_with_logits(logits, targets, name=None)` 解释：这个函数的作用是计算 `logits` 经 `sigmoid` 函数激活之后的交叉熵。
`tf.nn.softmax(logits, name=None)` 解释：这个函数的作用是计算 softmax 激活函数。对于每个批 i 和 分类 j，我们可以得到：`softmax[i, j] = exp(logits[i, j]) / sum(exp(logits[i])) `
`tf.nn.softmax_cross_entropy_with_logits(logits, labels, name=None)` 解释：这个函数的作用是计算` logits` 经 `softmax` 函数激活之后的交叉熵。



