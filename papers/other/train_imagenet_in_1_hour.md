## 论文信息
* 论文名：Accurate, Large Minibatch SGD: Training ImageNet in 1 Hour
* 作者：Priya Goyal, Piotr Dollar, Ross Girshick, Pieter Noordhuis, Lukasz Wesolowski, Aapo Kyrola, Andrew Tulloch, Yangqing Jia, Kaiming He(FAIR军团)
* github link
* [arvix link](https://arxiv.org/pdf/1706.02677.pdf)

## 主要贡献（数据，模型，loss）
* Linear Scaling Rule: When the minibatch size is multiplied by k, multiply the learning rate by k.
* Remark 1: Scaling the cross-entropy loss is not equivalent to scaling the learning rate.
* Remark 2: Apply momentum correction after changing learning rate if using (10).
* Remark 3: Normalize the per-worker loss by total minibatch size kn, not per-worker size n.
* Remark 4: Use a single random shuffling of the training data (per epoch) that is divided amongst all k workers.
* Constant warmup and Gradual warmup
* 炫富


## 文章细节(他山之石)
- Constant warmup. Particularly helpful for prototyping object detection and segmentation methods that fine-tune pre-trained layers together with newly initialized layers.
- Gradual warmup. Gradually ramps up the learning rate from a small to a large value.( linear or exp)


## 借鉴点(可以攻玉)
### Linear Scaling Rule

- mmcv里面有实现，作为一个配置项开关
```python
if args.autoscale_lr:
    # apply the linear scaling rule (https://arxiv.org/abs/1706.02677)
    cfg.optimizer['lr'] = cfg.optimizer['lr'] * cfg.gpus / 8
```

### warmup

- mmcv有实现，具体参见lr update hook
- params: base_lr, warmup_iter, warmup_ratio
- constant: base_lr * warmup_tatio
- linear: base_lr * [1- (1 - iter_cnt / warmup_iter) * (1- warmup_rato)]
- exp: base_lr * warmup_ratio ** (1-iter_cnt/warmup_iter)


### lr_updater(from mmcv, always with warmup)
- LrUpdaterHook: 包含基础功能，下面所有updaterhook都继承自此
- FixedLrUpdaterHook: lr = base_lr
- StepLrUpdaterHook: lr = base_lr * gamma**exp, usually gamma=0.1, exp基于是否达到step，exp=0,1,2,3,4
- ExpLrUpdaterHook: lr = base_lr * gamma**progress, usually gamma=0.1, progress = epoch_cnt
- PolyLrUpdaterHook: lr = (base_lr - min_lr) * coeff + min_lr, where coeff = (1 - progress / max_progress)**power, usually power=1, min_lr=0, progress=epoch_cnt, max_progress=max_epochs
- InvLrUpdaterHook: lr = base_lr * (1 + gamma * progress)**(-power), usually power=1, gamma=0.1, progress=epoch_cnt
- CosineLrUpdaterHook: lr = target_lr + 0.5 * (base_lr - target_lr) * (1 + cos(pi * (progress / max_progress)))