## 版本： 
Nginx官网（http://nginx.org/en/download.html）提供：
Mainline version：Mainline 是 Nginx 目前主力在做的版本，可以说是开发版
Stable version：最新稳定版，生产环境上建议使用的版本
Legacy versions：遗留的老版本的稳定版

启动: 双击nginx.exe启动 / 命令行 start nginx 
停止： nginx.exe –s quit
重新加载配置文件： nginx –s reload

访问 http://localhost/  出现nginx界面，证明安装成功


## 遇到的问题
#### 80端口被占用
解决方案 
参考 https://blog.csdn.net/Oraclesand/article/details/77847255
cmd窗口--------netstat -ano列出所有端口的情况----------打开资源管理器，找到PID是21548的进程

## virtualhost多站点配置
#### 1、修改系统hosts文件（C:\Windows\System32\drivers\etc）
在底部添加网址，如
127.0.0.1  www.seewo.com
#### 2、修改nginx配置 nginx/conf/nginx.conf
监听www.seewo.com 80端口，指向本地的  D:/nginx-example 这个文件
注： 如果www.seewo.com 对应端口没有配置，相当于访问127.0.0.1（系统host文件配置）
```
    # another virtual host using mix of IP-, name-, and port-based configuration                        
    #                        
    #modify by lirunkai 20181116 for virtual host www.seewo.com -s                         
        server {                        
            listen       80;                        
        access_log  logs/seewo.access.log;                    
            error_log  logs/seewo.error.log;                        
        server_name   www.seewo.com;                    
            location / {                        
                root   D:/nginx-example;                        
                index  index.html index.htm index.php;                        
            }                                       
        }                        
    #modify by lirunkai 20181116 for virtual host  www.seewo.com -e       
```









## 本地配置 https 证书
1、https://startssl.com/ 注册登录，获取证书


