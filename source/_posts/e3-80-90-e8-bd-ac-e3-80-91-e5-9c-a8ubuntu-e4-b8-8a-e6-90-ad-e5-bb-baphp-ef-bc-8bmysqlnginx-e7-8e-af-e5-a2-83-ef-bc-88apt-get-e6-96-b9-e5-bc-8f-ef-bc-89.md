---
title: '【转】在Ubuntu上搭建PHP＋Mysql+Nginx环境（apt-get方式） '
id: 430
categories:
  - Linux
  - 技术专栏
date: 2011-10-28 13:31:49
tags:
---

<div id="cnblogs_post_body">

 ubuntu版本：Ubuntu 10.04 LTS

1、首先使用apt-get下载Nginx,php,mysql,phpmyadmin,spawn-fcgi。

sudo apt-get install nginx php5-cgi php5-cli mysql-server-5.1 phpmyadmin  spawn-fcgi

期间可能要输入mysql的密码，按照提示一步一步安装就是了。

OK后，你在Firefox中访问http://127.0.0.1/或者http://localhost/应该就能看见Nginx的欢迎界面了。

2、此时Nginx并不能跑PHP程序。需要修改一些配置文件。

$ cd /etc/nginx

$ sudo vim fastcgi_params，修改如下（红色部分）：

fastcgi_ignore_client_abort  on;
fastcgi_pass   127.0.0.1:9000;
fastcgi_index  index.php;

fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;

fastcgi_param  SCRIPT_FILENAME      $document_root$fastcgi_script_name;
fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;

fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;

修改Nginx 配置文件nginx.conf

sudo vim nginx.conf,最后如下：

user codebean codebean;  ＃用户和用户组
worker_processes  2;

error_log  /var/log/nginx/error.log;
pid        /var/run/nginx.pid;

events {
worker_connections  1024;
# multi_accept on;
}

http {
include       /etc/nginx/mime.types;

access_log    /var/log/nginx/access.log;

sendfile        on;
#tcp_nopush     on;

#keepalive_timeout  0;
keepalive_timeout  65;
tcp_nodelay        on;

gzip  on;
gzip_disable "MSIE [1-6]\.(?!.*SV1)";

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
}

3、接下来我们来配置一个默认站点：

cd /etc/nginx/sites-available

sudo vim default

修改后如下:

server {
listen   80 default;  ＃default表示是默认站点
server_name  localhost;   ＃访问的名称
root   /var/www/nginx-default; ＃网站根目录

access_log  /var/log/nginx/localhost.access.log;

location / {
index  index.php index.html index.htm;
}

location ~ \.php$ {
include fastcgi_params;  #这个很重要
}

}

接下来你在目录/var/www/nginx-default新建一个index.php,输入：
<div>
<div>phpinfo();</div>
</div>
然后重启nginx服务和开启fastcgi：

$ sudo /etc/init.d/nginx restart
$ /usr/bin/spawn-fcgi -a 127.0.0.1 -p 9000 -C 5 /usr/bin/php-cgi

再访问http://127.0.0.1/或者[http://localhost/](http://localhost/)看看

</div>