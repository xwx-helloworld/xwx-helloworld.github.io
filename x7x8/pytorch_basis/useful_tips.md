
## pytorch相关操作
* [cheatsheet](https://pytorch.org/tutorials/beginner/ptcheat.html)
* [tensor相关](https://pytorch.org/docs/stable/tensors.html)
* torch中的squeeze与unsqueeze作用是去除/添加维度为1的行
* clamp将输入input张量每个元素的限制到不小于min ，并返回结果到一个新张量
* [pytorch常规操作集合](https://zhuanlan.zhihu.com/p/59205847?)[or this](https://www.jiqizhixin.com/articles/2019-04-25-8)

## MSELoss
* 默认参数： size_average=None, reduce=None, reduction='mean'
* size_average（已废弃）：默认情况下（True），返回一个值，整个batch的loss均值，否则（False），返回每个batch对应的loss
* reduce（已废弃）：默认情况下（True），返回一个avg或者sum，否则（False），每个batch一个loss
* reduction（none,mean,sum）：
* reduction=None:返回一个list，其中N是batchsize
* reduction='mean' or 'sum'，返回一个值