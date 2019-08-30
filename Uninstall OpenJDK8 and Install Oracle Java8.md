## Uninstall OpenJDK8 and Install Oracle Java8

#### Uninstall OpenJDK8

1. 登入管理員帳號

   ```
   sudo -i
   ```

   

   **備註 : **由於我安裝的OpenJDK為java-8-openjdk-amd64 , 如果您安裝的JDK的版本不一樣請將以下指令的java-8-openjdk-amd64改成您安裝的版本

   

2. 移除 Link

   ```
   update-alternatives --remove "java" "/usr/lib/jvm/java-8-openjdk-amd64/bin/java"
   update-alternatives --remove "javac" "/usr/lib/jvm/java-8-openjdk-amd64/bin/javac"
   update-alternatives --remove "javaws" "/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/javaws"
   ```

   

3. 移除 Package

   ```
   rm -r /usr/lib/jvm/java-8-openjdk-amd64
   ```

   

4. Uninstall OpenJDK8

   (1)如果只想移除OpenJDK

   ```
   apt-get remove openjdk*
   ```

   (2)如果要刪除Openjdk以及dependencies

   ```
   apt-get remove --auto-remove openjdk*
   ```

   

   (3)如果要刪除Openjdk及其配置文件

   ```
   apt-get purge openjdk*
   ```

   

   (4)如果要刪除Openjdk以及依賴項及其配置文件

   ```
   apt-get purge --auto-remove openjdk*
   ```



#### Install Oracle Java8

1. 請至[官網](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下載

   我下載的版本為 jdk-8u212-linux-x64

   **備註 : **如果沒有Oracle的帳號請先申請一個

   

2. 建立 /usr/lib/jvm 資料夾

   ```
   mkdir -p /usr/lib/jvm
   ```

   

3. 解壓縮

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

   

6.  更改權限

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

8. 檢查java是否安裝成功

   ```
   java -version
   ```

   會出現以下資訊

   ```
   java version "1.8.0_212"
   Java(TM) SE Runtime Environment (build 1.8.0_212-b10)
   Java HotSpot(TM) 64-Bit Server VM (build 25.212-b10, mixed mode)
   ```

9. 離開管理者帳號

   ```
   exit
   ```

   

10. 更改 hadoop帳號的環境變數

    登入hadoop帳號

    ```
    su - hadoop
    ```

    更改環境變數

    ```
    nano ~/.bashrc
    ```

    修改JAVA_HOME的值為

    ```
    /usr/lib/jvm/jdk1.8.0_212
    ```

    更新環境變數

    ```
    source ~/.bashrc
    ```

    

11. 修改hadoop-env.sh  

    ```
    nano /usr/local/hadoop/etc/hadoop/hadoop-env.sh  
    ```

    將第25行的JAVA_HOME的值修改為

    ```
    /usr/lib/jvm/jdk1.8.0_212
    ```

    
    
12. 如果遇到jps不能使用

    ```
    nano ~/.bashrc
    ```

    加入以下設定

    ```
    export PATH=$JAVA_HOME/bin:$PATH
    ```

    更新環境變數

    ```
    source ~/.bashrc
    ```

    



