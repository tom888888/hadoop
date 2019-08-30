## 安裝Spark

1. 前往[Spark官網](<https://spark.apache.org/>)下載

2. 使用管理員身分

   在master執行

   ```
   sudo -i
   ```

3. 下載Spark

   ```
   wget http://us.mirrors.quenda.co/apache/spark/spark-2.4.3/spark-2.4.3-bin-hadoop2.7.tgz
   ```

   這邊以安裝spark-2.4.3為例

4. 將檔案解壓縮

   ```
   tar -zxvf spark-2.4.3-bin-hadoop2.7.tgz
   ```

5. 將資料夾移至/usr/local/spark

   ```
   mv spark-2.4.3-bin-hadoop2.7 /usr/local/spark
   ```

6. 更改owner與groups

   ```
   chown -R hadoop:hadoop /usr/local/spark
   ```

7. 離開管理員身分

   ````
   exit
   ````

8. 進入hadoop帳號

   ```
   su - hadoop
   ```

9. 設定Spark環境變數

   ```
   nano ~/.bashrc
   ```

   增加以下程式

   ```shell
   export SPARK_HOME=/usr/local/spark
   #export LD_LIBRARY_PATH=$HADOOP_HOME/lib/native
   ```

   執行~/.bashrc

   ```
   source ~/.bashrc
   ```

   

10. 編輯spark_env.sh

    建立spark-env.sh

    ```
    cp /usr/local/spark/conf/spark-env.sh.template /usr/local/spark/conf/spark-env.sh
    ```

    編輯spark_env.sh

    ```
    nano /usr/local/spark/conf/spark-env.sh
    ```

    增加以下程式

    ```
    export SPARK_MASTER_HOST="master"
    export SPARK_DIST_CLASSPATH=$(/usr/local/hadoop/bin/hadoop classpath)
    export PYSPARK_PYTHON=python3
    ```

    刪除Hadoop jar files

    ```
    rm /usr/local/spark/jars/hadoop*.jar
    ```

11. 編輯slaves

    建立slaves

    ```
    cp /usr/local/spark/conf/slaves.template /usr/local/spark/conf/slaves
    ```

    編輯slaves

    ```
    nano /usr/local/spark/conf/slaves
    ```

    增加以下的程式並刪除原本的localhost

    ```
    worker1
    worker2
    worker3
    ```

    

12. 將編輯好的spark資料夾複製到其他worker

    複製到worker1

    ```
    scp -r /usr/local/spark worker1:/home/hadoop
    ```

    SSH 到worker1

    ```
    ssh worker1
    ```

    移動到/usr/local

    ```
    sudo mv spark/ /usr/local/
    ```

    加入環境變數

    ```
    nano ~/.bashrc
    ```

    增加以下程式

    ```
    export SPARK_HOME=/usr/local/spark
    ```

    執行~/.bashrc

    ```
    source ~/.bashrc
    ```

    離開worker1

    ```
    exit
    ```

    請重複上面步驟,將spark複製到worker2 , worker3

13. 啟動spark

    ```
    /usr/local/spark/sbin/start-all.sh
    ```

    確認是否啟動

    ```
    jps
    ```

    如果啟動成功會顯示類似下列

    在master

    ```
    4479 Master
    ```

    在worker

    ```
    4181 Worker
    ```

14. 在網頁顯示

    開啟FireFox 並輸入master:8080

    會出現類似下圖

    ![sparkweb](C:\Users\icw\Desktop\Doc\image\sparkweb.jpg)

15. 測試Spark功能

    請輸入以下程式

    ```
    /usr/local/spark/bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://master:7077 --executor-memory 1G --total-executor-cores 1 /usr/local/spark/examples/jars/spark-examples*.jar 10
    ```

    然後回到網頁可以看到正在RUNNING如下圖

    ![sparktest1](C:\Users\icw\Desktop\Doc\image\sparktest1.jpg)

    完成後可以在Completed Applications 的欄位看到如下

    ![sparktest2](C:\Users\icw\Desktop\Doc\image\sparktest2.jpg)



​       