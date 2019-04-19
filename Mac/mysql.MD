## 下载
http://www.mysql.com/downloads/ 
#### 将mysql加入系统环境变量
(1).进入/usr/local/mysql/bin,查看此目录下是否有mysql

(2).执行vim ~/.bash_profile
在该文件中添加mysql/bin的目录，见pic8：

PATH=$PATH:/usr/local/mysql/bin

添加完成后，按esc，然后输入wq保存。

最后在命令行输入source ~/.bash_profile

## 登录mysql
mysql -uroot -p
