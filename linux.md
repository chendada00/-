## 常用命令

- system [start|status|restart|stop] 服务名
- firewall-cmd --query-port=6380/tcp                            检查端口
- firewall-cmd --zone=public --add-port=6380/tcp    添加端口
- yum list installed                                                         查看yum安装的软件
- rpm -qa                                                                         查询rpm安装的软件
- rpm -e                                                                         卸载
- alter user root@localhost identified by 'mysql123';   mysql8改密码
- FLUSH PRIVILEGES;                                                        mysql刷新权限
- select host, user from user;
- update user set host = '%' where user='root';            查看和开启远程访问
- find . -type f -mtime +1                                                查找最近一天修改的文件

 

## rabbitmq（端口15672）

- rabbitmq-plugins list  查看rabbitmq的管理器插件
- rabbitmq-plugins enable rabbitmq_management                安装管理控制台
- [{rabbit, [{loopback_users. []}]}].            默认只能本地登录，新增配置文件修改配置

 

## mysql改密码

- 修改/etc/my.inf,添加
  - [mysqld]
  - skip-grant-tables
- update user set authentication_string = '' where user = 'root';    将密码置空
- 删掉my.inf中添加的语句
- ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'mysql12.com';

 

## mysql安装顺序

- mysql-community-client-plugins
- mysql-community-common
- mysql-community-libs
- mysql-community-client
- mysql-community-icu-data-files

密码存放路径：/var/log/mysqld.log







## 查看linux系统是32位还是64

``` shell
getconf LONG_BIT
```



## vim中一些默认配置

``` shell
#vim配置文件目录
/etc/vimrc

#设置方言，使用vi等同于vim
vi /etc/bashrc
alias vi=vim

set nocompatible            "去掉有关vi一致性模式，避免以前版本的bug和局限    
set nu!                     "显示行号
set guifont=Luxi/ Mono/ 9   " 设置字体，字体名称和字号
filetype on                 "检测文件的类型     
set history=1000            "记录历史的行数
set background=dark         "背景使用黑色
syntax on                   "语法高亮度显示
set autoindent              "vim使用自动对齐，也就是把当前行的对齐格式应用到下一行(自动缩进）
set cindent                 "（cindent是特别针对 C语言语法自动缩进）
set smartindent             "依据上面的对齐格式，智能的选择对齐方式，对于类似C语言编写上有用   
set tabstop=4               "设置tab键为4个空格，
set shiftwidth =4           "设置当行之间交错时使用4个空格     
set ai!                     " 设置自动缩进 
set showmatch               "设置匹配模式，类似当输入一个左括号时会匹配相应的右括号      
set guioptions-=T           "去除vim的GUI版本中得toolbar   
set vb t_vb=                "当vim进行编辑时，如果命令错误，会发出警报，该设置去掉警报       
set ruler                   "在编辑过程中，在右下角显示光标位置的状态行     
set nohls                   "默认情况下，寻找匹配是高亮度显示，该设置关闭高亮显示     
set incsearch               "在程序中查询一单词，自动匹配单词的位置；如查询desk单词，当输到/d时，会自动找到第一个d开头的单词，当输入到/de时，会自动找到第一个以ds开头的单词，以此类推，进行查找；当找到要匹配的单词时，别忘记回车 
set backspace=2             " 设置退格键可用
```

## 文本处理命令

- cut

``` shell
# 根据指定的分隔符将字符串分为多个列，并且显示指定列，指定列也可以使用1-10或-10表示1到10列
cut -d "分隔符" -f 列 文件名
```



- awk

``` shell
awk -F 指定文件分隔符，默认空格 -v 自定义变量 '/正则/BEGIN{命令执行前}{命令}END{命令执行后} ……' 文件名

# 内置变量
FILENAME	当前文件名
NR			已读的记录数
NF			切割后
```

