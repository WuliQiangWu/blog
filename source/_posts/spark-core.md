---
title: PySpark实战之Spark Core
date: 2018-12-24 17:14:51
tags: 
- PySpark 
- RDD
categories:
- 技术类
- Spark
- python
description: Spark Core核心
---
##### 什么是RDD
> 打开 spark 源码 `https://github.com/apache/spark/blob/master/core/src/main/scala/org/apache/spark/rdd/RDD.scala`
          

           Resilient Distributed Dataset (RDD) 弹性 分布式 数据集
           the basic abstraction in Spark. spark中一个基本的抽象
            Represents an
             immutable ：不可变
             partitioned collection of elements ：分区
             that can be operated on in parallel. ：并行计算
            
           abstract class RDD[T: ClassTag](
                @transient private var _sc: SparkContext,
                @transient private var deps: Seq[Dependency[_]]
              ) extends Serializable with Logging {
                ···
                }
           1> RDD是一个抽象类
           2> 带范型，可以支持多种类型：string 
> spark 源码解释说 
        
        A Resilient Distributed Dataset (RDD), the basic abstraction in Spark. Represents an immutable,
        partitioned collection of elements that can be operated on in parallel. This class contains the
        basic operations available on all RDDs, such as `map`, `filter`, and `persist`. In addition,
        [[org.apache.spark.rdd.PairRDDFunctions]] contains operations available only on RDDs of key-value
        pairs, such as `groupByKey` and `join`;
        [[org.apache.spark.rdd.DoubleRDDFunctions]] contains operations available only on RDDs of
        Doubles; and
        [[org.apache.spark.rdd.SequenceFileRDDFunctions]] contains operations available on RDDs that
        can be saved as SequenceFiles.
        
> 单机存储/计算 ==> 分布式存储/计算
> 数据的存储：切割  HDFS的block
> 数据的计算：切割(分布式计算) MapReduce/Spark
> 存储+计算 : (切割)  HDFS + Spark
##### RDD特性
> RDD有五大特点
    
     Internally, each RDD is characterized by five main properties:
     - A list of partitions     
        一个由一系列分区组成的列表
     - A function for computing each split
        一个会动态计算每一个分区的方法
     - A list of dependencies on other RDDs
        依赖在其他RDD上  （每一个rdd都是不可变的，一个方法作用到一个rdd上之后，会生成一个新的rdd ）
     - Optionally, a Partitioner for key-value RDDs (e.g. to say that the RDD is hash-partitioned)
        根据键值进行rdd分区的程序 （告诉rdd如何进行分区）
     - Optionally, a list of preferred locations to compute each split on (e.g. block locations for an HDFS file)
        数据在哪就优先把作业调度到数据所在的节点进行计算：移动数据不如移动计算
        
##### RDD特性在源码中的体现
       
       def compute(split: Partition, context: TaskContext): Iterator[T] 特性2
       def getPartitions: Array[Partition]  特性1
       def getDependencies: Seq[Dependency[_]] = deps 特性3
       def getPreferredLocations(split: Partition): Seq[String] = Nil 特性5
       val partitioner: Option[Partitioner] = None  特性4
       
##### 图解RDD
##### SparkContext&SparkConf
##### pyspark脚本解析
##### RDD创建方式
##### Spark应用程序的开发及运行