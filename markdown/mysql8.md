# Linux安装MySQL-8.0.30(二进制压缩包安装)

## 1.下载MySQL二进制压缩包

[MySQL :: Download MySQL Community Server (Archived Versions) --- MySQL ：： 下载 MySQL Community Server （存档版本）](https://downloads.mysql.com/archives/community/)

## 2.解压

```bash
root@node1:~$ tar -xf mysql-8.0.30-linux-glibc2.12-x86_64.tar.xz -C /opt/module

root@node1:/opt/module# mv mysql-8.0.30-linux-glibc2.12-x86_64/ mysql-8.0.30
```

## 3.创建数据库相关目录

```bash
root@node1:/opt/module/mysql-8.0.30# mkdir data
root@node1:/opt/module/mysql-8.0.30# mkdir sock
root@node1:/opt/module/mysql-8.0.30# mkdir log
root@node1:/opt/module/mysql-8.0.30# mkdir pid
```

## 4.添加mysql用户组与用户

```bash
root@node1:/opt/module/mysql-8.0.30# groupadd mysql
root@node1:/opt/module/mysql-8.0.30# useradd -r -g mysql -s /bin/false mysql
```

## 5.编辑配置文件

```bash
root@node1:/opt/module/mysql-8.0.30# vim /etc/my.cnf
```

### 5.1my.cnf

```ini
# For advice on how to change settings please see
# https://dev.mysql.com/doc/refman/8.0/en/server-configuration-defaults.html

[client]
# 不推荐使用默认端口3306
port=3308
default-character-set=utf8mb4
socket=/opt/module/mysql-8.0.30/sock/mysql.sock
[mysql]
default-character-set=utf8mb4

[mysqld]
# 不推荐使用默认端口3306
port=3308
# 绝对路径依据实际情况修改
basedir=/opt/module/mysql-8.0.30/
datadir=/opt/module/mysql-8.0.30/data/
# tmpdir=/opt/module/mysql-8.0.30/data/temp/
socket=/opt/module/mysql-8.0.30/sock/mysql.sock
pid-file=/opt/module/mysql-8.0.30/pid/mysql.pid

# General and Slow logging.
log-output=FILE
general-log=0
general_log_file=/opt/module/mysql-8.0.30/log/mysql-general.log
slow-query-log=1
slow_query_log_file=/opt/module/mysql-8.0.30/log/mysql-slow.log
long_query_time=10
# Error Logging.
log-error=/opt/module/mysql-8.0.30/log/mysql-8.0.err

# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
#
# Remove leading # to revert to previous value for default_authentication_plugin,
# this will increase compatibility with older clients. For background, see:
# https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_authentication_plugin
# default-authentication-plugin=mysql_native_password
# default_authentication_plugin=caching_sha2_password
default_authentication_plugin=mysql_native_password
default-storage-engine=INNODB
character-set-server=utf8mb4
max_connections=600
max_connect_errors=100
transaction_isolation=READ-COMMITTED
max_allowed_packet=64M
default-time-zone='+8:00'
log_timestamps=system
lower_case_table_names=1
table_open_cache=2000
tmp_table_size=512M
key_buffer_size=512M

innodb_flush_log_at_trx_commit=1
innodb_log_buffer_size=16M
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
innodb_buffer_pool_size=4G
#
# Remove the leading "# " to disable binary logging
# Binary logging captures changes between backups and is enabled by
# default. It's default setting is log_bin=binlog
# disable_log_bin
#
innodb_log_file_size=1G
innodb_autoextend_increment=64
innodb_buffer_pool_instances=8
innodb_open_files=300
innodb_file_per_table=1
```



## 6.更改目录所属组和用户为mysql

``` bash
root@node1:/opt/module/mysql-8.0.30# chown -R mysql:mysql /opt/module/mysql-8.0.30/
```

## 7.初始化

```bash
root@node1:/opt/module/mysql-8.0.30# bin/mysqld --defaults-file=/etc/my.cnf --basedir=/opt/module/mysql-8.0.30/ --datadir=/opt/module/mysql-8.0.30/data --user=mysql --initialize
```

### 7.1缺少依赖

```bash
bin/mysqld: error while loading shared libraries: libaio.so.1: cannot open shared object file: No such file or directory
# 安装依赖
root@node1:/opt/module/mysql-8.0.30# apt install libaio1
```

## 8.创建 SSL 证书和密钥文件以及 RSA 密钥对文件(可选)

```bash
root@node1:/opt/module/mysql-8.0.30# bin/mysql_ssl_rsa_setup --datadir=/opt/module/mysql-8.0.30/data
```

## 9.复制mysql启动配置到linux系统启动目录下

```bash
root@node1:~# cp /opt/module/mysql-8.0.30/support-files/mysql.server /etc/init.d/mysql
```

### 9.1修改 basedir datadir 参数

```bash
root@node1:~# vim /etc/init.d/mysql
```

## 10.启动mysql服务

```bash
root@node1:~# /etc/init.d/mysql start
Starting mysql (via systemctl): mysql.service.
```

### 10.1查看mysql服务状态

```bash
root@node1:~# systemctl status mysql
● mysql.service - LSB: start and stop MySQL
     Loaded: loaded (/etc/init.d/mysql; generated)
     Active: active (running) since Sat 2023-05-20 17:20:29 PDT; 11s ago
       Docs: man:systemd-sysv-generator(8)
    Process: 5117 ExecStart=/etc/init.d/mysql start (code=exited, status=0/SUCCESS)
      Tasks: 43 (limit: 4578)
     Memory: 1.9G
     CGroup: /system.slice/mysql.service
             ├─5133 /bin/sh /opt/module/mysql-8.0.30//bin/mysqld_safe --datadir=/opt/module/mysql-8.0.30/data/ -->
             └─5623 /opt/module/mysql-8.0.30/bin/mysqld --basedir=/opt/module/mysql-8.0.30/ --datadir=/opt/module>

May 20 17:20:27 node1 systemd[1]: Starting LSB: start and stop MySQL...
May 20 17:20:27 node1 mysql[5117]: Starting MySQL
May 20 17:20:29 node1 mysql[5117]: .. *
May 20 17:20:29 node1 systemd[1]: Started LSB: start and stop MySQL.
```

## 11.查看root用户初始密码

```bash
root@node1:/opt/module/mysql-8.0.30# cat log/mysql-8.0.err | grep "password"
2023-05-20T17:16:51.057092-07:00 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: CgoGakd+a8Lo
```

## 12.登录并修改root密码

```bash
root@node1:/opt/module/mysql-8.0.30# bin/mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.30

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
# 修改密码
mysql> alter user 'root'@'localhost' identified with mysql_native_password by '1234';
Query OK, 0 rows affected (0.00 sec)
# 允许root用户远程登录
mysql> update mysql.user set host = '%' where user = 'root';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
# 刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)
# 退出
mysql> exit;
Bye
```

### 12.1缺少依赖

```bash
bin/mysql: error while loading shared libraries: libtinfo.so.5: cannot open shared object file: No such file or directory
# 安装依赖
root@node1:/opt/module/mysql-8.0.30# apt install libtinfo5
```

## 13.mysql开机自启(可选)

```bash
root@node1:/home/hadoop# update-rc.d mysql defaults
```

## 14.配置环境变量

