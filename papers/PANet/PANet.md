## 论文信息
* 论文名：Path Aggregation Network for Instance Segmentation
* 作者：Shu Liu, Lu Qi, Haifang Qin, Jianping Shi, Jiaya Jia（港中大、北大、商汤、优图）

## 主要贡献

- 模型改进，路径聚合（path aggregation)，主要在top level加上low level的shortcut；

- adaptive feature pooling，主要是ROI Pooling（Align）的时候使用多层特征，然后做融合；

- FC fusion，针对mask分支，efficient and better generality.

## 文章细节(他山之石)

- 下图是论文整体结构，分别是：FPN主干网络、自下而上的信息增强、自适应特征pooling、分类回归分支、mask回归分支（FC fusion）

![](framework.png)

- P层-->N层：

![](P-layer-N-layer.png)

- 多层特征pooling（ROI Pooling/Align）

![](adaptive.png)

- FC fusion（仅针对mask分支）

![](fc-fusion.png)

## 借鉴点(可以攻玉)

- 路径聚合

- 多层特征pooling

- fusion
