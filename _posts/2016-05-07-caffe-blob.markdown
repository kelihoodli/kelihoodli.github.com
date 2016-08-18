---
layout: post
category: "reading"
title: "Caffe Blob"
tags: [learning,deep learning,caffe]
---    

Blob是一个4阶的tensor，表示为[N, K, H, W]. 在实际的代码实现中是row-major形式的一维数组存储的. W是粒度最小的维度，变化最快. 在(n, k, h, w)处的元素，其物理偏移量是$$((n\times K + k) \times H + h) \times W + w$$    
举例:   
在CTR预估中存储训练数据的Blob可以是    
{% highlight xml linenos=table %}
batch_size: 100   
channels: 1000   
height: 1   
width: 1
{% endhighlight %}    

N=batch_size表示min-batch SGD一次学习的样本量     

K=channels表示特征的维度       

<height, width>表示每个特征值的维度. CTR预估中特征值是离线的实值特征，维度是$$1 \times 1$$         

因此，总共有training_dataset.size() / N 个Blob来存储训练数据.    

此外, Blob的Update函数用来被Net中存储参数的Blob调用，完成梯度下降过程的参数更新:     
$$data_{k+1} = data_k - diff$$     
对应的代码实现是:    
{% highlight xml linenos=table %}    
// perform computation on CPU     
caffe_axpy<Dtype>(count_, Dtype(-1) 
			,static_cast<const Dtype*>(diff_->cpu_data())
			,static_cast<Dtype*>(data_->mutable_cpu_data())
		);
{% endhighlight %}     

Blob操作的代码示例:     
{% highlight xml linenos=table %}       
#include <vector>
#include <caffe/blob.hpp>
#include <caffe/util/io.hpp>
using namespace caffe;
using namespace std;

int main(int argc, char** argv) {
	Blob<float> a;
	cout << "Size: " << a.shape_string() << endl;
	a.Reshape(1, 2, 3, 4);
	cout << "Size: " << a.shape_string() << endl;
	float *p = a.mutable_cpu_data();
	float *q = a.mutable_cpu_diff();
	for (int i = 0; i < a.count(); i++) {
		p[i] = i;
		q[i] = a.count() - 1 - i;
	}

	a.Update();

	for (int u = 0; u < a.num(); u++) {
		for (int v = 0; v < a.channels(); v++) {
			for (int w = 0; w < a.height(); w++) {
				for (int x = 0; x < a.width(); x++) {
					cout << "a[" << u << "][" << v << "][" << "][" << w << "][" << x << "]" 
						 << a.data_at(u, v, w, x) << endl;
				}
			}
		}
	}

	cout << "ASUM: " << a.asum_data() << endl;
	cout << "SUMSQ: " << a.sumsq_data() << endl;

	BlobProto bp;
	a.ToProto(&bp, true);
	WriteProtoToTextFile(bp, "a.txt");

	BlobProto bpr;

	ReadProtoFromTextFile("a.txt", &bpr);
	Blob<float> b;
	b.FromProto(bpr, true);


	for (int u = 0; u < a.num(); u++) {
		for (int v = 0; v < a.channels(); v++) {
			for (int w = 0; w < a.height(); w++) {
				for (int x = 0; x < a.width(); x++) {
					cout << "b[" << u << "][" << v << "][" << "][" << w << "][" << x << "]"
						<< b.data_at(u, v, w, x) << endl;
				}
			}
		}
	}

	return 0;
}
{% endhighlight %}    



#### refs      
1. [Blobs, Layers, and Nets: anatomy of a Caffe model](http://caffe.berkeleyvision.org/tutorial/net_layer_blob.html)   
2. [Caffe源码解析之Blob](http://imbinwang.github.io/blog/inside-caffe-code-blob)

