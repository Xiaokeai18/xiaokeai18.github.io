---
layout:     post
title:      ubuntu环境下，系统无法与NVIDIA通信的解决方法
subtitle:   系统错误提示：NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver 的解决方法
date:       2019-01-19
author:     王沛
header-img: 
catalog: true
tags:
    - ubuntu
    - CUDA
    - TensorFlow
    - 深度学习
---

### ubuntu环境下，系统无法与NVIDIA通信的解决方法
#### 系统错误提示：NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver 的解决方法

笔者今日在自己的电脑上编译安装列tensorflow-gpu版（踩了无数的坑才装好），刚刚使用了一天，有一回跑程序时突然发现tensorflow运行极慢且伴有一些诡异的**warning**。

经过多方查资料，网上大多的说法时安装CUDA出现了问题导致显卡连不上，国外一些论坛推荐uninstall CUDA 然后重装。

由于重装CUDA，cuDNN过于麻烦，所以笔者尝试为ubuntu系统内核安装NVIDIA模块。

先安装dkms，很小，几秒钟就装好了。
> dkms可以帮我们维护内核外的驱动程序，在内核版本变动之后可以自动重新生成新的模块。  
详见[DKMS简介](https://blog.csdn.net/fouweng/article/details/53435602)

在终端输入以下命令：

```
sudo apt-get install dkms
```
然后再输入：
```
sudo dkms build -m nvidia -v 410.48    
```
这里的-v后面是驱动版本号，我的版本号的410.48,如果你们输入这个无效，可以进入/usr/src目录查看一个名为nvidia-\*\*\*.\*\*的文件夹。我的文件夹名为nvidia-410.48  
最后输入：
```
sudo dkms install -m nvidia -v 410.48    
```
同理，后面的数字依旧要改成你的显卡驱动版本号。 

-------   
最后**重启一下**应该就大功告成了！

输入：
```
watch -n 1 nvidia-smi
```
查看是否连接成功  
![screenshot](/img/post-2019-01-19-screenshot1.png)

如果是这样，就表示成功了。
