# nginx负载均衡配置代码

```
upstream favresin{      

    server 212.11.22.12:8090 weight=1;
    # server 173.11.22.61:8090 weight=6;
    # server 185.11.22.33:80 weight=1;
}
server
{
    listen 80;
	listen 443 ssl http2;
    server_name zhuyuming.com app.zhuyuming.buzz www.zhuyuming.com;
	index index.html index.php index.htm default.php default.htm default.html;
    root /www/wwwroot/zhuyuming.com;
    #   if ($host !~ "^www\.zhuyuming\.com$") {
    #     rewrite ^(.*) https://www.zhuyuming.com$1 redirect;
    # }    
# if (!-e $request_filename) {

# rewrite ^(.*)$ /index.php?s=$1 last;

# break;

# }
# location / {
# 		if (!-e $request_filename) {
#           rewrite ^/index.php(.*)$ /index.php?s=$1 last;
#           rewrite ^/admin.php(.*)$ /admin.php?s=$1 last;
#           rewrite ^/api.php(.*)$ /api.php?s=$1 last;
#           rewrite ^(.*)$ /index.php?s=$1 last;
#           break;
#         }

#     } 
location / {
    proxy_pass http://favresin ;
    proxy_set_header Host $host:$server_port;
        #这里$server_port是调取 listen的端口！该行的意思是，在转发后获取原始的ip与端口
        proxy_set_header X-Real-IP $remote_addr;
        #这里表示把原始的信息带入进来
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #实际要访问的域名地址；要跟实际访问域名对应
        proxy_set_header X-NginX-Proxy true;
    }


   
	
	
     #屏蔽非常见蜘蛛（爬虫） Build/LMY47V;wv
    if ($http_user_agent ~* (SemrushBot|python|MJ12bot|AhrefsBot|AhrefsBot|hubspot|opensiteexplorer|leiki|webmeup|Build/LMY47V)) {
    return 444;}
     
    #SSL-START SSL related configuration, do NOT delete or modify the next line of commented-out 404 rules
    #error_page 404/404.html;
    ssl_certificate    /www/server/panel/vhost/cert/zhuyuming.com/fullchain.pem;
    ssl_certificate_key    /www/server/panel/vhost/cert/zhuyuming.com/privkey.pem;
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;
    add_header Strict-Transport-Security "max-age=31536000";
    error_page 497  https://$host$request_uri;

    #SSL-END

    #ERROR-PAGE-START  Error page configuration, allowed to be commented, deleted or modified
    #error_page 404 /404.html;
    #error_page 502 /502.html;
    #ERROR-PAGE-END

    #PHP-INFO-START  PHP reference configuration, allowed to be commented, deleted or modified
    include enable-php-74.conf;
    #PHP-INFO-END

    #REWRITE-START URL rewrite rule reference, any modification will invalidate the rewrite rules set by the panel
    include /www/server/panel/vhost/rewrite/zhuyuming.com.conf;
    #REWRITE-END

    # Forbidden files or directories
    location ~ ^/(\.user.ini|\.htaccess|\.git|\.svn|\.project|LICENSE|README.md)
    {
        return 404;
    }

    # Directory verification related settings for one-click application for SSL certificate
    location ~ \.well-known{
        allow all;
    }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf)$
    {
        expires      30d;
        error_log /dev/null;
        access_log off;
    }

    location ~ .*\.(js|css)?$
    {
        expires      12h;
        error_log /dev/null;
        access_log off; 
    }
    #access_log  /www/wwwlogs/zhuyuming.com.log;
    error_log  /www/wwwlogs/zhuyuming.com.error.log;
}

```
# 核心代码
```
upstream favresin{      

    server 212.11.22.12:8090 weight=1;
    # server 173.11.22.61:8090 weight=6;
    # server 185.11.22.33:80 weight=1;
}


location / {
    proxy_pass http://favresin ;
    proxy_set_header Host $host:$server_port;
        #这里$server_port是调取 listen的端口！该行的意思是，在转发后获取原始的ip与端口
        proxy_set_header X-Real-IP $remote_addr;
        #这里表示把原始的信息带入进来
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #实际要访问的域名地址；要跟实际访问域名对应
        proxy_set_header X-NginX-Proxy true;
    }
```

# 注意事项
注释的部分不能要！
