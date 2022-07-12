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

