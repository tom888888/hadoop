### Demo

##### kafka to spark to MySQL

在 worker2 的MySQL建立一個新的 table 名為demo

```
drop table demo;
create table demo(data varchar(50) default null);
select COUNT(*) from demo;
select * from demo;
```

在 master 啟動 spark 程式

```
cd /usr/local/spark
```

```
bin/spark-shell --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar,../kafka-spark-stream-app/jars/spark-streaming-kafka-0-10_2.11-2.4.3.jar,../kafka-spark-stream-app/jars/kafka-clients-2.2.1.jar,../kafka-spark-stream-app/jars/spark-sql-kafka-0-10_2.11-2.4.3.jar -i ../kafka-spark-stream-app/demo.scala
```

在 worker3 執行python的程式送資料

```
python3 demo/send_data.py
```



完成後回到 worker2 的 MySQL查看

```
select COUNT(*) from demo;
select * from demo;
```



#####  table to HDFS

在 master 執行

```
cd /usr/local/spark
```

```
./bin/spark-shell --driver-class-path ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar --jars ../kafka-spark-stream-app/jars/mysql-connector-java-5.1.47.jar -i ../demo/mysql_to_hdfs.scala 
```

在hdfs的/user/demo

##### image to hdfs

在master上執行

```
cd demo/
sh demo_image.sh
```

複製一張照片進去

```
cp nba3.jpeg demo_image/

```

在hdfs的/user/demo_image