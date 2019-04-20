---
layout:     post
title:      使用Tensorflow搭建CNN训练MNIST遇到GPU显存资源耗尽问题的解决方法
subtitle:   tensorflow.python.framework.errors_impl.ResourceExhaustedError 的解决方法
date:       2019-01-20
author:     王沛
header-img: img/TensorFlow-logo.jpg
catalog: true
tags:
    - TensorFlow
    - 深度学习
    - CNN
    - ubuntu
---

## issue  
在阅读《Tensorflow实战》这本书的过程中，笔者从[Github](https://github.com/terrytangyuan/tensorflow-in-practice-code)上找到列了书中相应的实例代码（在书中也可找到）。在使用Tensorflow搭建简易的卷积神经网络时，依照书中的代码[5_2_CNN_MNIST.py](https://github.com/terrytangyuan/tensorflow-in-practice-code/blob/master/5_2_CNN_MNIST.py),在训练过程中没什么问题，但在测试过程中，如果你的GPU显存不够大，则会出现内存耗尽的错误。笔者的终端输出如下：
> I tensorflow/core/common_runtime/bfc_allocator.cc:643] Bin (256): 	Total Chunks: 0, Chunks in use: 0 0B allocated for chunks. 0B client-requested for chunks. 0B in use in bin. 0B client-requested in use in bin.
I tensorflow/core/common_runtime/bfc_allocator.cc:643] Bin (512): 	Total Chunks: 1, Chunks in use: 0 768B allocated for chunks. 4B client-requested for chunks. 0B in use in bin. 0B client-requested in use in bin.
I tensorflow/core/common_runtime/bfc_allocator.cc:643] Bin (1048576): 	Total Chunks: 0, Chunks in use: 0 0B allocated for chunks. 0B client-requested for chunks. 0B in use in bin. 0B client-requested in use in bin.
I tensorflow/core/common_runtime/bfc_allocator.cc:643] Bin (2097152): 	Total Chunks: 0, Chunks in use: 0 0B allocated for chunks. 0B client-requested for chunks. 0B in use in bin. 0B client-requested in use in bin.
I tensorflow/core/common_runtime/bfc_allocator.cc:660] Bin for 957.03MiB was 256.00MiB, Chunk State: 
I tensorflow/core/common_runtime/bfc_allocator.cc:678] Chunk at 0x7f93b8000000 of size 1280
I tensorflow/core/common_runtime/bfc_allocator.cc:678] Chunk at 0x7f93b8000500 of size 256  

报错内容为：
> W *tensorflow/core/common_runtime/bfc_allocator.cc:275] Ran out of memory trying to allocate 957.03MiB.  See logs for memory state.
W tensorflow/core/framework/op_kernel.cc:993] Resource exhausted: OOM when allocating tensor with shape[10000,32,28,28]*
Traceback (most recent call last):
  File "/home/xiaokeai/anaconda3/lib/python3.5/site-packages/tensorflow/python/client/session.py", line 1022, in _do_call
    return fn(*args)
  File "/home/xiaokeai/anaconda3/lib/python3.5/site-packages/tensorflow/python/client/session.py", line 1004, in _run_fn
    status, run_metadata)
tensorflow.python.framework.errors_impl.**ResourceExhaustedError**: OOM when allocating tensor with shape[10000,32,28,28]
	 [[Node: Conv2D = Conv2D[T=DT_FLOAT, data_format="NHWC", padding="SAME", strides=[1, 1, 1, 1], use_cudnn_on_gpu=true, _device="/job:localhost/replica:0/task:0/gpu:0"](Reshape, Variable/read)]]
	 [[Node: Mean_1/_7 = _Recv[client_terminated=false, recv_device="/job:localhost/replica:0/task:0/cpu:0"

------------------------
## Cause of Error
这是由于，该实例的最后一行代码：
```python
print("test accuracy %g"%accuracy.eval(feed_dict={
    x: mnist.test.images, y_: mnist.test.labels, keep_prob: 1.0}))
```
这行代码将全部的测试集一共10000张图片一次性全部喂入神经网络，由于灰度图片大小为28×28像素，安装每个像素的为float(64bit)类型来推算，本实例的第一层卷积核数为32个，则至少需要10000×28×28×32×64bit 大约为1914MByte，及消耗了约2GB显存，这对于显卡较小的人来说，当然会报错**ResourceExhaustedError**了。

-------------------
## Solution

解决方法自然有。   
-

**方法一**：土豪同学可以买个好的显卡，大一点的显存。  

**方法二**：对于我这种穷人，只能想办法让Tensorflow节省内存了。我的想法是，可以像训练过程一样，把10000个测试集也拆分，按照一定的BATCHSIZE，分成多轮计算，然后取平均就可以了。
以下是修改的代码，将原来的代码最后一句注释：
```python
'''
print("test accuracy %g"%accuracy.eval(feed_dict={
    x: mnist.test.images, y_: mnist.test.labels, keep_prob: 1.0}))
'''
opt = 0.0
for i in range(10):
  batch = mnist.test.next_batch(1000)

  test_accuracy = accuracy.eval(feed_dict={
      x:batch[0], y_: batch[1], keep_prob: 1.0})
  #print("step %d, test accuracy %g"%(i, test_accuracy))
  opt += float(test_accuracy)

print("test accuracy %.3f"% (opt/10))
```

这就是把BATCH设为1000, 将10000个数据分为10轮计算，取最终10次的平均值。  
大功告成！我的输出为：
![Tensorflow-CNN-MNIST-输出](/img/post-2019-1-20-screenshot.png)

同理，在遇到其他GPU MEMORY资源耗尽的情况时，也可以通过类似的方法，将数据拆分输入。
-
