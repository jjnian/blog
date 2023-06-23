---
title: pgsql
cover: >-
  https://images.pexels.com/photos/1120236/pexels-photo-1120236.jpeg?auto=compress&cs=tinysrgb&w=800
date: 2023-06-23 18:11:31
---



## [PostgresSQL](https://www.postgresql.org)

### [Cenos7安装pgsql](https://www.postgresql.org/download/linux/redhat/)

- 安装服务器

```shell
# 安装服务器
yum install postgresql-server

# 初始化服务器
/usr/bin/postgresql-setup initdb

# 设置服务为自启
systemctl enable postgresql.service

# 启动服务器
systemctl start postgresql.service
```

- 进入服务器

```shell
# 切换用户
su postgres

# 进入pgsql
psql
```

- 设置远程连接pgsql

​       修改/var/lib/pgsql/data/postgresql.conf

```shell
#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------

# - Connection Settings -
# 默认是关闭的，需要把能接连的ip和端口号打开
listen_addresses = '*'                  # what IP address(es) to listen on;
                                        # comma-separated list of addresses;
                                        # defaults to 'localhost'; use '*' for all
                                        # (change requires restart)
port = 5432                             # (change requires restart)
# Note: In RHEL/Fedora installations, you can't set the port number here;
```

​    修改/var/lib/pgsql/data/pg_hba.conf，在文件中添加如下

```shell
host    all             all              0.0.0.0/0               md5
```

- 重启pgsql服务器



