**ChenPengNotes**

# Spark
##  解决问题的层面不一样
- Hadoop和Spark两者都是大数据框架，但是各自存在的目的不尽相同。

- Hadoop实质上是解决大数据大到无法在一台计算机上进行存储、无法在要求的时间内进行处理的问题，是一个分布式数据基础设施。
- HDFS，它将巨大的数据集分派到一个由普通计算机组成的集群中的多个节点进行存储，通过将块保存到多个副本上，提供高可靠的文件存储。
- MapReduce，通过简单的Mapper和Reducer的抽象提供一个编程模型，可以在一个由几十台上百台的机器上并发地分布式处理大量数据集，而把并发、分布式和故障恢复等细节隐藏。
- Hadoop复杂的数据处理需要分解为多个Job（包含一个Mapper和一个Reducer）组成的有向无环图。
- Spark则允许程序开发者使用有向无环图（DAG）开发复杂的多步数据管道。而且还支持跨有向无环图的内存数据共享，以便不同的作业可以共同处理同一个数据。是一个专门用来对那些分布式存储的大数据进行处理的工具，它并不会进行分布式数据的存储。
- 可将Spark看作是Hadoop MapReduce的一个替代品而不是Hadoop的替代品。
# Hadoop的局限和不足
- 一个Job只有Map和Reduce两个阶段，复杂的计算需要大量的Job完成，Job间的依赖关系由开发人员进行管理。
- 中间结果也放到HDFS文件系统中。对于迭代式数据处理性能比较差。
- Reduce Task需要等待所有的Map Task都完成后才开始计算。
- 时延高，只适用批量数据处理，对于交互式数据处理，实时数据处理的支持不够。
# 两者可合可分
- Hadoop除了提供HDFS分布式数据存储功能之外，还提供了MapReduce的数据处理功能。所以我们完全可以抛开Spark，仅使用Hadoop自身的MapReduce来完成数据的处理。
- 相反，Spark也不是非要依附在Hadoop身上才能生存。但它没有提供文件管理系统，所以，它必须和其他的分布式文件系统进行集成才能运作。我们可以选择Hadoop的HDFS，也可以选择其他的基于云的数据系统平台。但Spark默认来说还是被用在Hadoop上面的，被认为它们的结合是最好的选择。


  **[✒spark入门与搭建](01.md)**		

  **[✒RDD弹性分布式数据集](02.md)**	

  **[✒自定义排序_JdbcRDD](03.md)**	

  **[✒Spark任务执行的流程](04.md)**	

  **[✒SparkSQL](05.md)**	

  **[✒SparkSQL_Join](06.md)**	

  **[✒SparkSQL_UDF](07.md)**	

  **[✒SparkSQL_Hive](08.md)**	

  **[✒SparkOnYarn](09.md)**	

# spark原理
![spark原理](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190508213424.png)

# RDD排序
![RDD排序](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190509163933.png)

# spark执行过程
![spark执行过程](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190509195131.png)