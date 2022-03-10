# 物联网概论 PART-11



MapReduce,Spark框架
<!--more-->

# MapReduce
MapReduce是一种分布式并行编程框架
## Handoop的概念
Hadoop是一个能够对大量数据进行分布式处理的软件框架，实现了Google的MapReduce编程模型和框架，能够把应用程序分割成许多的小的工作单元，并把这些单元放到任何集群节点上执行。在MapReduce中，一个准备提交执行的应用程序称为“作业（job）”，而从一个作业划分出 得、运行于各个计算节点的工作单元称为“任务（task）”。此外，Hadoop提供的分布式文件系统（HDFS）主要负责各个节点的数据存储，并实现了高吞吐率的数据读写。
其中HDFS在Part-9的最后有所介绍

## 数据处理能力提升的两种方式
 - 单机情况下的纵向扩展 
 - 分布式并行的横向扩展

## MapReduce核心思想
### 分而治之的策略
 - 当我们在做大规模数据处理的时候,MapReduce会把非常庞大的数据集切分成很多个小分片
 - 然后为每一个分片单独地启动一个Map任务
 - 最终通过多个Map任务,并行地在多个机器上去处理,从而实现分而治之

### 理念
计算向数据靠拢
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/1.png)

## 基本原理
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/2.png)
### Map函数
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/3.png)

### Reduce函数
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/4.png)

### MapReduce处理流程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/5.png)

## 应用实例
### 任务目标
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/6.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/7.png)

### 执行过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/8.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/9.png)

# 物联网并行处理 Spark框架
## 概念
 - Spark最初是由美国加州伯克利大学(UCBerkeley)的AMP实验室于2009年开发的,它是一个基于内存计算的大数据并行计算框架,可用于构建大型的、低延迟的数据分析应用程序
 - 2013年加入Apache孵化器项目后发展迅猛,已成为Apache软件基金会最重要的三大分布式计算系统开源项目之一(Hadoop,Spark,Storm)

## Scala语言
Scala 是一门现代的多范式编程语言
 - 函数式编程:Lisp语言,Haskell语言
 - 面向对象编程:Java
 - 面向过程编程:C语言
多范式: 集成了面向对象和函数式编程两种风格,可以运行于Java平台,并兼容现有的Java程序
 - Scala具备强大的并发性,支持函数式编程,可以更好地支持分布式系统
 - Scala语法简洁,能提供优雅的API
 - Scala兼容Java,运行速度快,且能融合到Hadoop生态圈中

## Spark与Hadoop对比
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/10.png)
2014年,Spark打破了Hadoop保持的基准排序记录
 - Spark: 23分钟,206个节点,100TB数据
 - Hadoop:72分钟,2000个节点,100TB数据
 - Spark用十分之一的计算资源获得了比Hadoop快3倍的速度
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/13.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/11.png)

## 计算过程对比
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/12.png)

## Spark组成
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/14.png)

## 运算过程
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/15.png)
![img](https://tronwei-1254020584.cos.ap-beijing.myqcloud.com/IOT-11/16.png)

## Spark的优点
 - 利用多线程来执行具体的任务,减少了任务的启动开销
 - Executor中有一个BlockManager存储模块,会将内存和磁盘共同作为存储设备,有效减少IO开销
