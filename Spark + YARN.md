## Spark + YARN

1. 請先確定spark沒有啟動

   如果啟動請用以下指令關閉

   ```
   /usr/local/spark/sbin/stop-all.sh
   ```

2. 使用hadoop帳號

   ```
   su - hadoop
   ```

3. 編輯spark-env.sh

   ```
   nano /usr/local/spark/conf/spark-env.sh
   ```

   增加下面這行程式

   ```
   export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
   ```

4. 建立HDFS資料夾存放 jar 檔

   ```
   hdfs dfs -mkdir -p /user/spark/share/jars
   ```

   ```
   hdfs dfs -put $SPARK_HOME/jars/* /user/spark/share/jars/
   ```

5. 編輯spark-defaults.conf

   ```
   cd /usr/local/spark/conf
   cp spark-defaults.conf.template spark-defaults.conf
   ```

   ```
   nano spark-defaults.conf
   ```

   加入以下程式

   ```
   spark.yarn.jars hdfs://hadoop-ha/user/spark/share/jars/*
   ```

   

6. 啟動spark

   請照之前的方式依序啟動 Zookeeper , HDFS , YARN , JobHistoryServer , Spark

   

7. 測試用yarn啟動spark shell

   ```
   /usr/local/spark/bin/spark-shell --master yarn --deploy-mode client --driver-memory 512M  --executor-memory 512M
   ```

   一樣成功會看到以下畫面

   ![sparkyarn](C:\Users\icw\Desktop\Doc\image\sparkyarn.jpg)

   讀取存入HDFS的文件

   ```scala
   val textfile=sc.textFile("hdfs:///user/spark/README.md")
   ```

   計算文件字數總數

   ```scala
   textfile.count
   ```

   Result :

   ```
   Long =105
   ```

   計算文件有多少"the"

   ```scala
   textfile.filter(_.contains("the")).count
   ```

   Result : 

   ```
   Long = 24
   ```

   與單純使用Spark的差異在於這裡用yarn啟動,所以可以在master:8088看到在RUNNING如下圖

   ![spark+yarn](C:\Users\icw\Desktop\Doc\image\spark+yarn.jpg)

8. 測試用yarn啟動spark shell Run Scala script

   創建一支scala程式

   ```
   nano wordcount.scala
   ```

   將以下程式貼上

   ```scala
   import org.apache.spark.SparkConf
   import org.apache.spark.SparkContext
   
   object WordCount {
     def main(args: Array[String]){
       val fileRDD = sc.textFile("/user/spark/README.md")
       val wordsRDD = fileRDD.flatMap(line => line.split("\\W"))
       val mapRDD = wordsRDD.map(word => (word , 1))
       val reduceRDD = mapRDD.reduceByKey((value1, value2) => value1+value2)
       
       reduceRDD.take(10).foreach(println)
       sc.stop
     }
   }
   ```

   存儲並離開

   啟動spark shell

   ```
   /usr/local/spark/bin/spark-shell --master yarn --deploy-mode client --driver-memory 512M  --executor-memory 512M
   ```

   成功開啟後執行以下程式

   ```scala
   :load wordcount.scala
   ```

   Result:

   ```scala
   Loading wordcount.scala...
   import org.apache.spark.SparkConf
   import org.apache.spark.SparkContext
   defined object WordCount
   ```

   

   ```scala
   WordCount.main(null)
   ```

   Result:

   ```
   (scala,1)                                                                  (package,3)
   (this,1)
   (integration,2)
   (Python,4)
   (Because,1)
   (its,1)
   (guide,2)
   (There,1)
   (general,3)
   ```

9. 測試用YARN啟動pyspark

   ```
   /usr/local/spark/bin/pyspark --master yarn --deploy-mode client --driver-memory 512M  --executor-memory 512M
   ```

   請依序執行以下程式

   查詢在執行哪種類型的spark shell

   ```python
    spark.sparkContext.appName
   ```

   Result:

   ```
   'PySparkShell'
   ```

   載入檔案

   ```python
   textFile=sc.textFile("/user/spark/README.md")
   ```

   計算字數

   ```python
   textFile.count()
   ```

   Result:

   ```
   105
   ```

   顯示第一行是甚麼

   ```python
   textFile.first()
   ```

   Result:

   ```
   '# Apache Spark'
   ```

   計算文件有多少"the"

   ```python
   textFile.filter(lambda line: "the" in line).count()
   ```

   Result:

   ```
   24
   ```

   可以在master:8088看到在RUNNING如下圖

   ![spark+yarn2](C:\Users\icw\Desktop\Doc\image\spark+yarn2.jpg)