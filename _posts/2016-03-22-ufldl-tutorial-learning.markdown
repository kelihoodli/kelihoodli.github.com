---
layout: post
category: "reading"
title: "ufldl-tutorial-learning"
tags: [learning,deep learning]
---    
1. 学习Sparse Autoencoder时，对于神经网络为什么要使用反向传播求梯度不理解:     
可以参考:  
[如何理解神经网络里面的反向传播算法?](https://www.zhihu.com/question/24827633/answer/91489990)中, [@Linglai Li](https://www.zhihu.com/people/lilinglai)的理解 以及 [@陈唯源](https://www.zhihu.com/people/chen-wei-yuan)提供的参考链接.    
2. 深度学习的Local optima问题      
当训练点击率预估的凸优化问题, 最小化cross-entropy, 时，还会有Local optima的问题吗，怎么理解?    
[li Eta](https://www.zhihu.com/people/li-eta)给出了[很好的解释](https://www.zhihu.com/question/38549801/answer/77027608). 虽然神经网络的最后一层可以采用凸函数,  
但是反向传播算法在计算整个网络的梯度时，需要和前面$$L-1$$层的激活函数一起考虑，导致最后的Loss function并不是凸的.   


3. Diffusion of gradients问题, 实践的时候观察一下.      
这个问题[Michael Nielsen](http://michaelnielsen.org/)在[Why are deep neural networks hard to train?](http://neuralnetworksanddeeplearning.com/chap5.html)  里面做了讨论.   
采用每个隐含层*i*的$$||w_i||$$和$$||b_i||$$来衡量每个隐含层的学习速率. 并不严谨的证明了是因为每个神经元的$$w_j\sigma^{'}(z_j)$$很小引起的.   


 

