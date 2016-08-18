---
layout: post
category: "reading"
title: "FM on Spark"
tags: [learning]
---    
对FM的模型公式解耦得到:        
$$y(x) = w_0 + \sum_{i=1}^nw_ix_i+\frac{1}{2}\sum_{f=1}^k((\sum_{i=1}^nv_{i,f}x_i)^2-\sum_{i=1}^nv_{i,f}^2x_{f}^2)$$       
其计算复杂度从$$O(kn^2)$$下降到线性复杂度$$O(kn)$$     

Spark上实现分布式算法仍然是典型的MPI的AllReduce方法. 首先将训练数据随机shuffle到不同的RDD中，在每个节点上对每个RDD训练一个局部模型. 每个epoch之后，合并(average)各个节点上的模型参数，broadcast到所有节点，作为下一个epoch训练的base model.      

一些可供参考的tricks:     
1. 对每阶FM按照不同threshod过滤噪声   
2. 二次抽样, 保证算法在合适的时间内收敛. 预测前再对ctr做calibration   
3. Shuffle训练数据. 很多训练数据是按时间先后收集的，排除样本顺序对模型训练的干扰.      
4.  在子训练数据上多轮训练. 充分利用训练数据, 提高收敛的迭代次数      
5.  采用SGD学习时，[利用权重向量/矩阵的稀疏性](https://github.com/npinto/bottou-sgd/blob/master/README.txt)，进行优化处理.    
6.  [AdaGrad](http://www.jmlr.org/papers/volume12/duchi11a/duchi11a.pdf)(Adaptive Subgradient Methods)      
7.  Hogwild!在数据非常稀疏的情况下，利用多CPU能够得到近似最优的收敛速度


#### useful ref   
1. [Factorization Machines 学习笔记（二）模型方程](http://blog.csdn.net/itplus/article/details/40534923)    
2. [Factorization Machines 学习笔记（四）学习算法](http://blog.csdn.net/itplus/article/details/40536025)    
3. [An overview of gradient descent optimization algorithms](http://sebastianruder.com/optimizing-gradient-descent/index.html#adagrad)     
4. [HOGWILD! Code and Data](http://i.stanford.edu/hazy/victor/Hogwild/) 
