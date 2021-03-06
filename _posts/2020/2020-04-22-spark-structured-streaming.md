---
title:  "《Spark编程基础(Python版)》"
date:   2020-04-22 10:10:01 +0800
---

《Spark编程基础（Python版）》(人民邮电出版社，ISBN:978-7-115-52439-3）由国内高校知名大数据教师厦门大学林子雨副教授主编，有幸在林子雨指导下，我也参与了其中第7章Structured Streaming的编写。教材官网为 http://dblab.xmu.edu.cn/post/spark-python/ 。

认识林子雨老师是被他的数字教师和实验室主页震撼，他几乎事无巨细地记录了工作学习中的点点滴滴，这还是很少见的。

| 数字教师：基础数据+对外智能服务。通过文本、图片、视频、音频等方式，全面记录个人工作、学习和交流活动的方方面面、点点滴滴，把现实物理世界中的我的科研、教学、培养学生的过程、方法和成果，以数据的形式，全面数字化到“数据自然界”，在另外一个世界（数据自然界）形成一个虚拟的数字人，这个数字人以数据的形式存在，保存在网络空间中，通过网页形式对外呈现，现在和未来的访问者，可以跨越时空界限，随时随地通过网络访问“我的数字人”（数字教师），了解我的所有历史信息，包括职业历程、工作内容、科研教学方法等。http://dblab.xmu.edu.cn/post/2988/

这个理念跟校园物理空间映射到虚拟空间的智慧校园建设不谋而合。想要智慧，必须在平时就要收集你现在可能看不到价值的数据。

## 教材

![](/images/2020/spark.jpg)

教材章节目录如下：

    第1章 大数据技术概述
    第2章 Spark的设计与运行原理
    第3章 Spark环境搭建和使用方法
    第4章 RDD编程
    第5章 Spark SQL
    第6章 Spark Streaming
    第7章 Structured Streaming
    第8章 Spark MLlib

## Spark是什么

| Apache Spark是一个开源集群运算框架，最初是由加州大学柏克莱分校AMPLab所开发。相对于Hadoop的MapReduce会在运行完工作后将中介数据存放到磁盘中，Spark使用了存储器内运算技术，能在数据尚未写入硬盘时即在存储器内分析运算。Spark在存储器内运行程序的运算速度能做到比Hadoop MapReduce的运算速度快上100倍，即便是运行程序于硬盘时，Spark也能快上10倍速度。Spark允许用户将数据加载至集群存储器，并多次对其进行查询，非常适合用于机器学习算法。

## Spark Structured Streaming是什么

Apache Spark在2016年启动了Structured Streaming项目，使用Spark2.0全新设计开发的流式引擎，整合了**批处理和流处理**，通过**一致的API使得使用者可以像写批处理程序一样编写流处理程序**。

Structured Streaming处理的数据跟Spark Streaming一样，也是源源不断的数据流，区别在于，Spark Streaming采用的数据抽象是DStream（本质上就是一系列RDD），而Structured Streaming采用的数据抽象是DataFrame。Structured Streaming可以使用Spark SQL的DataFrame/Dataset来处理数据流。虽然Spark SQL也是采用DataFrame作为数据抽象，但是，Spark SQL只能处理静态的数据，而Structured Streaming可以处理结构化的数据流。这样，Structured Streaming就将Spark SQL和Spark Streaming二者的特性结合了起来。

同类比较火的还有Flink和Blink等等。

## 唯快不破

Spark从2.3.0版本开始引入了持续处理的试验性功能，可以实现流计算的**毫秒级**延迟。采用微批处理模型时可以实现100毫秒级别的实时响应，采用持续处理模型时可以支持毫秒级的实时响应。在持续处理模式下，Spark不再根据触发器来周期性启动任务，而是启动一系列的连续读取、处理和写入结果的长时间运行的任务，并在其中实现了异步的预写日志和检查点机制。虽然持续处理模型要比微批处理模型获得更好的实时响应性能，但是，这是以**牺牲一致性为代价**的，微批处理可以保证端到端的完全一致性，而持续处理只能做到“至少一次”的一致性。

Spark演进还是比较快的，在我写的时候是2.3，现在已经是2.4.5了。有些内容可能已经变化了。

## 让你知道我去年夏天干了什么

我主要从 https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html 文档开始，利用Visio重画了图，对文档结构进行了调整，聚焦在偏中初级可实际操作的内容上，以更容易理解的方法翻译成中文，我设计了7个Python小程序（通过了PEP8检查）和一个实验，并设计了几个小习题。

## 第7章 Structured Streaming写了啥

第7章首先介绍Structured Streaming的基本概念，比较其与Spark SQL和Spark Streaming的关系，并以一个简单的实例来介绍编写Structured Streaming程序的基本步骤。接着详细介绍程序的输入输出操作，包括自带的输入源、输出模式和接收器。然后介绍流处理里面最重要的容错机制，包括如何从检查点进行故障恢复，以及故障恢复中如果对查询进行变更时会存在哪些限制等。接下来介绍Structured Streaming新引入的事件时间，介绍如何以数据生成的时间而不是Spark接收到数据的时间来做数据处理的方法，并介绍用水印来处理迟到数据的机制。最后，介绍如何实现查询的管理和监控。

子章节目录如下：

    第7章 Structured Streaming　150

    7.1　概述　151
        7.1.1　基本概念　151
        7.1.2　两种处理模型　152
        7.1.3　Structured Streaming 和Spark SQL、Spark Streaming 的关系　154
    7.2　编写Structured Streaming程序的基本步骤　154
        7.2.1　实现步骤　154
        7.2.2　测试运行　156
    7.3　输入源　158
        7.3.1　File 源　158
        7.3.2　Kafka 源　163
        7.3.3　Socket 源　167
        7.3.4　Rate 源　167
    7.4　输出操作　169
        7.4.1　启动流计算　169
        7.4.2　输出模式　170
        7.4.3　输出接收器　170
    7.5　容错处理　173
        7.5.1　从检查点恢复故障　173
        7.5.2　故障恢复中的限制　173
    7.6　迟到数据处理　174
        7.6.1　事件时间　174
        7.6.2　迟到数据　175
        7.6.3　水印　176
        7.6.4　多水印规则　177
        7.6.5　处理迟到数据的实例　178
    7.7　查询的管理和监控　181
        7.7.1　管理和监控的方法　181
        7.7.2　一个监控的实例　182
    7.8　本章小结　184
    7.9　习题　185

    实验6　Structured Streaming编程实践　185
