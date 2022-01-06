---
title: Mysql 初始化
date: 2020-07-24 09:42:55
tags: [mysql]
---

最近装了个数据库，死活连接不上，反正没什么东西，就重新初始化得了，记录下初始化的步骤

<!-- more -->

## 步骤

1. 查看数据存储的地方

```
cat /etc/my.conf
```

找到下面一句话

```
datadir=/var/lib/mysql
```

2. 备份数据

```
mv /var/lib/mysql /var/lib/mysql.bk
```

3. 重新初始化

```
mysqld --initialize --user=mysql
```

## 查看初始化密码

```
cat /var/log/mysqld.log | grep -i 'temporary password'
```

## 修改初始化密码

```
mysql -uroot -p
// 输入上面找到的密码
// 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```
