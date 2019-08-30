## 安裝 Hadoop(HDFS+YARN)

1. 啟動虛擬機並開啟 terminal
   
2. 使用root身分
   ```linux shell=
   sudo -i
   ```
   
3. 創建新帳號名為hadoop,並賦予root權限
   ```linux shell=
   adduser hadoop
   gpasswd -a hadoop sudo
   groups hadoop
   ```
   
4. install Oracle Java8 JDK
   ```linux shell=
   apt-get install default-jdk
   ```
   
5. 檢查Java version 
   ```linux shell=
   java -version
   update-alternatives --display java
   ```
   
6. install ssh and rsync
   ```linux shell=
   apt-get install ssh
   apt-get install ssh
   ```
   
7. 離開root
   ```
   exit
   ```
   
8. Download Hadoop
   請參考以下網址 : [hadoop 官網](https://hadoop.apache.org/)
   這裡是下載Hadoop 2.9.2
   ```linux shell=
   wget http://apache.stu.edu.tw/hadoop/common/hadoop-2.9.2/hadoop-2.9.2.tar.gz
   ```
   
9. 解壓縮並把hadoop移至/usr/local/hadoop
   ```linux shell=
   sudo tar -zxvf hadoop-2.9.2.tar.gz
   sudo mv hadoop-2.9.2 /usr/local/hadoop
   ```
   
10. 更改Hadoop擁有者與群組
    ```linux shell=
    chown -R hadoop:hadoop /usr/local/hadoop
    ```
    
11. 進入hadoop帳號
    ``` linux shell =
    su - hadoop
    ```
   
12. 產生 SSH Key (金鑰)
    ```linux shell=
    ssh-keygen -t rsa
    ```
    會出現一些訊息一直按Enter即可
    
    然後檢查有無成功
    ```linux shell=
    ls -l ~/.ssh
    ```
    將產生的Key放入授權檔案中
    ```
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    ```
    
13. 設定環境變數
    ```linux shell=
    nano ~/.bashrc
    ```
    在最下面加入以下程式
    ```linux shell=
    #Hadoop Variable
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
    export HADOOP_HOME=/usr/local/hadoop
    export PATH=$PATH:$HADOOP_HOME/bin
    export PATH=$PATH:$HADOOP_HOME/sbin
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    export HADOOP_HDFS_HOME=$HADOOP_HOME
    export YARN_HOME=$HADOOP_HOME
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
    export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
    export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH
    #Hadoop Variables
    ```
    存儲並離開再執行下列程式
    ```linux shell=
    source ~/.bashrc
    ```
    
14. 編輯 hadoop-env.sh設定檔 
    ```linux shell=
    nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh 
    ```
    將第25行的JAVA_HOME以及第33行的HADOOP_CONF_DIR更改為下面路徑
    ```
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
    ```
    儲存並離開
    
15. 編輯 core-site.xml
    ```linux shell=
    nano /usr/local/hadoop/etc/hadoop/core-site.xml
    ```
    在<configuration>與</configuration>之間貼上下列程式碼
    ```
    <property>
       <name>hadoop.tmp.dir</name>
       <value>/home/hadoop/tmp</value>
       <description>Temporary Directory.</description>
    </property>

    <property>
       <name>fs.defaultFS</name>
       <value>hdfs://master</value>
       <description>Use HDFS as file storage engine</description>
    </property>
    ```
    儲存並離開
    
16. 編輯 mapred-site.xml
    
    ```linux shell=
    cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml
    ```
    
    ```linux shell=
    nano /usr/local/hadoop/etc/hadoop/mapred-site.xml
    ```
    在<configuration>與</configuration>之間貼上下列程式碼
    
    ```
    <property>
       <name>mapreduce.framework.name</name>
       <value>yarn</value>
    </property>

    <property>
       <name>mapred.job.tracker</name>
       <value>master</value>
    </property>

    ```
    儲存並離開

17. 編輯 yarn-site.xml
    ```
    nano /usr/local/hadoop/etc/hadoop/yarn-site.xml
    ```
    在<configuration>與</configuration>之間貼上下列程式碼
    ```
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
       <name>yarn.scheduler.minimum-allocation-mb</name>
       <value>1024</value>
    </property>
    <property>
       <name>yarn.scheduler.maximum-allocation-mb</name>
       <value>1024</value>
    </property>
    <property>
       <name>yarn.nodemanager.resource.memory-mb</name>
       <value>1024</value>
    </property>
    <property>
       <name>yarn.resourcemanager.hostname</name>
       <value>master</value>
    </property>

    <property>
        <name>yarn.app.mapreduce.am.resource.mb</name>
        <value>1024</value>
    </property>
    
    <property>
       <name>yarn.nodemanager.vmem-check-enabled</name>
       <value>false</value>
    </property>
    
    ```
    
18. 編輯hdfs-site.xml
    ```
    nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml
    ```
    在<configuration>與</configuration>之間貼上下列程式碼
    ```
    <property>
       <name>dfs.permissions.superusergroup</name>
       <value>hadoop</value>
       <description>The name of the group of super-users. The value should be a sin$
    </property>
    <property>
       <name>dfs.replication</name>
       <value>3</value>
    </property>

    ```
    
19. 關閉所有的應用程式並且關閉虛擬機

20. 製作四台虛擬機,一台master三台worker
    
    以下將在VirtualBox操作
    
    (1)右鍵前面步驟的Ubuntu -> 點選**再製**
    ![](https://i.imgur.com/BxqKfTg.jpg)
    
    (2)更改**名稱**以及**MAC位址原則**
    ![](https://i.imgur.com/Y0u5UfQ.jpg)
    
    (3)點選**完整再製** -> 點選**再製**
    ![](https://i.imgur.com/3nGtz7a.jpg)
    
    (4)看到再製好的機器
    ![](https://i.imgur.com/huCu3V3.jpg)
    
    請重複上面(1)~(4)步驟,建立master,worker1,worker2,worker3四台虛擬機
    
21. 更改四台虛擬機的網路介面卡
    (1)更改介面卡1
    
    ![](https://i.imgur.com/OOZzKRj.jpg)
    
    (2)更改介面卡2
    
    ![](https://i.imgur.com/5kZ2P1C.jpg)
    
22. 更改虛擬機的記憶體大小
    (1)將master更改為2048 ,其他的worker一樣維持1024
    
    ![](https://i.imgur.com/9DsRIAy.jpg)

23. 編輯連線資訊
    (1)啟動master虛擬機
    (2)編輯interfaces 網路設定檔
    ```
    sudo nano /etc/network/interfaces
    ```
    在interfaces下面增加以下
    ```
    #NAT interface
    auto enp0s3
    iface enp0s3 inet dhcp

    auto enp0s8
    iface enp0s8 inet static
    address         192.168.56.100
    netmask         255.255.255.0
    network         192.168.56.0
    broadcast       192.168.56.255
    ```
    (3)編輯主機名稱
    ```
    sudo nano /etc/hostname
    ```
    ```
    master
    ```
    (4)編輯hosts檔案
    ```
    sudo nano /etc/hosts
    ```
    ```
    192.168.56.100 master
    192.168.56.101 worker1
    192.168.56.102 worker2
    192.168.56.103 worker3
    ```
    
    (5)重新開機
    確認網路設定
    ```
    ifconfig
    ```
    (6)編輯master檔案
    ```
    sudo nano /usr/local/hadoop/etc/hadoop/masters
    ```
    ```
    master
    ```
    (7)編輯slaves檔案
    ```
    sudo nano /usr/local/hadoop/etc/hadoop/slaves
    ```
    ```
    worker1
    worker2
    worker3
    ```
    開啟worker1,worker2,worker3虛擬機並依照(2)~(5)步驟做更改,以下為各個不同之處
    worker1的interfaces裡的adress請更改為
    192.168.56.101
    worker2的interfaces裡的adress請更改為
    192.168.56.102
    worker3的interfaces裡的adress請更改為
    192.168.56.103

    worker1的hostname請更改為worker1
    worker2的hostname請更改為worker2
    worker3的hostname請更改為worker3
    
24. 格式化NameNode HDFS
    此步驟必須在master執行
    進入hadoop帳號
    ```
    su - hadoop
    ```
    執行格式化
    ```
    hadoop namenode -format
    ```
    
25. 啟動HDFS and YARN
    此步驟必須在master執行
    
    ```
    start-all.sh
    ```
    或依次輸入以下指令
    ```
    start-dfs.sh
    start-yarn.sh
    ```
    
    檢查是否成功啟動輸入以下指令
    
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
    
    
    
26. 開啟Firefox 輸入master:8088
    此步驟必須在master執行
    會看到下圖
    ![](https://i.imgur.com/HeITkVJ.jpg)

​    

27.  關閉HDFS and YARN

    ```
    stop-all.sh
    ```

    或是依次輸入以下指令

    ```
    stop-yarn.sh
    stop-dfs.sh
    ```

    檢查是否成功啟動輸入以下指令

    ```
    jps
    ```

    會出現以下結果

    ```
    3216 Jps
    ```

    

