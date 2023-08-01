# MYSQL 5 & MYSQL 8

[Windows10同时安装MySQL5.7和MySQL8.0版本](https://blog.csdn.net/weixin_55983004/article/details/123225494?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1-123225494-blog-90030601.235^v36^pc_relevant_anti_vip&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2~default~CTRLIST~Rate-1-123225494-blog-90030601.235^v36^pc_relevant_anti_vip&utm_relevant_index=1)

## mysql8

my.ini

```ini
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]

#设置时区为东八区，此项设置后，在连接MySQL的时候可以不用每次都手动设置时区
default-time-zone = '+8:00'
# 设置3308端口
port=3308
# 设置mysql的安装目录，记得切换成自己的路径
basedir=D:\MySQL\mysql-8.0.30-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\MySQL\mysql-8.0.30-winx64\data

character-set-server=utf8
collation-server=utf8_general_ci
default-storage-engine=INNODB

log-output=FILE
general-log=1
general_log_file=D:\MySQL\mysql-8.0.30-winx64mysql.log
slow-query-log=1
slow_query_log_file=D:\MySQL\mysql-8.0.30-winx64mysql_slow.log
long_query_time=2
```





```powershell
Microsoft Windows [Version 10.0.19045.2965]
(c) Microsoft Corporation。保留所有权利。

C:\WINDOWS\system32>net start mysql2
The mysql2 service is starting.
The mysql2 service could not be started.

The service did not report an error.

More help is available by typing NET HELPMSG 3534.


C:\WINDOWS\system32>sc delete mysql2
[SC] DeleteService SUCCESS

安装服务
C:\WINDOWS\system32>mysqld --install mysql8 --defaults-file=D:\MySQL\mysql-8.0.30-winx64\my.ini
Service successfully installed.

初始化
C:\WINDOWS\system32>mysqld --initialize --console
2023-05-19T11:07:38.012091Z 0 [System] [MY-013169] [Server] D:\MySQL\mysql-8.0.30-winx64\bin\mysqld.exe (mysqld 8.0.30) initializing of server in progress as process 8312
2023-05-19T11:07:38.013075Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
2023-05-19T11:07:38.013085Z 0 [Warning] [MY-013244] [Server] --collation-server: 'utf8mb3_general_ci' is a collation of the deprecated character set UTF8MB3. Please consider using UTF8MB4 with an appropriate collation instead.
2023-05-19T11:07:38.028387Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2023-05-19T11:07:38.281435Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2023-05-19T11:07:38.910858Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: g4.dt%o%=EHX

C:\WINDOWS\system32>net start mysql8
The mysql8 service is starting.
The mysql8 service was started successfully.

C:\WINDOWS\system32>mysql -u root -P 3308 -p
Enter password: ************
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.30

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> alter user 'root'@'localhost' identified with mysql_native_password by '1234';
Query OK, 0 rows affected (0.01 sec)

mysql> exit
Bye

C:\WINDOWS\system32>mysql -u root -P 3308 -p
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.30 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```

## mysql5

my.ini

```ini
[client]
default-character-set=utf8

[mysql]
default-character-set=utf8

[mysqld]
port=3305
character-set-server=utf8
basedir=D:\MySQL\mysql-5.7.24-winx64
# 设置mysql数据库的数据的存放目录（自动生成，不然可能报错）
datadir=D:\MySQL\mysql-5.7.24-winx64\data
collation-server=utf8_general_ci
default-storage-engine=INNODB
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

log-output=FILE
general-log=1
general_log_file="D:\MySQL\mysql-5.7.24-winx64\mysql.log"
slow-query-log=1
slow_query_log_file="D:\MySQL\mysql-5.7.24-winx64\mysql_slow.log"
long_query_time=2
```



```powershell
Microsoft Windows [Version 10.0.19045.2965]
(c) Microsoft Corporation。保留所有权利。

C:\WINDOWS\system32>mysqld --install mysql5 --defaults-file=D:\MySQL\mysql-8.0.30-winx64\my.ini
Service successfully installed.

C:\WINDOWS\system32>mysqld --initialize --console
2023-05-19T11:07:38.012091Z 0 [System] [MY-013169] [Server] D:\MySQL\mysql-8.0.30-winx64\bin\mysqld.exe (mysqld 5.7.34) initializing of server in progress as process 8312
2023-05-19T11:07:38.013075Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
2023-05-19T11:07:38.013085Z 0 [Warning] [MY-013244] [Server] --collation-server: 'utf8mb3_general_ci' is a collation of the deprecated character set UTF8MB3. Please consider using UTF8MB4 with an appropriate collation instead.
2023-05-19T11:07:38.028387Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2023-05-19T11:07:38.281435Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2023-05-19T11:07:38.910858Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: g4.dt%o%=EHX

C:\WINDOWS\system32>net start mysql5
The mysql5 service is starting.
The mysql5 service was started successfully.

C:\WINDOWS\system32>mysql -u root -P 3305 -p
Enter password: ************
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 5.7.34

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> alter user 'root'@'localhost' identified with mysql_native_password by '1234';
Query OK, 0 rows affected (0.01 sec)

mysql> exit
Bye

C:\WINDOWS\system32>mysql -u root -P 3305 -p
Enter password: ****
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 5.7.34 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
```



## mysql8免密登录

```powershell
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Try the new cross-platform PowerShell https://aka.ms/pscore6

PS C:\Users\lenovo> mysqld --console --skip-grant-tables --shared-memory
2023-05-19T14:12:45.593803Z 0 [System] [MY-010116] [Server] D:\MySQL\mysql-8.0.30-winx64\bin\mysqld.exe (mysqld 8.0.30) starting as process 2428
2023-05-19T14:12:45.595299Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
2023-05-19T14:12:45.595311Z 0 [Warning] [MY-013244] [Server] --collation-server: 'utf8mb3_general_ci' is a collation of the deprecated character set UTF8MB3. Please consider using UTF8MB4 with an appropriate collation instead.
2023-05-19T14:12:45.609953Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2023-05-19T14:12:46.040100Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
2023-05-19T14:12:46.154119Z 0 [Warning] [MY-011311] [Server] Plugin mysqlx reported: 'All I/O interfaces are disabled, X Protocol won't be accessible'
2023-05-19T14:12:46.175589Z 0 [System] [MY-010229] [Server] Starting XA crash recovery...
2023-05-19T14:12:46.178662Z 0 [System] [MY-010232] [Server] XA crash recovery finished.
2023-05-19T14:12:46.273236Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2023-05-19T14:12:46.273357Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2023-05-19T14:12:46.286967Z 0 [System] [MY-010931] [Server] D:\MySQL\mysql-8.0.30-winx64\bin\mysqld.exe: ready for connections. Version: '8.0.30'  socket: ''  port: 0  MySQL Community Server - GPL.
窗口不要关闭 重新打开一个powershell
```





