### UNet
GitHub里的UNet
https://github.com/milesial/Pytorch-UNet

kaggle数据集
2017年的汽车图像分割比赛，数据大约为5000张图片
https://www.kaggle.com/competitions/carvana-image-masking-challenge

论文里提到的脑肿瘤分割数据集（BraTS）
https://www.kaggle.com/datasets/awsaf49/brats20-dataset-training-validation/code


#### 训练效果
用的汽车数据集
我按默认参数训练了5个Epoch。每个Epoch中batchsize为20%，会进行5次iteration
训练log（有DICE得分）
```
INFO: Starting training:
        Epochs:          5
        Batch size:      1
        Learning rate:   1e-05
        Training size:   4580
        Validation size: 508
        Checkpoints:     True
        Device:          cuda
        Images scaling:  0.5
        Mixed Precision: False

E:\Pytorch-UNet-master\train.py:80: FutureWarning: `torch.cuda.amp.GradScaler(args...)` is deprecated. Please use `torch.amp.GradScaler('cuda', args...)` instead.
  grad_scaler = torch.cuda.amp.GradScaler(enabled=amp)
Epoch 1/5:   0%|          | 0/4580 [00:00<?, ?img/s]wandb: Network error (ProxyError), entering retry loop.
Epoch 1/5:  20%|██        | 916/4580 [05:53<18:41,  3.27img/s, loss (bINFO: Validation Dice score: 0.8178815841674805
wandb: Network error (ProxyError), entering retry loop.                
Epoch 1/5:  40%|████      | 1832/4580 [14:54<14:00,  3.27img/s, loss (INFO: Validation Dice score: 0.935808539390564
Epoch 1/5:  60%|██████    | 2748/4580 [21:13<09:20,  3.27img/s, loss (INFO: Validation Dice score: 0.9368261098861694
Epoch 1/5:  80%|████████  | 3664/4580 [27:31<04:39,  3.27img/s, loss (INFO: Validation Dice score: 0.9292181134223938
Epoch 1/5: 100%|██████████| 4580/4580 [33:50<00:00,  3.27img/s, loss (INFO: Validation Dice score: 0.9473786950111389
Epoch 1/5: 100%|██████████| 4580/4580 [35:31<00:00,  2.15img/s, loss (batch)=0.038]
INFO: Checkpoint 1 saved!
Epoch 2/5:  20%|██        | 916/4580 [05:52<18:41,  3.27img/s, loss (bINFO: Validation Dice score: 0.9624014496803284
Epoch 2/5:  40%|████      | 1832/4580 [12:11<14:00,  3.27img/s, loss (INFO: Validation Dice score: 0.9716830253601074
Epoch 2/5:  60%|██████    | 2748/4580 [18:28<09:10,  3.33img/s, loss (INFO: Validation Dice score: 0.9792711734771729
Epoch 2/5:  80%|████████  | 3664/4580 [24:41<04:35,  3.33img/s, loss (INFO: Validation Dice score: 0.9665918350219727
Epoch 2/5: 100%|██████████| 4580/4580 [4:01:45<00:00,  3.24img/s, lossINFO: Validation Dice score: 0.9786570072174072
Epoch 2/5: 100%|██████████| 4580/4580 [4:03:28<00:00,  3.19s/img, loss (batch)=0.0259]
INFO: Checkpoint 2 saved!
Epoch 3/5:  20%|██        | 916/4580 [05:54<18:51,  3.24img/s, loss (bINFO: Validation Dice score: 0.9811988472938538
Epoch 3/5:  40%|████      | 1832/4580 [12:14<13:54,  3.29img/s, loss (INFO: Validation Dice score: 0.9822417497634888
Epoch 3/5:  60%|██████    | 2748/4580 [18:31<09:16,  3.29img/s, loss (INFO: Validation Dice score: 0.9845955967903137
Epoch 3/5:  80%|████████  | 3664/4580 [5:44:19<04:43,  3.23img/s, lossINFO: Validation Dice score: 0.9488869309425354
Epoch 3/5: 100%|██████████| 4580/4580 [5:50:41<00:00,  3.30img/s, lossINFO: Validation Dice score: 0.9573242664337158
Epoch 3/5: 100%|██████████| 4580/4580 [5:52:18<00:00,  4.62s/img, loss (batch)=0.00995]
INFO: Checkpoint 3 saved!
Epoch 4/5:  20%|██        | 916/4580 [06:22<19:33,  3.12img/s, loss (bINFO: Validation Dice score: 0.9694526195526123
Epoch 4/5:  40%|████      | 1832/4580 [12:46<15:04,  3.04img/s, loss (INFO: Validation Dice score: 0.9422013163566589
Epoch 4/5:  60%|██████    | 2748/4580 [19:09<09:26,  3.24img/s, loss (INFO: Validation Dice score: 0.9814373850822449
Epoch 4/5:  80%|████████  | 3664/4580 [25:30<04:42,  3.25img/s, loss (INFO: Validation Dice score: 0.9753067493438721
Epoch 4/5: 100%|██████████| 4580/4580 [31:52<00:00,  3.24img/s, loss (INFO: Validation Dice score: 0.9903499484062195
Epoch 4/5: 100%|██████████| 4580/4580 [33:32<00:00,  2.28img/s, loss (batch)=0.00994]
INFO: Checkpoint 4 saved!
Epoch 5/5:  20%|██        | 916/4580 [05:55<18:49,  3.24img/s, loss (bINFO: Validation Dice score: 0.9867383241653442
Epoch 5/5:  40%|████      | 1832/4580 [12:16<14:06,  3.25img/s, loss (INFO: Validation Dice score: 0.9899384379386902
Epoch 5/5:  60%|██████    | 2748/4580 [18:38<09:25,  3.24img/s, loss (INFO: Validation Dice score: 0.9891010522842407
Epoch 5/5:  80%|████████  | 3664/4580 [25:01<04:43,  3.23img/s, loss (INFO: Validation Dice score: 0.992168664932251
Epoch 5/5: 100%|██████████| 4580/4580 [31:24<00:00,  3.23img/s, loss (INFO: Validation Dice score: 0.9891524910926819
Epoch 5/5: 100%|██████████| 4580/4580 [33:05<00:00,  2.31img/s, loss (batch)=0.0117]
INFO: Checkpoint 5 saved!

```
训练数据里的图像
![[Pasted image 20241107072446.png]]
从左到右依次为：原始图像、本次生成图像、原始训练样例

我又在网上随机找了两张汽车照片，非常不robust
![[Pasted image 20241107073416.png|400]]
可能是训练数据不足或者形式单一导致的

#### 总结与反思：
1. 虚拟环境里包可能要重安一遍（后来我用的系统解释器，需要管理员权限添加包）
2. 注意是不是用的GPU，程序里写的是如果没有GPU就用CPU。一开始没有安装CUDA所以就用的CPU，开任务管理器发现的（CPU占用80%多，显卡没用上）。或者可以看终端输出。用CUDA后显卡能跑满![[e5143526779b9fade13d12071533c16.png]]
3. 运行的时候不能开梯子不知道为什么
4. 中间报错内存不足，我又给了32g的虚拟内存
5. 每个Epoch训练要花50分钟左右
6. 这个是单模态的，之后肯定要用多模态，这个玩玩就好
### Docker
##### 概念
掉包的时候我顺便了解了一下docker
镜像和容器
docker仓库：Dockerhub
不需要麻烦的环境配置，根据docker镜像直接可以复现
##### 使用
Dockerfile D要大写

Docker官方教程
https://docs.docker.com/get-started/get-docker/

Docker拉取镜像回遇到网络问题，连了梯子也不好使，好像国内不让用Dockerhub
可以尝试手动拉取
~~~
docker pull node:20
~~~

跟着教程调包了个list小网站，各种参数都可以修改，前端用的vite
	直接改文件保存，网站刷新就可以看到变化
![[Pasted image 20241106113800.png]]
更深入的以后再学吧

## 论文摘要
### 基于深度学习的多模态医学图像分割综述_窦猛
##### 数据集
脑肿瘤分割（Brain Tumor Segmentation， BraTS）
缺血性中风病变分割（Ischemic Stroke LEsion Segmentation， ISLES）
MR 脑图像分割（MR Brain image Segmentation，MRBrainS）
新生儿脑分割（Neonatal BrainSegmentation， NeoBrainS
组合（CT-MR）腹部器官分割（Combined CT-MR Healthy Abdominal Organ
Segmentation， CHAOS）
婴儿脑MRI 分割（6-monthInfant brain MRI segmentation， Iseg）
多模态MR图像自动椎间盘定位分割（automatic Inter Vertebral Disc localization and segmentation from 3D MR images， IVDM3Seg）
##### 方法分类
2D
- 以3D 图像抽取出的2D 图像为输入，利用2D卷积核提取出图像空间上下文信息但忽略了Z 轴上的空间信息
- 先在水平位、冠状位及矢状位上对图像进行2D 特征提取，再将2D 特征作为3D CNN 模型的附加输入
3D
- 直接
- 3D patch 拼接
##### 图像处理
N4ITK

**多模态图像分割**：在分割过程中使用多个模态的图像输入，将每种模态的特征信息融合在一起，以提高模型对特定组织或病变区域的分割效果
	MRI
	- **T1ce**：注射对比剂后，病灶区域更亮，适用于肿瘤、炎症等的检测
	- **T2w**：适合显示水肿、炎症等含水量高的病变，水亮、脂肪暗
	- **FLAIR**：适合检测白质病变，抑制液体信号，病变区域突出

损失函数 (以后再了解)
1. 交叉熵损失（Cross-Entropy loss）
2. 加权交叉熵损失（Weighted Cross-Entropy loss， WCE）
3. 骰子损失（DICE loss）
4. 焦点损失（Focal loss，FL）

数据后处理（略）
##### 融合策略
![[Pasted image 20241109092549.png]]
现有的多模态医学图像分割网络大多为输入级融合网络，直接将多模态图像在原始输入空间融合
FCNN、UNet

基于Transformer: Vision Transformer (ViT), Swin Transformer

对于中间级（特征级）融合，典型的融合策略往往以DenseNet作为基础网络

网络结构搜索（Network Architecture Search， NAS）允许网络从预定义好的网络空间中学习出最优的子空间，如果将NAS 技术与多模态图像融合技术相结合，使模型可以自动学习最优的图像融合方式将是一个极富前景的探索方向
##### 个人感想
- 对于基本的深度学习概念如GAN、CNN、transform等缺乏，下阶段主要任务是学习
- 之后可以在融合策略上作文章，先把UNet网络结构和代码实现搞清楚，再找几篇初级融合策略的文章复现，最后尝试提出新的策略
- 这篇文章是去年的，最新的进展也要关注

