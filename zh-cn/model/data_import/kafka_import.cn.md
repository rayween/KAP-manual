## 导入Kafka数据源
本节介绍如何导入 Kafka 数据源，以及从流式数据定义事实表。

### 环境准备

在开始本教程前，请确保您已准备好Hadoop环境并且已经安装了**KAP 2.3**及以上版本和**Kafka v2.10-0.10.1.0**及以上版本。在本教程中，我们使用Hortonworks HDP 2.4 Sandbox虚拟机作为Hadoop环境。

务必确保KAP启动前，已经将*KAFKA_HOME*变量正确导出。

### 创建Kafka topic并导入数据

首先，我们需要启动Kafka服务器，并且创建一个名为"kylin_demo"的topic。

```shell
curl -s http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/0.10.1.0/kafka_2.10-0.10.1.0.tgz | tar -xz -C /usr/local/
cd /usr/local/kafka_2.10-0.10.1.0/
./bin/kafka-server-start.sh config/server.properties &
./bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic kylindemo
```

接着，我们需要启动一个生产者，持续往topic中导入数据。KAP提供了一个简单的Producer用于产生数据。这里假设KAP安装在${KYLIN_HOME}目录。

```shell
cd $KYLIN_HOME
./bin/kylin.sh org.apache.kylin.source.kafka.util.KafkaSampleProducer --topic kylindemo --broker localhost:9092
```
```
export KAFKA_HOME=/usr/local/kafka_2.10-0.10.1.0
```

> 提示：必须在另一个命令窗口中，先停止 KAP，再运行 `export KAFKA_HOME=/usr/local/kafka_2.10-0.10.1.0`，然后在该命令窗口中启动 KAP。

这个工具类每秒会向Kafka中发送100条消息。在学习本教程的过程中，请保持本程序持续运行。同时，你可以使用Kafka自带的消费者控制台来检查消息是否成功导入。

```shell
cd $KAFKA_HOME
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic kylindemo --from-beginning
```



### 从流式数据中定义事实表

1. 启动KAP, 登录KAP web GUI, 新建一个project或者选择一个已有的project。点击 "Studio" -> "Data Source"，点击"Kafka"按钮。
   ![](images/s1.png)
2. 输入Broker集群信息
   ![](images/s2.png)


3. 点击 √ 确认Broker后，点击 Get Cluster Info -> sandbox -> kylindemo, Kafka的采样消息会出现在右边，点击 Convert。
   ![](images/s3.png)


4. 接着，您需要为流式数据源定义一个表名。定义的表名会用于后续的 SQL 查询。 假设我们将表命名为 "KAFKA_TABLE_1" 。
   ![](images/s4.png)

5. 检查表结构是否正确，确保至少有一列被选择为timestamp。

   ![](images/s5.png)

6. 设置解析器

   解析器名称: 默认为org.apache.kylin.source.kafka.TimedJsonStreamParser，您也可以自定义解析器

   时间戳字段名称: 必须为解析器指定一列用于分段的时间字段，本例选择了order_time

   解析器属性: 为解析器定义更多属性

   ![](images/s6.png)

7. 点击"提交"

