# Contrastive Learning

> 参考 [李沐论文精读系列三：MoCo、对比学习综述（MoCov1/v2/v3、SimCLR v1/v2、DINO等）](https://blog.csdn.net/qq_56591814/article/details/127564330)

* 对比学习是无监督学习的一种，着重于学习同类实例之间的共同特征，区分非同类实例之间的不同之处。
* 对比学习希望相似数据最终学到的特征是相似的，在特征空间（embedding space）中，特征向量尽量靠近；反之还希望不同的数据学到的特征向量尽量远离。
* **NCE loss（noise contrastive estimation ）：** 将超级多分类转为二分类——数据类别data sample和噪声类别noisy sample。这样解决了类别多的问题。

## NCE Loss

NCE Loss: Noise Contrastive Estimation  
直观想法：**将多分类问题转化为二分类**

## [MoCo](https://arxiv.org/abs/1911.05722v3)

MoCo虽然是基于对比学习的，但是本文是从另外一个角度来看对比学习，即把对比学习看作是一个**字典查询任务**。

* **pretext task（代理任务）:** 对比学习是不需要标签的（比如不需要知道图片是哪一类），但模型还是需要知道哪些图片是类似的，哪些是不相似的，才能训练。这就需要通过通过设计一些巧妙的代理任务，人为指定一些任务来实现。
  * 简单说就是，从一堆图片中调出任意一张图片x_i，将其做一次转换（transformation ，比如随机裁剪等数据增广），得到新的图片x_i1、x_i2。那么样本x_i1叫做基准点（**锚点**），x_i2被认为是正样本（两者都是从x_i变化得到的，虽然看起来有差异，但语义信息不应该发生变化），数据集中其它所有图片都是负样本。

* **momentum encoder:** 如果只有当前batch的key是从当前的编码器得到特征，其它的key都是另外时刻的编码器输出的特征，这样就无法保证字典中key的一致性。所以作者又提出了动量编码器
