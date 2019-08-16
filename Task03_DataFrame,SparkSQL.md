### Task03_DataFrame,SparkSQL

第4章

[4.1 Spark SQL简介](http://dblab.xmu.edu.cn/blog/1717-2/)

[4.2 DataFrame与RDD的区别](http://dblab.xmu.edu.cn/blog/1718-2/)

[4.3 DataFrame的创建](http://dblab.xmu.edu.cn/blog/1719-2/)

[4.4 从RDD转换得到DataFrame](http://dblab.xmu.edu.cn/blog/1720-2/)

[4.5.2 通过JDBC连接数据库(DataFrame)](http://dblab.xmu.edu.cn/blog/1724-2/)

第5章

[5.1 流计算简介](http://dblab.xmu.edu.cn/blog/1732-2/)

[5.2 Spark Streaming简介](http://dblab.xmu.edu.cn/blog/1733-2/)

第5.3节 DStream操作

[5.3.1 DStream操作概述](http://dblab.xmu.edu.cn/blog/1737-2/)

问题扩展：请简述下spark sql的运行机制。



----------------------

#### Spark SQL设计

Spark SQL增加了SchemaRDD（即带有Schema信息的RDD），使用户可以在Spark SQL中执行SQL语句，数据既可以来自RDD，也可以来自Hive、HDFS、Cassandra等外部数据源，还可以是JSON格式的数据。Spark SQL目前支持Scala、Java、Python三种语言，支持SQL-92规范。

[Spark-SQL支持的数据格式和编程语言](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE16-13-Spark-SQL%E6%94%AF%E6%8C%81%E7%9A%84%E6%95%B0%E6%8D%AE%E6%A0%BC%E5%BC%8F%E5%92%8C%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80.jpg)

DataFrame的推出，让Spark具备了处理大规模结构化数据的能力，不仅比原有的RDD转化方式更加简单易用，而且获得了更高的计算性能。**Spark能够轻松实现从MySQL到DataFrame的转化，并且支持SQL查询。**

打开pyspark：

```
cd /usr/local/spark
./bin/pyspark
```

```
>>> spark=SparkSession.builder.getOrCreate()
>>> df = spark.read.json("file:///usr/local/spark/examples/src/main/resources/people.json")
>>> df.show()
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+
```

常用的DataFrame操作:

```
// 打印模式信息
>>> df.printSchema()
root
 |-- age: long (nullable = true)
 |-- name: string (nullable = true)
 
// 选择多列
>>> df.select(df.name,df.age + 1).show()
+-------+---------+
|   name|(age + 1)|
+-------+---------+
|Michael|     null|
|   Andy|       31|
| Justin|       20|
+-------+---------+
 
// 条件过滤
>>> df.filter(df.age > 20 ).show()
+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+
 
// 分组聚合
>>> df.groupBy("age").count().show()
+----+-----+
| age|count|
+----+-----+
|  19|    1|
|null|    1|
|  30|    1|
+----+-----+
 
// 排序
>>> df.sort(df.age.desc()).show()
+----+-------+
| age|   name|
+----+-------+
|  30|   Andy|
|  19| Justin|
|null|Michael|
+----+-------+
 
//多列排序
>>> df.sort(df.age.desc(), df.name.asc()).show()
+----+-------+
| age|   name|
+----+-------+
|  30|   Andy|
|  19| Justin|
|null|Michael|
+----+-------+
 
//对列进行重命名
>>> df.select(df.name.alias("username"),df.age).show()
+--------+----+
|username| age|
+--------+----+
| Michael|null|
|    Andy|  30|
|  Justin|  19|
+--------+----+
```

------------------

#### Spark Streaming设计

Spark Streaming是Spark的核心组件之一，为Spark提供了可拓展、高吞吐、容错的流计算能力。如下图所示，Spark Streaming可整合多种输入数据源，如Kafka、Flume、HDFS，甚至是普通的TCP套接字。经处理后的数据可存储至文件系统、数据库，或显示在仪表盘里。

[Spark-Streaming支持的输入、输出数据源](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/11/%E5%9B%BE10-19-Spark-Streaming%E6%94%AF%E6%8C%81%E7%9A%84%E8%BE%93%E5%85%A5%E3%80%81%E8%BE%93%E5%87%BA%E6%95%B0%E6%8D%AE%E6%BA%90.jpg)

Spark Streaming最主要的抽象是DStream（Discretized Stream，离散化数据流），表示连续不断的数据流。

#### Spark Streaming与Storm的对比

**Spark Streaming和Storm最大的区别在于，Spark Streaming无法实现毫秒级的流计算，而Storm可以实现毫秒级响应。**
**Spark Streaming难以满足对实时性要求非常高（如高频实时交易）的场景，但足以胜任其他流式准实时计算场景。相比之下，Storm处理的单位为Tuple，只需要极小的延迟。**
Spark Streaming构建在Spark上，一方面是因为Spark的低延迟执行引擎（100毫秒左右）可以用于实时计算，另一方面，相比于Storm，RDD数据集更容易做高效的容错处理。此外，Spark Streaming采用的小批量处理的方式使得它可以同时兼容批量和实时数据处理的逻辑和算法，因此，方便了一些需要历史数据和实时数据联合分析的特定应用场合。

#### 请简述下spark sql的运行机制。

> **通用SQL执行原理**
>
> 1. 词法和语法解析Parse：生成逻辑计划
> 2. 绑定Bind：生成可执行计划
> 3. 优化Optimize：生成最优执行计划
> 4. 执行Execute：返回实际数据
>
> ##### Spark SQL运行架构
>
> Spark SQL对SQL语句的处理和关系型数据库类似，即词法/语法解析、绑定、优化、执行。Spark SQL会先将SQL语句解析成一棵树，然后使用规则(Rule)对Tree进行绑定、优化等处理过程。Spark SQL由Core、Catalyst、Hive、Hive-ThriftServer四部分构成：
>
> Core: 负责处理数据的输入和输出，如获取数据，查询结果输出成DataFrame等
>
> Catalyst: 负责处理整个查询过程，包括解析、绑定、优化等
>
> Hive: 负责对Hive数据进行处理
>
> Hive-ThriftServer: 主要用于对hive的访问
>
> 参考:https://blog.csdn.net/zhanglh046/article/details/78360980

