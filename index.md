## 论文阅读笔记汇总

现阶段聚焦于图像算法，后续会研究其他领域。

### 一般论文改进点

基于深度学习的图像算法一般包括：数据、模型、计算loss（反向）。

* 数据：部分论文会对图像常见问题进行重新建模，导致数据这块需要新的处理方式，比如xyxy, xywh，left-top+bottom-right等，常伴随着模型的改进；
* 模型：此处改进最多，新的网络、新的分支、新的连接，都有可能会产生新的论文；
* loss：一般伴随着新的建模方式，或者处理一些特定问题（样本不均衡、某个指标有误差等）。

### 论文汇总

* [RetinaNet](papers/retinanet/retinanet.md)
* [PANet](papers/PANet/PANet.md)

### 联系方式

如果有问题或者需要讨论的话，欢迎联系我。e-mail: xwx.helloworld@outlook.com