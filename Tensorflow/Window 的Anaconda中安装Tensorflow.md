# Window的Anaconda中安装Tensorflow

## 安装Anaconda

Anaconda 安装包下载`https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/`,然后安装。

安装完以后，打开Anaconda Prompt，执行：
```python
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```


## 安装TensorFlow
一定要安装64位的大于等于3.5.2的Python版本。
#### 打开Anaconda Prompt，输入：
`conda create -n tensorflow python=3.5`

#### 安装完以后，并激活：
`activate tensorflow`

#### 1.选择安装CPU版本，输入:
```
pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/cpu/tensorflow-0.12.0-cp35-cp35m-win_amd64.whl
```

#### 2.如果安装GPU版本：  
先下载CUDA8.0,`https://developer.nvidia.com/cuda-downloads`，然后安装。    
下载cuDNN for CUDA 8.0，`https://developer.nvidia.com/cudnn`,解压cuDNN，拷贝`lib, include, bin`文件夹到CUDA的安装目录下,如` D:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0`    
再执行，`pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-0.12.0rc0-cp35-cp35m-win_amd64.whl`  ，验证是否安装成功。  
最后，Windows环境下本地直接安装的方法同上。

#### 依次安装`ipython`、`ipykernel`:
```
(tensorflow) C:\Users>conda install ipython
(tensorflow) C:\Users>conda install notebook ipykernel
(tensorflow) C:\Users>ipython kernel install --user --name=python3.5
```
#### 运行代码：  
`(tensorflow) C:\Users>jupyter notebook`

#### 退出环境：
`deactivate`
