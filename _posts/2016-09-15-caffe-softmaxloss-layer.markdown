---
layout: post
category: "reading"
title: "Caffe SoftMaxLoss Layer"
tags: [learning,deep learning,caffe]
---     

#### 一、    
Reformulate UFLDL<sup>[1](http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95)</sup>关于方差代价函数的反向传播算法为SoftMaxLoss的计算过程:      
![drawing](/assets/image/backpropagation.png)     
#### 二、   
研读SoftMaxLossLayer源码可参考<sup>[2](http://blog.csdn.net/l691899397/article/details/52291909)</sup>.    

#### ref   
1. [反向传播算法](http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95)
2. [深度学习笔记8：softmax层的实现](http://blog.csdn.net/l691899397/article/details/52291909)    