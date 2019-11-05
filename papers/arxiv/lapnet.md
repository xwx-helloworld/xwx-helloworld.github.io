
## 论文信息
* 论文名：LapNet : Automatic Balanced Loss and Optimal Assignment for Real-Time Dense Object Detection
* 作者：Florian Chabot, Mohamed Chaouch, Quoc Cuong Pham(CEA, LIST, Vision and Learning Lab for Scene Analysis)
* [github link]
* [arvix link](https://arxiv.org/pdf/1911.01149.pdf)

## 主要贡献（数据，模型，loss）
- 提出了新的网络LapNet（CPN中gloabalNet最后一层concat，叠加RetinaNet的Head）；
- 独特的anchor生成机制（借鉴yolo系列的kmeans方法）；
- 速度快（512*512大概需要55ms）；
- anchor通过PONO（Per-Object Normalized Overlap）方式选取，与传统AO（Absolute Overlap）方法不同；