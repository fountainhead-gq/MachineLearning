### depthwise_conv2d

`depthwise_conv2d`来源于深度可分离卷积:  [Xception: Deep Learning with Depthwise Separable Convolutions](https://arxiv.org/abs/1610.02357)

```python
tf.nn.depthwise_conv2d(input,filter,strides,padding,rate=None,name=None,data_format=None)
```
除去`name`参数用以指定该操作的名称，`data_format`指定数据格式，与方法有关的一共五个参数：
- `input`： 指需要做卷积的输入图像，要求是一个4维Tensor，具有`[batch, height, width, in_channels]`这样的shape，具体含义是[训练时一个batch的图片数量, 图片高度, 图片宽度, 图像通道数] 
- `filter`： 相当于CNN中的卷积核，要求是一个4维Tensor，具有`[filter_height, filter_width, in_channels, channel_multiplier]`这样的shape，具体含义是[卷积核的高度，卷积核的宽度，输入通道数，输出卷积乘子]
- `strides`： 卷积的滑动步长。 
- `padding`： string类型的量，只能是`”SAME”,”VALID”`其中之一，这个值决定了不同边缘填充方式。
- `rate`：  见`tf.nn.atrous_conv2d`  
  结果返回一个Tensor，shape为`[batch, out_height, out_width, in_channels * channel_multiplier]`，注意这里输出通道变成了`in_channels * channel_multiplier`

### 示例

现在我们可以形象的解释一下`depthwise_conv2d`卷积了。看普通的卷积，我们对卷积核每一个`out_channel`的两个通道分别和输入的两个通道做卷积相加，得到feature map的一个channel   
![conv2d_1](https://github.com/fountainhead-gq/MachineLearning/blob/master/Tensorflow/images/conv2d_1.png)
![conv2d_2](https://github.com/fountainhead-gq/MachineLearning/blob/master/Tensorflow/images/conv2d_2.png)

而`depthwise_conv2d`卷积，我们对每一个对应的`in_channel`，分别卷积生成两个`out_channel`，所以获得的feature map的通道数量可以用`in_channel* channel_multiplier`来表达，这个`channel_multiplier`，就可以理解为卷积核的第四维。
![depthwise_conv2d_1](https://github.com/fountainhead-gq/MachineLearning/blob/master/Tensorflow/images/depthwise_conv2d_1.png)
![depthwise_conv2d_2](https://github.com/fountainhead-gq/MachineLearning/blob/master/Tensorflow/images/depthwise_conv2d_2.png)

```python
import tensorflow as tf

img1 = tf.constant(value=[[[[1],[2],[3],[4]],[[1],[2],[3],[4]],[[1],[2],[3],[4]],[[1],[2],[3],[4]]]]
                   dtype=tf.float32)
img2 = tf.constant(value=[[[[1],[1],[1],[1]],[[1],[1],[1],[1]],[[1],[1],[1],[1]],[[1],[1],[1],[1]]]],
                   dtype=tf.float32)
img = tf.concat(values=[img1,img2],axis=3)

filter1 = tf.constant(value=0, shape=[3,3,1,1],dtype=tf.float32)
filter2 = tf.constant(value=1, shape=[3,3,1,1],dtype=tf.float32)
filter3 = tf.constant(value=2, shape=[3,3,1,1],dtype=tf.float32)
filter4 = tf.constant(value=3, shape=[3,3,1,1],dtype=tf.float32)
filter_out1 = tf.concat(values=[filter1,filter2],axis=2)
filter_out2 = tf.concat(values=[filter3,filter4],axis=2)
filter = tf.concat(values=[filter_out1,filter_out2],axis=3)

dw_conv_out = tf.nn.depthwise_conv2d(input=img, filter=filter, strides=[1,1,1,1], rate=[1,1], padding='VALID')
conv_out = tf.nn.conv2d(input=img, filter=filter, strides=[1,1,1,1], padding='VALID')

with tf.Session() as sess:
    print(sess.run(conv_out))
    print(sess.run(dw_conv_out))
    
# [[[[  9.  63.]
#    [  9.  81.]]
#   [[  9.  63.]
#    [  9.  81.]]]]

# [[[[  0.  36.   9.  27.]
#    [  0.  54.   9.  27.]]
#   [[  0.  36.   9.  27.]
#    [  0.  54.   9.  27.]]]]
```
