**ChenPengNotes**

# Redis

[TOC]

>支持网络、可基于内存亦可持久化的日志型、Key-Value数据库

# 1. 安装
1. 下载[redis](https://redis.io/download)
2. 上传到Linux服务器
3. 解压
4. cd到解压地址，编译并安装
    `make && make install`
5. 如果报错缺少依赖包   
    1. 安装gcc
    2. 回到第四步
6. 如果还报错可能是没有安装jemalloc内存分配器，可以安装或者直接输入 `make MALLOC=libc && make install`
7. 自己创建一个目录
    1. 拷贝redis自带的配置文件`redis.conf` 到创建的目录
    2. 修改`redis.conf`文件(vi 模式 / 可以搜索)
        1. `daemonize yes`  #redis后台运行
        2. appendonly yes  #开启aof日志，它会每次写操作都记录一条日志
        3. bind 192.169.1.11
8. 启动 
    `redis-server redis.conf`
9. `ps -ef | grep redis` 查看redis 状态
10. `redis-cli -h 192.169.1.11` 连接
11. 关闭 `redis-cli shutdown`  `redis-cli -h 192.169.1.11 shutdown`
12. 修改密码 
    修改： `config set requirepass <password>`
    每次登录：`auth <password>`

# 2. 基本操作
操作|命令
---|---
查看全部的key|keys *
查看某个key的value|get <key>
写入某个值|set <key>
Incr key  |  ++
Incrby key 100 |   +=100
decr key   |   --
decrby key 100  |  -=100

 # 3. java操作
 ```java
 package com.cp.demo1;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;

/**
 * @author chenPeng
 * @version 1.0.0
 * @ClassName Demo1Redis.java
 * @Description TODO
 * @createTime 2019年05月22日 23:29:00
 */
public class Demo1Redis {
    public static void main(String[] args) {
        JedisPoolConfig config = new JedisPoolConfig();

        //设置最大连接数
        config.setMaxTotal(20);
        //设置最大空闲连接
        config.setMaxIdle(10);

        JedisPool pool = new JedisPool(config, "192.169.1.11",6379, 10000, "root");

        Jedis resource = pool.getResource();

        String chenpeng = resource.get("chenpeng");

        System.out.println("<chenpeng,"+chenpeng+">");

        //测试java方法
        String set = resource.set("javaTest", "1");
        System.out.println(set);

        resource.close();
    }
}
 ```
 
 
 
 # 4. 其他redis操作
 http://blog.csdn.net/xyang81/article/details/51918129
