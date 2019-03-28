## mysql数据库
参考 https://www.jianshu.com/p/7cccdaa2d177
```
$wget 'https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm'
$sudo rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
$yum repolist all | grep mysql
```

开始安装
```
$sudo yum install mysql-community-server
```

使用

```
$sudo service mysqld start 
$sudo systemctl start mysqld #CentOS 7
$sudo systemctl status mysqld
```

// 查看密码
` $sudo grep 'temporary password' /var/log/mysqld.log `
// 输入查看到的密码
```
$ mysql -uroot -p  #输入查看到的密码
-> ALTER USER 'root'@'localhost' IDENTIFIED BY '你的新密码';
-> flush privileges;
```
// 创建数据库、新用户（<>不要输）
```
-> CREATE DATABASE <datebasename> CHARACTER SET utf8;
-> CREATE USER 'username'@'host' IDENTIFIED BY 'password';
-> GRANT privileges ON databasename.tablename TO 'username'@'host';
-> SHOW GRANTS FOR 'username'@'host';
-> REVOKE privilege ON databasename.tablename FROM 'username'@'host';
-> DROP USER 'username'@'host';
// username：你将创建的用户名
// host：指定该用户在哪个主机上可以登陆，如果是本地用户可用 localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符 %
// password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
// privileges：用户的操作权限，如 SELECT，INSERT，UPDATE 等，如果要授予所的权限则使用ALL
// databasename：数据库名
// tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用 * 表示，如 *.*
```

// 开放防火墙端口
https://www.jianshu.com/p/bad33004bb4f
```
sudo firewall-cmd --zone=public --add-port=3306/tcp --permanent
sudo firewall-cmd --reload
firewall-cmd --list-all

```

## 搭建node服务器
参考 https://segmentfault.com/a/1190000009393665#articleHeader7
#### 搭建nginx 
```
yum install -y nginx
service nginx start
wget http://127.0.0.1
```
防火墙开放 80 端口，重启防火墙

#### 安装nodeJs
```
yum install -y gcc gcc-c++ openssl-devel
gcc -v
// 检查Python的版本 > 2.6
python 
// 开始安装node
cd /usr/local/src
wget http://nodejs.org/dist/v8.15.1/node-v8.15.1-linux-x64.tar.gz
tar -zxvf node-v8.15.1-linux-x64.tar.gz
cd node-v8.15.1-linux-x64
./bin/node -v
pwd
ln -s /usr/local/src/node-v8.15.1-linux-x64/bin/node /usr/local/bin/node 
ln -s /usr/local/src/node-v8.15.1-linux-x64/bin/npm  /usr/local/bin/npm 

```

去 /var/www/ 下面建立一个index.js ,测试node
 ```
 // index.js
 // 监听 3000 端口
const http = require('http');
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});
server.listen(3000, () => {
  console.log(`node server is now running/`);
});

 ```
 
 nginx 配置
 ```
 location / {
   proxy_pass http://127.0.0.1:3000;

}
 ```
 
 重启nginx及打开node服务
 ```
  service nginx restart
  node /var/www/index
 ```

