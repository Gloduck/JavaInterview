# 简介

+ [docker原理](https://cloud.tencent.com/developer/article/1445113)
+ Docker是一个开源的应用容器引擎
+ Docker支持将软件编译成一个镜像，然后再镜像中各种软件做好配置，将镜像发布除去，其他使用者可以直接使用这个镜像
+ 概念：
  + docker主机(Host)：安装了Docker程序的机器（Docker直接安装在操作系统之上
  + docker客户端(Client)：连接docker主机进行操
  + docker仓库(Registry)：用来保存各种打包好的软件镜
  + docker镜像(Images)：软件打包好的镜像；放在docker仓库
  + docker容器(Container)：镜像启动后的实例称为一个容器；容器是独立运行的一个或一组
+ Docker使用了Linux的命名空间来解决了进程和网络隔离的问题。
  + 命名空间简介：
    + 进程命名空间CLONE_NEWPID：进程编号。
    + IPC命名空间CLONE_NEWPIPC：信号量、消息队列何共享内存。
    + 网络命名空间CLONE_NEWNET：网络设备、网络栈、端口等等。
    + 挂载命名空间CLONE_NEWNS：挂载点（文件系统）。
    + UTS命名空间CLONE_NEWUTS：主机名与域名。
    + 用户命名空间CLONE_NEWUSER：用户和用户组。

# Docker镜像

## 镜像基本原理

+ Docker的镜像实际上是由多层镜像组成，通过`docker images`只会列出最终的镜像。事实上一个docker镜像中间还包含多个中间镜像。这也是docker镜像拉取的时候会出现多个进度条的原因。

# 常用命令

+ 删除所有实例：
  
+ `sudo docker rm $(sudo docker ps -a -q)`
  
+ 运行：

  + docker run

  + 参数：

    + **-d:** 后台运行容器，并返回容器ID；
    + **-p:** 指定端口映射，格式为：**主机(宿主)端口:容器端口**
    + **-t:** 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
    + **--name="nginx-lb":** 为容器指定一个名称；
    + **-h "mars":** 指定容器的hostname；
    + **-v:** 指定容器的路径映射，格式为：**主机路径:容器路径** ；
    + **-i:** 以交互模式运行容器，通常与 -t 同时使用；

  + 实例：

    + ```shell
      # 运行mysql
      sudo docker run -i -t -e MYSQL_ROOT_PASSWORD=12345ke54321 -p 3306:3306 --name mysql -d -v /media/gloduck/EACC0428CC03EE1F/Code/Docker/Mysql/my.cnf:/etc/mysql/my.cnf -v /media/gloduck/EACC0428CC03EE1F/Code/Docker/Mysql/data:/var/lib/mysql mysql:5.7
      
      ```

+ 日志：

  + docker logs id

+ 启动：

  + docker start 

  + 参数：

  + 实例：

    + ```shell
      sudo docker start $(sudo docker ps -aq) # 启动所有
      ```

    + 

# 命令备份

```shell
docker run -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=12345ke54321 -v /home/gloduck/Docker/MySql/my.cnf:/etc/mysql/my.cnf -v /home/gloduck/Docker/MySql/data:/var/lib/mysql -v /home/gloduck/Docker/MySql/log:/var/log/mysql --name mysql5.7 mysql:5.7	# 启动Mysql

docker run -p 6379:6379 --name redis -v /home/gloduck/Docker/Redis/redis.conf:/etc/redis/redis.conf -v /home/gloduck/Docker/Redis/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes # 启动redis

docker run -d -p 9876:9876 -v /home/gloduck/Docker/RocketMQ/namesrv/logs:/root/logs -v /home/gloduck/Docker/RocketMQ/namesrv/store:/root/store --name rmqnamesrv -e "MAX_POSSIBLE_HEAP=100000000" rocketmqinc/rocketmq sh mqnamesrv # 启动rocketmq namesrv

docker run -d -p 10911:10911 -p 10909:10909 -v /home/gloduck/Docker/RocketMQ/broker/logs:/root/logs -v /home/gloduck/Docker/RocketMQ/broker/store:/root/store --name rmqbroker --link rmqnamesrv:namesrv -e "NAMESRV_ADDR=namesrv:9876" -e "MAX_POSSIBLE_HEAP=200000000" rocketmqinc/rocketmq sh mqbroker # 启动rocketmq broker

docker run --name rocketmq-console -e "JAVA_OPTS=-Drocketmq.namesrv.addr=192.168.21.152:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" -p 8080:8080 -t styletang/rocketmq-console-ng # 启动rocketmq控制台

docker run -it ubuntu:16.04 /bin/bash # 进入已经运行的容器
```



