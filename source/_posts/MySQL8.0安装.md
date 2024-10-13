---
title: MySQL8.0安装
description: "install mysql8.0"
comments: true
cover: https://github.com/jjnian/images/blob/main/blog/%E7%81%AB%E8%BD%A6%E7%AB%99.png?raw=true
---

## MySQL8.0 安装

### 1.安装

```shell
# 拉取包
wget https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm

# yum安装包
yum localinstall -y mysql80-community-release-el7-7.noarch.rpm

# 安装mysql
yum install -y mysql-community-server

# 启动mysql
systemctl start mysqld

# 查看初始密码
grep "password" /var/log/mysqld.log
```

### 2.修改密码

```shell
# 第一次进去必须修改
ALTER USER 'root'@'localhost' IDENTIFIED BY '密码'
```

### 3.创建外网访问的用户

```shell
# 方式一：设置root用户外网访问
update user set host = '%' where name = 'root'

# 方式二：创建一个新的用户mysql
CREATE USER '用户名'@'%' IDENTIFIED BY '密码';

# 给这个用户增加权限
GRANT ALL PRIVILEGES ON *.* TO 'mysql'@'%';
```

### 4.mysql 文件

配置文件：/etc/my.cnf

日志文件：/var/log/mysqld.log
