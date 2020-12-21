# 快捷手册09nginx
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