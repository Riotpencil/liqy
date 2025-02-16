由于比较原始，不需要详尽细节
### 摘要（技术报告）
`ImageNet` 经典数据集，数据量大类别多
	2012 年后停止公布测试集（`test` , 区分于 `val` ）
	比赛 120M ，完整 890M（效果更好），1W类
	标号错误率 1% 左右

参数众多，显著多于SVM
	60 million parameters and 650,000 neurons, consists of **five convolutional layers**, some of which are followed by max-pooling layers, and **three fully-connected layers** with **a final 1000-way softmax**

GPU实现，当时已经有matlab GPU加速包

`dropout` 方法首次提出

### 讨论
结尾有 discussion 但是没有 conclusion

深度是很重要的，但是拿掉一层不能说明（可能是参数没设好）；宽度也很重要

`supervised learning` , 需要标注
不需要 `unsupervised` 预训练（无标号预热，把权重确定在相对好的范围)
	最近几年 `BERT` `GAN` 又为 usl 带来热度
	在 Alex_Net 之前，usl 效果和SVM差不多，效果不如 sl

video 数据训练现在依然很难，因为数据量大和版权

### 图片
ImageNet 分辨率不一样，作者将图片裁为 256\*256 大小

在 raw_RGB_value 上训练
	有 `SIFT` 版本的特征，但是作者在原始的像素上做

将倒数第二层输出拿出来（向量），找出最近的，进行聚类
	深度学习的强项：图片训练出来的向量在语义空间里的表示非常好

### Introduction
深度学习使用很大的模型，通过正则 `Regularization`避免过拟合
	正则不是最重要的，神经网络的设计才是最重要的

CNN 做大不容易，容易 overfitting 或者训练不动，并非当时主流

GPU(3G) 上训练，多个，需要将网络切开，
	很大的工程量，但是后续来说工程细节并不重要

即使结果很好，工程性太多不容易获得高引用率，关键是有创新

### Architecture
Hinton 首次提出`ReLU` , 原来用的比较多的是 `tanh` `Sigmoid`
	`f(x) = max(0,x)`
	训练更快，但是没有快很多，现在用的多是因为足够简单

`ReLU` 不需要更多的 input normalization 来避免饱和
	现在有更好的 normalization 技术

overlapping pooling
	一般来说不重叠

##### 卷积层
中间有输出通道维度合并
压缩空间信息，增加通道数（感受野）
##### 全连接层
最后用线性分类器连接

##### 数据增强
256 中扣 224 增加样本量
RGB 通道用 PCA 进行改变


##### Dropout
随机将隐藏层输出设为 50% 的概率为 0 
等价为 L2 正则项（另一篇文章）
	防止过拟合，但是降低训练速度
	现在的 `CNN` 没有这么大的全连接，所以 dropout 不是很重要
全连接 `RNN` `Attention` 用的比较多

### 训练
`SGD` 调参麻烦，但是噪音对于模型训练有好处，而且速度快

weight decay
	L2 正则项

现在用一个简单的 `momentum` 也不错

权重用 `EX = 0, Var = 0.01` 高斯函数初始化
	`BERT` 用的 0.02

2/4/5卷积层和全连接层偏移量初始化为1（玄学？）

初始每个层用 0.01 的学习率，手动调整
	可以规则性地降低学习率，但是现在很少用
	可以用更平滑的曲线，如 `cos`

GPU_1 上学习到的和颜色无关，而 GPU_2 的与颜色相关
	公平性，神经网络的偏移


