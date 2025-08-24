# 论文精读-SegAgent

**论文链接：**

<https://arxiv.org/pdf/2503.06520>

用RL做segmentation中比较有impact的工作。

## Method

模型输入输出以及rewards如下

![assets/segzero01.png](assets/segzero01.png)

值得注意的是，他计算了ground truth的bbox以及points，从而有数据进行SFT。


