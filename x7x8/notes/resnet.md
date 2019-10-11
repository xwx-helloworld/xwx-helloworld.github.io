# BasicNeck and BottleNeck

## Basicneck
![](basicneck.png)
* path1: x--> (conv3*3+norm+relu) + (conv3*3+norm)
* path2: x--> x(downsample or not)
* path1 + path2 --> relu

## Bottleneck
![](bottleneck.png)
* path1: x--> (conv1*1+norm+relu) + (conv3*3+norm+relu) + (conv1*1+norm) 
* path2: x--> x(downsample or not)
* path1 + path2 --> relu

## Bottle2neck

![](res2net.png)

* path1: x--> (conv1*1+norm+relu) + split_and_cat + (conv1*1+norm) 
* path2: x--> x(downsample or not)
* path1 + path2 --> relu

* split_and_cat: split tensor into nums, each splits should be (conv3*3+norm+relu)