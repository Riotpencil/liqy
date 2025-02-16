何凯明
非常深（152 / 1000 layers），但是复杂度更低

`CIFAR-10` 数据集
物体检测 `COCO` 数据集

CVPR 每篇文章不能超过 8 页，没有结论
计算机视觉论文图片放第一页，图形学图片放在标题上面（CMU Randy）

训练很深的神经网络困难：layers 更多 error 反而更高
	使用 `ResNet` 后可以训练比较深的模型

### Introduction
不同的 level 可以得到不同的 feature

网络比较深的时候梯度爆炸或者消失
	初始化权重合适
	中间 batch normalization (intermediate normalization) BN 校验每个层的输出和 var

训练误差变高，而不是 overfitting

==深的网络按理说不应该比浅的差，大不了 identity mapping，但是 SGD 找不到==

`F(x) = H(x) - x; Output = F(x) + x`
输出加上了输入
shortcut connections
	90s 已经提出
非常简单，在 Caffe 里不用改代码可以直接实现

技术并非原创，旧的技术有新的应用也很重要
Residual 在机器学习中比较常见，如线形模型迭代求解、gradient boosting（将弱的分类器叠加为强的分类器，标号上做 residual，本文在 feature 上）GBDT
	有点像拉格朗日对偶的弱对偶定理？

输入和输出尺寸不匹配
- extra zeros
- 投影：1\*1 convolution 
	- 空间维度不变，通道维度改变 
	- 输出通道是输入通道的两倍，输入的高和宽减半，使用 stride 为 2
	- 看代码
	- 方案C：每一个都投影，即使形状匹配，效果好但是比较贵
增强随机性：随机放置再切割 224
颜色增强（PS），而不是 PCA
学习率下降和 AlexNet 一样，现在不用了

iteration 次数不用写，因为和 batch size 相关
没有用 dropout ，因为没有全连接层

@刷榜技巧
10-crop testing 降低方差
融合

`FLOPs` 计算


训练精度一开始比测试精度高：训练的时候用到了大量的数据增强
明显下降：学习率乘以 0.1（微调，晚一点跳一开始方向找的准对后期比较好）

构建更深的 ResNet
降低计算复杂度
	卷积来回投影
	最后的输出是 256，因为要匹配

看一下后面的层有没有用上

梯度保持的比较好：ResNet 引入了浅层的梯度，误差反传

SGD 收敛没有意义，可能是训练不动了
	“SGD 的精髓是一直能跑得动”

其他观点
为什么 `SIFAR` 1000 layers 过拟合没有很严重
虽然层数增加，但是模型内在复杂度降低；模型引导


```math 
x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a} 
```

