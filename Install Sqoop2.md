##  Install Sqoop2

#### System Requirement

1. Oracle JDK 至少要有 7 , 我這裡使用的 JDK 版本為1.8.0_212

   **備註**:在安裝過程中嘗試使用過openjdk,但是在操作Sqoop的功能時會出錯

2. MySQL

​      

#### Begin install

以下操作在master執行

1. 切換至管理員帳號

   ```
   sudo -i 
   ```

2. 前往[Sqoop2官網](https://sqoop.apache.org/)下載sqoop2(sqoop1.99.7)

   ```
   wget http://apache.stu.edu.tw/sqoop/1.99.7/sqoop-1.99.7-bin-hadoop200.tar.gz
   ```

3. 解壓縮

   ```
   tar -zvxf sqoop-1.99.7-bin-hadoop200.tar.gz
   ```

4. 將sqoop2 移至到usr/local/sqoop

   ```
   mv sqoop-1.99.7-bin-hadoop200 /usr/local/sqoop
   ```

5. 更改sqoop2的owner與group

   ```
   chown -R hadoop:hadoop /usr/local/sqoop
   ```

6. 離開管理員帳號

   ```
   exit
   ```

7. 使用hadoop帳號

   ```
   su - hadoop
   ```

8. 加入環境變數

   ```
   nano ~/.bashrc
   ```

   加入以下幾行

   ```
   export SQOOP_HOME=/usr/local/sqoop
   export PATH=$PATH:$SQOOP_HOME/bin
   export SQOOP_SERVER_EXTRA_LIB=/usr/local/sqoop/extra
   export CATALINA_BASE=/usr/local/sqoop/server
   export LOGDIR=$SQOOP_HOME/logs/
   ```

   儲存並關閉

   ```
   source ~/.bashrc
   ```

9. 編輯 sqoop.properties

   ```
   nano /usr/local/sqoop/conf/sqoop.properties
   ```

   打開以下的設定

   ```
   org.apache.sqoop.security.authentication.type=SIMPLE
   org.apache.sqoop.security.authentication.handler=org.apache.sqoop.security.authentication.SimpleAuthenticationHandler
   org.apache.sqoop.security.authentication.anonymous=true
   org.apache.sqoop.jetty.port=12000
   ```

   修改以下設定

   ```
   org.apache.sqoop.submission.engine.mapreduce.configuration.directory=/usr/local/hadoop/etc/hadoop
   org.apache.sqoop.log4j.appender.file.File=/usr/local/sqoop/logs/sqoop.log
   org.apache.sqoop.log4j.appender.audit.File=/usr/local/sqoop/logs/audit.log
   org.apache.sqoop.repository.sysprop.derby.stream.error.file=/usr/local/sqoop/logs/derbyrepo.log
   ```

   儲存並離開

10. 編輯 core-site.xml

    ```
    nano /usr/local/hadoop/etc/hadoop/core-site.xml
    ```

    請加入以下幾行

    ```
    <property>
       <name>hadoop.proxyuser.hadoop.hosts</name>
       <value>*</value>
    </property>
    <property>
       <name>hadoop.proxyuser.hadoop.groups</name>
       <value>*</value>
    </property>
    
    ```

    儲存並離開

11. 編輯 container-executor.cfg 

    ```
    nano /usr/local/hadoop/etc/hadoop/container-executor.cfg
    ```

    請加入以下幾行

    ```
    allowed.system.users=hadoop
    ```

12. 下載MySQL的連結器

    [官網連結](https://dev.mysql.com/downloads/connector/j/)可以參考不同版本

    我目前是使用5.1.47版,因為最新的版本在安裝時有出現過錯誤

    下載的程式碼如下

    ```
    wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.47.tar.gz
    ```

    解壓縮

    ```
    sudo tar -zvxf mysql-connector-java-5.1.47.tar.gz
    ```

    將mysql-connector-java-5.1.47-bin.jar移至sqoop/extra

    ```
    cp mysql-connector-java-5.1.47/mysql-connector-java-5.1.47-bin.jar /usr/local/sqoop/extra
    ```

    **備註 : **資料夾mysql-connector-java-5.1.47 裡有兩個jar檔mysql-connector-java-5.1.47-bin.jar與mysql-connector-java-5.1.47.jar , 這兩個jar檔內容一樣只是名稱不一樣

    

13. 將一些sqoop2需要的檔案複製到sqoop裡

    ```
    sudo cp -R $HADOOP_HOME/share/hadoop/common/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/common/lib/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/hdfs/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/hdfs/lib/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/mapreduce/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/mapreduce/lib/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/yarn/* $SQOOP_HOME/server/lib/
    sudo cp -R $HADOOP_HOME/share/hadoop/yarn/lib/* $SQOOP_HOME/server/lib/
    ```

14. 更改權限

    ```
    sudo chmod -R 777 /usr/local/hadoop/etc/hadoop
    ```

    

15. 初始化並且驗證Sqoop2配置是否正確

    ```
    sqoop2-tool upgrade
    ```

    ```
    sqoop2-tool verify
    ```

    成功會看到類似下列的訊息

    ```
    Verification was successful.
    Tool class org.apache.sqoop.tools.tool.VerifyTool has finished correctly.
    ```

16. 啟動sqoop2 server

    ```
    sqoop2-server start
    ```

    **備註**:必須先啟動HDFS與YARN才可以啟動Sqoop2 server

    

    確認有無啟動成功

    ```
    jps
    ```

    會類似看到有以下的東西進程

    ```
    5231 SqoopJettyServer
    ```

17. 啟動 sqoop2 client

    ```
    sqoop2-shell
    ```

    **備註一**:如果出現以下錯誤

    ```
    java.lang.InternalError: Can't connect to X11 window server using ':0.0' as the value of the DISPLAY variable
    ```

    請在開啟sqoop.sh的檔案

    ```
    nano /usr/local/sqoop/bin/sqoop.sh
    ```

    找尋JAVA_OPTS這行,並在後面加入

    ```
    -Djava.awt.headless=true
    ```

    結果類似這樣

    ```
    JAVA_OPTS="$JAVA_OPTS -Dsqoop.config.dir=$SQOOP_CONF_DIR -Djava.awt.headless=true"
    ```

    **備註二**: sqoop2 client 端不一定要跟server或hadoop的群集在同一台電腦上

    如果要在其他設備執行 client 只要做第2,3步的下載與解壓縮就可以執行sqoop client

    

    啟動sqoop-shell時會看到類似下圖的互動式介面:

    ![sqoop2_shell](C:\Users\icw\Desktop\Doc\image\sqoop2_shell.jpg)

    

    