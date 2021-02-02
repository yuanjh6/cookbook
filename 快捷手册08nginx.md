# 快捷手册08nginx
快捷手册nginx


|          操作or命令          |                                                含义                                                 |
| ------------------------- | -------------------------------------------------------------------- |
| service nginx restart | 重启服务                                                                                        |
| nginx -s reload            | 重新加载Nginx配置文件，然后以优雅的方式重启Nginx      |
| nginx -s stop                | 强制停止Nginx服务                                                                     |
| nginx -s quit                | 优雅地停止Nginx服务（即处理完所有请求后再停止服务） |
| nginx -t -c xxx.conf    | 检测配置文件是否有语法错误，然后退出                                |
| nginx -t                         | 检测配置文件是否有语法错误，然后退出                                |
| nginx -p prefix            | 设置前缀路径(默认是:/usr/share/nginx/)                                |
| nginx -c filename       | 利用特定配置文件启动nginx                                                      |
| killall nginx                 | 杀死所有nginx进程                                                                      |


典型配置

```
# nginx的工作进程运行时的身份，也就是进程文件的属主和属组属性，如果在源码安装时configure配置已经指定用户和组，这里可以注释掉
#user  nobody;
# 定义nginx的工作进程的数量，一般为CPU核数或核数的倍数，该参数与并发量有关
worker_processes  1;
# 错误日志的位置
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
# nginx的master进程的pid存储文件，默认为/usr/local/nginx/logs/nginx.pid
#pid        logs/nginx.pid;
 
 
events {
    # 每一个工作进程可以接收的请求连接数，一般与系统的进程可以打开的文件描述符数量相同,
    worker_connections  1024;    # 并发量=工作进程数量*连接数
}
 
 
http {
    # mime.types文件含有nginx支持的媒体类型，include可以加载该文件
    include       mime.types;
    default_type  application/octet-stream;
    # 访问日志的格式，可以自定义
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    # 指定访问日志的位置和格式main
    #access_log  logs/access.log  main;
    # 自定义日志格式，名称为shoplog，便于后续使用
    log_format shoplog '$remote_addr@$time_local@$status@$http_user_agent';
    # 调用系统的方法传输文件，速度更快，
    sendfile        on;
    #tcp_nopush     on;
    #keepalive_timeout  0;
    keepalive_timeout  65;    # 客户端连接的超市时间，单位为秒，65s后自动断开
    
    # 隐藏nginx版本号，不再浏览显示
    server_tokens off;
    #gzip  on;
    server {
        listen 80;
        server_name 192.168.211.109;
        # 根目录，浏览器访问时默认在该目录下寻找资源文件
        root html/web;
        
        # 开启列表展示功能
        #autoindex on;
        # 人性化方式显示文件的大小
        #autoindex_exact_size off;
        # 显示本地时间
        #autoindex_localtime on;
        
        # fancy-index第三方模块提供的功能，使文件列表展示的更优美
        #fancyindex on;
        #fancyindex_exact_size off;        # 人性化方式显示文件的大小
        
        location / {
            # 执行默认寻找index.html
            index index.html;
            # 反向代理，将请求交给http://192.168.211.101:80服务器处理
        #    proxy_pass http://192.168.211.101:80;
        }
        #location ~* /1.HTML {
        #    error_page 404 = @html_err;
        #}
        #location @html_err {
        #    return 501;
        #}
    } 
    
    server {
        listen 80;
        server_name score.devops.com;
        # 域名重定向
        rewrite / http://shop.devops.com permanent;
    }
    server {
        listen 80;
        server_name shop.devops.com;
        root html/tp5shop/public;
        
        # gzip on压缩功能，将服务器传输的文件压缩返回给浏览器，可以减少传输的数据量，提供性能，一般浏览器是支持解压的
        gzip on;    #开启压缩功能
        gzip_http_version 1.0;    # 指定http的版本
        gzip_disable 'MSIE [1-6]';    # 禁止IE的1~6版本使用该功能
        gzip_types application/javascript text/css image/jpeg image/png;    # 指定压缩哪些类型的文件
        
        # 禁止ip访问，当有匹配时，就不会在向下匹配
        # deny all;        # 拒绝所有
        # allow 192.168.211.1;         # 允许192.168.211.1
        
        # 用户访问限制
        # auth_basic 'pls login:';        # 指定提示语"pls login:"
        # auth_basic_user_file /usr/local/nginx/conf/userlist;        # 指定授权用户所在文件
        
        # 基于域名的日志分割，所有访问shop.devops.com域名的访问日志记录在该文件中
        access_log /usr/local/nginx/logs/shop.devops.com shoplog;
        
        
        location / {
            # expires 设置客户端缓存
            #expires 1h;
            index index.php index.html;
            
            # 资源重定向，如访问http://shop.devops.com/index.html后会被重写为访问http://shop.devops.com/index.php，permanent表示永久重定向
            rewrite /index.html /index.php permanent;
            
            # 资源重定向，$request_filename为nginx的内置变量，表示资源文件路径
            if (!-e $request_filename) {
                rewrite ^(.*)$ /index.php?s=/$1 last;
                break;
            }
        }
        # 资源重定向
        #location /index {
        #    rewrite /index.html /index.php last;
        #}
        location ~ \.(js|css|jpg|png) {
            # 告诉客户端所有js,css,jpg,png文件都可以缓存1小时，不用重新在服务器下载
            expires 1h;
            # 防盗链实现，所有不是从shop.devops.com跳转过去访问js|css|jpg|png文件的都被拦截，返回404
            valid_referers shop.devops.com;
            if ($invalid_referer) {
                return 404;
            } 
        }
        # php解析
        location ~ \.php$ {
        #    root           html;
            fastcgi_pass   127.0.0.1:9000;
                fastcgi_index  index.php;
            fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
            include        fastcgi_params;
        }
    }
 
 
}

```