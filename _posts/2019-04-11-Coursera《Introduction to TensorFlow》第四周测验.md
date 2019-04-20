---
layout:     post
title:      Coursera《Introduction to TensorFlow》第四周测验
subtitle:   《Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning》第四周(Using Real-world Images)的测验答案
date:       2019-04-11
author:     王沛
header-img: img/introduction-to-tf.jpg
catalog: true
tags:
    - 机器学习
    - TensorFlow
    - 深度学习
    - Keras
---


# Coursera《Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning》(Quiz of Week4)

Using Real-world Images  
--
本博客为Coursera上的课程《Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning》第四周的测验。



### 目录

1. [第一题](#1) 
2. [第二题](#2) 
3. [第三题](#3) 
4. [第四题](#4) 
5. [第五题](#5) 
6. [第六题](#6) 
7. [第七题](#7) 


Coursera[课程地址](https://www.coursera.org/learn/introduction-tensorflow/)  
--
---


<h2 id="1">第一题</h2>

1. Using Image Generator, how do you label images?

	***A. It’s based on the directory the image is contained in***  
	B. You have to manually do it  
	C. It’s based on the file name  
	D. TensorFlow figures it out from the contents  

	**答案：A**

<h2 id="2">第二题</h2>

2. What method on the Image Generator is used to normalize the image?

	A. Rescale_image  
	B. normalize  
	C. normalize_image  
	***D. rescale***  

	**答案：D**

<h2 id="3">第三题</h2>

3. How did we specify the training size for the images?

	A. The training_size parameter on the validation generator  
	***B. target_size parameter on the training generator***  
	C. The target_size parameter on the validation generator  
	D. The training_size parameter on the training generator

	**答案：B**  


<h2 id="4">第四题</h2>

4. When we specify the input_shape to be (300, 300, 3), what does that mean?

	A. Every Image will be 300x300 pixels, and there should be 3 Convolutional Layers  
	***B. Every Image will be 300x300 pixels, with 3 bytes to define color***  
	C. There will be 300 horses and 300 humans, loaded in batches of 3  
	D. There will be 300 images, each size 300, loaded in batches of 3


	**答案：B**  


<h2 id="5">第五题</h2>

5. If your training data is close to 1.000 accuracy, but your validation data isn’t, what’s the risk here?  

	A. No risk, that’s a great result  
	B. You’re overfitting on your validation data  
	***C. You’re overfitting on your training data***  
	D. You’re underfitting on your validation data  
	**答案：C**  

<h2 id="6">第六题</h2>

6. Convolutional Neural Networks are better for classifying images like horses and humans because:

	A. In these images, the features may be in different parts of the frame    
	B. There’s a wide variety of horses  
	C. There’s a wide variety of humans  
	***D. All of the above***  
 
	**答案：D**  

<h2 id="7">第七题</h2>

7. After reducing the size of the images, the training results were different. Why?  
   
	***A. We removed some convolutions to handle the smaller images***  
	B. There was more condensed information in the images  
	C. The training was faster  
	D. There was less information in the images   
 
	**答案：A**  
