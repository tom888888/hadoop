## Test Sqoop2 and Install MySQL

#### Install MySQL

我將MySQL安裝在worker2上,所以以下指令都是在worker2下執行

1. 安裝MySQL

   ```
   sudo apt-get install mysql-server
   ```

2. 更改設定檔

   ```
   sudo nano /etc/mysql/my.cnf
   ```

   將下列增加至my.cnf

   ```
   [mysqld]
   bind_address=192.168.56.102
   ```

   bind_address設定為此台機器的ip才能允許遠端存取

3. 啟動MySQL

   ```
   mysql -u root -p
   ```

   會看到類似下圖的互動式介面

   ![mysql](C:\Users\icw\Desktop\Doc\image\mysql.jpg)

4. 建立database與table

   建立database

   ```
   create database hidb;
   ```

   使用剛剛建立的database

   ```
   use hidb;
   ```

   建立table

   ```
   create table mytable(school char(5),name char(10),id int);
   ```

   輸入資料

   ```
   insert into mytable(school, name, id) values ('NCTU','Jerry','123');
   insert into mytable(school, name, id) values ('NCTU','Tom','124');
   ```

5. 設定權限

   ```
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mysql' WITH GRANT OPTION;
   GRANT ALL PRIVILEGES ON *.* TO 'hadoop'@'%' IDENTIFIED BY 'mysql' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```

6. restart mysql

   ```
   sudo service mysql restart
   ```

   

#### Test Sqoop2



1. 必須先在安裝sqoop2 server的機器啟動sqoop2 server以及啟動HDFS與YARN

   

2. 啟動sqoop2 client

   ```
   sqoop2-shell
   ```

   會進入類似下圖的互動式介面

   ![sqoop2_shell](C:\Users\icw\Desktop\Doc\image\sqoop2_shell.jpg)

   

3. 設定server位址

   ```
   set server --host 127.0.0.1 --port 12000 --webapp sqoop
   ```

   因為我把server與client放在同一台使用,所以設定host為127.0.0.1 , 如果是其他機器啟動client的話則要更改host值(ex:192.168.56.100 master的ip)

4. 查看有哪些連結

   ```
   show connector
   ```

   會看到下圖:

   ![sqoop2_shell2](C:\Users\icw\Desktop\Doc\image\sqoop2_shell2.jpg)

   

5. 建立MySQL的連結

   ```
   create link -connector generic-jdbc-connector
   ```

   會出現以下必填資訊:

   ```
   Creating link for connector with name generic-jdbc-connector
   Please fill following values to create new link object
   Name: mysql         
   
   Database connection
   
   Driver class: com.mysql.jdbc.Driver
   Connection String: jdbc:mysql://192.168.56.102:3306/hidb?useSSL=false
   Username: root
   Password: *****
   Fetch Size: 
   Connection Properties: 
   There are currently 0 values in the map:
   entry# protocol=tcp
   There are currently 1 values in the map:
   protocol = tcp
   entry# 
   
   SQL Dialect
   
   Identifier enclose:  
   New link was successfully created with validation status OK and name mysql
   ```

   Name : 輸入想取的名字

   Driver class : 這裡輸入com.mysql.jdbc.Driver

   Connection String : 連線資訊

   Username : 使用者名稱
   Password : 使用者密碼

   entry: 這裡輸入protocol=tcp

   Identifier enclose : 這裡請先按一下**空格**在案**Enter**

   最後出現New link was successfully created with validation status OK and name mysql代表成功

6. 建立hdfs的連結

   ```
   create link -connector hdfs-connector
   ```

   會出現以下必填資訊:

   ```
   Creating link for connector with name hdfs-connector
   Please fill following values to create new link object
   Name: hdfs
   
   HDFS cluster
   
   URI: hdfs://master
   Conf directory: /usr/local/hadoop/etc/hadoop
   Additional configs:: 
   There are currently 0 values in the map:
   entry# 
   New link was successfully created with validation status OK and name hdfs
   ```

   Name : 輸入想取的名字

   URI : 請填Hadoop的core-site.xml文件中的fs.defaultFS屬性的value值

   Conf directory : 請輸入Hadoop存放設定檔的位置

   entry : 直接Enter即可

   

7. 建立job

   ```
   create job --f mysql --t hdfs
   ```

   會出現以下必填資訊:

   ```
   Creating job for links with from name mysql and to name hdfs
   Please fill following values to create new job object
   Name: test
   
   Database source
   
   Schema name: hidb
   Table name: mytable
   SQL statement: 
   Column names: 
   There are currently 0 values in the list:
   element# 
   Partition column: id
   Partition column nullable: 
   Boundary query: 
   
   Incremental read
   
   Check column: 
   Last value: 
   
   Target configuration
   
   Override null value: 
   Null value: 
   File format: 
     0 : TEXT_FILE
     1 : SEQUENCE_FILE
     2 : PARQUET_FILE
   Choose: 0
   Compression codec: 
     0 : NONE
     1 : DEFAULT
     2 : DEFLATE
     3 : GZIP
     4 : BZIP2
     5 : LZO
     6 : LZ4
     7 : SNAPPY
     8 : CUSTOM
   Choose: 0
   Custom codec: 
   Output directory: /user/mysql3
   Append mode: 
   
   Throttling resources
   
   Extractors: 
   Loaders: 
   
   Classpath configuration
   
   Extra mapper jars: 
   There are currently 0 values in the list:
   element# 
   New job was successfully created with validation status OK  and name test
   ```

   Name : 輸入想取的名字

   Schema name : 請填入想連結的database名稱

   Table name : 請填入存的table名稱

   Partition column : 這邊輸入id

   File format : 這邊輸入0

   Compression codec : 這邊輸入0

   Output directory: 輸入想輸出到HDFS的哪個資料夾,注意此資料夾必須為空的

   其他的資訊直接Enter即可

   

8. 執行job

   ```
   start job -n test
   ```

   test為job名稱

   會看到類似下圖結果

   ```
   Submission details
   Job Name: test
   Server URL: http://localhost:12000/sqoop/
   Created by: hadoop
   Creation date: 2019-05-24 16:12:26 TST
   Lastly updated by: hadoop
   External ID: job_1558682513232_0001
   	http://master:8088/proxy/application_1558682513232_0001/
   2019-05-24 16:12:26 TST: BOOTING  - Progress is not available
   ```

   代表job正在執行

   

9. 查看job的執行狀況

   ```
   status job -n test
   ```

   會看到以下狀況

   ```
   Submission details
   Job Name: test
   Server URL: http://localhost:12000/sqoop/
   Created by: hadoop
   Creation date: 2019-05-24 16:12:26 TST
   Lastly updated by: hadoop
   External ID: job_1558682513232_0001
   	http://master:8088/proxy/application_1558682513232_0001/
   2019-05-24 16:12:46 TST: RUNNING  - 0.00 %
   sqoop:000> status job -n test
   Submission details
   Job Name: test
   Server URL: http://localhost:12000/sqoop/
   Created by: hadoop
   Creation date: 2019-05-24 16:12:26 TST
   Lastly updated by: hadoop
   External ID: job_1558682513232_0001
   	http://master:8088/proxy/application_1558682513232_0001/
   2019-05-24 16:12:56 TST: RUNNING  - 0.00 %
   ```

   當執行成功時在輸入一次

   ```
   status job -n test
   ```

   會看到以下狀況

   ```
   Submission details
   Job Name: test
   Server URL: http://localhost:12000/sqoop/
   Created by: hadoop
   Creation date: 2019-05-24 16:12:26 TST
   Lastly updated by: hadoop
   External ID: job_1558682513232_0001
   	http://master:8088/proxy/application_1558682513232_0001/
   2019-05-24 16:13:15 TST: SUCCEEDED 
   Counters:
   	org.apache.hadoop.mapreduce.FileSystemCounter
   		FILE_LARGE_READ_OPS: 0
   		FILE_WRITE_OPS: 0
   		HDFS_READ_OPS: 1
   		HDFS_BYTES_READ: 142
   		HDFS_LARGE_READ_OPS: 0
   		FILE_READ_OPS: 0
   		FILE_BYTES_WRITTEN: 453338
   		FILE_BYTES_READ: 0
   		HDFS_WRITE_OPS: 1
   		HDFS_BYTES_WRITTEN: 36
   	org.apache.hadoop.mapreduce.lib.output.FileOutputFormatCounter
   		BYTES_WRITTEN: 0
   	org.apache.hadoop.mapreduce.lib.input.FileInputFormatCounter
   		BYTES_READ: 0
   	org.apache.hadoop.mapreduce.JobCounter
   		TOTAL_LAUNCHED_MAPS: 1
   		MB_MILLIS_MAPS: 16802816
   		SLOTS_MILLIS_REDUCES: 0
   		VCORES_MILLIS_MAPS: 16409
   		SLOTS_MILLIS_MAPS: 16409
   		OTHER_LOCAL_MAPS: 1
   		MILLIS_MAPS: 16409
   	org.apache.sqoop.submission.counter.SqoopCounters
   		ROWS_READ: 2
   		ROWS_WRITTEN: 2
   	org.apache.hadoop.mapreduce.TaskCounter
   		SPILLED_RECORDS: 0
   		MERGED_MAP_OUTPUTS: 0
   		VIRTUAL_MEMORY_BYTES: 1957888000
   		MAP_INPUT_RECORDS: 0
   		SPLIT_RAW_BYTES: 142
   		MAP_OUTPUT_RECORDS: 2
   		FAILED_SHUFFLE: 0
   		PHYSICAL_MEMORY_BYTES: 219869184
   		GC_TIME_MILLIS: 518
   		CPU_MILLISECONDS: 4060
   		COMMITTED_HEAP_BYTES: 169029632
   Job executed successfully
   ```

   代表成功了

   

10. 前往HDFS看看是否成功

    開啟瀏覽器輸入

    ```
    http://master:50070/explorer.html#
    ```

    選擇剛剛的存儲位置,這裡是儲存在/user/mysql3

    會看到存入的table如下圖

    ![sqoop2_shell3](C:\Users\icw\Desktop\Doc\image\sqoop2_shell3.jpg)

    可以下載下來看看table的內容是否正確