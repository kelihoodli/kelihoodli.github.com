---
layout: post
category: "excerpt"
title: "Training vernus Testing"
tags: [excerpt,reading]
---
1). generalization bound
![drawing](/assets/image/errorbound.png) 

The bound is polynomia is crucial. If $$m_{\cal H}(N)$$ can be bounded by a polynomial - any polynomial -, the generalizatoin error will go to zero as $$ N \rightarrow \infty$$, since 

2). ***shatter***   
if $$\cal H$$ is capable of generating all possible dichotomies on $$\mit x_1, ..., x_N$$, then $$\cal H$$ $$\mit(x_1, ..., x_N) = {-1, +1}^N$$ and we say that $$\mit H$$ can *shatter* $$\mit x_1, ..., x_N$$.    

3). The VC Dimension    
The Vapink-Chervoncenkis dimension of a hypothesis set $$\cal H$$, denoted by $$d_{VC}$$ $$\cal (H)$$ or $$d_{VC}$$, is the largest value of N for which $$m_{\cal H}(N)=2^N.$$ If $$m_{\cal H}=2^N$$ for all N, then $$d_{VC}(\cal H)=\infty$$  

$$
    m_{\cal H}(N) \le \sum_{i=0}^{d_{VC}}(\begin{matrix} N \\ i \\ \end{matrix})
$$

4). VC generation bound  
For any tolerance $$\delta \gt 0$$, 

$$
    E_{out}(g) \le E_{in}(g) + \sqrt{\frac{8}{N}\text{ln}\frac{4m_{\cal H}(2N)}{\delta}}
$$

with probability $$\ge 1 - \delta$$

5). Sample Complexity   
    The sample complexity denotes how many training examples $$N$$ are needed to achieve a certain generalization performance.   

$$
    N \ge \frac{8}{\epsilon^2}\text{ln}(\frac{4(2N)^{d_{VC}}+1}{\delta})
$$  

where $$\epsilon$$ is generalization error, $$\delta$$ the confidence parameter.   
6).  Penalty for Model Complexity   

$$
    E_{\text{out}}(g) \le E_{\text{in}}(g) + \Omega(N, H, \delta)
$$  

where 
\begin{align}
    \Omega(N, H, \delta) = \sqrt{\frac{8}{N}\text{ln}\frac{4m_{\cal H}(2N)}{\delta}} \\ 
\le \sqrt{\frac{8}{N}\text{ln}\frac{4((2N)^{d_{VC}}+1)}{\delta}}
\end{align}
![drawing](/assets/image/we.jpeg)   

![drawing](/assets/image/wesec.jpeg)

