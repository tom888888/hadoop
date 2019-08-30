### Install Kafka

kafka 在 master , worker1 , worker2 , worker3 上皆要安裝

1. 使用管理員帳號

   ```
   sudo -i
   ```

2. 到[官網](https://kafka.apache.org/)下載

   ```
   wget http://ftp.twaren.net/Unix/Web/apache/kafka/2.2.1/kafka_2.11-2.2.1.tgz
   ```

   這邊下載的版本為 2.2.1 , 選擇 kafka 的版本須配合 scala 的版本

   

3. 解壓縮

   ```
   tar -zvxf kafka_2.11-2.2.1.tgz
   ```

   

4. 將檔案移至 /usr/local/

   ```
   mv kafka_2.11-2.2.1 /usr/local/kafka
   ```

5. 更改 owner 與 group為 hadoop

   ```
   chown -R hadoop:hadoop /usr/local/kafka
   ```

   

6. 離開管理員帳號

   ```
   exit
   ```

7. 使用hadoop帳號

   ```
   su - hadoop
   ```

8. 設定環境變數

   ```
   nano ~/.bashrc
   ```

   增加以下的環境變數

   ```
   KAFKA_HOME=/usr/local/kafka
   PATH=$PATH:$KAFKA_HOME/bin
   ```

   更新環境變數

   ```
   source ~/.bashrc
   ```

   

9. 更改設定檔

   ```
   cd /usr/local/kafka/config/
   ```

   ```
   nano server.properties 
   ```

   加入以下設定 , host.name這個參數在 master , worker1 , worker2 , worker3 上都不一樣

   ```
   listeners=PLAINTEXT://:9092
   port=9092
   host.name=192.168.56.100
   advertised.host.name=192.168.56.100
   advertised.port=9092
   ```

   更改broker.id的值在master設定為 0 , 在worker1 設定為 1 , 在worker2 設定為 2 , 在worker3 設定為 3

   ```
   broker.id=0
   ```

   更改zookeeper的位置

   ```
   zookeeper.connect=master:2181,worker1:2181,worker2:2181
   ```

   更改 log 的輸出資料夾

   ```
   log.dirs=/usr/local/kafka/kafka-logs
   ```

10. 啟動kafka

    先啟動 zookeeper 再啟動 kafka

    在 master , worker1 , worker2 上啟動zookeeper

    ```
    zkServer.sh start
    ```

    在master , worker1 , worker2  , worker3 上啟動 kafka

    ```
    kafka-server-start.sh -daemon /usr/local/kafka/config/server.properties
    ```

    檢查是否成功啟動

    ```
    jps
    ```

    在master , worker1 , worker2 上會看到下列結果

    ```
    QuorumPeerMain
    Kafka
    ```

    在worker3 上會看到以下結果

    ```
    Kafka
    ```

    

11. 測試 kafka功能

    ```
    cd /usr/local/kafka/bin/
    ```

    在第一個 terminal 輸入以下指令,會進入一個互動式介面

    ```
    ./kafka-topics.sh --create --zookeeper 192.168.56.100:2181 --replication-factor 2 --partitions 1 --topic test
    ```

    在另一個 terminal 輸入以下指令,會進入一個互動式介面
    
    ```
./kafka-console-producer.sh --broker-list 192.168.56.100:9092 --topic test
    ```

    出現互動式介面後可輸入以下字
    
    ```
test kafka
    ```

    在開啟一個 terminal 一樣使用 hadoop 帳號並輸入以下指令
    
    ```
./kafka-console-consumer.sh --bootstrap-server 192.168.1.100:9092 --topic test --from-beginning
    ```

    可以看到出現以下結果
    
    ```
test kafka
    ```
    
    代表kafka的基本功能可以使用