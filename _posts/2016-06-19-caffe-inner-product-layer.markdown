---
layout: post
category: "reading"
title: "Caffe Inner Product Layer"
tags: [learning,deep learning,caffe]
---

#### 一、LayerSetup     
**1. 基本变量**        
1.1 M_ feeds给当前layer(InnerProductLayer)的样本量, 由于每次SGD用一个batch_size的样本量学习，因此M_等于batch_size    
1.2 N_ 当前layer(InnerProductLayer)的输出神经元个数    
1.3 K_ 样本特征个数count(1, num_axis)=CHW     
**2. 初始化**   
2.1 N_ $$\times$$ K_的权重矩阵`this->blob_[0]`     
2.2 1 $$\times$$ N_的bias矩阵`this->blob_[1]`      

#### 二、LayerSetUp     
根据InnerProductLayer的num_output,N_,和样本的特征个数K_, ReShape Weight矩阵(this->blobs_[0])为$$R^{N_\times K_}$$, bias向量(this->blobs_[1])为$$R^{1\times N_}$$， 并调用InnerProductParam中设置的参数初始化Filler初始化Weight和bias.    

#### 三、Reshape     
Reshape top[0]为$R^{M_ \times N_}$, 表示每个样本与Weight矩阵相乘的线性变换输出.     
Reshape bias_multiplier_为$R^{1 \times M_}$, 并初始化为1      

#### 三、Forward_cpu     
实现$$Y=XW^T+b$$    
X在bottom[0]->cpu_data()     
Y在top[0]->mutable_cpu_data()    
W在this->blobs_[0]_.cpu_data()    
b在this->blobs_[1]_.cpu_data()     
实现方式采用cblas:     
{% highlight xml linenos=table %}         
/**
 * C←αAB + βC
 * A: M × K
 * B: K × N
 * C: M × N
 * https://developer.apple.com/library/mac/documentation/Accelerate/Reference/BLAS_Ref/#//apple_ref/c/func/cblas_sgemm
 * M		Number of rows in matrices A and C.
 * N		Number of columns in matrices B and C.
 * K		Number of columns in matrix A; number of rows in matrix B.
 * alpha	Scaling factor for the product of matrices A and B.
 * A		Matrix A.
 * lda		The size of the first dimention of matrix A; if you are passing a matrix A[m][n], the value should be m.
 * B		Matrix B.
 * ldb		The size of the first dimention of matrix B; if you are passing a matrix B[m][n], the value should be m.
 * beta		Scaling factor for matrix C.
 * C		Matrix C.
 * ldc		The size of the first dimention of matrix C; if you are passing a matrix C[m][n], the value should be m.
 */
{% endhighlight %}      

#### 四、Backword_cpu   
1. top_diff是error term, $$\delta_i^{l+1}$$, 其推导过程见<sup>[2](http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95)</sup>, 从top[0]->cpu_diff()获取    
2. bottom_data是activation,即$$a_i^{(l)}=f(Wx)$$, 从bottom[0]->cpu_data()获取. 该buttom[0]->cpu_data()即是Forward时计算的top[0]->mutable_cpu_data()         
3. 关于权重矩阵的partial derivatives存储在this->blobs_[0]->mutable_cpu_diff()   
4. 关于bias的partial derivatives存储在this-blobs_[1]->mutable_cpu_diff()        
得到3), 4)之后，计算前一个layer的error term:$$\delta_i^{l}=(\sum_{j=1}^{s_l+1}W_{ji}^{(l)}\delta_j^{(l+1)})f^{'}(z_i^{(l)})$$   
5. 最后，计算当前Layer的error term和Weight, 计算下一层反向传播需要使用到的error term.     

#### ref   
1. [Caffe源码阅读(1) 全连接层](http://zhangliliang.com/2014/09/15/about-caffe-code-full-connected-layer/)
2. [UFLDL Backpropagation Algorithm](http://deeplearning.stanford.edu/wiki/index.php/Backpropagation_Algorithm)    
3. [BLAS Reference](https://developer.apple.com/library/mac/documentation/Accelerate/Reference/BLAS_Ref/index.html#//apple_ref/c/func/cblas_sgemm)
