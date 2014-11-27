---
layout: post
title: "Ios 百度 云推送"
categories:
- Nginx
tags:
- Nginx 

--- 

1.生成apns证书
登陆apple开发者中心  
注册Identifiers-App IDs时勾选打开Push Notifications，或者已经注册过了appid，修改打开Push Notifications为可用。  
然后使用证书助理，生成请求开发机构请求证书文件   
在到开发者中心上传开发机构请求证书文件。  
还需要pro 

{% highlight console %}

{% endhighlight %}

{% highlight console %}

{% endhighlight %}

编辑nginx配置文件nginx.conf，(注意: 27.xx.xx.xx替换成域名或者IP)  
文件下载地址<http://pan.baidu.com/s/1mg5evXm>  

{% highlight console %}

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    proxy_cache_path /var/nginx/cache/one  levels=1:2   keys_zone=one:10m max_size=10g;
    proxy_cache_key "$host$request_uri";
    upstream google {
		server 74.125.235.80:80 max_fails=3;
		server 74.125.235.81:80 max_fails=3;
		server 74.125.235.82:80 max_fails=3;
		server 74.125.235.83:80 max_fails=3;
		server 74.125.235.84:80 max_fails=3;
    }

    server {
        listen       80;
        server_name 27.xx.xx.xx;
	rewrite ^(.*) https://27.120.120.125$1 permanent;
    }
    
    server {
        listen 443 ssl;
        server_name 27.xx.xx.xx;
        ssl_certificate /root/server.crt;
        ssl_certificate_key /root/server.key;
        location / {
                proxy_cache one;
                proxy_cache_valid  200 302 1h;
                proxy_cache_valid  404 1m;
                proxy_redirect https://www.google.co.jp/ /;
                proxy_cookie_domain google.co.jp 27.xx.xx.xx;
                proxy_pass http://google;
                proxy_set_header Host "www.google.co.jp";
                proxy_set_header Accept-Encoding "";
                proxy_set_header User-Agent $http_user_agent;
                proxy_set_header Accept-Language "zh-CN";
                proxy_set_header Cookie "PREF=ID=047808f19f6de346:U=0f62f33dd8549d11:FF=2:LD=zh-CN:NR=100:NW=1:TM=1325338577:LM=1332142444:GM=1:SG=2:S=rE0SyJh2w1IQ-Maw";
                sub_filter www.google.co.jp 27.xx.xx.xx;
                sub_filter_once off; 
	    }
    }
}
{% endhighlight %}

注释:  
1、监听了80和443端口，可以在Linux自己生成证书。  
2、定义了个upstream google，放了5个谷歌的ip（通过nslookup www.google.com命令获取（yum -y install bind-utils）），如果不这样做，就等着被谷歌的验证码搞崩溃吧。   
3、也设置了反向代理缓存，某些资源不用重复去请求谷歌获取，加快搜索速度  
4、proxy_redirect https://www.google.co.jp/ /; 这行的作用是把谷歌服务器返回的302响应头里的域名替换成我们的，不然浏览器还是会直接请求www.google.com，那样反向代理就失效了。  
5、proxy_cookie_domain google.co.jp 27.xx.xx.xx; 把cookie的作用域替换成我们的域名  
6、proxy_pass http://google; 反向代理到upstream google
7、proxy_set_header Accept-Encoding “”; 防止谷歌返回压缩的内容，因为压缩的内容我们无法作域名替换  
8、proxy_set_header Accept-Language “zh-CN”;设置语言为中文  
9、proxy_set_header Cookie   “PREF=ID=047808f19f6de346:U=0f62f33dd8549d11:FF=2:LD=zh-CN:NW=1:TM=1325338577:LM=1332142444:GM=1:SG=2:S=rE0SyJh2w1IQ-Maw”; 这行很关键，传固定的cookie给谷歌，是为了禁止即时搜索，因为开启即时搜索无法替换内容。还有设置为新窗口打开网站，这个符合我们打开链接的习惯  
10、sub_filter www.google.co.jp 27.xx.xx.xx当然是把谷歌的域名替换成我们的了，注意需要安装nginx的sub_filter模块(编译加上–with-http_sub_module参数)  

参考:<http://blog.linuxeye.com/399.html>
