---
layout: post
title: Windows平台下kafka环境的搭建
tag: kafka
---

### 1.简介
Kafka是一种高吞吐量的分布式发布订阅消息系统。详细介绍可查阅官网：[kafka官网][1]

### 2.环境搭建
#### 2.1安装JDK
下载地址：[jre下载][2]<br/>
具体安装过程略...

#### 2.2安装Zookeeper
 下载地址：[https://zookeeper.apache.org/releases.html][3] <br/>
 or [http://archive.apache.org/dist/zookeeper/][4]<br/>
下载后解压，**关于zookeeper以及kafka的目录，路径中最好不要出现空格，比如D:\Program Files，尽量别用，运行脚本时会有问题。**<br/>


 - 1.在主目录下创建data和logs两个目录用于存储数据和日志：data logs
 - 2.进入zookeeper的相关设置所在的文件目录，例如本文的：D:\zookeeper-3.4.13\conf
 - 在conf目录下新建zoo.cfg文件，写入以下内容保存：(根据自己的安装目录做改变)
   

     tickTime=2000
        dataDir=D:/zookeeper-3.4.13/data
        dataLogDir=D:/zookeeper-3.4.13/logs
        clientPort=2181

 - 3.与配置jre类似，在系统环境变量中添加：
 - a.系统变量中添加ZOOKEEPER_HOME=D:\zookeeper-3.4.13
 - b.编辑系统变量中的path变量，增加%ZOOKEEPER_HOME%\bin
 - 4.在zoo.cfg文件中修改默认的Zookeeper端口(默认端口2181)
 - 5.打开cmd窗口，输入zkserver，运行Zookeeper，运行结果如下：
 ![此处输入图片的描述][5]

#### Zookeeper主要目录介绍

> (1).bin目录下存放的是程序运行时使用的脚本文件，window平台是一个独立的文件夹里面存放着 .bat 文件，bin的目录下存放的是
> Linux 平台使用的 .sh 的shell脚本，在window平台上用不到，嫌麻烦可以删了。
> 
> (2).config目录下存放的是一些程序运行的配置文件，在后期自定义使用kafka的时候需要修改里面的文件内容。
> 
> (3).libs目录是打包好的jar包，这个版本自带了zookeeper的jar包，所以在安装的过程中不需要再在本地安装zookeeper了。

#### 2.3 安装kafka
下载地址：[http://kafka.apache.org/downloads][6] <br/>
要下载Binary downloads这个类型，不要下载源文件，这种方便使用。下载后解压。<br/>

 - 1.进入kafka配置文件所在目录，D:\kafka_2.11-2.0.0\config
 - 2.编辑文件"server.properties"，找到并编辑：

 log.dirs=/tmp/kafka-logs  to  log.dirs=D:/kafka_2.11-2.0.0/kafka-logs 或者<br/> D:\\bigdata\\kafka_2.11-2.0.0\\kafka-logs<br/>

**注意**：路径要么是"/"分割，要么是转义字符"\\"，这样会生成正确的路径(层级，子目录)。<br/>

 - 3.在server.properties文件中，zookeeper.connect=localhost:2181代表kafka所连接的zookeeper所在的服务器IP以及端口，可根据需要更改。本文在同一台机器上使用，故不用修改。
 - 4.kafka会按照默认配置，在9092端口上运行，并连接zookeeper的默认端口2181。

#### 2.4运行kafka

> 提示：请确保启动kafka服务器前，Zookeeper实例已经在运行，因为kafka的运行是需要zookeeper这种分布式应用程序协调服务。

 - 1.进入kafka安装目录D:\kafka_2.11-2.0.0\
 - 2.按下shift+鼠标右键，选择"在此处打开命令窗口"，打开命令行。
 - 在命令行中输入：.\bin\windows\kafka-server-start.bat .\config\server.properties   回车。
 - 正确运行的情况为：
![此处输入图片的描述][7]<br/>
 到目前为止，zookeeper以及kafka都已正确运行。**保持运行状态，不要关闭。**

#### 2.5 创建主题

    bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic ydc1Test
    
   
![此处输入图片的描述][8]

    使用如下命令查看创建的主题列表：
    
    bin\windows\kafka-topics.bat --list --zookeeper localhost:2181
    
   
    
    
#### 2.6 创建生产者(producer)和消费者(consumer)

 - 1.在D:\kafka_2.11-2.0.0目录下打开新的命令行。
 - 2.输入命令，启动producer：
 



     bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic ydc1Test
     
     

该窗口不要关闭。
 - 同样在该目录下打开新的命令行。(再打开新的cmd界面)
 - 3.输入命令，启动consumer：

     `bin\windows\kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic test --from-beginning`

 - 4.在producer命令行窗口中任意输入内容，回车  在consumer命令行窗口中即可看到相应的内容。
 ![此处输入图片的描述][9]
![此处输入图片的描述][10]


### 3.安装过程中遇到的问题

> [Kafka][错误: 找不到或无法加载主类 Files\Java\jdk1.8.0_101\lib\dt.jar;C:\Program]
![此处输入图片的描述][11]

**解决方法**：<br/>
网上查找解决办法，自身主要有两个问题：<br/>

> 1、CLASSPATH配置有误，应该是：**.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;**
> 最前面是“小数点”不是“逗号”。
> 
> 2、java环境由JRE换成JDK的话，需要修改kafka_2.12-1.0.0\bin\windows\kafka-run-class.bat文件。
> 
> 具体修改内容是，将：
> 
> set COMMAND=%JAVA% %KAFKA_HEAP_OPTS% %KAFKA_JVM_PERFORMANCE_OPTS%
> %KAFKA_JMX_OPTS% %KAFKA_LOG4J_OPTS% -cp %CLASSPATH% %KAFKA_OPTS% %
> 
> 改为：set COMMAND=%JAVA% %KAFKA_HEAP_OPTS% %KAFKA_JVM_PERFORMANCE_OPTS%
> %KAFKA_JMX_OPTS% %KAFKA_LOG4J_OPTS% -cp "%CLASSPATH%" %KAFKA_OPTS% %
> 
> <font color="red">%CLASSPATH%要用双引号。</font>


  [1]: http://kafka.apache.org
  [2]: http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html
  [3]: https://zookeeper.apache.org/releases.html
  [4]: http://archive.apache.org/dist/zookeeper/
  [5]: http://omztq7zo1.bkt.clouddn.com/zookeeper.png
  [6]: http://kafka.apache.org/downloads
  [7]: http://omztq7zo1.bkt.clouddn.com/kafka.png
  [8]: http://omztq7zo1.bkt.clouddn.com/kafka%20create%20topic3.png
  [9]: http://omztq7zo1.bkt.clouddn.com/kafka%20create%20topic2.png
  [10]: http://omztq7zo1.bkt.clouddn.com/kafka%20consumer1.png
  [11]: http://omztq7zo1.bkt.clouddn.com/kafka%20wrong.png