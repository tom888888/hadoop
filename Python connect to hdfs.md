## Python connect to hdfs

1. 安裝hdfs套件

   ```
   pip3 install hdfs
   ```

2. 讀取HDFS檔案

   ```python
   from hdfs import InsecureClient
   client = InsecureClient('http://192.168.56.100:50070', user='hadoop')
   with client.read("/user/hadoop/test1.txt") as reader:
   	data = reader.read()
   ```

3. 列出資料夾下有哪些檔案

   ```python
   hdfs_path = "/user/image"
   client.list(hdfs_path)
   ```

4. 寫檔案到hdfs

   ```python
   with client.write('/user/hadoop/writetest2.txt') as writer:
   	writer.write(bytes("hello", 'utf-8'))
   ```

5. 如果是讀圖片檔

   ```python
   import os
   import io
   from PIL import Image
   
   with client.read("/user/image/nba.jpeg") as reader:
   	data2 = reader.read()
   
   image = Image.open(io.BytesIO(data2))
   
   image.save("/home/hduser/imagetest.jpeg")
   
   ```

   可以發現是存一樣的圖片

