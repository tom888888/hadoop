## Spark Streaming

1. 啟動spark shell

2. 輸入程式

   ```scala
   import org.apache.spark.SparkConf
   import org.apache.spark.streaming.{Seconds,StreamingContext}
   
   val ssc = new StreamingContext(sc, Seconds(10))
   
   val lines = ssc.socketTextStream("192.168.56.100",9999)
   
   val words = lines.flatMap(_.split(" "))
   val pairs = words.map(word =>(word,1))
   val wordCounts = pairs.reduceByKey(_+_)
   
   wordCounts.print()
   ssc.start()
   
   ```

3. 開啟另一個terminal

   輸入

   ```
   nc -lk 9999
   ```

   隨意輸入一些字

### spark + kafka

1. 建立程式 test.scala

   ```scala
   import org.apache.spark._
   import org.apache.spark.streaming._
   import org.apache.spark.streaming.StreamingContext._
   import org.apache.spark.streaming.kafka010._
   import org.apache.spark.streaming.kafka010.KafkaUtils
   import org.apache.kafka.common.serialization.StringDeserializer
   import org.apache.spark.streaming.kafka010.LocationStrategies.PreferConsistent
   import org.apache.spark.streaming.kafka010.ConsumerStrategies.Subscribe
   
   
   val ssc = new StreamingContext(sc, Seconds(10))
   
   val kafkaParams = Map[String, Object](
     "bootstrap.servers" -> "192.168.56.100:9092",
     "key.deserializer" -> classOf[StringDeserializer],
     "value.deserializer" -> classOf[StringDeserializer],
     "group.id" -> "kafka_group"
   )
   
   val topics = Set("spark_kafka")
   
   val stream = KafkaUtils.createDirectStream[String, String](
     ssc,
     PreferConsistent,
     Subscribe[String, String](topics, kafkaParams)
   )
   val lines = stream.map(_.value)
   val words = lines.flatMap(_.split(" "))
   val wordCounts = words.map(x => (x, 1L)).reduceByKey(_ + _)
   wordCounts.print()
   ssc.start()
   ssc.awaitTermination()
   ```

2. 建立資料夾在/usr/local

   ```
   mkdir kafka-spark-stream-app
   ```

   ```
   mkdir kafka-spark-stream-app/jars
   ```

3. 將test.scala放在kafka-spark-stream-app底下

   在 mkdir kafka-spark-stream-app/jars 底下放入兩個jar檔

   ```shell 
   wget https://repo1.maven.org/maven2/org/apache/kafka/kafka-clients/2.2.1/kafka-clients-2.2.1.jar
   wget https://repo1.maven.org/maven2/org/apache/spark/spark-streaming-kafka-0-10_2.11/2.4.3/spark-streaming-kafka-0-10_2.11-2.4.3.jar
   wget https://repo1.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.11/2.4.3/spark-sql-kafka-0-10_2.11-2.4.3.jar
   wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.47.tar.gz
   ```

4. 執行test.scala

   ```
   cd /usr/local/spark
   ```

   ```shell
   bin/spark-shell --jars ../kafka-spark-stream-app/jars/spark-streaming-kafka-0-10_2.11-2.4.3.jar,../kafka-spark-stream-app/jars/kafka-clients-2.2.1.jar -i ../kafka-spark-stream-app/test.scala
   ```

   開啟另一個terminal輸入以下資訊

   ```shell
   kafka-console-producer.sh --broker-list 192.168.56.100:9092 --topic spark_kafka
   ```

   然後輸入一些字,就可以在第一個 terminal 看到字數統計的結果 



### spark SQL + MySQL

1. 執行spark shell

   ```
   cd /usr/local/spark
   ```

   ```
   ./bin/spark-shell --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar
   ```

   需要在/usr/local/kafka-spark-stream-app/jars 裡放mysql-connector-java-5.1.47.jar

2. 在spark shell裡執行

   **Read MySQL table:**

   ```scala
   val jdbcHostname = "192.168.56.102"
   val jdbcPort = 3306
   val jdbcDatabase = "hidb"
   val jdbcUsername = "root"
   val jdbcPassword = "mysql"
   
   ```

   ```scala
   import java.util.Properties
   val connectionProperties = new Properties()
   connectionProperties.put("user", jdbcUsername)
   connectionProperties.put("password", jdbcPassword)
   val jdbc_url = s"jdbc:mysql://${jdbcHostname}:${jdbcPort}/${jdbcDatabase}"
   val df_student = spark.read.jdbc(jdbc_url, "mytable", connectionProperties)
   ```

   ```scala
   df_student.show()
   ```

   會出現table 的值

   **Write data to MySQL table**

   建 spark Dataframe

   ```scala
   case class Person(school:String , name: String, id: Long)
   val df = Seq(Person("ABC","Andy", 312)).toDF()
   ```

   將資料存進MySQL 的 mytable

   ```scala
   import org.apache.spark.sql.SaveMode
   val jdbc_url2 = "jdbc:mysql://192.168.56.102:3306/hidb?useSSL=false"
   df.write.mode(SaveMode.Append).jdbc(jdbc_url2, "mytable", connectionProperties)
   ```

   如果 mytable 不存在的話 , spark 會在 MySQL 裡建一個新的 table
   
   也可以使用以下方法一條一條 import 到 MSQL
   
   ```scala
   case class x2(name:String,age:Int,height:Int)
   data.foreach(x=>{val x1 = x.split(" ")
         val df = Seq(x2(x1(0),x1(1).toInt, x1(2).toInt))
         val df2 = df.toDF()
         df2.write.mode(SaveMode.Append).jdbc(jdbc_url2, "spark_test1", connectionProperties)
         })
   
   
   ```
   
   

### Kafka + SparkStreaming + MySQL

1. 建立 kafka_to_mysql.scala

   ```
   cd /usr/local/kafka-spark-stream-app/
   ```

   ```
   nano kafka_to_mysql.scala
   ```

   增加以下程式

   ```scala
   import java.sql._
   import org.apache.spark.sql.{ForeachWriter, Row}
   import org.apache.spark.sql.{ForeachWriter, Row, SparkSession}
   import org.apache.spark.sql.streaming.ProcessingTime
   import spark.implicits._
   
   class jdbcSink(url:String,user:String,pwd:String) extends ForeachWriter[Row]{
     val driver = "com.mysql.jdbc.Driver";
     var statement:Statement = _;
     var connection:Connection  = _;
   
      def open(partitionId: Long, version: Long): Boolean = {
        Class.forName(driver);
        connection = DriverManager.getConnection(url,user,pwd);
        this.statement = connection.createStatement();
   
        true;
      }
    override def process(value: Row): Unit = {
       statement.executeUpdate("insert into wordcount values('"+value.getAs("value")+"',"+value.getAs("count")+")")
     }
   
     override def close(errorOrNull: Throwable): Unit = {
       connection.close()
     }
   }
   
   val df = spark.readStream
           .format("kafka")
           .option("kafka.bootstrap.servers","192.168.56.100:9092")
           //.option("startingOffsets", "latest")尚未測試,是否會只收最新資料
   		.option("subscribe","spark_kafka")
           .load()
   val lines = df.selectExpr("CAST(value as STRING)")
              .as[String]
   
   val wordCount = lines.flatMap(_.split(" ")).groupBy("value").count()
   
   
   val url = "jdbc:mysql://192.168.56.102:3306/hidb?useSSL=false"
   val user  = "root"
   val pwd = "mysql"
   val writer:ForeachWriter[Row] = new jdbcSink(url,user,pwd);
   val query = wordCount
              .writeStream
              .foreach(writer)
              .outputMode("update")
              .trigger(ProcessingTime("25 seconds"))
              .start()
   query.awaitTermination()
   
   
   ```

2. 下載依賴的 jar 檔

   ```
   cd jars/
   ```

   

   ```
   wget http://central.maven.org/maven2/org/apache/spark/spark-sql-kafka-0-10_2.11/2.4.3/spark-sql-kafka-0-10_2.11-2.4.3.jar
   ```

   

3. 請先再MySQL建立一個table

   ```
   create table wordcount(value varchar(50) default null, count int(10) default null);
   ```

   

4. 執行

   ```
   cd /usr/local/spark
   ```

   

   ```
   bin/spark-shell --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar,../kafka-spark-stream-app/jars/spark-streaming-kafka-0-10_2.11-2.4.3.jar,../kafka-spark-stream-app/jars/kafka-clients-2.2.1.jar,../kafka-spark-stream-app/jars/spark-sql-kafka-0-10_2.11-2.4.3.jar -i ../kafka-spark-stream-app/kafka_to_mysql.scala
   
   ```

   開啟另一個 terminal

   ```
   kafka-console-producer.sh --broker-list 192.168.56.100:9092 --topic spark_kafka
   ```

   出現互動式介面後可以輸入一些字

   此時第一個 terminal 會出現類似下圖

   ![](C:\Users\icw\Desktop\Doc\image\kafka_to_MySQL.jpg)

   如果出現此 WARN 請不用擔心 , 此為 Hadoop 的 bug 無執行上的影響

   ```
   /06/18 16:41:10 WARN hdfs.DataStreamer: Caught exception
   java.lang.InterruptedException
   	at java.lang.Object.wait(Native Method)
   	at java.lang.Thread.join(Thread.java:1252)
   	at java.lang.Thread.join(Thread.java:1326)
   	at org.apache.hadoop.hdfs.DataStreamer.closeResponder(DataStreamer.java:980)
   	at org.apache.hadoop.hdfs.DataStreamer.endBlock(DataStreamer.java:630)
   	at org.apache.hadoop.hdfs.DataStreamer.run(DataStreamer.java:807)
   
   ```

   

5. 在MySQL 確認有無輸入成功

   ```mysql
   use hidb;
   ```

   ```mysql
   select * from wordcount;
   ```

   會顯示剛剛輸入的字的計數
   
   

#### Python code send data

```python
from kafka import KafkaProducer
import random
import time
producer = KafkaProducer(bootstrap_servers=['192.168.1.103:9092'])
for i in range(1000000):
	a = time.localtime(time.time())  
	a2 = time.strftime("%Y-%m-%d_%H:%M:%S", a)   
	b = random.randint(1,30)     
	c = random.choice("ABCD")     
	data = bytes(a2 , encoding="utf8")+b" "+bytes(c , encoding="utf8")+b" "+bytes(str(b) , encoding="utf8")+b" "+bytes(str(i) , encoding="utf8")
	producer.send('spark_kafka', data)
	print(data)
	time.sleep(3)
```



#### Save data to HDFS

1. parquet檔

   從MySQL讀檔

   ```scala
   import org.apache.spark.sql.SaveMode
   val df_test = spark.read.jdbc(jdbc_url, "test5", connectionProperties)
   ```

   寫入HDFS

   ```scala
   df_test.write.mode(SaveMode.Overwrite).parquet("hdfs://master:8020/user/spark/parquet_test")
   ```

   從HDFS讀取

   ```scala
   val df_parquet = spark.read.parquet("hdfs://master:8020/user/spark/parquet_test")
   ```

2. CSV檔

   寫入HDFS

   ```scala
   df_test.write.mode(SaveMode.Overwrite).csv("hdfs://master:8020/user/spark/parquet_test.csv")
   ```

   從HDFS讀取

   ```scala
   val df_append = spark.read.csv("hdfs://master:8020/user/spark/append_test.csv")
   ```

   PS(1) : 有SaveMode.Overwrite與SaveMode.Append兩種模式

   PS(2) : df_test有1000000筆資料

   開啟terminal

   ```
   ./bin/spark-shell --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar
   
   ```

   

#### demo kafka to mysql

```scala
import java.sql._
import org.apache.spark.sql.{ForeachWriter, Row}
import org.apache.spark.sql.{ForeachWriter, Row, SparkSession}
import org.apache.spark.sql.streaming.ProcessingTime
import spark.implicits._

class jdbcSink(url:String,user:String,pwd:String) extends ForeachWriter[Row]{
  val driver = "com.mysql.jdbc.Driver";
  var statement:Statement = _;
  var connection:Connection  = _;

   def open(partitionId: Long, version: Long): Boolean = {
     Class.forName(driver);
     connection = DriverManager.getConnection(url,user,pwd);
     this.statement = connection.createStatement();

     true;
   }
 override def process(value: Row): Unit = {
 statement.executeUpdate("insert into demo values('"+value.getAs("value")+"')")


  }

  override def close(errorOrNull: Throwable): Unit = {
    connection.close()
  }
}

val df = spark.readStream
        .format("kafka")
		.option("kafka.bootstrap.servers","192.168.56.100:9092")
        .option("startingOffsets", "latest")
        .option("subscribe","spark_kafka")
        .load()
val lines = df.selectExpr("CAST(value as STRING) as data")
           .as[String]


val wordCount = lines.flatMap(_.split(",")).groupBy("value").count()
val url = "jdbc:mysql://192.168.56.102:3306/hidb?useSSL=false"
val user  = "root"
val pwd = "mysql"
val writer:ForeachWriter[Row] = new jdbcSink(url,user,pwd);
val query = wordCount
           .writeStream
          // .format("console")
           .foreach(writer)
           .outputMode("update")
//           .trigger()
           .start()
query.awaitTermination()

```



#### mysql to hdfs

```scala
val jdbcHostname = "192.168.56.102"
val jdbcPort = 3306
val jdbcDatabase = "hidb"
val jdbcUsername = "root"
val jdbcPassword = "mysql"
import java.util.Properties
val connectionProperties = new Properties()
connectionProperties.put("user", jdbcUsername)
connectionProperties.put("password", jdbcPassword)

import org.apache.spark.sql.SaveMode
val jdbc_url = "jdbc:mysql://192.168.56.102:3306/hidb?useSSL=false"

val df_test = spark.read.jdbc(jdbc_url, "demo", connectionProperties)
df_test.write.mode(SaveMode.Overwrite).csv("hdfs://master:8020/user/spark/demo_test.csv")


```

在terminal執行

```
./bin/spark-shell --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar -i ../demo/mysql_to_hdfs.scala 
```

demo最新測試

```
bin/spark-shell --num-executors 1 --driver-memory 2g --executor-memory 2g --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar,../kafka-spark-stream-app/jars/spark-streaming-kafka-0-10_2.11-2.4.3.jar,../kafka-spark-stream-app/jars/kafka-clients-2.2.1.jar,../kafka-spark-stream-app/jars/spark-sql-kafka-0-10_2.11-2.4.3.jar -i ../kafka-spark-stream-app/demo.scala 

```



#### kafka to mysql

```
import java.sql._
import org.apache.spark.sql.{ForeachWriter, Row}
import org.apache.spark.sql.{ForeachWriter, Row, SparkSession}
import org.apache.spark.sql.streaming.ProcessingTime
import spark.implicits._

class jdbcSink(url:String,user:String,pwd:String) extends ForeachWriter[Row]{
  val driver = "com.mysql.jdbc.Driver";
  var statement:Statement = _;
  var connection:Connection  = _;

   def open(partitionId: Long, version: Long): Boolean = {
     Class.forName(driver);
     connection = DriverManager.getConnection(url,user,pwd);
     this.statement = connection.createStatement();

     true;
   }
 override def process(value: Row): Unit = {
 val date = value.getAs[String]("date")
 val location = value.getAs[String]("location")
 val number = value.getAs[String]("number").toInt
 val id = value.getAs[String]("id").toInt
 
statement.executeUpdate("insert into demo2(date,location,number,id) values('"+date+"','"+location+"',"+number+","+id+")")


  }

  override def close(errorOrNull: Throwable): Unit = {
    connection.close()
  }
}

val df = spark.readStream
        .format("kafka")
	.option("kafka.bootstrap.servers","192.168.56.102:9092")
        .option("startingOffsets", "latest")
        .option("subscribe","spark_kafka")
        .option("maxOffsetsPerTrigger", 10000)
//	.option("failOnDataLoss", false)
	.load()
val lines = df.selectExpr("CAST(value as STRING) as data")
           .as[String]


//val wordCount = lines.flatMap(_.split(",")).groupBy("value").count()
val wordCount = lines.flatMap(_.split(",")).toDF("data")
val b = wordCount.withColumn("date", split(col("data"), " ").getItem(0))
                 .withColumn("location", split(col("data"), " ").getItem(1))
                 .withColumn("number", split(col("data"), " ").getItem(2))
                 .withColumn("id", split(col("data"), " ").getItem(3))
val url = "jdbc:mysql://192.168.56.102:3306/hidb?useSSL=false"
val user  = "root"
val pwd = "mysql"
val writer:ForeachWriter[Row] = new jdbcSink(url,user,pwd);
val query = b
           .writeStream
//           .format("console")
           .foreach(writer)
           .outputMode("update")
//	   .option("checkpointLocation", "hdfs://master:8020/user/spark/checkpoint")
           .trigger(ProcessingTime("5 seconds"))
           .start()
query.awaitTermination()

```



可以將"2019-07-05 A 233 9999"的資料切割成四個欄位存進MySQL



create table demo(date varchar(50) default null,location varchar(50) default null, number int(10) default null, id int(10) default null,TF varchar(50) default null)

