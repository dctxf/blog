---
title: Centos7安装PHP+MySql+Nginx+Wordpress
date: 2019-08-22 16:52:16
tags: [centos, nginx, mysql, php, wordpress, 运维, 博客]
---

公司需要一个新闻发布的系统，WordPress 是一个不错选择，记录下安装过程，以便今后使用有所参考

<!-- more -->

## 安装 PHP 及其扩展包

### 增加仓库

#### CentOS/RHEL 7.x:

```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

#### CentOS/RHEL 6.x:

```
rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm
```

#### 按需安装

```
yum install php71w-bcmath php71w-cli php71w-common php71w-dba php71w-devel php71w-embedded php71w-enchant php71w-fpm php71w-gd php71w-imap php71w-interbase php71w-intl php71w-ldap php71w-mbstring php71w-mcrypt php71w-mysqlnd php71w-odbc php71w-opcache php71w-pdo php71w-pdo_dblib php71w-pear php71w-pecl-apcu php71w-pecl-apcu-devel php71w-pecl-geoip php71w-pecl-igbinary php71w-pecl-igbinary-devel php71w-pecl-imagick php71w-pecl-imagick-devel php71w-pecl-memcached php71w-pecl-mongodb php71w-pecl-redis  php71w-pgsql php71w-process php71w-pspell php71w-recode php71w-snmp php71w-soap php71w-tidy php71w-xml php71w-xmlrpc
```

#### 全部安装

```
yum install -y php71w-bcmath php71w-cli php71w-common php71w-dba php71w-devel php71w-embedded php71w-enchant php71w-fpm php71w-gd php71w-imap php71w-interbase php71w-intl php71w-ldap php71w-mbstring php71w-mcrypt php71w-mysql php71w-mysqlnd php71w-odbc php71w-opcache php71w-pdo php71w-pdo_dblib php71w-pear php71w-pecl-apcu php71w-pecl-apcu-devel php71w-pecl-geoip php71w-pecl-igbinary php71w-pecl-igbinary-devel php71w-pecl-imagick php71w-pecl-imagick-devel php71w-pecl-memcached php71w-pecl-mongodb php71w-pecl-redis php71w-pecl-xdebug php71w-pgsql php71w-phpdbg php71w-process php71w-pspell php71w-recode php71w-snmp php71w-soap php71w-tidy php71w-xml php71w-xmlrpc
```

## 安装 MySQL

如果有 RDS 一类的服务，可跳过此步骤

```
yum localinstall http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm

yum install mysql-community-server

//开启mysql
service mysqld start

//查看mysql的root账号的密码
grep 'temporary password' /var/log/mysqld.log

//登录mysql
mysql -uroot -p

//修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';
```

如果修改密码提示密码不合法，想要设置更加简单的方法可以设置校验等级

```
set global validate_password_policy=0;
```

## 安装 Apache 和 PHP 模块

```
yum install -y httpd
yum install -y mod_php71w
```

如果需要 nginx 作为代理服务器需配置 httpd

```
vim /etc/httpd/conf/httpd.conf
```

找到`Listen 80`改为`Listen 8080`

## 安装 Nginx

```
yum install -y nginx
```

### 配置 Nginx

进入 `/etc/nginx/conf.d` 目录新建一个配置文件，比如：`wutong.conf`

增加一个 server 配置段来当做虚拟主机

```
server {

    listen 80;

    server_name demo.com;

    error_log logs/demo.com_error.log error;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    #处理PHP格式的文件
    location ~ \.php$ {
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_param  SCRIPT_FILENAME  /var/www/html$fastcgi_script_name;
        fastcgi_index  index.php;
        fastcgi_buffers 256 128k;
        fastcgi_connect_timeout 300s;
        fastcgi_send_timeout 300s;
        fastcgi_read_timeout 300s;
        include fastcgi_params;
    }
}
```

## 安装 wordpress

### 下载安装 wordpress

```
wget https://cn.wordpress.org/latest-zh_CN.zip   //下载 unzip
unzip latest-zh_CN.zip
mv wordpress/* /var/www/html
```

### 配置 wordpress 的配置文件，如果没有 vim 使用下面命令即可

```
cp wp-config-simple.php wp-config.php
vim wp-config.php  //编辑wordpress的配置文件
```

数据库配置
然后输入上面创建的数据库名称，用户名及密码。

```php
// ** MySQL 设置 - 具体信息来自您正在使用的主机 ** //
/** WordPress数据库的名称 */
define( 'DB_NAME', '数据库名' );

/** MySQL数据库用户名 */
define( 'DB_USER', '数据库用户名' );

/** MySQL数据库密码 */
define( 'DB_PASSWORD', '数据库密' );

/** MySQL主机 */
define( 'DB_HOST', 'localhost' );

/** 创建数据表时默认的文字编码 */
define( 'DB_CHARSET', 'utf8' );

/** 数据库整理类型。如不确定请勿更改 */
define( 'DB_COLLATE', '' );
```

## 启动

```
service nginx start
service php-fpm start
service httpd start
service mysqld start
```
