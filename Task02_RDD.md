# Task 2

内容：RDD编程，熟悉算子，读写文件

第3章 Spark编程基础

[3.1 Spark入门：RDD编程](http://dblab.xmu.edu.cn/blog/1700-2/)

[3.2 Spark入门：键值对RDD](http://dblab.xmu.edu.cn/blog/1706-2/)

[3.3 Spark入门：共享变量](http://dblab.xmu.edu.cn/blog/1707-2/)（提升-分布式必备）

3.4 数据读写

[3.4.1 Spark入门：文件数据读写](http://dblab.xmu.edu.cn/blog/1708-2/)

问题扩展：请用RDD实现一个PAT排名器，题目要求

[参考](https://pintia.cn/problem-sets/16/problems/677)

------------------



[RDD编程PPT](https://study.163.com/course/courseLearn.htm?courseId=1209408816#/learn/text?lessonId=1279275509&courseId=1209408816)

#### 持久化

在 Spark 中, RDD 采用惰性求值的机制,每次遇到行动操作,都会从头开始执行计算.每次调用行动操作,都会触发一次从头开始的计算.这对于迭代计算而言,代价很大的,迭代计算经常需要多次重复使用同一组数据

* 持久化(缓存)机制避免这种重复计算的开销,可以使用 persist()  方法对一个 RDD 标记为持久化
* 使用 cache() 方法时,会调用 persist(MEMORY_ONLY)
* 使用 unpersist() 方法手动把持久化的 RDD 从缓存中移除

#### 共享变量

Spark中的两个重要抽象是RDD和共享变量。上一章我们已经介绍了RDD，这里介绍共享变量。

在默认情况下，当Spark在集群的多个不同节点的多个任务上并行运行一个函数时，它会把函数中涉及到的每个变量，在每个任务上都生成一个副本。但是，有时候，需要在多个任务之间共享变量，或者在任务（Task）和任务控制节点（Driver Program）之间共享变量。为了满足这种需求，Spark提供了两种类型的变量：广播变量（broadcast variables）和累加器（accumulators）。广播变量用来把变量在所有节点的内存之间进行共享。累加器则支持在所有不同节点之间进行累加计算（比如计数或者求和）

#### **aggregateByKey函数：**

```
对PairRDD中相同的Key值进行聚合操作，在聚合过程中同样使用了一个中立的初始值。和aggregate函数类似，aggregateByKey返回值的类型不需要和RDD中value的类型一致。因为aggregateByKey是对相同Key中的值进行聚合操作，所以aggregateByKey'函数最终返回的类型还是PairRDD，对应的结果是Key和聚合后的值，而aggregate函数直接返回的是非RDD的结果。
```



例子程序：

import org.apache.spark.SparkConf
import org.apache.spark.SparkContext

object AggregateByKeyOp {
  def main(args:Array[String]){
     val sparkConf: SparkConf = new SparkConf().setAppName("AggregateByKey").setMaster("local")
    val sc: SparkContext = new SparkContext(sparkConf)
     
     val data=List((1,3),(1,2),(1,4),(2,3))
     val rdd=sc.parallelize(data, 2)
     
     //合并不同partition中的值，a，b得数据类型为zeroValue的数据类型
     def combOp(a:String,b:String):String={
       println("combOp: "+a+"\t"+b)
       a+b
     }
     //合并在同一个partition中的值，a的数据类型为zeroValue的数据类型，b的数据类型为原value的数据类型
      def seqOp(a:String,b:Int):String={
        println("SeqOp:"+a+"\t"+b)
        a+b
      }
      rdd.foreach(println)
      //zeroValue:中立值,定义返回value的类型，并参与运算
      //seqOp:用来在同一个partition中合并值
      //combOp:用来在不同partiton中合并值
      val aggregateByKeyRDD=rdd.aggregateByKey("100")(seqOp, combOp)
      sc.stop()
  }
}

运行结果：

将数据拆分成两个分区

//分区一数据
(1,3)
(1,2)
//分区二数据
(1,4)
(2,3)

//分区一相同key的数据进行合并
seq: 100     3   //(1,3)开始和中立值进行合并  合并结果为 1003
seq: 1003     2   //(1,2)再次合并 结果为 10032

//分区二相同key的数据进行合并
seq: 100     4  //(1,4) 开始和中立值进行合并 1004
seq: 100     3  //(2,3) 开始和中立值进行合并 1003

将两个分区的结果进行合并
//key为2的，只在一个分区存在，不需要合并 (2,1003)

### (2,1003)

//key为1的, 在两个分区存在，并且数据类型一致，合并
comb: 10032     1004
### (1,100321004)

原文链接：https://blog.csdn.net/u013514928/article/details/56680825