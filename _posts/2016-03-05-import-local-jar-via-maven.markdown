---
layout: "post"
category: "records"
title: "一个在IDEA上Maven使用本地Jar的问题"
tags: [IEDA,Maven]
---  
一开始我是一直使用Eclipse开发Spark程序的，因为它的自动编译很方便. 直到后来接受不了长时间的等待     
`The user operation is waiting for Building workspace to complete`

故而开始转入Idea，并使用Maven构建工程.   

公司使用的Spark版本是编译后的`spark-assembly-1.0.2-hadoop2.2.0.jar`, 在Maven库中没有, 需要从本地导入.   

在google了几种方法后, 个人认为通过`mvn install`命令将Jar包打入到本地repository中是比较好的方式，这与根据pom.xml配置, 从Maven库中下载到repository, 之后再使用是较为一致的.   
具体的命令是:   
`mvn install:install-file -Dfile=spark-assembly-1.0.2-hadoop2.2.0.jar -DartifactId=spark-assembly -Dversion=1.0.2 -Dpackage=jar 
`

之后就可以在pom.xml中配置:

{% highlight xml linenos=table %}
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-assembly</artifactId>
    <version>1.0.2</version>
    <scope>provided</scope>
<dependency>
{% endhighlight %}


在学习一个新工具时，以往我是google，有答案了之后就接着做后面的事情，很有可能下次还会遇到相同的问题。虽然当时解决过，然而已经记不清具体的细节。所以，我还是写写记录一下吧。 






