**ChenPengNotes**

# Flume	

## 1. 简介
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190527163756.png)

## 2. 配置
1. [下载](https://flume.apache.org/download.html)
2. 上传
3. 解压
4. 根据数据采集的需求配置采集方案，描述在配置文件中(文件名可任意自定义)
    ```
    #定义三大组件的名称
    ag1.sources = source1
    ag1.sinks = sink1
    ag1.channels = channel1
    
    # 配置source组件 spooldir只要目录中有新的文件产生就执行
    ag1.sources.source1.type = spooldir
    ag1.sources.source1.spoolDir = /home/chenpeng/flume_test_logs/
    ag1.sources.source1.fileSuffix=.FINISHED
    ag1.sources.source1.deserializer.maxLineLength=5120
    
    # 配置sink组件
    ag1.sinks.sink1.type = hdfs
    ag1.sinks.sink1.hdfs.path =hdfs://vm01:9000/flume_test/%y-%m-%d/%H-%M
    ag1.sinks.sink1.hdfs.filePrefix = app_log
    ag1.sinks.sink1.hdfs.fileSuffix = .log
    ag1.sinks.sink1.hdfs.batchSize= 100
    ag1.sinks.sink1.hdfs.fileType = DataStream
    ag1.sinks.sink1.hdfs.writeFormat =Text
    
    ## roll：滚动切换：控制写文件的切换规则
    ## 按文件体积（字节）来切   
    ag1.sinks.sink1.hdfs.rollSize = 512000   
    ## 按event条数切
    ag1.sinks.sink1.hdfs.rollCount = 1000000  
    ## 按时间间隔切换文件
    ag1.sinks.sink1.hdfs.rollInterval = 60   
    
    ## 控制生成目录的规则
    ag1.sinks.sink1.hdfs.round = true
    ag1.sinks.sink1.hdfs.roundValue = 10
    ag1.sinks.sink1.hdfs.roundUnit = minute
    
    ag1.sinks.sink1.hdfs.useLocalTimeStamp = true
    
    # channel组件配置
    ag1.channels.channel1.type = memory
    ## event条数
    ag1.channels.channel1.capacity = 500000   
    ##flume事务控制所需要的缓存容量600条event
    ag1.channels.channel1.transactionCapacity = 600  
    
    # 绑定source、channel和sink之间的连接
    ag1.sources.source1.channels = channel1
    ag1.sinks.sink1.channel = channel1
    
    
    
    数据源组件，即source ——监控文件目录 :  spooldir
    spooldir特性：
       1、监视一个目录，只要目录中出现新文件，就会采集文件中的内容
       2、采集完成的文件，会被agent自动添加一个后缀：COMPLETED
       3、所监视的目录中不允许重复出现相同文件名的文件
    下沉组件，即sink——HDFS文件系统  :  hdfs sink
    通道组件，即channel——可用file channel 也可以用内存channel
    Channel参数解释：
    capacity：默认该通道中最大的可以存储的event数量
    trasactionCapacity：每次最大可以从source中拿到或者送到sink中的event数量
    keep-alive：event添加到通道中或者移出的允许时间
    ```
    
5. 启动agent去采集数据
    1. 测试阶段     
    ```bin/flume-ng  agent  -c  ./conf  -f ./dir-hdfs.conf -n  agent1 -Dflume.root.logger=DEBUG,console```
    2. 生产中       
    ```nohup bin/flume-ng  agent  -c  ./conf  -f ./dir-hdfs.conf -n  agent1 1>/dev/null 2>&1 &```
    

## 更多source和sink组件
Flume支持众多的source和sink类型，详细手册可参考官方文档
http://flume.apache.org/FlumeUserGuide.html
