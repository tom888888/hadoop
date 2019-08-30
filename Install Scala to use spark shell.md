## Install Scala to use spark shell

1. 進入管理員身分

   ```
   sudo -i
   ```

2. 下載 Scala [官網](https://www.scala-lang.org/download/2.11.12.html) ,請參考Spark [官網](https://spark.apache.org/) 安裝相對應的版本

   ```
   wget https://downloads.lightbend.com/scala/2.11.12/scala-2.11.12.tgz
   ```

   這邊安裝scala2.11.12

3. 解壓縮

   ```
   tar -zxvf scala-2.11.12.tgz
   ```

4. 將檔案移至/usr/local

   ```
   mv scala-2.11.12 /usr/local/scala
   ```

5. 更改owner與groups

   ```
   chown -R hadoop:hadoop /usr/local/scala
   ```

6. 離開管理員身分

   ```
   exit
   ```

7. 使用hadoop的帳號

   ```
   su - hadoop
   ```

8. 設定環境變數

   ```
   nano ~/.bashrc
   ```

   增加以下程式

   ```
   export SCALA_HOME=/usr/local/scala
   export PATH=$PATH:$SCALA_HOME/bin
   ```

   存儲並離開

   執行 ~/.bashrc

   ```
   source ~/.bashrc
   ```

9. 確認Scala版本

   ```
   scala -version
   ```

   PS:如果有出現以下錯誤訊息

   ```
   cat: /usr/lib/jvm/java-8-openjdk-amd64/release: No such file or directory
   ```

   我的解決方法為建立一個空的/release

   ```
   sudo touch $JAVA_HOME/release
   ```

10. 測試 spark shell

    請先啟動HDFS ,YARN , Spark 

    在HDFS 創建一個資料夾
    
    ```
    hdfs dfs -mkdir /user/spark
    ```
    
11. 將測試資料存進

    ```
    hdfs dfs -put /usr/local/spark/README.md /user/spark/README.md
    ```

    啟動spark shell

    ```
    /usr/local/spark/bin/spark-shell
    ```

    成功會看到類似下列結果

    ![sparkshell1](C:\Users\icw\Desktop\Doc\image\sparkshell1.jpg)

    在此spark shell輸入以下程式 , 讀取剛剛所存入HDFS的文件

    ```
    val textfile=sc.textFile("hdfs:///user/spark/README.md")
    ```

    計算文件字數總數

    ```
    textfile.count
    ```

    Result :

    ```
    Long =105
    ```

    計算文件有多少"the"

    ```
    textfile.filter(_.contains("the")).count
    ```

    Result :

    ```
    Long = 24
    ```

    測試Run Scala script

    創建一支scala程式

    ```
    nano wordcount.scala
    ```

    ```
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
    /usr/local/spark/bin/spark-shell --master spark://master:7077 --deploy-mode client
    ```

    成功開啟後執行以下程式

    ```
    :load wordcount.scala
    ```

    Result:

    ```
    Loading wordcount.scala...
    import org.apache.spark.SparkConf
    import org.apache.spark.SparkContext
    defined object WordCount
    ```

    ```
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

12. 測試pyspark

    由於我在另一篇Install and Test Spark的第10步在spark-env.sh有設定pyspark的路徑,所以這裡可以使用pyspark

    ```
    /usr/local/spark/bin/pyspark --master spark://master:7077 --deploy-mode client
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

    可以與上面的第10步比較一下結果是否相同

    