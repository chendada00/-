## Docker

### docker镜像用命令

``` shell
docker version			#显示docker版本信息
docker info				#显示docker的系统信息
docker 命令 --hepl	  #帮助命令

docker images			#查询当前容器中的镜像

#搜索镜像
docker search 软件名

#下载镜像，默认下载最新版本---latest
docker pull 软件名
#指定版本下载
docker pull 软件名:版本号（指定把版本号时必须保证dockhub中存在）
https://hub.docker.com/


#删除镜像
docker rmi -f 软件名或者id，多个用空格隔开
#删除所有
docker rmi -f $(docker images) 

```





### docker容器命令

``` shell
#下载一个容器
docker pull centos

#启动一个容器
docker run [参数] 容器名

--name = ""			#容器名称
-d					#后台运行
-it					#启动并且进入容器
-p					#指定端口
	-p ip:主机端口:容器端口
	-p 主机端口:容器端口
	-p 容器端口
	不指定则随机端口
--rm				#用完即删，用于测试

#运行nginx，指定名称为nginx1，后台方式运行，指定主机端口3344，容器端口80
docker run -d --name nginx1 -p 3344:80 nginx

#进入正在运行的容器
docker exec -it 容器id		#进入容器
docker attach 容器id			#进入容器正在执行的终端


#查询运行的容器
docker ps []
	-a				#查询运行过的容器
	-n = 1			#显示指定个数

#退出容器
exit				#退出并且停止
ctrl+p+q			#退出不停止

#删除容器
docker rm 容器id或名称
	-f				#强制删除，无论是否运行
	
#查看容器的占用情况
docker stats 容器id
	
#启动容器
docker start 容器id
#停止容器
docker stop 容器id
#重启容器
docker restart 容器id
#强制停止
docker kill 容器id
```

### 查看日志

``` shell
docker logs [参数] 容器id
	-ft				#显示所有日志以及时间戳
	--tail 10		#显示10行
```

### 查看容器中的进程

``` shell
docker top 容器id
```

### 从容器内拷贝到主机

``` shell
docker cp 容器id:文件路径	主机路径
```

### 测试

``` shell
 #安装nginx
 docker pull nginx
 

```

### 可视化(portsiner)[文档](https://docs.portainer.io/start/install/agent/docker/linux)

``` shell
#官方文档
[文档](https://docs.portainer.io/start/install/agent/docker/linux)

#安装命令
docker run -d -p 9001:9001 --name portainer_agent --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/volumes:/var/lib/docker/volumes portainer/agent:latest
```

### commit镜像

``` shell

 docker commit -a cb e146099b156 tomcatcb:1.0
 	-a			#提交作者名称
 	-m			#提交的内容注释
 
```



## 容器数据卷

### 指定路径挂载

- 将容器中的数据挂载到主机中，其实就是容器的持久化和同步

``` shell
docker run -it -v 主机目录:容器目录

#将tomcat的webapps目录挂载到主机中
docker run -d  -p 3344:8080 -v /root/tomcat/:/usr/local/tomcat/webapps tomcatcb:1.0
```

### 匿名挂载

``` shell
#-v时不指定主机目录
docker run -d  -p 3344:8080 --name niming-tomcat -v /usr/local/tomcat/webapps tomcatcb:1.0

#查看镜像配置
docker inspect niming-tomcat

"Mounts": [
            {
                "Type": "volume",
                "Name": "9001da5b9732a3a17890977ed04e38eedf3db29573c606f674df2673f95b6d37",
                "Source": 
				#这里目录为随机目录
"/var/lib/docker/volumes/9001da5b9732a3a17890977ed04e38eedf3db29573c606f674df2673f95b6d37/_data", 
                "Destination": "/usr/local/tomcat/webapps",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ]
```

### 具名挂载

``` shell
#-v时指定一个主机名称而不指定目录
docker run -d --name jvanming-tomcat -p 3302:8080 -v jvanming-tomcat:/usr/local/tomcat/webapps tomcatcb:1.0


#查看镜像配置
docker inspect jvanming-tomcat

"Mounts": [
            {
                "Type": "volume",
                "Name": "jvanming-tomcat",
                "Source": "/var/lib/docker/volumes/jvanming-tomcat/_data",		#这里目录为我们指定的目录
                "Destination": "/usr/local/tomcat/webapps",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ]
```

### 扩展

``` shell
#还可以指定权限
docker run -v jvming:/etc:[ro|rw]

ro	readOnly	只读
rw	readWrite	可读可写
如果为只读那么容器中的配置文件无法修改，只能主机来修改
```







## 安装MySQL并且将数据文件挂载到主机中

``` shell
# 1.下载mysql
docker pull mysql:8

# 2.启动
docker run -d -p 3308:3306 
-v /root/dockerData/mysql/conf:/etc/mysql/conf.d 		#指定挂载目录，配置文件
-v /root/dockerData/mysql/data:/var/lib/mysql 			#指定挂载目录，数据库文件
-e MYSQL_ROOT_PASSWORD=123456 							#指定连接密码
--name mysql01 mysql:8									#指定版本
```

 

## DockerFile

``` shell
FROM			# 基础镜像
MAINTAINER		# 指定镜像作者
RUN				# 镜像构建是需要运行的命令
ADD				# 添加内容
WORDDIR			# 镜像工作目录
VOLOME			# 挂载的目录
EXPOSE			# 端口
ONBUILD			# 继承镜像时执行该命令
COPY			# 类似add。将文件拷贝到镜像中
ENV				# 设置环境变量
```

## 配置tomcat环境的DockerFile脚本

``` shell
# 1. 编写dockerfile脚本
from centos:8		#初始容器centos8

maintainer chenbo	#作者名称

copy readme.txt /usr/local/		#拷贝文件

add apache-tomcat-8.5.79.tar.gz /usr/local		#添加依赖
add jdk-8u333-linux-x64.tar.gz /usr/local		#添加依赖

workdir /usr/local								#工作目录

env JAVA_HOME /usr/local/jdk1.8.0_333			#配置环境变量
env CLASSPATH $JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib

env CATALINA_HOME /usr/local/apache-tomcat-8.5.79
env CATALINA_BASH /usr/local/apache-tomcat-8.5.79

env PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_BASH/bin

expose 8080					#端口


#启动tomcat脚本
cmd  /usr/local/apache-tomcat-8.5.79/bin/startup.sh && tail -F /usr/local/apache-tomcat-8.5.79/logs/log.out

# 2.编译脚本文件
#-t指定镜像名称和版本	-f指定脚本文件	.指定当前目录
docker build -t mytomcat-diy:1.0 -f my-tomcat-test .

# 启动镜像
docker run -d -p 8080:8080 --name chenbotomcatdiy -v /root/dockerData/my-tomcat/jar/:/usr/local/apache-tomcat-8.5.79/webapps/test -v /root/dockerData/my-tomcat/logs:/usr/local/apache-tomcat-8.5.79/logs -it mytomcat-diy:1.0


```



## 提交到远程仓库

``` shell
docker 
```





## docker-compose

- 容器编排技术

### compose相关命令

``` shell
# 查看当前正在运行的项目
docker-compose ps
# 创建并启动容器
docker-compose up
	-f				#指定docker-compose文件位置，不指定默认当前位置
	-p				#指定项目名称，默认目录名
	-d				#后台运行
# 构建容器
docker-compose build
# 停止
docker-compose down
# 验证并查看 Compose 文件，有错误时会提示行号
docker-compose config 


```

``` yaml
version: "3"
services:
  nginx:
    image: nginx:1.21.6
    ports:
      - "8022:80"
    volumes:
      - "/docker-project-data/nginx/nginx.conf:/etc/nginx/nginx.conf"
 
  rabbitmq:
    image: rabbitmq:3.10.2-management
    ports:
      - "5672:5672"
      - "15672:15672"
    environment:
      - RABBITMQ_DEFAULT_USER:guest
      - RABBITMQ_DEFAULT_PASS:guest
      - loopback_users:none             #远程访问
    volumes:
      - "/docker-project-data/rabbitmq/rabbitmq.conf:/etc/rabbitmq/rabbitmq.conf"
 
  redis:
    image: redis:7.0.0
    ports:
      - "6381:6379"
    volumes:
      - "/docker-project-data/redis/redis.conf:/usr/local/etc/redis/redis.conf"
      - "/docker-project-data/redis/data:/data"
    environment:
      - “appendonly=yes”        #数据持久化
    command:
      --requirepass "123456" #这一行是设置密码
 
  mysql:
    image: mysql:8
    ports:
      - "3307:3306"
    volumes:
      - "/docker-project-data/mysql/conf.d:/etc/mysql/conf.d"             #指定挂载目录，配置文件
      - "/docker-project-data/mysql/data:/var/lib/mysql"                  #指定挂载目录，数据库文件
    environment:
      - MYSQL_ROOT_PASSWORD:"123456"
 
  jar:
    image: chendd1314/getwebdata-dockerfile:1.0
    volumes:
      - "/docker-project-data/jar/log/:/log/"
    ports:
      - "9091:9091"
      
  centos:
    image: c

```





## 镜像和容器的导入导出

==镜像导入导出==

``` shell
# 导出
docker save -o 指定路径 指定镜像名称

# 导入
docker load -i 镜像路径
```

==容器导入导出==

``` shell
#导出
docker export -o 文件全路径 镜像名称

# 导入
docker import 文件全路径
```

> 导出是文件后缀为tar
>
> save保存的是镜像，export保存的是容器
> load用来载入镜像包，import用来载入容器包，但两者都会恢复为镜像
> load不能对载入的镜像重命名，而import可以为镜像指定新名称
> load不能载入容器包，import能载入镜像包，但不能使用
