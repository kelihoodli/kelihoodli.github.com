---
layout: post
category: "reading"
title: "Caffe Inner Product Layer"
tags: [learning,deep learning,caffe]
---

#### 一、LayerSetup     
**1. 基本变量**       
1.1 N_ 当前layer(InnerProductLayer)的输出神经元个数    
1.2 K_ 样本特征个数count(1, num_axis)=CHW     
**2. 初始化**   
2.1 N_ $$\times$$ K_的权重矩阵`this->blob_[0]`     
2.2 1 $$\times$$ N_的bias矩阵`this->blob_[1]`      

#### 二、Forward_cpu    
调用cblas, 实现$$y=wx+b$$    

#### 三、Backword_cpu   
1. top_diff是error term, $$\delta_i^{l+1}$$, 其推导过程见<sup>[2](http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95)</sup>, 从top[0]->cpu_diff()获取    
2. bottom_data是activation, $$\alpha$$, 从bottom[0]->cpu_data()获取     
3. 关于权重矩阵的partial derivatives存储在this->blobs_[0]->mutable_cpu_diff()   
4. 关于bias的partial derivatives存储在this-blobs_[1]->mutable_cpu_diff()  
得到3), 4)之后，计算前一个layer的error term:$$\delta_i^{l}=(\sum_{j=1}^{s_l+1}W_{ji}^{(l)}\delta_j^{(l+1)})f^{'}(z_i^{(l)})$$   
 

#### ref   
1. [Caffe源码阅读(1) 全连接层](http://zhangliliang.com/2014/09/15/about-caffe-code-full-connected-layer/)
2. [UFLDL Backpropagation Algorithm](http://deeplearning.stanford.edu/wiki/index.php/Backpropagation_Algorithm)    
3. [BLAS Reference](https://developer.apple.com/library/mac/documentation/Accelerate/Reference/BLAS_Ref/index.html#//apple_ref/c/func/cblas_sgemm)
