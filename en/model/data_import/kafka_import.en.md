## Import Kafka Data Source
This section introduces how to import Kafka data source and define a table from streaming.

### Preparation

To finish this tutorial, you need a Hadoop environment which has KAP 2.3 or above installed, and have Kafka be ready to use. In this tutorial, we use Hortonworks HDP 2.4 Sandbox VM as the Hadoop environment.

Environment variable *KAFKA_HOME* should be exported successfully before KAP starts.

### Create sample Kafka topic and populate data

Firstly, we need a Kafka topic for the incoming data. A sample topic "kylin_demo" will be created here:

```shell
curl -s http://mirrors.tuna.tsinghua.edu.cn/apache/kafka/0.10.1.0/kafka_2.10-0.10.1.0.tgz | tar -xz -C /usr/local/
cd /usr/local/kafka_2.10-0.10.1.0/
./bin/kafka-server-start.sh config/server.properties &
bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 3 --topic kylindemo
```

Secondly, we need to put some sample data to this topic. KAP has an utility class which can do this. Assuming KAP is installed in ${KYLIN_HOME}

```shell
cd $KYLIN_HOME
./bin/kylin.sh org.apache.kylin.source.kafka.util.KafkaSampleProducer --topic kylindemo --broker localhost:9092
```

```
export KAFKA_HOME=/usr/local/kafka_2.10-0.10.1.0
```

> Tips: you need open another command window, first stop KAP, run `export KAFKA_HOME=/usr/local/kafka_2.10-0.10.1.0`, and then start KAP in this command window.

This tool sends 100 records to Kafka per second. Please keep it running during this tutorial. You can check the sample messages by running kafka-console-consumer.sh 

```shell
cd $KAFKA_HOME
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic kylindemo --from-beginning
```

### Define a table from streaming

1. Start KAP server, login KAP web GUI, select or create a project. Click "Studio" -> "Data Source", then click the icon "Kafka".

   ![](images/a.png)

2.Input your broker info.
   ![](images/b.png)

3.Click √ to confirm your broker info, then click Get Cluster Info -> sandbox -> kylindemo, the kafka sample message would appear in the right box, click Convert.
   ![](images/c.png)

4.You need to give a logic table name for this streaming data source. The name will be used for SQL query later. Here please enter "KAFKA_TABLE_1" in the "Table Name" field.
   ![](images/d.png)

5.Review the table schema, make sure there is at least one column chosen as 'timestamp'.

   ![](images/e.png)

6.Set parser

Parser Name: org.apache.kylin.source.kafka.TimedJsonStreamParser (default), you can also use customized parser

Parser Timestamp Field: you are required to set a timestamp field for the parser. In this example, we use order_time

ParserProperties: Properties of the parser should as least include the timestamp field. In this example, tsColName=order_time. You can further define customized properties.

![](images/f.png)

7.click 'submit'