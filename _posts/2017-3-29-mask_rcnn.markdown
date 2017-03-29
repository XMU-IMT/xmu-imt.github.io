---
layout:     post
title:      "Mask R-CNN小结"
subtitle:   " \"Hello World, Hello Blog\""
date:       2017-3-29 12:00:00
author:     "LancerLian"
header-img: "img/post-bg-2015.jpg"
tags:
    - 学术
---

#Mask R-CNN 小结
>连盛 2017.3.26

>[IMT Lab](http://imt.xmu.edu.cn/index.php) in XMU

>Author：Kaiming He, Georgia Gkioxari, Piotr Dollar,  Ross Girshick

>From Facebook AI Research (FAIR) 
>
>2017.3 published & reviewed

##Introduction

**简介**：

这篇文章是何凯明大神和RBG大神17年新出的又一篇力作。论文地址：[arxiv](https://arxiv.org/abs/1703.06870) .

**目的**： 

用于 ***目标实例分割*** 的框架（ *Object instance segmentation* ），能够有效地检测图像中的目标，同时还能为每个实例生成一个高质量的 ***分割掩码***（ *segmentation mask*）。即：目标实例分割可以看做由 ***object detection*** 和 ***semantic segmentation*** 组成。

**特点**： 

- Mask R-CNN 是在Faster R-CNN上的扩展----在其已有的用于边框识别的分支上添加一个并行的用于预测目标掩码的分支。

- 训练简单。仅比Faster R-CNN多一点计算开销，5fps。

- 易于泛化到其他任务。比如估计人类姿态。

- 实验：实例分割、边界框目标检测和人物关键点检测。

- 没使用fine-tuning的情况下，Mask R-CNN的表现超越了在每个任务上已有的所有single-modle entries。

- We hope our simple and effective approach will serve as a solid baseline and help ease future research in instance-level recognition

**结构**：

mask rcnn是对Faster R-CNN 的扩展。结构如图1。采用方法：
![Mask R-CNN](http://static.leiphone.com/uploads/new/article/740_740/201703/58d2200bc3bc0.png?imageMogr2/format/jpg/quality/90  "Framework of Mask R-CNN")

- 第一个分支是原始的Faster R-CNN结构，用于对候选bounding box进行分类和bbox坐标回归。

- 第二个分支对每个ROI区域预测分割mask, 它的结构实质上是一个小的FCN。

- 两个分支是平行/并行的

##Mask R-CNN

**Faster R-CNN**：对于每个候选物体，输出是类别标签和bbox。

- 第一步：RPN: 给出候选区域的bbox

- 第二步：通过RoIPooling, 在各个候选框中进行分类和bbox的回归

**Mask R-CNN**：采用和Faster R-CNN类似的两步过程

- 第一步：RPN: 给出候选区域的bbox

- 第二步：分支一：各个候选框中进行分类和bbox offset。分支二：对每个RoI输出binary mask

    ***损失函数***：![loss function](https://zhihu.com/equation?tex=L%3DL_%7Bcls%7D%2BL_%7Bbox%7D%2BL_%7Bmask%7D+) . mask分支对于每个RoI有Km<sup>2</sup> 维度的输出。K个（类别数）分辨率为m*m的二值mask。因此作者利用了a per-pixel sigmoid，并且定义 L<sub>mask</sub> 为平均二值交叉熵损失（the average binary cross-entropy loss）.对于一个属于第k个类别的RoI， L<sub>mask</sub> 仅仅考虑第k个mask（其他的掩模输入不会贡献到损失函数中）。这样的定义会允许对每个类别都会生成掩模，并且不会存在类间竞争。

![](http://i4.buimg.com/567571/29ec597900938c85.png)

**RoIAlign**: 是对RoI Pooling的改进。RoI Pooling在 Pooling时可能会有misalignment。解决方法：参考 [Spatial Transformer Networks (NIPS2015)][1], 使用双线性插值，再做聚合。(**个人理解**：misalignment在做分类时问题不大，可是当需要做pixel级别的mask时就不得不考虑其影响了。比如8 * 7的数据原先使用ROI Pooling时要pooling到7 * 7，会有misalignment。当使用ROIAlign后，先双线性插值到14 * 14，再pooling到7 * 7)

##Implementation Details

Each mini-batch has 2 images per GPU and each image has N sampled RoIs, with a ratio of 1:3 of positive to negatives。
使用了8个GPU来训练。

详见论文。


##Experiments

**整体效果**

![](http://i4.buimg.com/567571/40dcae736c9b275d.png)


###1. Instance segmentation
![](http://i2.muimg.com/567571/408c3148fb2a107f.png)

####不同主干网络模型之间的对比 + 使用softmax和sigmoid之间的对比 + RoIAlign效果 + Mask Branch的模型对比。 如下表：

![](http://i2.muimg.com/567571/b2664b45e801b777.png)

####与FCIS+++的对比
![](http://i2.muimg.com/567571/9381de958babea0d.png)

###2.  Object Detection

![](http://i2.muimg.com/567571/c8eea44bde3eaa83.png)

###3. Mask R-CNN for Human Pose Estimation

预测K个masks，每个mask对应一个keypoint type（比如一个mask对应左肩，另一个mask对应右肘等等）

![](http://i1.piimg.com/567571/335af4db53c6ac1f.png)







##补充材料

**RoI Pooling**： 将一个个大小不同的bbox映射到w<sub>xh</sub>的矩形框里。

- 输入包括 1）data：Feature Map  2）RPN层的输出（一堆矩形框）

- 输出：batch个vector。batch=roi的个数，vector的大小为channel xw xh


STN:spatial transformer network

[1]: https://arxiv.org/abs/1506.02025  