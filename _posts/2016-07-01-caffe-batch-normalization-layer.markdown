---
layout: post
category: "reading"
title: "Caffe Batch Normalization Layer"
tags: [learning,deep learning,caffe]
---

Batch Normalization Layer没有学习$$\gamma$$和$$\beta$$，需要再后面加Scale Layer  
{% highlight xml linenos=table %}
layer {
  name: "scale"   
  type: "ScaleLayer"
  bottom: "BatchNorm"
  top: "scale"
  scale_param {bias_term: true}
}
{% endhighlight %}    

#### ref   
1. [Ioffe S, Szegedy C. Batch normalization: Accelerating deep network training by reducing internal covariate shift[J]. arXiv preprint arXiv:1502.03167, 2015.](http://arxiv.org/abs/1502.03167)
2. [Understanding the backward pass through Batch Normalization Layer](https://kratzert.github.io/2016/02/12/understanding-the-gradient-flow-through-the-batch-normalization-layer.html)
3. [Batch Normalization](http://shuokay.com/2016/05/28/batch-norm/)   
4. [从Bayesian角度浅析Batch Normalization](http://www.cnblogs.com/neopenx/p/5211969.html)      
5. [batch_norm_layer.hpp](https://github.com/BVLC/caffe/blob/master/include/caffe/layers/batch_norm_layer.hpp#L25-L27)    
6. [Batch Normalization   Loss is not decreased](https://github.com/BVLC/caffe/issues/3347)     
7. [is Batch Normalization supported by Caffe?](https://groups.google.com/forum/#!topic/caffe-users/h4E6FV_XkfA)    
8. [解读Batch Normalization](http://blog.csdn.net/shuzfan/article/details/50723877)        
9. [caffe层解读系列——BatchNorm](http://blog.csdn.net/shuzfan/article/details/52729424)      

