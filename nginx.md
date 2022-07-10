## 配置文件

- 最小化配置

``` conf
# 子进程个数，nginx运行后会有一个主进程和多个子进程
woker_processes 1;		


events {
	# 每个子进程可创建的连接数
    worker_connections  1024;
}


http {
	# 引入外部文件
    include       mime.types;#文件类型映射
    # 默认文件类型
    default_type  application/octet-stream;

	# 零拷贝（不需要从内存-cpu-内存，）
    sendfile        on;

	#超时时间
    keepalive_timeout  65;

    server {
        listen       83;
        server_name  localhost;

		location ^~ /api {
			proxy_pass   http://127.0.0.1:9090;
		}
        location / {
        	#指定文件目录
            root   E:\html;
            index  index.html index.htm;
        }

		# 发成错误时跳转
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }


    }



}

```

## 负载均衡

1. 轮询：逐一转发到不同的服务器
2. 权重：根据配置的权重进行转发
3. ip_hash：根据不同的ip转发到不同的服务器
4. least_conn：最少连接访问
5. url_hash：根据url计算hash，如果hash相同则转发到相同的服务器
6. fair：根据响应时间转发

==weight==

- 权重，权重越大表示请求到达的次数越多

==down==

- 表示不参与负载均衡

==backup==

- 备用主机

``` conf
upstream 名称{
	server 地址 weight 8;
	server 地址 weight 2;
}

server{
	listen 80
	
	location /{
	proxy_pass http://名称
	}

}
```



> proxy_pass指定具体地址时，可以实现代理





## 分离项目中的静态文件



``` conf
location ~*/[css|js|fonts] {
	root D:/app/nginx/nginx-1.23.0/app/webdata;
}
```

> 将项目中的静态文件存到到nginx的代理目录，然后通过location来配置代理
>
> ~ 表示正则表达式开始
>
> ​     * 表示不区分大小写

