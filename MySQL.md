# MySQL

## mysql日志类别

**重写日志**`（redo log）`

- 是一种基于磁盘的数据结构，用来在MySQL宕机情况下将不完整的事务执行数据纠正，redo日志记录事务执行后的状态。

**回滚日志**`（undo log）`

- 主要用来回滚到某一个版本，是一种逻辑日志。undo log记录的是修改之前的数据。

**二进制日志**`（bin log）`

- 用来记录MySQL中增删改时的记录日志。

**错误日志**`（error log）`

- 主要记录MySQL在启动、关闭或者运行过程中的错误信息，在MySQL的配置文件my.cnf中。

**慢查询日志**`（slow query log）`

- 用来记录执行时间超过指定阈值的SQL语句。

**一般查询日志**`（general log）`

- 记录了客户端连接信息以及执行的SQL语句信息。













## window安装mysql

- my.ini

``` ini

[mysqld]
#设置3306端口
port=3306
#设置mysql的安装目录，这个大家需要根据自己的安装目录来改一下
#值得一提的是，这个地方建议大家用\\表示一层目录，用\有时候会报错，也不知道为什么
basedir=D:\\ProgramFiles\\MySQL\\mysql-8.0.25
#设置mysql数据库的数据的存放目录
datadir=D:\\ProgramFiles\MySQL\\mysql-8.0.25\\data
#允许最大连接数
max_connections=200
#允许连接失败的次数。
max_connect_errors=10
#服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
#创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
#默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
#设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
#设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4

```

- 初始化数据库
  - mysqld --initialize --console
- 安装mysql服务
  - mysqld --install
- 启动
  - net start mysql
- 改密码
  - ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';

- 开启远程访问
  - update user set host='%' where user='root';

## 流程控制

1. if

``` sql
if		表达式 then 操作
elseif 	表达式 then 操作
else	操作
end if
```

2. CASE

``` sql
case 变量
	when 值 then 返回值
    when 值 then 返回值
    when 值 then 返回值
    else 都不符合时执行
end case;
```

3 循环

- loop-------自定义跳出条件

``` sql
declare num int;
lp:loop
	set num = num + 1;
	#如果大于5跳出循环
	if num > 5 then leave lp;
	end if;
end loop;
```



- while---------符合条件时循环

```sql
declare num int;
while num < 5 do
	num = num + 1;
end while;
```

- repeat---------符合条件跳出循环

``` mysql
declare num int;
repeat
	set num = num + 1;
	until i > 5;
end repeat;
```

- 循环控制-------常用于符合某个条件跳出事务或者函数

``` sql
leave_lab:[循环|create|……]
	#跳出到leave_lab标签
	then leave leave_lab;
```

## MySQL中权限相关的表

1. user

记录允许连接到服务器的用户账号信息，里面的权限是全局的。

2. db

记录各个账号在各个数据库上的操作权限。

3. table_priv

记录表级的操作权限。

4. columns_priv

记录列级的操作权限。

5. host

配合db对给定主机上的数据库级操作权限更加细致的控制。

## 查询mysql中的连接情况

``` sql
show PROCESSLIST;
```

|  Id  |      User       |      Host       |  db   | Command | Time  |         State          |       Info       |
| :--: | :-------------: | :-------------: | :---: | :-----: | :---: | :--------------------: | :--------------: |
|  5   | event_scheduler |    localhost    |       | Daemon  | 75989 | Waiting on empty queue |                  |
|  8   |      root       | localhost:58474 |       |  Sleep  | 10922 |                        |                  |
|  9   |      root       | localhost:58478 | mysql |  Query  |   0   |          init          | show PROCESSLIST |
|  10  |      root       | localhost:58479 | mysql |  Sleep  |  411  |                        |                  |
