## 安裝 Hadoop(HDFS+YARN)

Hadoop架構圖如下

![](C:\Users\icw\Desktop\Doc\image\hadoop架構圖.jpg)

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
   
4. **install Oracle Java8 JDK**
   
   1. 請至[官網](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下載
   
      我下載的版本為 jdk-8u212-linux-x64
   
      **備註 : **如果沒有Oracle的帳號請先申請一個
   
   2. 建立 /usr/lib/jvm 資料夾
   
      ```
      mkdir -p /usr/lib/jvm
      ```
   
   3. 解壓縮
   
      請先將 jdk-8u212-linux-x64.tar.gz 移至 /root
   
      ```
      cd /usr/lib/jvm
      ```
   
      ```
      tar -xvzf /root/jdk-8u212-linux-x64.tar.gz 
      ```
   
   4. 編輯 /etc/environment
   
      ```
      nano /etc/environment
      ```
   
      在PATH的雙引號裡加入以下東西
   
      ```
      /usr/lib/jvm/jdk1.8.0_212/bin:/usr/lib/jvm/jdk1.8.0_212/jre/bin
      ```
   
      並在下面空白處加入以下程式
   
      ```
      J2SDKDIR="/usr/lib/jvm/jdk1.8.0_212"
      J2REDIR="/usr/lib/jvm/jdk1.8.0_212/jre"
      JAVA_HOME="/usr/lib/jvm/jdk1.8.0_212"
      ```
   
      編輯好的/etc/environment會變成以下這樣
   
      ```
      PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk1.8.0_212/bin:/usr/lib/jvm/jdk1.8.0_212/jre/bin"
      J2SDKDIR="/usr/lib/jvm/jdk1.8.0_212"
      J2REDIR="/usr/lib/jvm/jdk1.8.0_212/jre"
      JAVA_HOME="/usr/lib/jvm/jdk1.8.0_212"
      ```
   
   5. 增加 Link
   
      ```
      update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_212/bin/java" 100
      update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_212/bin/javac" 100
      update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/lib/jvm/jdk1.8.0_212/bin/javaws" 100
      update-alternatives --set java /usr/lib/jvm/jdk1.8.0_212/bin/java
      update-alternatives --set javac /usr/lib/jvm/jdk1.8.0_212/bin/javac
      ```
   
   6. 更改權限
   
      ```
      chmod a+x /usr/bin/java
      chmod a+x /usr/bin/javac
      chmod a+x /usr/bin/javaws
      chown -R root:root /usr/lib/jvm/jdk1.8.0_212/
      ```
   
   7. Setup verification
   
      ```
      update-alternatives --list java
      update-alternatives --list javac
      update-alternatives --list javaws
      ```
   
   8. 更改 hadoop帳號的環境變數
   
      ```
      exit
      ```
   
      ```
      su - hadoop
      ```
   
      更改環境變數
   
      ```
      nano ~/.bashrc
      ```
   
      加入以下設定
   
      ```
      export PATH=$JAVA_HOME/bin:$PATH
      ```
   
      儲存離開並更新環境變數
   
      ```
      source ~/.bashrc
      ```
   
   9. 測試 jps 與J ava version 
   
      ```
      java -version
      update-alternatives --display java
      ```
   
      ```
      jps
      ```
   
   10. 離開 hadoop 帳號並回到管理員帳號
   
       ```
       exit
       ```
   
       ```
       sudo -i
       ```
   
5. install ssh and rsync
   ```linux shell=
   apt-get install ssh
   apt-get install ssh
   ```

6. 離開root
   ```
   exit
   ```

7. Download Hadoop
   請參考以下網址 : [hadoop 官網](https://hadoop.apache.org/)
   這裡是下載Hadoop 2.9.2
   ```linux shell=
   wget http://apache.stu.edu.tw/hadoop/common/hadoop-2.9.2/hadoop-2.9.2.tar.gz
   ```

8. 解壓縮並把hadoop移至/usr/local/hadoop
   ```linux shell=
   sudo tar -zxvf hadoop-2.9.2.tar.gz
   sudo mv hadoop-2.9.2 /usr/local/hadoop
   ```

9. 更改Hadoop擁有者與群組
   ```linux shell=
   sudo chown -R hadoop:hadoop /usr/local/hadoop
   ```

10. 進入hadoop帳號
    ``` linux shell =
    su - hadoop
    ```

11. 產生 SSH Key (金鑰)
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

12. 設定環境變數
    ```linux shell=
    nano ~/.bashrc
    ```
    在最下面加入以下程式
    ```linux shell=
    #Hadoop Variable
    export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_212
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
    
    ```
    存儲並離開再執行下列程式
    ```linux shell=
    source ~/.bashrc
    ```

13. 編輯 hadoop-env.sh設定檔 
    ```linux shell=
    nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh 
    ```
    將第25行的JAVA_HOME以及第33行的HADOOP_CONF_DIR更改為下面路徑
    ```
    export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_212
    HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
    ```
儲存並離開
    
14. 編輯 core-site.xml
    ```linux shell=
    nano /usr/local/hadoop/etc/hadoop/core-site.xml
    ```
    在<configuration>與</configuration>之間貼上下列程式碼
    ```
    <property>
       <name>hadoop.tmp.dir</name>
       <value>/home/hadoop/data</value>
       <description>Temporary Directory.</description>
    </property>
    
    <property>
       <name>fs.defaultFS</name>
       <value>hdfs://hadoop-ha</value>
       <description>Use HDFS as file storage engine</description>
    </property>
    
    <property>
       <name>ha.zookeeper.quorum</name>
       <value>master:2181,worker1:2181,worker2:2181</value>
    </property>
    
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
    
15. 編輯 mapred-site.xml

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
    
    <property>
            <name>mapreduce.jobhistory.address</name>
            <value>worker2:10020</value>
    </property>
    
    <property>
            <name>mapreduce.jobhistory.webapp.address</name>
            <value>worker2:19888</value>
    </property>
    
    
    ```
    儲存並離開

16. 編輯 yarn-site.xml
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
       <value>3072</value>
    </property>
    
    <property>
       <name>yarn.nodemanager.resource.memory-mb</name>
       <value>4096</value>
    </property>
    
    <property>
           <name>yarn.app.mapreduce.am.resource.mb</name>
           <value>1024</value>
    </property>
    
    <property>
       <name>yarn.nodemanager.vmem-check-enabled</name>
       <value>false</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.ha.enabled</name>
       <value>true</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.cluster-id</name>
       <value>rm_ha</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.ha.rm-ids</name>
       <value>rm1,rm2</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.hostname.rm1</name>
       <value>master</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.hostname.rm2</name>
       <value>worker1</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.recovery.enabled</name>
       <value>true</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.store.class</name>
       <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
    </property>
    
    <property>
       <name>yarn.resourcemanager.zk-address</name>
       <value>master:2181,worker1:2181,worker2:2181</value>
    </property>
    
    <property>
        <name>yarn.resourcemanager.address.rm1</name>
        <value>master:8032</value>
    </property>
    
    <property>
        <name>yarn.resourcemanager.scheduler.address.rm1</name>
        <value>master:8034</value>
    </property>
    
    <property>
        <name>yarn.resourcemanager.webapp.address.rm1</name>
        <value>master:8088</value>
    </property>
    
    <property>
        <name>yarn.resourcemanager.address.rm2</name>
        <value>worker1:8032</value>
    </property>
    
    <property>
        <name>yarn.resourcemanager.scheduler.address.rm2</name>
        <value>worker1:8034</value>
    </property>
    
    <property>
        <name>yarn.resourcemanager.webapp.address.rm2</name>
        <value>worker1:8088</value>
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
       <description>The name of the group of super-users. The value should be a single group name.</description>
    </property>
    
    <property>
       <name>dfs.replication</name>
       <value>3</value>
</property>
    
    <property>
         <name>dfs.nameservices</name>
         <value>hadoop-ha</value>
    </property>
    
    <property>
         <name>dfs.ha.namenodes.hadoop-ha</name>
         <value>nn1,nn2</value>
    </property>
    
    <property>
         <name>dfs.namenode.rpc-address.hadoop-ha.nn1</name>
         <value>master:8020</value>
    </property>
    
    <property>
         <name>dfs.namenode.http-address.hadoop-ha.nn1</name>
         <value>master:50070</value>
    </property>
    
    <property>
         <name>dfs.namenode.rpc-address.hadoop-ha.nn2</name>
         <value>worker1:8020</value>
    </property>
    
    <property>
         <name>dfs.namenode.http-address.hadoop-ha.nn2</name>
         <value>worker1:50070</value>
    </property>
    
    <property>
         <name>dfs.namenode.shared.edits.dir</name>
         <value>qjournal://master:8485;worker1:8485;worker2:8485/hadoop-ha</value>
    </property>
    
    <property>
         <name>dfs.journalnode.edits.dir</name>
         <value>/home/hadoop/journalnode</value>
    </property>
    
    <property>
         <name>dfs.ha.fencing.methods</name>
         <value>sshfence</value>
    </property>
    
    <property>
         <name>dfs.ha.fencing.ssh.private-key-files</name>
         <value>/home/hadoop/.ssh/id_rsa</value>
    </property>
    
    <property>
         <name>dfs.client.failover.proxy.provider.hadoop-ha</name>
         <value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>
    
    <property>
         <name>dfs.ha.automatic-failover.enabled</name>
         <value>true</value>
    </property>
    
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
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
    (1)將master更改為5120 , worker1更改為3072 , worker2,worker3更改為2048
    
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
    (6)編輯slaves檔案
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
    
24. 建資料夾(在journalnode節點上,master,worker1,worker2)
    
    ```
    mkdir ~/journalnode
    ```
    
    ```
    ssh worker1
    mkdir ~/journalnode
    exit
    ```
    
    ```
    ssh worker2
    mkdir ~/journalnode
    exit
    ```
    
    
    
25. 與 zookeeper 結合
    
    在 master , worker1 , worker2 啟動 zookeeper

    ```
    zkServer.sh start
    ```
    

**PS : ** 如果還沒安裝 zookeeper 請參考 **Install Zookeeper.md** 安裝
    
26. 格式化NameNode HDFS
    此步驟必須在master執行
    進入hadoop帳號

    ```
    su - hadoop
    ```
    啟動journalnode,在master,worker1,worker2上均要執行

    ```\
    hadoop-daemon.sh start journalnode
    ```

    請先確定data 以及journalnode是否為空(在 nn1(master1) 執行)

    ```
    hdfs namenode -format
    ```

    在master上啟動namenode

    ```
    hadoop-daemon.sh start namenode
    ```

    在worker1 上啟動namenode

    ```
    hdfs namenode -bootstrapStandby
    hadoop-daemon.sh start namenode
    ```

    重新啟動hdfs

    ```
    stop-dfs.sh
    start-dfs.sh
    ```

    確認 NameNode 的狀態(在 master , worker1 執行皆可)

    ```
    hdfs haadmin -getServiceState nn1
    hdfs haadmin -getServiceState nn2
    ```

    目前會顯示 standby

    將 master 設定為主要的 namenode , 在 master 上執行

    ```
    hdfs haadmin -transitionToActive --forcemanual nn1
    ```

    再次確認 NameNode 的狀態(在 master , worker1 執行皆可)

    ```
    hdfs haadmin -getServiceState nn1
    ```

    會顯示 active

    ```
    hdfs haadmin -getServiceState nn2
    ```

    會顯示 standby

27. 格式化(必須先將HDFS與YARN先stop)

    ```
    hdfs zkfc -formatZK
    ```

    

28. 啟動YARN

    Zookeeper必須啟動

    先在 master 執行

    ```
    start-yarn.sh
    ```

    然後再 worker1 執行

    ```
    start-yarn.sh
    ```

    在 worker2 啟動 JobHistoryServer

    ```
    mr-jobhistory-daemon.sh start historyserver
    ```

    確認 ResourceManager 的狀態(在 master , worker1 執行皆可)

    ```
    yarn rmadmin -getServiceState rm1
    ```

    會顯示 active

    ```
    yarn rmadmin -getServiceState rm2
    ```

    會顯示 standby

    **PS : **rm1代表 master 的 ResourceManager  , rm2 代表 worker1 的 ResourceManager

29. 重新啟動HDFS and YARN

    1. 在 worker2 停止 JobHistoryServer

       ```
       mr-jobhistory-daemon.sh stop historyserver
       ```

       先在master 停止 YARN 

       ```
       stop-yarn.sh
       ```

       然後在 worker1 停止 YARN 

       ```
       stop-yarn.sh
       ```

       在 master 停止 HDFS

       ```
       stop-dfs.sh
       ```

       在 master , worker1 , worker2 停止 zookeeper

       ```
       zkServer.sh stop
       ```

       檢查是否全部停止 , 在每台機器輸入以下指令

       ```
       jps
       ```

       結果只會出現如下

       ```
       jps
       ```

       

    2. 啟動 HDFS and YARN

       在 master , worker1 , worker2 啟動 zookeeper

       ```
       zkServer.sh start
       ```

       在 master 啟動 HDFS

       ```
       start-dfs.sh
       ```

       先在master 啟動 YARN 

       ```
       start-yarn.sh
       ```

       然後在 worker1 啟動 YARN

       ```
       start-yarn.sh
       ```

       在 worker2 啟動 JobHistoryServer

       ```
       mr-jobhistory-daemon.sh start historyserver
       ```

       檢查是否成功啟動,在每台機器輸入以下指令

       ```
       jps
       ```

       在master會出現以下結果

       ```
       NameNode
       DFSZKFailoverController
       QuorumPeerMain
       JournalNode
       ResourceManager
       Jps
       ```

       在 worker1 會出現以下結果

       ```
       ResourceManager
       NodeManager
       JournalNode
       DFSZKFailoverController
       NameNode
       DataNode
       QuorumPeerMain
       Jps
       ```

       在 worker2 會出現以下結果

       ```
       QuorumPeerMain
       DataNode
       JobHistoryServer
       NodeManager
       JournalNode
       Jps
       ```

       在 worker3 會出現以下結果

       ```
       NodeManager
       DataNode
       Jps
       ```

       

30. 開啟Firefox 輸入master:8088
    此步驟必須在master執行
    會看到下圖
    ![](https://i.imgur.com/HeITkVJ.jpg)

​    

31. 關閉HDFS and YARN

    請依照 第 29 步驟的第一步依序關閉



https://drive.google.com/file/d/1cZPpF8bi_-1nmzi32aRxRMamdXgaSNHy/view?usp=sharing