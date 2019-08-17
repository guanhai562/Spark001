### Task04_MLlib流设计，特征工程 

第6章 Spark MLlib

[6.1 Spark MLlib简介](http://dblab.xmu.edu.cn/blog/1762-2/)

6.2 机器学习工作流

[6.2.1 机器学习工作流(ML Pipelines) ](http://dblab.xmu.edu.cn/blog/1763-2/)

[6.2.2 构建一个机器学习工作流](http://dblab.xmu.edu.cn/blog/1764-2/)

6.3 特征抽取、转化和选择

[6.3.1 特征抽取：TF-IDF](http://dblab.xmu.edu.cn/blog/1766-2/)

[6.3.4 特征变换：标签和索引的转化](http://dblab.xmu.edu.cn/blog/1770-2/)

[6.3.5 特征选取：卡方选择器](http://dblab.xmu.edu.cn/blog/1771-2/)

-------------------

#### 基于大数据的机器学习

在大数据上进行机器学习，需要处理全量数据并进行大量的迭代计算，这要求机器学习平台具备强大的处理能力。Spark 立足于内存计算，天然的适应于迭代式计算。即便如此，对于普通开发者来说，实现一个分布式机器学习算法仍然是一件极具挑战的事情。幸运的是，Spark提供了一个基于海量数据的机器学习库，它提供了常用机器学习算法的分布式实现，开发者只需要有 Spark 基础并且了解机器学习算法的原理，以及方法相关参数的含义，就可以轻松的通过调用相应的 API 来实现基于海量数据的机器学习过程。其次，Spark-Shell的即席查询也是一个关键。算法工程师可以边写代码边运行，边看结果。spark提供的各种高效的工具正使得机器学习过程更加直观便捷。比如通过sample函数，可以非常方便的进行抽样。**当然，Spark发展到后面，拥有了实时批计算，批处理，算法库，SQL、流计算等模块等，基本可以看做是全平台的系统。把机器学习作为一个模块加入到Spark中，也是大势所趋。**

#### Spark 机器学习库MLLib

MLlib目前支持4种常见的机器学习问题: 分类、回归、聚类和协同过滤。下表列出了目前MLlib支持的主要的机器学习算法：

http://dblab.xmu.edu.cn/blog/wp-content/uploads/2016/12/MLTable.png

#### 机器学习工作流(ML Pipelines)

一个典型的机器学习过程从数据收集开始，要经历多个步骤，才能得到需要的输出。这非常类似于流水线式工作，即通常会包含源数据ETL（抽取、转化、加载），数据预处理，指标提取，模型训练与交叉验证，新数据预测等步骤。

在介绍工作流之前，我们先来了解几个重要概念：

> - DataFrame：使用Spark SQL中的DataFrame作为数据集，它可以容纳各种数据类型。 较之 RDD，包含了 schema 信息，更类似传统数据库中的二维表格。它被 ML Pipeline 用来存储源数据。例如，DataFrame中的列可以是存储的文本，特征向量，真实标签和预测的标签等。
>
> - Transformer：翻译成转换器，是一种可以将一个DataFrame转换为另一个DataFrame的算法。比如一个模型就是一个 Transformer。它可以把 一个不包含预测标签的测试数据集 DataFrame 打上标签，转化成另一个包含预测标签的 DataFrame。技术上，Transformer实现了一个方法transform（），它通过附加一个或多个列将一个DataFrame转换为另一个DataFrame。
>
>   
>
> - Estimator：翻译成估计器或评估器，它是学习算法或在训练数据上的训练方法的概念抽象。在 Pipeline 里通常是被用来操作 DataFrame 数据并生产一个 Transformer。从技术上讲，Estimator实现了一个方法fit（），它接受一个DataFrame并产生一个转换器。如一个随机森林算法就是一个 Estimator，它可以调用fit（），通过训练特征数据而得到一个随机森林模型。
>
> - Parameter：Parameter 被用来设置 Transformer 或者 Estimator 的参数。现在，所有转换器和估计器可共享用于指定参数的公共API。ParamMap是一组（参数，值）对。
>
> - PipeLine：翻译为工作流或者管道。工作流将多个工作流阶段（转换器和估计器）连接在一起，形成机器学习的工作流，并获得结果输出。

#### 特征提取:TF-IDF (HashingTF and IDF)

在Spark ML库中，TF-IDF被分成两部分：TF (+hashing) 和 IDF。

TF: HashingTF 是一个Transformer，在文本处理中，接收词条的集合然后把这些集合转化成固定长度的特征向量。这个算法在哈希的同时会统计各个词条的词频。

IDF: IDF是一个Estimator，在一个数据集上应用它的fit（）方法，产生一个IDFModel。 该IDFModel 接收特征向量（由HashingTF产生），然后计算每一个词在文档中出现的频次。IDF会减少那些在语料库中出现频率较高的词的权重。

#### 特征变换–标签和索引的转化

Spark ML包中提供了几个相关的转换器，例如：StringIndexer、IndexToString、OneHotEncoder、VectorIndexer，它们提供了十分方便的特征转换功能，这些转换器类都位于org.apache.spark.ml.feature包下。

#### 特征选取–卡方选择器(Python版)

特征选择（Feature Selection）指的是在特征向量中选择出那些“优秀”的特征，组成新的、更“精简”的特征向量的过程。它在高维数据分析中十分常用，可以剔除掉“冗余”和“无关”的特征，提升学习器的性能。

特征选择方法和分类方法一样，也主要分为有监督（Supervised）和无监督（Unsupervised）两种，卡方选择则是统计学上常用的一种有监督特征选择方法，它通过对特征和真实标签之间进行卡方检验，来判断该特征和真实标签的关联程度，进而确定是否对其进行选择。

和ML库中的大多数学习方法一样，ML中的卡方选择也是以estimator+transformer的形式出现的，其主要由ChiSqSelector和ChiSqSelectorModel两个类来实现。