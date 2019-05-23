---
layout: post
title: nginx+php+wordpress部署安装
date: 2018-09-07 14:05
author: chuwei
comments: true
categories: [未分类]
---
## 一、Nginx部署安装
CentOS下nginx安装可以直接使用yum命令。安装完毕后nginx安装目录在 /etc/nginx
```
yum -y install nginx
```
手工编译安装方式如下：
安装Nginx依赖：c++环境
```
yum -y install gcc-c++
yum -y install pcre pcre-devel
yum -y install zlib zlib-devel
yum -y install openssl openssl-devel
```
解压nginx.
```
tar -zxvf nginx-1.15.0.tar.gz
```
进入nginx目录。
执行命令：
```
./configure
```
nginx 安装目录在 /usr/local/nginx

输入命令：
```
make
make install;
```
就可以在/usr/local/ 目录下找到nginx。
启动则是在/usr/local/nginx/sbin

命令：./nginx 启动nginx
停止命令：./nginx -s stop
重启命令：./nginx -s reload

添加Nginx到系统服务：

在/etc/init.d/目录下编写脚本，名为nginx
```
vi /etc/init.d/nginx
```
```
#! /bin/bash
# chkconfig: - 85 15
PATH=/usr/local/nginx
DESC="nginx daemon"
NAME=nginx
DAEMON=$PATH/sbin/$NAME
CONFIGFILE=$PATH/conf/$NAME.conf
PIDFILE=$PATH/logs/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME
set -e
[ -x "$DAEMON" ] || exit 0
do_start() {
$DAEMON -c $CONFIGFILE || echo -n "nginx already running"
}
do_stop() {
$DAEMON -s stop || echo -n "nginx not running"
}
do_reload() {
$DAEMON -s reload || echo -n "nginx can't reload"
}
case "$1" in
start)
echo -n "Starting $DESC: $NAME"
do_start
echo "."
;;
stop)
echo -n "Stopping $DESC: $NAME"
do_stop
echo "."
;;
reload|graceful)
echo -n "Reloading $DESC configuration..."
do_reload
echo "."
;;
restart)
echo -n "Restarting $DESC: $NAME"
do_stop
do_start
echo "."
;;
*)
echo "Usage: $SCRIPTNAME {start|stop|reload|restart}" &gt;&amp;2
exit 3
;;
esac
exit 0
```
设置执行权限
```
chmod a+x /etc/init.d/nginx
```
注册成服务
```
chkconfig --add nginix
```
设置开机启动
```
chkconfig nginx on
```
## 二、PHP部署安装

php下载地址：[http://www.php.net/downloads.php](http://www.php.net/downloads.php)

解压安装
```
tar -zxfv php-7.2.9.tar.gz
cd  php-7.2.9
./configure   --help
./configure --prefix=/usr/local/php \
--with-curl \
--with-freetype-dir \
--with-gd \
--with-gettext \
--with-iconv-dir \
--with-kerberos \
--with-libdir=lib64 \
--with-libxml-dir \
--with-mysqli \
--with-openssl \
--with-pcre-regex \
--with-pdo-mysql \
--with-pdo-sqlite \
--with-pear \
--with-png-dir \
--with-xmlrpc \
--with-xsl \
--with-zlib \
--enable-fpm \
--enable-bcmath \
--enable-libxml \
--enable-inline-optimization \
--enable-gd-native-ttf \
--enable-mbregex \
--enable-mbstring \
--enable-opcache \
--enable-pcntl \
--enable-shmop \
--enable-soap \
--enable-sockets \
--enable-sysvsem \
--enable-xml \
--enable-zip
```
如果配置错误，需要安装需要的模块，直接yum一并安装依赖库
```
yum -y install libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel mysql pcre-devel
```
编译安装
```
make
make install
```
配置文件
```
cp php.ini-development /usr/local/php/lib/php.ini
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
cp -R ./sapi/fpm/php-fpm /etc/init.d/php-fpm
```
需要注意的是php7中www.conf这个配置文件配置phpfpm的端口号等信息，如果修改默认的9000端口号需在这里改，再改nginx的配置

php-fpm后台启动参数，在php-fpm.conf中
```
daemonize = yes
#后台执行fpm,默认值为yes，如果为了调试可以改为no。在FPM中，可以使用不同的设置来运行多个进程池。 这些设置可以针对每个进程池单独设置。
```
启动
```
/etc/init.d/php-fpm
```
php-fpm的启动参数
```
#测试php-fpm配置
/usr/local/php/sbin/php-fpm -t
/usr/local/php/sbin/php-fpm -c /usr/local/php/etc/php.ini -y /usr/local/php/etc/php-fpm.conf -t

#启动php-fpm
/usr/local/php/sbin/php-fpm
/usr/local/php/sbin/php-fpm -c /usr/local/php/etc/php.ini -y /usr/local/php/etc/php-fpm.conf

#关闭php-fpm
kill -INT `cat /usr/local/php/var/run/php-fpm.pid`

#重启php-fpm
kill -USR2 `cat /usr/local/php/var/run/php-fpm.pid`
```
## 三、配置Nginx访问PHP文件
```
cd /usr/local/nginx/conf
vim nginx.conf
```
```
location / {
root /var/www;
index index.html index.htm index.php;
}
location ~ \.php$ {
root /var/www;
fastcgi_pass 127.0.0.1:9000;
fastcgi_index index.php;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
include fastcgi_params;
}
```

## 四、安装WordPress
下载WordPress，解压到 /var/www/wordpress
```
cp wp-config-sample.php wp-config.php
vim wp-config.php
```
```
设置数据库连接用户名密码
// ** MySQL 设置 - 具体信息来自您正在使用的主机 ** //
/** WordPress数据库的名称 */
define('DB_NAME', '######');

/** MySQL数据库用户名 */
define('DB_USER','######');

/** MySQL数据库密码 */
define('DB_PASSWORD', '######');

/** MySQL主机 */
define('DB_HOST', 'localhost');
```
