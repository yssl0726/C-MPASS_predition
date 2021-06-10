# 基于深度学习的航空发动机剩余寿命预测
数据集使用NASA开源的C-MAPSS进行实验

这是一个应用深度学习的方法来对航空发动机的剩余寿命预测的问题，共有4个子数据集，每个子数据集都包含发动机整个运行期间采样得到的传感器数据；

由于当时实验室没有设备，所以代码都是在	Google Colab 上面的GPU进行试验的，所以两个文件都是以 ipynb 文件呈现。

首先 /data/dataprocessing.ipynb 是对原始数据进行一些特征选择和特征工程的操作，把处理生成的数据存放到 ./processing_data 中。

主要内容：

1.通过EDA剔除一些故障的传感器（特征）；

2.滑动时间窗构造片段数据；

3.使用逻辑回归学习每个特征的特征值与时间之间的线性相关系数（斜率）以及每个特征沿时序上求均值，作为片段数据的时序扩充；

4.对数据进行MinMaxScaler()；

然后 /data/VAE_PATCN.ipynb 包含深度学习的模型、训练的内容，训练过程中产生或保存的一些文件都存放到 ./model 中。

![proposed_method](./picture/proposed_method.png)

主要内容：

1.使用变分自编码器（VAE）进行预训练，然后使用编码器部分进行特征提取，使得特征由14维 -> 10维；

2.基于注意力机制的并行时间卷积网络（PATCN），分别从特征维度和时间维度提取数据中的抽象信息，最后进行concat，把融合之后的
信息通过全连接层进行学习，最后得到RUL预测值。

3.PA-TCN中涉及了对残差连接、注意力机制、same一维卷积、扩张因果（casual）一维卷积、特征图之间的一维卷积。
