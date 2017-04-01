---
layout:     post
title:      "daily papers mark down~"
subtitle:   " \"一系列文章的小结,不断更新中\""
date:       2017-4-1 12:00:00
author:     "LancerLian"
header-img: "img/post-bg-2015.jpg"
tags:
    - 学术
    
    - 论文
---

# 日常文献 mark down~

> 连盛 2017.4.1

> [IMT Lab](http://imt.xmu.edu.cn/index.php) in XMU


这篇博文会是我对日常读过的比较有意思的文章的一个小结。更多的像导读/知识点小回顾，没有精力写太多技术细节，若对文章感兴趣，我对每篇文章都会附上arxiv链接，欢迎交流讨论。我的新浪微博：[lancerlian](http://weibo.com/lancer123) 其他的联系方式自己挖掘哈！

## 文章列表

- [ ***【2017.03.23】***  ▒▒ **CoGAN**: Coupled generative adversarial networks， Ming-Yu Liu et al]( #CoGAN ) 

- [ ***【2017.04.01】***  ▒▒ **SeGAN**: Segmenting and Generating the Invisible, Ehsani et al]( #SeGAN )


<br /> <br /> <br /> <br /> <br /> <br /> 







<div id="CoGAN">

## ***【2017.03.23】*** ▒▒  **CoGAN**: Coupled generative adversarial networks

这篇文章是NIPS 2016中涌现出来的众多GAN的模型改进和应用文章中的一篇。这篇文章吸引我的地方在于效果不错，而且跨域样本生成也蛮有意义的。文章链接： [arxiv](https://arxiv.org/abs/1606.07536),

P.S. ： 这篇文章的代码开源了。链接如下： [caffe版本](https://github.com/mingyuliutw/cogan
) , [tensorflow版本](https://github.com/andrewliao11/CoGAN-tensorflow
)。 开源的代码能够跑一跑MNIST数据集。

再P.S. ： [Ian Goodfellow](http://www.iangoodfellow.com/)大神（生成对抗网络GAN的提出者,青年才俊）在NIPS 2016做了个关于GAN的Tutorial，相当优秀。链接如下：[tutorial](https://arxiv.org/pdf/1701.00160.pdf), [slides](https://media.nips.cc/Conferences/2016/Slides/6202-Slides.pdf) 值得一看。

### 1. 摘要：

![CoGAN_abstract](http://i4.buimg.com/567571/509cd797e188db25.png)

### 2. 主要工作：

作者利用两个通过**权值共享**耦合的GAN网络，生成跨域样本。举个例子，输入一堆正常人脸和一堆戴眼镜的人脸，会生成**长得一样**的 **with or without glasses** 的人脸。这个模型可以用在很多不同的地方，具体的自己挖掘咯~不细讲。

### 3. 模型设计：

将两个GAN网络通过权值共享耦合起来。其中，生成器共享低层的几层权值，判别器共享高层的几层权值。

![CoGAN_model](http://i2.muimg.com/567571/7c36106a1911ba8f.png)

### 4. 原理分析：

- **优化目标**：和GAN的min-max优化函数类似，只是变成了两个GAN网络耦合后的函数。
![](http://i1.piimg.com/567571/c709aff140081dba.png)

- **训练阶段**： GAN1 和 GAN2 分别输入两个域的样本。两域之间不需要相关。以戴眼镜人脸举例来说即：两个训练输入不需要是同一个人w/o眼镜的样本。如下图所示。

![](http://i2.muimg.com/567571/c2a49ba75173bb4d.png)

- **测试阶段** ： 两个GAN的G输入一样的满足一定分布（如高斯分布）的随机噪声。

- **权值共享** ： 生成器网络采用类似DCGAN的模型，与CNN不同的是采用了DeConv，这里我的理解是CNN的Conv是downsampling, GAN中G的Conv是upsampling，即放大图像，并非进行卷积的反运算，而是通过修改padding、stride等参数来实现upsampling. 举例如下2图。因此， ***在生成器阶段***，G与传统CNN是相反的。头几层解码的是高层语义信息，最后解码的是底层纹理信息； ***在判别器阶段***，D与传统CNN则类似，低层解码底层纹理信息，高层解码高层语义信息。因此，通过共享生成器的头几层的权值，可以保证生成的两个域的数据在高层语义特征上类似，比如人脸长得一样。而高层不共享，则保证了两个域间有各自的特征，比如戴不戴眼镜。而作者通过实验证明，判别器的权值共享对性能影响不大，最大的作用是减少参数。

![deconv1](https://github.com/vdumoulin/conv_arithmetic/raw/master/gif/no_padding_no_strides_transposed.gif) ![deconv2](https://github.com/vdumoulin/conv_arithmetic/raw/master/gif/padding_strides_odd_transposed.gif)

### 5. 一些实验结果

作者show了一些生成的样本，并与Conditional-GAN进行了比较。实验证明，Conditional-GAN在生成跨域样本时效果不好。两个域之间关联性很弱。

![](http://i4.buimg.com/567571/8f362b802b38f4e1.png)
![](http://i1.piimg.com/567571/911a1cba45c49325.png)

### 6. 其他应用场景

除了MNIST,人脸（CelebA dataset），作者还在RBGD、室内3D数据集等场景进行了实验。不细说。

<br /> <br /> <br /> <br /> <br /> <br /> 

---------------------------------------------------------------------------------------


<div id="SeGAN">
## ***【2017.4.1】*** ▒▒  **SeGAN**: Segmenting and Generating the Invisible

文章链接： [arxiv](https://arxiv.org/abs/1703.10239)

摘要：

![SeGAN_abstract](http://i4.buimg.com/567571/92757435836c0a7d.png)

**主要工作**：图片中的物体的被遮挡部分对于后续的detection和analysis是重要的信息。作者通过一个segmentor + generator + discriminator来生成物体包括被遮挡部分的Mask，并生成被遮挡部分的可能图像。最终目的：**补全图像中物体被遮挡的部分**。主要分为如下两部分：

- 将物体不可见的部分分割出来，得到一个物体整体的Mask；

- 利用GAN生成不可见的部分的图像。

模型的输入是图像以及每个物体可见部分的mask.

![SeGAN](http://i2.muimg.com/567571/24a13b8f4fd45bcf.png)
![Model](http://i4.buimg.com/567571/fcdfd6dd62eaaa70.png)

