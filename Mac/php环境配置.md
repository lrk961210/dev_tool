
开启Apache：sudo apachectl start
关闭Apache：sudo apachectl stop
重启Apache：sudo apachectl restart

开启Apache
sudo vim /etc/apache2/httpd.conf
打开注释：
LoadModule php7_module libexec/apache2/libphp7.so

在 /Library/WebServer/Documents 创建文件 index.php
<?php phpinfo(); ?>
删除 index.html
重启Apache

可以看见php信息页


#### 修改默认页面

sudo vim /etc/apache2/httpd.conf

DocumentRoot "/Users/LRK/Downloads/basic"
<Directory "/Users/LRK/Downloads/basic">

未完成
