
## torch.distributed

- reference: [image](distributed.png) or [bilibili](https://t.bilibili.com/317274366646993515?tab=2) or [medium](https://medium.com/intel-student-ambassadors/distributed-training-of-deep-learning-models-with-pytorch-1123fa538848)
- 分布式系统五大要素：非常快的带宽、所有机器同的用户名（linux）、免密登陆、MPI实现、可访问的文件系统；
- SGD与同步SGD
- 通信方式：peer-to-peer, all-reduce
- 为了更好利用分布式系统效率：增加batch，增加节点，降低数据同步时间（提高带宽）
- 分布式系统中，有效的Batch=R*B（R=ranks， B=mini-batch），，此时lr需要调小相应倍数。

## pytorch相关操作
* [cheatsheet](https://pytorch.org/tutorials/beginner/ptcheat.html)
* [tensor相关](https://pytorch.org/docs/stable/tensors.html)
* torch中的squeeze与unsqueeze作用是去除/添加维度为1的行
* clamp将输入input张量每个元素的限制到不小于min ，并返回结果到一个新张量
* [pytorch常规操作集合](https://zhuanlan.zhihu.com/p/59205847?)[or this](https://www.jiqizhixin.com/articles/2019-04-25-8)
* pytorch分布式介绍[知乎](https://zhuanlan.zhihu.com/p/76638962) [微信公众号](https://mp.weixin.qq.com/s?__biz=MzI5MDUyMDIxNA==&mid=2247491747&idx=1&sn=af1c91180456cd851053f4f754db9c9d&chksm=ec1c0d5adb6b844c83d5febcd8a90a8a06a7a5eeb6fbc2a48c19fdbeb9cc0cd0f56ac5c59c1b&mpshare=1&scene=1&srcid=&sharer_sharetime=1572101433143&sharer_shareid=491844c88dff94f0e7d8712bb6f1b13f&key=1559c9758c58e71543c818270fa45a04d9c9c659803ec536f80bd3a7c116b20061805ac146f75e69cdf672bf6bbde263cc74509a5ae9efe9d75e5ce834ead168c6b3283b138c4dbc7788da8276449abc&ascene=1&uin=MTc3MTUyMDMwMw%3D%3D&devicetype=Windows+10&version=62060833&lang=zh_CN&pass_ticket=9K06ZMGyaIsSuHrUd6xyr7I7PGDRhEzyHYD7jQAeQ65jsPe%2BtC9dkOxRK8XmZyxg)
### MSELoss
* 默认参数： size_average=None, reduce=None, reduction='mean'
* size_average（已废弃）：默认情况下（True），返回一个值，整个batch的loss均值，否则（False），返回每个batch对应的loss
* reduce（已废弃）：默认情况下（True），返回一个avg或者sum，否则（False），每个batch一个loss
* reduction（none,mean,sum）：
* reduction=None:返回一个list，其中N是batchsize
* reduction='mean' or 'sum'，返回一个值

## Load dataparallel model

* load pretrained model

```
if cfg.TRAIN.resume_model is not None:  # added by Henson
	checkpoint = torch.load(cfg.TRAIN.resume_model)

	model_dict = model.state_dict()
	pretrained_dict = {k: v for k, v in checkpoint['state_dict'].items()}
	model_dict.update(pretrained_dict)
	model.load_state_dict(model_dict)

	print("resuming model is successful!!!")

if gpu_nums > 1:
	model = nn.DataParallel(model)
if torch.cuda.is_available():
	model = model.cuda()
model.train()
```

* save model

```
if gpu_nums > 1:
	state_dict = model.module.state_dict()
else:
	state_dict = model.state_dict()

torch.save({
	'epoch': epoch_num,
	'save_dir': cfg.TRAIN.save_dir,
	'state_dict': state_dict},
	os.path.join(cfg.TRAIN.save_dir, cfg.TRAIN.model_name + '%04d.ckpt' % epoch_num))
```

* load model when inference phase

```
checkpoint = torch.load(cfg.model)
model.load_state_dict(checkpoint['state_dict'])

if torch.cuda.is_available():
	model = model.cuda()
model.eval()
```

