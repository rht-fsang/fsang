---
title: 使用docker在服务器上部署静态文件
tags: docker
categories: 前端
typora-copy-images-to: upload
top: 20
---

如果你目前已经拥有镜像，那么就可以直接跳到最后一步启动镜像

如果你目前有一个写好了nginx配置文件和Dockerfile的项目，那你可以直接跳到倒数第二歩，构建镜像然后启动镜像。
<!--more-->
1.  上传静态资源文件到服务器上

```javascript
scp 本地文件路径 root@196.0.0.1:服务器路径
//scp guess root@47.97.16.67:local  把本地的guess文件上传到服务器的local目录里

```

2.在服务器中下载docker

```javascript
yun install docker

```

3.在docker中下载nginx

```javascript
docker pull nginx:1.14
```

4.查看docker已经安装的镜像

```javascript
docker images
```

5.在服务器中进入local目录

```javascript
cd local
```

6.生成nginx配置文件 `nginx.conf`

```javascript
touch nginx.conf
```

7.编辑`nginx.conf`文件

```javascript
vim nginx.conf
```

7.在文件中写入以下配置

```nginx
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    server {
        listen 80 default_server;
        server_name _;

        location  / {
        root /usr/share/nginx/html;
        index  index.html ;
        try_files $uri $uri/ /index.html;
        }
    }

}

```

8.回到local目录下创建`Dockerfile`文件

```nginx
touch Dockerfile
```

9.编辑Dockerfile文件

```nginx
vim Dockerfile
```

10.在Dockerfile文件写入以下配置

```nginx
FROM nginx:1.14
ADD ./guess-number/ /usr/share/nginx/html
ADD nginx.conf /etc/nginx/
EXPOSE 8080
```

11.构建镜像

```javascript
docker build -t demo . //demo是镜像名字
```

12.启动镜像

```javascript
docker run -d -p 3002:80 demo 

//-d: 后台运行容器，并返回容器ID
//-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
```

13.访问部署的静态地址`服务器id：3002`，注意服务器要确认防火墙是否放开`3002`端口，使用`telnet 服务器id 端口号`来确认服务器是否对外开放该端口，图中3002端口为已开放，8080为开放，可以去服务器管理网页找到防火墙增加端口

![](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/Snipaste_2023-03-22_17-48-25.png)
