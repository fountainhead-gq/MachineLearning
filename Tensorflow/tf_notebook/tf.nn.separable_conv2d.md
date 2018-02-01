### tf.nn.separable_conv2d

`tf.nn.separable_conv2d`深度可分卷积可以看做是深度卷积`tf.nn.depthwise_conv2`d的扩展。
```python
tf.nn.separable_conv2d(input,depthwise_filter,pointwise_filter,strides,padding,rate=None,name=None,data_format=None)
```

`data_format`指定数据格式，与方法有关的一共六个参数：
- `input`: 指需要做卷积的输入图像，要求是一个4维Tensor，具有`[batch, height, width, in_channels]`这样的shape，具体含义是[图片数量, 图片高度, 图片宽度, 图像通道数]
- `depthwise_filter`： 用来做`depthwise_conv2d`的卷积核，也就是说这个函数对输入首先做了一个深度卷积。它的shape规定是`[filter_height, filter_width, in_channels, channel_multiplier]`
- `pointwise_filter`： 用来做pointwise卷积的卷积核，我们可以把它和GoogLeNet最原始版本Inception结构中后面的1*1卷积核做channel降维来做对比，这里也是用1*1的卷积核，输入通道是`depthwise_conv2d`的输出通道也就是`in_channels * channel_multiplier`，输出通道数可以自己定义。`depthwise_conv2d`是对输入图像的每一个channel分别做卷积输出的，那么这个操作我们可以看做是将深度卷积得到的分离的各个channel的信息做一个融合。它的shape规定是`[1, 1, channel_multiplier * in_channels, out_channels]`
- `strides`： 卷积的滑动步长。
- `padding`： string类型的量，只能是`”SAME”,”VALID”`其中之一，这个值决定了不同边缘填充方式。
- `rate`： 见`tf.nn.atrous_conv2d`  
输出shape为`[batch, out_height, out_width, out_channels]`的Tensor

```python
import tensorflow as tf
img1 = tf.constant(value=[[[[1],[2],[3],[4]],[[1],[2],[3],[4]],[[1],[2],[3],[4]],[[1],[2],[3],[4]]]],
                   dtype=tf.float32)
img2 = tf.constant(value=[[[[1],[1],[1],[1]],[[1],[1],[1],[1]],[[1],[1],[1],[1]],[[1],[1],[1],[1]]]],dtype=tf.float32)
img = tf.concat(values=[img1,img2],axis=3)
filter1 = tf.constant(value=0, shape=[3,3,1,1],dtype=tf.float32)
filter2 = tf.constant(value=1, shape=[3,3,1,1],dtype=tf.float32)
filter3 = tf.constant(value=2, shape=[3,3,1,1],dtype=tf.float32)
filter4 = tf.constant(value=3, shape=[3,3,1,1],dtype=tf.float32)
filter_out1 = tf.concat(values=[filter1,filter2],axis=2)
filter_out2 = tf.concat(values=[filter3,filter4],axis=2)
filter = tf.concat(values=[filter_out1,filter_out2],axis=3)

dw_conv_out = tf.nn.depthwise_conv2d(input=img, filter=filter, strides=[1,1,1,1], rate=[1,1], padding='VALID')
# conv_out = tf.nn.conv2d(input=img, filter=filter, strides=[1,1,1,1], padding='VALID')


point_filter = tf.constant(value=1, shape=[1,1,4,4],dtype=tf.float32)
conv_out2 = tf.nn.conv2d(input=dw_conv_out, filter=point_filter, strides=[1,1,1,1], padding='VALID')

# separable_conv2d等价conv2d和depthwise_conv2d
sep_conv_out = tf.nn.separable_conv2d(input=img, depthwise_filter=filter, pointwise_filter=point_filter,
                                      strides=[1,1,1,1], rate=[1,1], padding='VALID')  

with tf.Session() as sess:
    print(sess.run(sep_conv_out))
    print(sess.run(conv_out2))

# [[[[ 72.  72.  72.  72.]
#    [ 90.  90.  90.  90.]]
#   [[ 72.  72.  72.  72.]
#    [ 90.  90.  90.  90.]]]]

# [[[[ 72.  72.  72.  72.]
#    [ 90.  90.  90.  90.]]
#   [[ 72.  72.  72.  72.]
#    [ 90.  90.  90.  90.]]]]
```    