---
layout: post
category: "reading"
title: "Caffe SoftMax Layer"
tags: [learning,deep learning,caffe]
---     

#### 一、Reshape       
multiplier_data初始化为$$R^{1\times2}$$的矩阵，每个元素设置为1.     
outer_num_ = M_, 等于一次测试所用样本量大小batch_size    
inner_num = HW = 1    


#### 二、Forward_cpu     
实现SoftMax: $$y_i = \max(0, x_i) + \alpha_i \min(0, x_i)$$       
1. 拷贝$$W^Tx$$到top_data$$\in R^{M\_ \times N\_}$$    
`caffe_copy(inner_num_, bottom_data + i * dim, scale_data);`      
2. 计算每个label下，最大的$$W^Tx$$       
{% highlight xml linenos=table %}   
for (int j = 0; j < channels; j++) {
      for (int k = 0; k < inner_num_; k++) {
        scale_data[k] = std::max(scale_data[k],
            bottom_data[i * dim + j * inner_num_ + k]);
      }
}
{% endhighlight %}  
3. 每个$$W^Tx$$减去最大值，避免指数运算结果太大              
{% highlight xml linenos=table %}   
// subtraction
// top_data = -1 * sum_multiplier_.cpu_data() * scale_data + 1 * top_data
// top_data: 			channels * inner_num_
// sum_multiplier_.cpu_data: 	channels * 1
// scale_data:			1 * inner_num_
caffe_cpu_gemm<Dtype>(CblasNoTrans, CblasNoTrans
		      , channels, inner_num_, 1, -1.
  		      , sum_multiplier_.cpu_data(), scale_data, 1., top_data);      
{% endhighlight %}  

4.  计算指数       
{% highlight xml linenos=table %}   
// exponentiation
// top_data[i...dim] = exp(top_data[i...dim])
caffe_exp<Dtype>(dim, top_data, top_data);    
{% endhighlight %}     
5. 计算归一项      
{% highlight xml linenos=table %}   
// sum after exp
// scale_data = top_data * sum_multiplier_.cpu_data
// top_data: 			channels * inner_num_
// sum_multiplier_.cpu_data:	channels * 1
// scale_data:			inner_num_ * 1
caffe_cpu_gemv<Dtype>(CblasTrans, channels, inner_num_, 1.
		      , top_data, sum_multiplier_.cpu_data(), 0., scale_data);     
{% endhighlight %}      
6. 对每个label output计算SoftMax    
{% highlight xml linenos=table %}     
// division
for (int j = 0; j < channels; j++) {
	// top_data = top_data / scale_data
	caffe_div(inner_num_, top_data, scale_data, top_data);
	top_data += inner_num_;
}
{% endhighlight %}      
