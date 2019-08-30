### Install Zookeeper

將在master , worker1 , worker2上安裝

1. 登入管理員帳號

   ```
   sudo -i
   ```

2. 下載zookeeper [官網](https://zookeeper.apache.org/index.html)

   ```
   wget https://apache.org/dist/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
   ```

   我這裡安裝的版本為3.4.14

3. 解壓縮並移置 /usr/local/

   ```
   tar -zxvf zookeeper-3.4.14.tar.gz -C /usr/local
   mv /usr/local/zookeeper-3.4.14 /usr/local/zookeeper
   ```

4. 更改owner與groups

   ```
   chown -R hadoop:hadoop /usr/local/zookeeper
   ```

   **備註**:在master , worker1 , worker2均需完成1到4步驟

5. 離開管理員帳號

   ```
   exit
   ```

6. 切換至hadoop帳號(6~9步驟在master執行)

   ```
   su - hadoop
   ```

7. 編輯zoo.cfg

   ```
   cd /usr/local/zookeeper/conf/
   cp zoo_sample.cfg zoo.cfg
   nano zoo.cfg
   ```

   更改以下設定

   ```
   dataDir=/usr/local/zookeeper/zoodata
   ```

   並增加以下幾行設定

   ```
   server.1=master:2888:3888
   server.2=worker1:2888:3888
   server.3=worker2:2888:3888
   ```

   儲存並離開

8. 編輯 zkEnv.sh

   ```
   cd /usr/local/zookeeper/bin
   nano zkEnv.sh
   ```

   在 **ZOOKEEPER_PREFIX="${ZOOBINDIR}/.."** 這行下面增加以下設定

   ```
   ZOO_LOG_DIR="/usr/local/zookeeper/logs"
   ZOO_LOG4J_PROP="INFO,ROLLINGFILE"
   ```

   儲存並離開

9. 將 zoo.cfg 與 zkEnv.sh 複製到 worker1 , worker2

   ```
   cd usr/local/zookeeper
   scp conf/zoo.cfg worker1:/usr/local/zookeeper/conf/
   scp conf/zoo.cfg worker2:/usr/local/zookeeper/conf/
   scp bin/zkEnv.sh  worker1:/usr/local/zookeeper/bin/
   scp bin/zkEnv.sh  worker2:/usr/local/zookeeper/bin/
   ```

10. 在 master , worker1 , worker2 建立資料夾

    ```
    cd /usr/local/zookeeper
    mkdir logs
    mkdir zoodata
    ```

11. 建利myid 的file

    在 master 上執行

    ```
    cd /usr/local/zookeeper
    echo "1" > zoodata/myid
    ```

    在 worker1上執行

    ```
    cd /usr/local/zookeeper
    echo "2" > zoodata/myid
    ```

    在 worker2上執行

    ```
    cd /usr/local/zookeeper
    echo "3" > zoodata/myid
    ```

12. 設定環境變數

    master , worker1 , worker2均需做

    ```
    nano ~/.bashrc
    ```

    加入以下設定

    ```
    export ZOOKEEPER_HOME=/usr/local/zookeeper
    export PATH=$PATH:$ZOOKEEPER_HOME/bin
    ```

    儲存並離開

    更新環境變數

    ```
    source ~/.bashrc
    ```

13. 啟動 zookeeper

    ```
    zkServer.sh start
    ```

    必須在 master , worker1 , worker2 上面都執行