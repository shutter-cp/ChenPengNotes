**ChenPengNotes**

# Nginx	
## 1. nginx搭建
1. 上传nginx安装包
2. 解压nginx
3. 进入到nginx的源码目录
4. 预编译
	`./configure`
5. 安静gcc编译器
    ```
		yum -y install gcc pcre-devel openssl openssl-devel
		
		
		zlib
	    $ sudo apt-get install zlib1g-dev

	    pcre
	    $ sudo apt-get install libpcre3 libpcre3-dev  

	    openssl
	    $ sudo apt-get install openssl libssl-dev  
	```

6. 然后再执行
	./configure
7. 编译安装nginx
	make && make install
8. 启动nginx
	sbin/nginx
9. 查看nginx进程
	ps -ef | grep nginx
	netstat -anpt | grep nginx


10. 关闭        
    ./nginx -s stop

11. 修改过配置文件之后  
    sbin/nginx -s reload //从新加载配置文件
    sbin/nginx -t

https://www.jianshu.com/p/a7c86efe1987

https://blog.csdn.net/hunter_wyh/article/details/78261457

## 2. 负载均衡配置	

### Nginx配置负载均衡
> 修改配置文件  
如果是`make && make install` 安装的   
那么在`/usr/local/nginx/conf/nginx.conf`


### 测试code
```
/**
 * @Author chenpeng
 * @Description //TODO 获取服务器的编号
 * @Date 20:59 
 * @Param []
 * @return java.lang.String
 **/
@GetMapping("/host")
@ResponseBody
public String host(){
    String host = null;
    try {
        host = InetAddress.getLocalHost().getHostName();
    } catch (UnknownHostException e) {
        e.printStackTrace();
    }
    return host;
}
```

### 配置Nginx

1. 将springboot程序部署在多台服务器上，然后启动springboot
```
java -jar niubike-0.0.1-SNAPSHOT.war >> ./logs 2>&1 &
```
2. 修改Nginx配置文件
```
#响应数据来源
    upstream tomcats{
        server vm01:8888 weight=1;
        server vm02:8888 weight=1;
        server vm03:8888 weight=1;
    }
    server {
        listen       80;
        server_name  localhost;

        location / {
                proxy_pass http://tomcats;
        }
    }
```

3. 启动

### 测试
1. 访问Nginx 服务器
2. 查看输出数据

![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190527014242.png)
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190527014300.png)

> Nginx配置中如果weight一样那么每台服务器被访问的概率一样

## redis 实现session共享
https://blog.csdn.net/l1028386804/article/details/65081718

## 	3. 配置kafka+Nginx
### 1. 安装nginx-kafka插件
1. 安装git
    ```
    #centos
	yum install -y git
	#ubuntu
	apt-get install git
    ```
2. 切换到`/usr/local/src`目录，然后将kafka的c客户端源码clone到本地
    ```
	cd /usr/local/src
    git clone https://github.com/edenhill/librdkafka
    ```
3. 进入到librdkafka，然后进行编译
	```
	cd librdkafka
	
	# centos
	yum install -y gcc gcc-c++ pcre-devel zlib-devel
	# ubuntu
	apt-get install g++ 
	apt-get install zlib1g-dev
	apt-get install pcre-dev
	
	
	./configure
	make && make install
	```
4. 安装nginx整合kafka的插件，进入到`/usr/local/src，clone nginx`整合kafka的源码    
	```
	cd /usr/local/src
	git clone https://github.com/brg-liuwei/ngx_kafka_module
	```
5. 进入到nginx的源码包目录下	（编译nginx，然后将将插件同时编译）
```
    #zlib
    sudo apt-get install zlib1g-dev
    
    #pcre
    sudo apt-get install libpcre3 libpcre3-dev  
    
    #openssl
    sudo apt-get install openssl libssl-dev 
    
    
	./configure --add-module=/usr/local/src/ngx_kafka_module/
	
	make
	make install
```
6. 修改配置nginx文件 conf/nginx.conf
	![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190526233035.png)
```
    # kafka的位置
    kafka;
    kafka_broker_list vm01:9092 vm02:9092 vm03:9092;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        # 访问 ip/kafka/track  nginx就会向kafka的track topic中生产数据
        location = /kafka/track {
                kafka_topic track;
        }

        location = /kafka/user {
                kafka_topic user;
        }

```

7. 启动zk以及kafka
8. 如果 启动nginx，报错，找不到kafka.so.1的文件
	```
	error while loading shared libraries: librdkafka.so.1: cannot open shared object file: No such file or directory
	```
9. 加载so库
	```
	echo "/usr/local/lib" >> /etc/ld.so.conf
	ldconfig
	```

10. 测试，向nginx中写入数据，然后观察kafka的消费者能不能消费到数据
	```
	curl localhost/kafka/track -d "message send to kafka topic"
	curl localhost/kafka/track -d "老赵666" 
	```