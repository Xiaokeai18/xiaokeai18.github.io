---
layout:     post
title:      文献总结《Learning Spatiotemporal Features using 3DCNN and Convolutional LSTM for Gesture Recognition》
subtitle:   使用3D卷积和卷积LSTM学习时空特征用于手势识别
date:       2019-05-12
author:     王沛
header-img: img/iccv17.jpg
catalog: true
tags:
    - CNN
    - 深度学习
    - LSTM
    - Action Recognition
---

## 《Learning Spatiotemporal Features with 3D Convolutional Networks》  

[原文链接](http://openaccess.thecvf.com/content_ICCV_2017_workshops/w44/html/Zhang_Learning_Spatiotemporal_Features_ICCV_2017_paper.html)在这里 [pdf](http://openaccess.thecvf.com/content_ICCV_2017_workshops/papers/w44/Zhang_Learning_Spatiotemporal_Features_ICCV_2017_paper.pdf)    


这是G. Zhu等人于2017年发表于ICCV的一篇关于深度学习用于手势识别的文献，提出了3D卷积神经网络与卷积LSTM的结合使用，进行时空特征提取。  

### 1.	主要内容： 
  - 提出了新的深度神经网络从而学习时空特征，用作手势识别；
  -	通过3D卷积网络和convLSTM来学习2D特征信息；
  - 在三种不同的数据集上表现最优；


### 2. 相关研究：
 &ensp;  与动作识别不同，动作识别的背景可以成为某种线索，而手势识别则成为影响准确率的不利因素。手势识别更注重手和胳膊的移动。  

    a. 双流卷积网络，动作识别表现出色，但是不能很好地识别手势；  
    b. LRCN ；  
    c. 双向循环网络和时域卷积可以提升手势识别性能；  
    d. 3D CNN表现优秀，但是使用固定池化减小size，因此需要更多的层数和更大的步长；  


### 3. 创新点：  

  - 使用3DCNN和bidirectional conv LSTM学习到2D时空特征信息，2D时空信息可以编码全局时间信息和局部空间信息。时空相关性可以在整个学习过程中被保留；  
  - 提出的深度网络可以把视频转为2D时空特征信息；  
  - 提出的时空特征，通过线性SVM输出，在两个不同的benchmark上得到了最好的结果；  
  - 这是第一种方法，使用3DCNN和双向convLSTM学习2D时空特征，然后使用2DCNN学习高阶特征，进行手势识别。  


### 4. 网络结构：  


![net1](/img/post5-net1.png)  
a． 上图为整体架构，3DCNN和convLSTM被应用来学习短期和长期时空特征，然后2DCNN用来学习高阶时空特征，用作最后的识别。  

![net2](/img/post5-net2.png)  
b． 3DCNN模块，全部采用3×3×3，步长为1卷积核。该模块用于学习局部时空特征，因此只用来了个卷积层；  
c． ConvLSTM用来学习长期时空特征，卷积和循环操作可以完整保留时空相关性。  

![net3](/img/post5-net3.png)  

d． 2DCNN，在2D特征信息上进行分类。2D特征信息仍然拥有很大的空间size，需要进行降维。2DCNN能在降维的同时学习到更多的高阶特征。  

![net4](/img/post5-net4.png)  

### 5. 实验设置：  

  -	数据集：IsoGD和SKIG；
  - 在ISOGD上Scratch训练，在SKIG上微调。
  - 使用Batch Normalization加快训练速度。
  - 学习率设为0.01，每1000次迭代后下降1/10
  - Weight decay为0.00004
  - 输入图像固定为112×112



### 6. 结果：  

平均池化效果比最大池化好，SVM可以提升性能：  

 ![result1](/img/post5-result1.png)  

在SKIG上的结果最优：  

 ![result2](/img/post5-result2.png)  

### 8. 总结：   

  &ensp; 提出了一个深度网络用来学习时空特征，通过3D-CNN和convLSTM学习到的2D特征信息可以编码全局时间信息和局部空间信息。  
