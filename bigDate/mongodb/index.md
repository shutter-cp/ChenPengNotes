**ChenPengNotes**

# MongoDB

# 1. 简介
> MongoDB是一个NoSQL数据库，存储的是json（bson），支持集群，高可用，可扩展

MongoDB与MySQL  
-|MongoDB | MySQL
---|---|---
数据库|database| database
表|collection| table
存储数据|json|二维表
查询|不支持SQL|SQL
主键|_id|主键

# 2. 安装
1. [去官网下载](https://www.mongodb.com/download-center/community)
![](https://raw.githubusercontent.com/shutter-cp/imgBed/master/img/20190525172246.png)
2. 上传服务器并解压
3. 解压
4. 创建data目录
5. 创建log目录
6. 将MongoDB配置进环境（可选）
7. 启动 
    ```
    # --fork 是指后台运行
    # --dbpath 是指数据库存储的位置
    # --logpath
    是指logs目录下面，用mongod.log 记录日志
    # --bind_ip 指定那些ip可以访问 0.0.0.0为全部的ip都可以访问
    # --port 为指定端口 27017为默认
    # --auth 为验证登录 默认免密
    # 通过mongo shell配置用户。如果不存在用户，则localhost接口将继续访问数据库，直到您创建第一个用户
    ./mongod --dbpath ../../file/ --logpath ../../logs/mongod.log --fork --bind_ip 0.0.0.0  --port 27017 --auth
    ```
    [参数](https://docs.mongodb.com/manual/reference/program/mongod/#mongod-options)
8. shell使用  直接通过./mongo使用（配置环境过，如果么有就去安装目录/bin 下启动）
9. 关闭
    1. 启动的时候回显示端口号
    2. 也可以查询ps -ef | grep mongod
    3. kill -2 ???? 即可


# 3. mongo安全认证配置
- 如果修改了mongo存储是的目录那么一定要修改该目录的所属用户和组为mongod
- chown -R mongod:mongod  /mongo/

1. `./mongo`
2. `use admin`
3. 设置一个admin用户
    ```
    db.createUser({
           user: "root",
           pwd: "root",
           roles: [ {role: "root", db: "admin" }]
         }
    )
    ```
4. `登录 ./mongo -u root -p root`
5. `use chenpeng` //进入一个数据库
6. 创建一个普通用户
    ```
    db.createUser({ 
        user:"chenpeng", 
        pwd:"root",
        roles:["readWrite"] 
    });
    ```

# 4. 使用
1. 使用或创建database   
use chenpeng

2. 创建集合（表）   
db.createCollection("bike")

3. 插入数据  
```
    db.bike.insert({
        "_id": 100001, 
        "status": 1, 
        "desc": "test"
    })
    
    db.bike.insert({
        "_id": 100002, 
        "status": 1, 
        "desc": "test"
    })
```

4. 查找数据
    1. db.bine.find()    （所有，bine中的数据)）
    2. 条件查询     
        `db.logs.find({"name":"chen"})`
5. 修改     
    ```
    db.logs.update({
        'name':'chen2'
        },
        {$set:{'age': 18}},
        {multi:true})
    ```
`{multi:true}`为全部修改，否则 只修改第一条

6. 删除     
`db.logs.remove({'name':'chen2'})` 全部删除
`db.logs.remove({'name':'chen2'},1)` 只删除第一条

7. 索引
```
#查看当前db的索引
db.logs.getIndexes()

#创建索引  1代表升序
db.logs.ensureIndex({"name":1})

#删除索引
db.logs.dropIndex({"name":1})
```

8. 退出   
exit


### 其他
```
数据库用户角色：read、readWrite;
数据库管理角色：dbAdmin、dbOwner、userAdmin；
集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
备份恢复角色：backup、restore；
所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
超级用户角色：root
// 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
内部角色：__system
```