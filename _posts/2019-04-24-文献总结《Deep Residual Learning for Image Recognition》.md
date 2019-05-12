---
layout:     post
title:      文献总结《Deep Residual Learning for Image Recognition》(附TensorFlow代码)
subtitle:   图像识别的深度残差学习
date:       2019-04-24
author:     王沛
header-img: img/CVPR2018.jpg
catalog: true
tags:
    - CNN
    - 深度学习
    - 图像处理
    - Image Recognition
---

## 《Deep Residual Learning for Image Recognition》  

[原文链接](https://arxiv.org/abs/1512.03385)在这里 [pdf](https://arxiv.org/pdf/1512.03385.pdf)    
我的TensorFlow版本的代码在[这里](https://github.com/Xiaokeai18/resnet-tensorflow-implementation)

这是Kaiming He等人于2015年发表于CVPR的一篇关于深层卷积神经网络用于图像识别的文献，第一次提出了Residual Network (残差网络),即著名的ResNet。  

### 1.	主要内容：  
  - 提出了一个残差学习框架，能够简化深度神经网络的训练；  
  - 用残差函数替代原始原始函数，重组了框架的各层；  
  - 这些残差网络更容易优化，而且能通过更深的层来提升准确率；
  - 在ImageNet/ILSVRC&COCO上都获得了当时最好的成绩。  

### 2. 介绍：  
此文章研究思路为：在图像识别领域，神经网络的深度至关重要——> 但是层数越深，准确率反而降低（如下图）——> 提出了残差网络(ResNet)来解决这个问题。  
![layers](/img/post3-layers.png)   

可见随着网络层数的增加，准确率达到饱和后迅速退化，但是这种退化并不少过拟合造成的，且再一个合理的深度模型中，增加更多的层却导致了**更高的错误率**。  
事实上
**退化**的出现表明了并非所有的系统都是容易优化的。设想，对于一个浅层网络，给它加上更多的层，有这样一种方法：用**恒等映射**来构建增加的层。这说明，一个更深的模型，至少不应该带来更高的错误率。  
本文提出了一种深度**残差学习**框架来解决退化问题。我们明确让神经网络来拟合残差映射$F(x)=H(x)-x$，而不是原来的底层映射$H(X)$。而残差映射相对更容易优化，例如，在极端情况下，恒等映射时，将残差变为全0,比原来用非线性层堆叠更加简单。  
![block](/img/post3-block.png)   
公式$F(x)=H(x)-x$可以通过前馈神经网络的"shortcut connections"来实现，如上图。

### 3.	创新点：深度残差学习
- 残差学习：  
  1. $H(x)$由非线性层拟合而来，x为输入，既然能拟合$H(x)$，同样也能拟合$H(X)-X$；
  2. 明确让这些层，逼近残差函数$F(x)=H(x)-x$，则原始函数变为$Hx)=F(x)+x$；
  3. 两种函数都可以优化，但是残差函数学习更加简单；
- 恒等映射：  
  1. 构建一个模块如上图figure2,公式为：  
          $y=F(x,{W_i})+x$
  2. 上图figure2的例子包含两层$F=W_2\sigma{(W_1x)}$,$F+x$由shortcut和一个加法来完成；
  3. shortcut连接没有增加额外的参数和计算复杂度；
  4. 如果x和F的维度不相同（例如, 当改变了输入/输出的通道），可以通过shortcut连接执行一个线性映射$W_s$ 来匹配两者的维度:$y=F(x,{W_i})+W_sx$
  5. 该方法对卷积层和全连接层都适用；


### 4. 网络结构：
![architecture](/img/post3-architecture.png)   
左边为VGG-19,中间为‘Plain Network’,右边为‘Residual Network’  
下图Table1展示了ResNet更多的架构：  
![architecture1](/img/post3-architecture1.png)   


### 5. 实验设置：
- cropsize为224×224，从一张图像或它的水平翻转中随机采样；
- 每个像素减去均值；
- 图像使用标准颜色增强；
- 每个卷积层和激活层之间使用Batch Normalization; 
- 初始化权重后，对plain和resnet两个网络都从头开始训练；
- 分mini-batch尺寸为256，训练600000轮迭代；
- 权值衰减设为0.0001，不使用dropout；
- 学习率从0.1开始，每次错误率平稳时，除以10；


### 6. 结果：
- 在ImageNet上的实验结果：  
  ![result1](/img/post3-result1.png)   
  上图曲线可见，plain网络出现了明显的退化问题，较高的层数产生了高的错误率。这种情况不是梯度消失造成的，因为BN层的存在，不存在信号消失的问题。  
  而对于resnet，34层的结果比18层更优，且在验证集上也出现了较高的准确率。说明这种设置解决了退化的问题。18层的plain和18层的ResNet的准确率差不多，但是残差网络的收敛更快。    
  ![result2](/img/post3-result2.png)    
  在ImageNet验证集上的Top-1错误率。  
  ![result3](/img/post3-result3.png)   
  在ImageNet验证集上的错误率 (%, 10-crop testing)。可见层数较深的ResNet是最优的。  
- CIFAR-10  
  结果为：  
  ![result4](/img/post3-result4.png)   



### 7. My Implementation  

使用TensorFlow复现的代码点击 [这里](https://github.com/Xiaokeai18/resnet-tensorflow-implementation)  
该实验再**CIFAR10**数据集上进行。  

训练阶段的loss：  
![tf1](/img/post3-tf1.png)  
验证集的loss：  
![tf2](/img/post3-tf2.png)  
验证集Top1的错误率曲线：  
![tf3](/img/post3-tf3.png)  

最终80000STEP的准确率可达**91.8%**  
![tf4](/img/post3-tf4.png)  

尝试将层数变为62层（10个block），准确率达到
![tf5](/img/post3-tf5.png)  

**欢迎大家留言交流！** 
-
