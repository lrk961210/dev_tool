## mysql数据库
参考 https://www.jianshu.com/p/7cccdaa2d177
$wget 'https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm'
$sudo rpm -Uvh mysql57-community-release-el7-11.noarch.rpm
$yum repolist all | grep mysql

开始安装
$sudo yum install mysql-community-server

使用
$sudo service mysqld start 
$sudo systemctl start mysqld #CentOS 7
$sudo systemctl status mysqld

// 查看密码
$sudo grep 'temporary password' /var/log/mysqld.log
// 输入查看到的密码
$ mysql -uroot -p  #输入查看到的密码
-> ALTER USER 'root'@'localhost' IDENTIFIED BY '你的新密码';
