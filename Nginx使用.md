## 版本： 
Nginx官网（http://nginx.org/en/download.html）提供：
Mainline version：Mainline 是 Nginx 目前主力在做的版本，可以说是开发版
Stable version：最新稳定版，生产环境上建议使用的版本
Legacy versions：遗留的老版本的稳定版

- 启动: 双击nginx.exe启动 / 命令行 start nginx 
- 停止： nginx.exe –s quit
- 重新加载配置文件： nginx –s reload

访问 http://localhost/  出现nginx界面，证明安装成功


## 遇到的问题
#### 80端口被占用
解决方案 
参考 https://blog.csdn.net/Oraclesand/article/details/77847255
cmd窗口--------netstat -ano列出所有端口的情况----------打开资源管理器，找到PID是21548的进程

#### nginx: [emerg] unknown log format "main" in D:/nginx-1.14.1/nginx-1.14.1/conf.d/ng-cloud.conf:28
原因： 没有开启log_format选项（主配置文件中）

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

#### 运行node程序时指向本地特定端口
host配置
···
    127.0.0.1 www.node.com
···

host配置
···
    server {
        listen 80;
        server_name www.node.com;
        location / {
            proxy_pass http://127.0.0.1:3008;
        }
    }
···









## 本地配置 https 证书
#### 1、安装Openssl（http://slproweb.com/products/Win32OpenSSL.html）
#### 2、配置环境变量以及生成签名证书
```
> cd c:\ssl
// 设置变量
> set OPENSSL_CONF=C:\OpenSSL-Win64\bin\openssl.cfg
> echo %OPENSSL_CONF%
//生成server.key
> cd bin
> openssl genrsa -out server.key 4096
//生成request文件
> openssl req -new -key server.key -out server.csr
//获取私钥
> openssl x509 -req -days 730 -in server.csr -signkey server.key -out server.crt
```
其中，server.crt就是我们的证书，server.key就是私钥。

#### 3、在nginx中配置SSL
```
    # HTTPS server
    #
    server {
       listen       443 ssl;
       server_name  localhost;

       ssl_certificate      C:\OpenSSL-Win64\bin\server.crt; # 修改之处
       ssl_certificate_key  C:\OpenSSL-Win64\bin\server.key; # 修改之处

       ssl_session_cache    shared:SSL:1m;
       ssl_session_timeout  5m;

       ssl_ciphers  HIGH:!aNULL:!MD5;
       ssl_prefer_server_ciphers  on;

       location / {
           root   html;
           index  index.html index.htm;
       }
    }
```

重启nginx后，访问https://localhost/  会出现安全提示，继续前往 看到nginx主页面


注： 参考 https://troyyang.com/2017/11/07/windows-ssl-node-nginx/



注： 一篇介绍ssl根证书的文章 https://support.dnsimple.com/articles/what-is-ssl-root-certificate/


## mac 下面的使用
#### 安装brew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

#### 安装linux
brew search nginx

brew install nginx

安装完以后，可以在终端输出的信息里看到一些配置路径：
/usr/local/etc/nginx/nginx.conf （配置文件路径）
/usr/local/var/www （服务器默认路径）
/usr/local/Cellar/nginx/1.6.2 （安装路径）

## mac 下https的配置
https://www.jianshu.com/p/092049445f15

#### 常见问题
```
cvterdeMacBook-Pro:nginx cvter$ sudo nginx
nginx: [emerg] bind() to 0.0.0.0:8181 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8181 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8181 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8181 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8181 failed (48: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:80 failed (48: Address already in use)
nginx: [emerg] still could not bind()
cvterdeMacBook-Pro:nginx cvter$ sudo nginx -s stop
cvterdeMacBook-Pro:nginx cvter$ sudo nginx 
```

#### 清除dns缓存
```
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```
