## Test HDFS and YARN

#### Test HDFS

1. 啟動HDFS+YARN

   在master執行以下程式

   登入hadoop帳號

   ```
   su - hadoop
   ```

   啟動HDFS+YARN

   ```
   start-all.sh
   ```

   確認有無成功請輸入以下程式

   ```
   jps
   ```

   在master會出現以下結果

   ```
   3216 Jps
   2583 NameNode
   2809 SecondaryNameNode
   2956 ResourceManager
   ```

   在worker會出現以下結果

   ```
   2653 DataNode
   2782 NodeManager
   2894 Jps
   ```

2. 請上網下載一份 csv 檔並存放在/home/hadoop

3. 把資料存放進HDFS

   在HDFS下建立資料夾

   ```
   hadoop fs -mkdir /user
   hadoop fs -mkdir /user/hadoop
   ```

   把資料放進HDFS

   ```
   hadoop fs -put [yourdownloaddata.csv] /user/hadoop/test.csv
   ```

4. 確認有無成功

   ```
   hadoop fs -ls /user/hadoop
   ```

   會看到剛剛放進來的test.csv的檔案

5. 如何確認檔案有無備份

   可以在其他的worker的以下路徑看到備份檔案

   ```linux
   data/dfs/data/current/BP-1529117068-192.168.56.100-1557729111486/current/finalized/subdir0/subdir0
   ```

   PS:上面的BP-1529117068-192.168.56.100-1557729111486會因為不同機器有不一樣的結果

   會以類似下圖的檔案

   ![copyfile1](C:\Users\icw\Desktop\Doc\image\copyfile1.jpg)

6. 使用網頁版確認

   開啟FireFox輸入以下網址

   ```
   http://master:50070/explorer.html#/
   ```

   可以看到以下頁面

   ![copyfile2](C:\Users\icw\Desktop\Doc\image\copyfile2.jpg)

   點選test.csv的存放路徑/user/hadoop會出現如下圖

   ![copyfile3](C:\Users\icw\Desktop\Doc\image\copyfile3.jpg)

   點選test.csv 會出現如下圖

   ![copyfile4](C:\Users\icw\Desktop\Doc\image\copyfile4.jpg)

​       由上圖顯示得知一些關於備份資料的訊息,也可從這下載資料



#### Test YARN

1.Run YARN example

   ```
hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar pi 30 100
   ```

執行後會看到類似下面的圖片

![testyarn2](C:\Users\icw\Desktop\Doc\image\testyarn2.jpg)

此時開啟FireFox輸入以下網址

```
master:8088
```

點選RUNNING會看到下圖

![testyarn](C:\Users\icw\Desktop\Doc\image\testyarn.jpg)

上圖代表正在running的app

當此任務執行完畢後點選FINISHED會看到類似下圖

![testyarn3](C:\Users\icw\Desktop\Doc\image\testyarn3.jpg)

以上步驟都執行成功代表測試完畢