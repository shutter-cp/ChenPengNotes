**ChenPengNotes**

# Kafka
## 1. 实时计相关技术 
Strom/JStrom | Spark Streming | Flink
---|---|---
实时性高|有延迟|实时性高
吞吐量低|吞吐量高|吞吐量高
只能实时计算|离线+实时|离线+实时
算计比较少|算计丰富|算子丰富
没有|机器学习|没有
没有|图计算|没有
比较少|非常火|一般

## 2. 大数据项目简单描述
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190516150634.png)


  **[✒Kafka搭建，demo，分区原理](01.md)**		

  **[✒SparkStreaming、Kafka整合,DStream](02.md)**		

  **[✒demo订单量统计、SparkStreaming、Kafka、Redis](03.md)**		

  **✒ kafka的默认读取数据格式是UTF-8  
	如果是要采用其他格式 可以设置参数   
	```
	deserializer.encoding  GB2312
	```

# kafka分区原理
![kafka分区原理](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190519220042.png)

# sparkStreaming 
![sparkStreaming](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190519220042.png)