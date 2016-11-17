---
layout: post
category: "reading"
title: "Caffe Parameterized Rectified Linear Unit Layer"
tags: [learning,deep learning,caffe]
---  

#### 一、LayerSetUp     
根据样本特征个数channels设置Weight矩阵为$$R^{channels}$$.   
根据PReLUParameter初始化Weight矩阵     

#### 三、Reshape     
PReLULayer的input blob和output blob的shape一致.       

#### 三、Forward_cpu     
实现$$y_i = \max(0, x_i) + \alpha_i \min(0, x_i)$$       
count是$$R^{\text{batch_size} \times \text{num_outputs}}$$    
dim是$$height \times width$$      
channels是num_inputs or num_outputs, 即输入神经元和输出神经元个数.   
`int c = (i / dim) % channels /div_factor;`等价于`int c = i % channles;`    
因为`dim=height *  width = 1 * 1, div_factor = 1;`      

#### 四、Backword_cpu   
1. 计算PReLU关于$$\alpha_i$$的梯度, 结果放入slope_diff          
$$\frac{\nabla E}{\nabla \alpha_i}=\sum_{y_i}\frac{\nabla E}{\nabla f(y_i)}\frac{\nabla f(y_i)}{\nabla \alpha_i}$$. 具体地: 
$$
        \frac{\nabla E}{\nabla \alpha_i^{(l)}} =
        \begin{cases}
        \alpha_i^{(l)}\sigma_i^{(l+1)},  & \text{if $a_j^{(l)} \le 0$} \\
        0, & \text{if $a_j^{(l)} \gt 0$}
        \end{cases}
$$
2. 计算PReLU关于error term的梯度, 结果放入bottom_diff         
$$
        \sigma_i^{(l)} =
        \begin{cases}
        \sigma_i^{(l)},  & \text{if $a_i^{(l)} \gt 0$} \\
        \alpha_i^{(l)}\sigma_i^{(l)}, & \text{if $a_j^{(l)} \le 0$}
        \end{cases}
$$
    
PReLU的反向传播算法思路和InnerProductLayer是一致的, 可以参考UFLDL<sup>[2](http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95)</sup>自己做一次简单的推导.   

#### ref     
1. [caffe::PReLULayer< Dtype > Class Template](http://caffe.berkeleyvision.org/doxygen/classcaffe_1_1PReLULayer.html)     
2. [反向传导算法](http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95)
3. [Cmd Markdown 公式指导手册](https://www.zybuluo.com/codeep/note/163962)      
4. [《Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification》阅读笔记与实现](http://blog.csdn.net/happynear/article/details/45440811)

