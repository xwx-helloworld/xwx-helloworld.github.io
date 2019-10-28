## 论文阅读笔记汇总

现阶段聚焦于图像算法，后续会研究其他领域。

### 一般论文改进点

基于深度学习的图像算法一般包括：数据、模型、计算loss（反向）。

* 数据：部分论文会对图像常见问题进行重新建模，导致数据这块需要新的处理方式，比如xyxy, xywh，left-top+bottom-right等，常伴随着模型的改进；
* 模型：此处改进最多，新的网络、新的分支、新的连接，都有可能会产生新的论文；
* loss：一般伴随着新的建模方式，或者处理一些特定问题（样本不均衡、某个指标有误差等）。

### 论文汇总

models:

- [UnitBox](papers/unitbox/unitbox.md)
- [RetinaNet](papers/retinanet/retinanet.md)
- [PANet](papers/PANet/PANet.md)
- FreeAnchor
- [FSAF](papers/FSAF/FSAF.md)
- FCOS
- Associate Embbeding
- CornerNet(Objects as Points)
- CenterNet(CenterNet: Keypoint Triplets for Object Detection)
- CenterNet
- CRAFT
- M2Det
- [EfficientNet](papers/efficientnet/efficientnet.md)
- [AutoAugment](papers/autoaugment/autoaugment.md)
- [CrowdPose](papers/crowdpose/crowdpose.md)

tips:

- [Training ImageNet in 1 Hour](papers/others/train_imagenet_in_1_hour.md)


### 视频系列

- [STM](papers/STM/STM.md)

### 源码解读系列

- [mmdetection](images/mmdet-two-stage-detector-call-stack.png)


### 其他系列
- [ELASTIC](x7x8/notes/elastic.md)
- [basicneck and bottleneck](x7x8/notes/resnet.md)
- [pytorch intro, also with Module and Function ](x7x8/pytorch_basis/pytorch.md)
- [pytorch basis](x7x8/pytorch_basis/useful_tips.md)
- [mmdet-tricks](x7x8/pytorch_basis/tricks.md)
- [datasets](x7x8/datasets.md)

### 踩坑系列

- [explore jit](x7x8/pytorch_basis/jit_bugs.md)
- [bugs-daily](x7x8/useful/bugs-daily.md)
- [docker-tips](x7x8/useful/docker.md)
- [bash-tips](x7x8/useful/bash-tips.md)
- [pytorch-tips](x7x8/useful/pytorch-tips.md)
- [ubuntu-tips](x7x8/useful/ubuntu-tips.md)
- [python-tips](x7x8/useful/python-tips.md)

### 联系方式

如果有问题或者需要讨论的话，欢迎联系我。e-mail: xwx.helloworld@outlook.com
