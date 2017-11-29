---
title: nginx多虚拟主机配置
id: 28
categories:
  - Nginx
  - 技术专栏
date: 2011-08-21 03:46:18
tags:
---

1.nginx.conf内容如下:
<div>
<div> 程序代码</div>
<div>worker_processes 1;error_log  /host/nginx/logs/error.log  crit;

pid        /host/nginx/logs/nginx.pid;

events {
worker_connections  64;
}

http {
include       /host/nginx/conf/mime.types;
default_type  application/octet-stream;

#charset  gb2312;

server_names_hash_bucket_size 128;
client_header_buffer_size 32k;
large_client_header_buffers 4 32k;

keepalive_timeout 60;

fastcgi_connect_timeout 300;
fastcgi_send_timeout 300;
fastcgi_read_timeout 300;
fastcgi_buffer_size 128k;
fastcgi_buffers 4 128k;
fastcgi_busy_buffers_size 128k;
fastcgi_temp_file_write_size 128k;
client_body_temp_path /host/nginx/client_body_temp;
proxy_temp_path /host/nginx/proxy_temp;
fastcgi_temp_path /host/nginx/fastcgi_temp;

gzip **on**;
gzip_min_length  1k;
gzip_buffers     4 16k;
gzip_http_version 1.0;
gzip_comp_level 2;
gzip_types       text/plain application/x-javascript text/css application/xml;
gzip_vary **on**;

client_header_timeout  3m;
client_body_timeout    3m;
send_timeout          3m;
sendfile                **on**;
tcp_nopush              **on**;
tcp_nodelay            **on**;
#设定虚拟主机
include       /host/nginx/conf/vhost/www_test_com.conf;
include       /host/nginx/conf/vhost/www_test1_com.conf;
include       /host/nginx/conf/vhost/www_test2_com.conf;
}

</div>
</div>
2.在conf目录下建立个vhost目录,在vhost目录下分别建立 www_test_com.conf,www_test1_com.conf,www_test2_com.conf 3个文件

www_test_com.conf 代码如下:
<div>
<div> 程序代码</div>
<div>server {
listen 202.***.***.***:80;            #换成你的IP地址
client_max_body_size 100M;
server_name  www.test.com;  #换成你的域名
charset gb2312;
index index.html index.htm index.php;
root   /host/wwwroot/test;         #你的站点路径#打开目录浏览，这样当没有找到index文件，就也已浏览目录中的文件
autoindex **on**;

**if**(-d $request_filename){
rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
}

error_page  404              /404.html;
location =/40x.html{
root  /host/wwwroot/test;       #你的站点路径
charset   **on**;
}

# redirect server error pages to the **static** page /50x.html
#
error_page   500 502 503 504  /50x.html;
location =/50x.html{
root   /host/wwwroot/test;      # 你的站点路径
charset   **on**;
}

#将客户端的请求转交给fastcgi
location ~.*\.(php|php5|php4|shtml|xhtml|phtml)?$ {
fastcgi_pass   127.0.0.1:9000;
include /host/nginx/conf/fastcgi_params;
}

#网站的图片较多，更改较少，将它们在浏览器本地缓存15天
location ~.*\.(gif|jpg|jpeg|png|bmp|swf)$
{
expires      15d;
}

#网站会加载很多JS、CSS，将它们在浏览器本地缓存1天
location ~.*\.(js|css)?$
{
expires      1d;
}

location /(WEB-INF)/{
deny all;
}

#设定日志格式
log_format  access  '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
#设定本虚拟主机的访问日志
access_log  /host/nginx/logs/down/access.log  access;   #日志的路径,每个虚拟机一个,不 能相同
server_name_in_redirect  off;
}

</div>
</div>
3.www_test1_com.conf和www_test2_com.conf,文件和上面的基本相同,具体的日志内容如下:
www_test1_com.conf 如下:
<div>
<div> 程序代码</div>
<div>..........#设定日志格式
log_format  test1  '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
#设定本虚拟主机的访问日志
access_log  /host/nginx/logs/test1/test1.log  test1;   #日志的路径,每个虚拟机一个,不 能相同
server_name_in_redirect  off;
}

</div>
</div>
www_test2_com.conf如下:
<div> 程序代码</div>
..........

#设定日志格式
log_format  test2  '$remote_addr - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
#设定本虚拟主机的访问日志
access_log  /host/nginx/logs/test2/test2.log  test2;   #日志的路径,每个虚拟机一个,不 能相同
server_name_in_redirect  off;

}

&nbsp;

&nbsp;

########################我的测试配置###############

location ~ \.php$ {
#  root           html;
fastcgi_pass   localhost:9000;
fastcgi_index  index.php;
fastcgi_param  SCRIPT_FILENAME  /home/wwwroot/test$fastcgi_script_name;
include        fastcgi_params;
}