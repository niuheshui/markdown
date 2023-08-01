# Hive

## 最小化部署

开启hadoop集群

解压

配置环境变量

报错

```bash
[root@node1 hive-3.1.3]# bin/schematool -dbType derby -initSchema
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1380)
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1361)
	at org.apache.hadoop.mapred.JobConf.setJar(JobConf.java:536)
	at org.apache.hadoop.mapred.JobConf.setJarByClass(JobConf.java:554)
	at org.apache.hadoop.mapred.JobConf.<init>(JobConf.java:448)
	at org.apache.hadoop.hive.conf.HiveConf.initialize(HiveConf.java:5144)
	at org.apache.hadoop.hive.conf.HiveConf.<init>(HiveConf.java:5107)
	at org.apache.hive.beeline.HiveSchemaTool.<init>(HiveSchemaTool.java:96)
	at org.apache.hive.beeline.HiveSchemaTool.main(HiveSchemaTool.java:1473)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:323)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:236)
```


conf目录下新建log4j.properties

```properties
log4j.rootLogger=ERROR, CA
log4j.appender.CA=org.apache.log4j.ConsoleAppender
log4j.appender.CA.layout=org.apache.log4j.PatternLayout
log4j.appender.CA.layout.ConversionPattern=%-4r [%t] %-5p %c %x - %m%n
```

初始化元数据库

```bash
[root@node1 hive-3.1.3]# bin/schematool -dbType derby -initSchema
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Metastore connection URL:	 jdbc:derby:;databaseName=metastore_db;create=true
Metastore Connection Driver :	 org.apache.derby.jdbc.EmbeddedDriver
Metastore connection User:	 APP
Starting metastore schema initialization to 3.1.0
Initialization script hive-schema-3.1.0.derby.sql
省略200多空行...
Initialization script completed
schemaTool completed
```

```bash
[root@node1 conf]# ll
总用量 332
-rw-r--r-- 1 root root   1596 10月 24 2019 beeline-log4j2.properties.template
-rw-r--r-- 1 root root 300727 4月   4 2022 hive-default.xml.template
-rw-r--r-- 1 root root   2365 10月 24 2019 hive-env.sh.template
-rw-r--r-- 1 root root   2274 10月 24 2019 hive-exec-log4j2.properties.template
-rw-r--r-- 1 root root   3086 9月   5 2020 hive-log4j2.properties.template
-rw-r--r-- 1 root root   2060 10月 24 2019 ivysettings.xml
-rw-r--r-- 1 root root   3558 9月   5 2020 llap-cli-log4j2.properties.template
-rw-r--r-- 1 root root   6937 3月  29 2022 llap-daemon-log4j2.properties.template
-rw-r--r-- 1 root root   2662 10月 24 2019 parquet-logging.properties
[root@node1 conf]# cd ..
[root@node1 hive-3.1.3]# ll
总用量 56
drwxr-xr-x 3 root root   157 5月  20 09:40 bin
drwxr-xr-x 2 root root  4096 5月  20 09:40 binary-package-licenses
drwxr-xr-x 2 root root  4096 5月  20 09:40 conf
drwxr-xr-x 4 root root    34 5月  20 09:40 examples
drwxr-xr-x 7 root root    68 5月  20 09:40 hcatalog
drwxr-xr-x 2 root root    44 5月  20 09:40 jdbc
drwxr-xr-x 4 root root 12288 5月  20 09:40 lib
-rw-r--r-- 1 root root 20798 3月  29 2022 LICENSE
-rw-r--r-- 1 root root   230 3月  29 2022 NOTICE
-rw-r--r-- 1 root root   540 3月  29 2022 RELEASE_NOTES.txt
drwxr-xr-x 4 root root    35 5月  20 09:40 scripts
[root@node1 hive-3.1.3]# vim conf/log4j.properties
[root@node1 hive-3.1.3]# bin/schematool -dbType derby -initSchema
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1380)
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1361)
	at org.apache.hadoop.mapred.JobConf.setJar(JobConf.java:536)
	at org.apache.hadoop.mapred.JobConf.setJarByClass(JobConf.java:554)
	at org.apache.hadoop.mapred.JobConf.<init>(JobConf.java:448)
	at org.apache.hadoop.hive.conf.HiveConf.initialize(HiveConf.java:5144)
	at org.apache.hadoop.hive.conf.HiveConf.<init>(HiveConf.java:5107)
	at org.apache.hive.beeline.HiveSchemaTool.<init>(HiveSchemaTool.java:96)
	at org.apache.hive.beeline.HiveSchemaTool.main(HiveSchemaTool.java:1473)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:323)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:236)
[root@node1 hive-3.1.3]# cd lib/        
[root@node1 lib]# mv guava-19.0.jar guava-19.0.jar_bak
[root@node1 lib]# cp /opt/module/hadoop-3.3.0/share/hadoop/common/lib/guava-27.0-jre.jar ./
[root@node1 lib]# ll guava-
guava-19.0.jar_bak  guava-27.0-jre.jar  
[root@node1 lib]# cd ..
[root@node1 hive-3.1.3]# bin/schematool -dbType derby -initSchema
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Metastore connection URL:	 jdbc:derby:;databaseName=metastore_db;create=true
Metastore Connection Driver :	 org.apache.derby.jdbc.EmbeddedDriver
Metastore connection User:	 APP
Starting metastore schema initialization to 3.1.0
Initialization script hive-schema-3.1.0.derby.sql
 ---- 200空行 ----
Initialization script completed
schemaTool completed
[root@node1 hive-3.1.3]# hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
0    [main] WARN  org.apache.hadoop.hive.common.LogUtils  - hive-site.xml not found on CLASSPATH
Hive Session ID = c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
1210 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  org.apache.hadoop.hive.metastore.ObjectStore  - datanucleus.autoStartMechanismMode is set to unsupported value null . Setting it to value: ignored
1512 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  com.zaxxer.hikari.util.DriverDataSource  - Registered driver with driverClassName=org.apache.derby.jdbc.EmbeddedDriver was not found, trying direct instantiation.
1867 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  com.zaxxer.hikari.util.DriverDataSource  - Registered driver with driverClassName=org.apache.derby.jdbc.EmbeddedDriver was not found, trying direct instantiation.
2389 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
2389 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
2389 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
2389 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
2389 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
2389 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
3162 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
3163 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
3163 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
3163 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
3163 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
3163 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  DataNucleus.MetaData  - Metadata has jdbc-type of null yet this is not valid. Ignored
4336 [c71a6bcf-59d6-4dbc-8df1-d9abbb48c06d main] WARN  org.apache.hadoop.hive.metastore.ObjectStore  - Failed to get database hive.default, returning NoSuchObjectException
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 3dc0f205-fcd6-43aa-aec8-0091f41c9f6a
hive> show databases;
OK
default
Time taken: 0.378 seconds, Fetched: 1 row(s)
hive> exit;
您在 /var/spool/mail/root 中有新邮件
[root@node1 hive-3.1.3]# vim conf/log4j.properties 
[root@node1 hive-3.1.3]# hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = 9d1bc39a-35b8-4777-97f3-5bf1a50fb2bb

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 563d752c-b92b-4e86-8662-228d4828bdc8
hive> show databases;
OK
default
Time taken: 0.396 seconds, Fetched: 1 row(s)
hive> 
```

测试

```bash
[root@node1 hive-3.1.3]# hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = 9d1bc39a-35b8-4777-97f3-5bf1a50fb2bb

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 563d752c-b92b-4e86-8662-228d4828bdc8
hive> show databases;
OK
default
Time taken: 0.396 seconds, Fetched: 1 row(s)
hive> create table stu(id int, name string);
OK
Time taken: 0.439 seconds
hive> insert into stu values(1, "ss");
Query ID = root_20230520094824_1ba51d73-26d4-4a04-bd82-f0b0dbeeadf6
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1684546520162_0001, Tracking URL = http://node2:8088/proxy/application_1684546520162_0001/
Kill Command = /opt/module/hadoop-3.3.0/bin/mapred job  -kill job_1684546520162_0001
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2023-05-20 09:48:34,849 Stage-1 map = 0%,  reduce = 0%
2023-05-20 09:48:41,068 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 2.19 sec
2023-05-20 09:48:45,255 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.54 sec
MapReduce Total cumulative CPU time: 3 seconds 540 msec
Ended Job = job_1684546520162_0001
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to directory hdfs://node1:8020/user/hive/warehouse/stu/.hive-staging_hive_2023-05-20_09-48-24_924_2642881142958873638-1/-ext-10000
Loading data to table default.stu
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.54 sec   HDFS Read: 15116 HDFS Write: 235 SUCCESS
Total MapReduce CPU Time Spent: 3 seconds 540 msec
OK
Time taken: 21.805 seconds
hive> select * from stu;
OK
1	ss
Time taken: 0.112 seconds, Fetched: 1 row(s)
hive> exit;
```

## 安装mysql

```bash
卸载 mariadb 数据库
[root@node1 mysql]# rpm -qa | grep mariadb | xargs rpm -e --nodeps
[root@node1 mysql]# tar -xf mysql-5.7.28-1.el7.x86_64.rpm-bundle.tar
[root@node1 mysql]# rpm -ivh mysql-community-common-5.7.28-1.el7.x86_64.rpm 
警告：mysql-community-common-5.7.28-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-common-5.7.28-1.e################################# [100%]
[root@node1 mysql]# rpm -ivh mysql-community-libs- 
错误：打开 mysql-community-libs- 失败： 没有那个文件或目录
[root@node1 mysql]# rpm -ivh mysql-community-libs-5.7.28-1.el7.x86_64.rpm 
警告：mysql-community-libs-5.7.28-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-libs-5.7.28-1.el7################################# [100%]
[root@node1 mysql]# rpm -ivh mysql-community-libs-compat-5.7.28-1.el7.x86_64.rpm 
警告：mysql-community-libs-compat-5.7.28-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-libs-compat-5.7.2################################# [100%]
[root@node1 mysql]# rpm -ivh mysql-community-client-5.7.28-1.el7.x86_64.rpm 
警告：mysql-community-client-5.7.28-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-client-5.7.28-1.e################################# [100%]
[root@node1 mysql]# 
[root@node1 mysql]# rpm -ivh mysql-community-server-5.7.28-1.el7.x86_64.rpm 
警告：mysql-community-server-5.7.28-1.el7.x86_64.rpm: 头V3 DSA/SHA1 Signature, 密钥 ID 5072e1f5: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:mysql-community-server-5.7.28-1.e################################# [100%]
[root@node1 mysql]# systemctl start mysqld
[root@node1 mysql]# cat /var/log/mysqld.log | grep password
2023-05-20T02:42:15.938450Z 1 [Note] A temporary password is generated for root@localhost: 2zs>/(xRmbos
[root@node1 mysql]# 

```



初始化mysql

```bash
[root@node1 mysql]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.28

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> set password=password("1234");
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> set global validate_password_policy=0;
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password_length=4;
Query OK, 0 rows affected (0.00 sec)

mysql> set password=password("1234");
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> update user set host="%" where user="root";
ERROR 1046 (3D000): No database selected
mysql> update mysql.user set host="%" where user="root";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select user, host from mysql.user;
+---------------+-----------+
| user          | host      |
+---------------+-----------+
| root          | %         |
| mysql.session | localhost |
| mysql.sys     | localhost |
+---------------+-----------+
3 rows in set (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> exit;
```

新建Hive元数据库

```bash
[root@node1 mysql]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.28 MySQL Community Server (GPL)

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database metastore;
Query OK, 1 row affected (0.00 sec)

mysql> exit;
Bye

```





将mysql驱动拷贝到Hive的lib目录

```bash
[root@node1 mysql]# cp mysql-connector-java-5.1.34.jar /opt/module/hive-3.1.3/lib/
```



在$HIVE_HOME/conf目录新建hive-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- jdbc 连接的 URL -->
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://node1:3306/metastore?useSSL=false</value>
    </property>
    <!-- jdbc 连接的 Driver-->
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    <!-- jdbc 连接的 username-->
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>
    <!-- jdbc 连接的 password -->
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>1234</value>
    </property>
    <!-- Hive 默认在 HDFS 的工作目录 -->
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
    </property>

</configuration>

```

初始化hive元数据库

```bash
[root@node1 hive-3.1.3]# bin/schematool -dbType mysql -initSchema -verbose
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Metastore connection URL:	 jdbc:mysql://node1:3306/metastore?useSSL=false
Metastore Connection Driver :	 com.mysql.jdbc.Driver
Metastore connection User:	 root
Starting metastore schema initialization to 3.1.0
Initialization script hive-schema-3.1.0.mysql.sql
Connecting to jdbc:mysql://node1:3306/metastore?useSSL=false
Connected to: MySQL (version 5.7.28)
Driver: MySQL Connector Java (version mysql-connector-java-5.1.34 ( Revision: jess.balint@oracle.com-20141014163213-wqbwpf1ok2kvo1om ))
Transaction isolation: TRANSACTION_READ_COMMITTED
0: jdbc:mysql://node1:3306/metastore> !autocommit on
Autocommit status: true
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET NAMES utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40103 SET TIME_ZONE='+00:00' */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `BUCKETING_COLS` ( `SD_ID` bigint(20) NOT NULL, `BUCKET_COL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID`,`INTEGER_IDX`), KEY `BUCKETING_COLS_N49` (`SD_ID`), CONSTRAINT `BUCKETING_COLS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `CDS` ( `CD_ID` bigint(20) NOT NULL, PRIMARY KEY (`CD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `COLUMNS_V2` ( `CD_ID` bigint(20) NOT NULL, `COMMENT` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TYPE_NAME` MEDIUMTEXT DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`CD_ID`,`COLUMN_NAME`), KEY `COLUMNS_V2_N49` (`CD_ID`), CONSTRAINT `COLUMNS_V2_FK1` FOREIGN KEY (`CD_ID`) REFERENCES `CDS` (`CD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `DATABASE_PARAMS` ( `DB_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(180) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`DB_ID`,`PARAM_KEY`), KEY `DATABASE_PARAMS_N49` (`DB_ID`), CONSTRAINT `DATABASE_PARAMS_FK1` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE `CTLGS` ( `CTLG_ID` BIGINT PRIMARY KEY, `NAME` VARCHAR(256), `DESC` VARCHAR(4000), `LOCATION_URI` VARCHAR(4000) NOT NULL, UNIQUE KEY `UNIQUE_CATALOG` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO `CTLGS` VALUES (1, 'hive', 'Default catalog for Hive', 'TBD')
1 row affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `DBS` ( `DB_ID` bigint(20) NOT NULL, `DESC` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DB_LOCATION_URI` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `OWNER_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `OWNER_TYPE` varchar(10) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `CTLG_NAME` varchar(256) NOT NULL DEFAULT 'hive', PRIMARY KEY (`DB_ID`), UNIQUE KEY `UNIQUE_DATABASE` (`NAME`, `CTLG_NAME`), CONSTRAINT `CTLG_FK1` FOREIGN KEY (`CTLG_NAME`) REFERENCES `CTLGS` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `DB_PRIVS` ( `DB_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `DB_ID` bigint(20) DEFAULT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DB_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`DB_GRANT_ID`), UNIQUE KEY `DBPRIVILEGEINDEX` (`AUTHORIZER`,`DB_ID`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`DB_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), KEY `DB_PRIVS_N49` (`DB_ID`), CONSTRAINT `DB_PRIVS_FK1` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `GLOBAL_PRIVS` ( `USER_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `USER_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`USER_GRANT_ID`), UNIQUE KEY `GLOBALPRIVILEGEINDEX` (`AUTHORIZER`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`USER_PRIV`,`GRANTOR`,`GRANTOR_TYPE`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `IDXS` ( `INDEX_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `DEFERRED_REBUILD` bit(1) NOT NULL, `INDEX_HANDLER_CLASS` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INDEX_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INDEX_TBL_ID` bigint(20) DEFAULT NULL, `LAST_ACCESS_TIME` int(11) NOT NULL, `ORIG_TBL_ID` bigint(20) DEFAULT NULL, `SD_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`INDEX_ID`), UNIQUE KEY `UNIQUEINDEX` (`INDEX_NAME`,`ORIG_TBL_ID`), KEY `IDXS_N51` (`SD_ID`), KEY `IDXS_N50` (`INDEX_TBL_ID`), KEY `IDXS_N49` (`ORIG_TBL_ID`), CONSTRAINT `IDXS_FK1` FOREIGN KEY (`ORIG_TBL_ID`) REFERENCES `TBLS` (`TBL_ID`), CONSTRAINT `IDXS_FK2` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`), CONSTRAINT `IDXS_FK3` FOREIGN KEY (`INDEX_TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `INDEX_PARAMS` ( `INDEX_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`INDEX_ID`,`PARAM_KEY`), KEY `INDEX_PARAMS_N49` (`INDEX_ID`), CONSTRAINT `INDEX_PARAMS_FK1` FOREIGN KEY (`INDEX_ID`) REFERENCES `IDXS` (`INDEX_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `NUCLEUS_TABLES` ( `CLASS_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TABLE_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TYPE` varchar(4) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `OWNER` varchar(2) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `VERSION` varchar(20) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `INTERFACE_NAME` varchar(255) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`CLASS_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PARTITIONS` ( `PART_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `LAST_ACCESS_TIME` int(11) NOT NULL, `PART_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SD_ID` bigint(20) DEFAULT NULL, `TBL_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`PART_ID`), UNIQUE KEY `UNIQUEPARTITION` (`PART_NAME`,`TBL_ID`), KEY `PARTITIONS_N49` (`TBL_ID`), KEY `PARTITIONS_N50` (`SD_ID`), CONSTRAINT `PARTITIONS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`), CONSTRAINT `PARTITIONS_FK2` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_EVENTS` ( `PART_NAME_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `EVENT_TIME` bigint(20) NOT NULL, `EVENT_TYPE` int(11) NOT NULL, `PARTITION_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_NAME_ID`), KEY `PARTITIONEVENTINDEX` (`PARTITION_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_KEYS` ( `TBL_ID` bigint(20) NOT NULL, `PKEY_COMMENT` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PKEY_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PKEY_TYPE` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`TBL_ID`,`PKEY_NAME`), KEY `PARTITION_KEYS_N49` (`TBL_ID`), CONSTRAINT `PARTITION_KEYS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_KEY_VALS` ( `PART_ID` bigint(20) NOT NULL, `PART_KEY_VAL` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`PART_ID`,`INTEGER_IDX`), KEY `PARTITION_KEY_VALS_N49` (`PART_ID`), CONSTRAINT `PARTITION_KEY_VALS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_PARAMS` ( `PART_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_ID`,`PARAM_KEY`), KEY `PARTITION_PARAMS_N49` (`PART_ID`), CONSTRAINT `PARTITION_PARAMS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PART_COL_PRIVS` ( `PART_COLUMN_GRANT_ID` bigint(20) NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_ID` bigint(20) DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_COL_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_COLUMN_GRANT_ID`), KEY `PART_COL_PRIVS_N49` (`PART_ID`), KEY `PARTITIONCOLUMNPRIVILEGEINDEX` (`AUTHORIZER`,`PART_ID`,`COLUMN_NAME`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`PART_COL_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), CONSTRAINT `PART_COL_PRIVS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PART_PRIVS` ( `PART_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_ID` bigint(20) DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_GRANT_ID`), KEY `PARTPRIVILEGEINDEX` (`AUTHORIZER`,`PART_ID`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`PART_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), KEY `PART_PRIVS_N49` (`PART_ID`), CONSTRAINT `PART_PRIVS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `ROLES` ( `ROLE_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `OWNER_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `ROLE_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`ROLE_ID`), UNIQUE KEY `ROLEENTITYINDEX` (`ROLE_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `ROLE_MAP` ( `ROLE_GRANT_ID` bigint(20) NOT NULL, `ADD_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `ROLE_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`ROLE_GRANT_ID`), UNIQUE KEY `USERROLEMAPINDEX` (`PRINCIPAL_NAME`,`ROLE_ID`,`GRANTOR`,`GRANTOR_TYPE`), KEY `ROLE_MAP_N49` (`ROLE_ID`), CONSTRAINT `ROLE_MAP_FK1` FOREIGN KEY (`ROLE_ID`) REFERENCES `ROLES` (`ROLE_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SDS` ( `SD_ID` bigint(20) NOT NULL, `CD_ID` bigint(20) DEFAULT NULL, `INPUT_FORMAT` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `IS_COMPRESSED` bit(1) NOT NULL, `IS_STOREDASSUBDIRECTORIES` bit(1) NOT NULL, `LOCATION` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `NUM_BUCKETS` int(11) NOT NULL, `OUTPUT_FORMAT` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SERDE_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`SD_ID`), KEY `SDS_N49` (`SERDE_ID`), KEY `SDS_N50` (`CD_ID`), CONSTRAINT `SDS_FK1` FOREIGN KEY (`SERDE_ID`) REFERENCES `SERDES` (`SERDE_ID`), CONSTRAINT `SDS_FK2` FOREIGN KEY (`CD_ID`) REFERENCES `CDS` (`CD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.007 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SD_PARAMS` ( `SD_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` MEDIUMTEXT CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`SD_ID`,`PARAM_KEY`), KEY `SD_PARAMS_N49` (`SD_ID`), CONSTRAINT `SD_PARAMS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SEQUENCE_TABLE` ( `SEQUENCE_NAME` varchar(255) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `NEXT_VAL` bigint(20) NOT NULL, PRIMARY KEY (`SEQUENCE_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO `SEQUENCE_TABLE` (`SEQUENCE_NAME`, `NEXT_VAL`) VALUES ('org.apache.hadoop.hive.metastore.model.MNotificationLog', 1) 
1 row affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SERDES` ( `SERDE_ID` bigint(20) NOT NULL, `NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SLIB` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DESCRIPTION` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SERIALIZER_CLASS` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DESERIALIZER_CLASS` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SERDE_TYPE` integer, PRIMARY KEY (`SERDE_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SERDE_PARAMS` ( `SERDE_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` MEDIUMTEXT CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`SERDE_ID`,`PARAM_KEY`), KEY `SERDE_PARAMS_N49` (`SERDE_ID`), CONSTRAINT `SERDE_PARAMS_FK1` FOREIGN KEY (`SERDE_ID`) REFERENCES `SERDES` (`SERDE_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_COL_NAMES` ( `SD_ID` bigint(20) NOT NULL, `SKEWED_COL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID`,`INTEGER_IDX`), KEY `SKEWED_COL_NAMES_N49` (`SD_ID`), CONSTRAINT `SKEWED_COL_NAMES_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_COL_VALUE_LOC_MAP` ( `SD_ID` bigint(20) NOT NULL, `STRING_LIST_ID_KID` bigint(20) NOT NULL, `LOCATION` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`SD_ID`,`STRING_LIST_ID_KID`), KEY `SKEWED_COL_VALUE_LOC_MAP_N49` (`STRING_LIST_ID_KID`), KEY `SKEWED_COL_VALUE_LOC_MAP_N50` (`SD_ID`), CONSTRAINT `SKEWED_COL_VALUE_LOC_MAP_FK2` FOREIGN KEY (`STRING_LIST_ID_KID`) REFERENCES `SKEWED_STRING_LIST` (`STRING_LIST_ID`), CONSTRAINT `SKEWED_COL_VALUE_LOC_MAP_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_STRING_LIST` ( `STRING_LIST_ID` bigint(20) NOT NULL, PRIMARY KEY (`STRING_LIST_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_STRING_LIST_VALUES` ( `STRING_LIST_ID` bigint(20) NOT NULL, `STRING_LIST_VALUE` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`STRING_LIST_ID`,`INTEGER_IDX`), KEY `SKEWED_STRING_LIST_VALUES_N49` (`STRING_LIST_ID`), CONSTRAINT `SKEWED_STRING_LIST_VALUES_FK1` FOREIGN KEY (`STRING_LIST_ID`) REFERENCES `SKEWED_STRING_LIST` (`STRING_LIST_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_VALUES` ( `SD_ID_OID` bigint(20) NOT NULL, `STRING_LIST_ID_EID` bigint(20) NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID_OID`,`INTEGER_IDX`), KEY `SKEWED_VALUES_N50` (`SD_ID_OID`), KEY `SKEWED_VALUES_N49` (`STRING_LIST_ID_EID`), CONSTRAINT `SKEWED_VALUES_FK2` FOREIGN KEY (`STRING_LIST_ID_EID`) REFERENCES `SKEWED_STRING_LIST` (`STRING_LIST_ID`), CONSTRAINT `SKEWED_VALUES_FK1` FOREIGN KEY (`SD_ID_OID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `SORT_COLS` ( `SD_ID` bigint(20) NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `ORDER` int(11) NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID`,`INTEGER_IDX`), KEY `SORT_COLS_N49` (`SD_ID`), CONSTRAINT `SORT_COLS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TABLE_PARAMS` ( `TBL_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` MEDIUMTEXT CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TBL_ID`,`PARAM_KEY`), KEY `TABLE_PARAMS_N49` (`TBL_ID`), CONSTRAINT `TABLE_PARAMS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `MV_CREATION_METADATA` ( `MV_CREATION_METADATA_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TBL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TXN_LIST` TEXT DEFAULT NULL, `MATERIALIZATION_TIME` bigint(20) NOT NULL, PRIMARY KEY (`MV_CREATION_METADATA_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX MV_UNIQUE_TABLE ON MV_CREATION_METADATA (TBL_NAME, DB_NAME) USING BTREE
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TBLS` ( `TBL_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `DB_ID` bigint(20) DEFAULT NULL, `LAST_ACCESS_TIME` int(11) NOT NULL, `OWNER` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `OWNER_TYPE` varchar(10) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `RETENTION` int(11) NOT NULL, `SD_ID` bigint(20) DEFAULT NULL, `TBL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `VIEW_EXPANDED_TEXT` mediumtext, `VIEW_ORIGINAL_TEXT` mediumtext, `IS_REWRITE_ENABLED` bit(1) NOT NULL DEFAULT 0, PRIMARY KEY (`TBL_ID`), UNIQUE KEY `UNIQUETABLE` (`TBL_NAME`,`DB_ID`), KEY `TBLS_N50` (`SD_ID`), KEY `TBLS_N49` (`DB_ID`), CONSTRAINT `TBLS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`), CONSTRAINT `TBLS_FK2` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `MV_TABLES_USED` ( `MV_CREATION_METADATA_ID` bigint(20) NOT NULL, `TBL_ID` bigint(20) NOT NULL, CONSTRAINT `MV_TABLES_USED_FK1` FOREIGN KEY (`MV_CREATION_METADATA_ID`) REFERENCES `MV_CREATION_METADATA` (`MV_CREATION_METADATA_ID`), CONSTRAINT `MV_TABLES_USED_FK2` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TBL_COL_PRIVS` ( `TBL_COLUMN_GRANT_ID` bigint(20) NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_COL_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_ID` bigint(20) DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TBL_COLUMN_GRANT_ID`), KEY `TABLECOLUMNPRIVILEGEINDEX` (`AUTHORIZER`,`TBL_ID`,`COLUMN_NAME`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`TBL_COL_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), KEY `TBL_COL_PRIVS_N49` (`TBL_ID`), CONSTRAINT `TBL_COL_PRIVS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TBL_PRIVS` ( `TBL_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_ID` bigint(20) DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TBL_GRANT_ID`), KEY `TBL_PRIVS_N49` (`TBL_ID`), KEY `TABLEPRIVILEGEINDEX` (`AUTHORIZER`,`TBL_ID`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`TBL_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), CONSTRAINT `TBL_PRIVS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.007 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TAB_COL_STATS` ( `CS_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TABLE_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TBL_ID` bigint(20) NOT NULL, `LONG_LOW_VALUE` bigint(20), `LONG_HIGH_VALUE` bigint(20), `DOUBLE_HIGH_VALUE` double(53,4), `DOUBLE_LOW_VALUE` double(53,4), `BIG_DECIMAL_LOW_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `BIG_DECIMAL_HIGH_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `NUM_NULLS` bigint(20) NOT NULL, `NUM_DISTINCTS` bigint(20), `BIT_VECTOR` blob, `AVG_COL_LEN` double(53,4), `MAX_COL_LEN` bigint(20), `NUM_TRUES` bigint(20), `NUM_FALSES` bigint(20), `LAST_ANALYZED` bigint(20) NOT NULL, PRIMARY KEY (`CS_ID`), CONSTRAINT `TAB_COL_STATS_FK` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.12 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX TAB_COL_STATS_IDX ON TAB_COL_STATS (CAT_NAME, DB_NAME, TABLE_NAME, COLUMN_NAME) USING BTREE
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `PART_COL_STATS` ( `CS_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TABLE_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARTITION_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PART_ID` bigint(20) NOT NULL, `LONG_LOW_VALUE` bigint(20), `LONG_HIGH_VALUE` bigint(20), `DOUBLE_HIGH_VALUE` double(53,4), `DOUBLE_LOW_VALUE` double(53,4), `BIG_DECIMAL_LOW_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `BIG_DECIMAL_HIGH_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `NUM_NULLS` bigint(20) NOT NULL, `NUM_DISTINCTS` bigint(20), `BIT_VECTOR` blob, `AVG_COL_LEN` double(53,4), `MAX_COL_LEN` bigint(20), `NUM_TRUES` bigint(20), `NUM_FALSES` bigint(20), `LAST_ANALYZED` bigint(20) NOT NULL, PRIMARY KEY (`CS_ID`), CONSTRAINT `PART_COL_STATS_FK` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX PCS_STATS_IDX ON PART_COL_STATS (CAT_NAME, DB_NAME,TABLE_NAME,COLUMN_NAME,PARTITION_NAME) USING BTREE
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TYPES` ( `TYPES_ID` bigint(20) NOT NULL, `TYPE_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TYPE1` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TYPE2` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TYPES_ID`), UNIQUE KEY `UNIQUE_TYPE` (`TYPE_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `TYPE_FIELDS` ( `TYPE_NAME` bigint(20) NOT NULL, `COMMENT` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `FIELD_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `FIELD_TYPE` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`TYPE_NAME`,`FIELD_NAME`), KEY `TYPE_FIELDS_N49` (`TYPE_NAME`), CONSTRAINT `TYPE_FIELDS_FK1` FOREIGN KEY (`TYPE_NAME`) REFERENCES `TYPES` (`TYPES_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `MASTER_KEYS` ( `KEY_ID` INTEGER NOT NULL AUTO_INCREMENT, `MASTER_KEY` VARCHAR(767) BINARY NULL, PRIMARY KEY (`KEY_ID`) ) ENGINE=INNODB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `DELEGATION_TOKENS` ( `TOKEN_IDENT` VARCHAR(767) BINARY NOT NULL, `TOKEN` VARCHAR(767) BINARY NULL, PRIMARY KEY (`TOKEN_IDENT`) ) ENGINE=INNODB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `VERSION` ( `VER_ID` BIGINT NOT NULL, `SCHEMA_VERSION` VARCHAR(127) NOT NULL, `VERSION_COMMENT` VARCHAR(255), PRIMARY KEY (`VER_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `FUNCS` ( `FUNC_ID` BIGINT(20) NOT NULL, `CLASS_NAME` VARCHAR(4000) CHARACTER SET latin1 COLLATE latin1_bin, `CREATE_TIME` INT(11) NOT NULL, `DB_ID` BIGINT(20), `FUNC_NAME` VARCHAR(128) CHARACTER SET latin1 COLLATE latin1_bin, `FUNC_TYPE` INT(11) NOT NULL, `OWNER_NAME` VARCHAR(128) CHARACTER SET latin1 COLLATE latin1_bin, `OWNER_TYPE` VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_bin, PRIMARY KEY (`FUNC_ID`), UNIQUE KEY `UNIQUEFUNCTION` (`FUNC_NAME`, `DB_ID`), KEY `FUNCS_N49` (`DB_ID`), CONSTRAINT `FUNCS_FK1` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `FUNC_RU` ( `FUNC_ID` BIGINT(20) NOT NULL, `RESOURCE_TYPE` INT(11) NOT NULL, `RESOURCE_URI` VARCHAR(4000) CHARACTER SET latin1 COLLATE latin1_bin, `INTEGER_IDX` INT(11) NOT NULL, PRIMARY KEY (`FUNC_ID`, `INTEGER_IDX`), CONSTRAINT `FUNC_RU_FK1` FOREIGN KEY (`FUNC_ID`) REFERENCES `FUNCS` (`FUNC_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `NOTIFICATION_LOG` ( `NL_ID` BIGINT(20) NOT NULL, `EVENT_ID` BIGINT(20) NOT NULL, `EVENT_TIME` INT(11) NOT NULL, `EVENT_TYPE` varchar(32) NOT NULL, `CAT_NAME` varchar(256), `DB_NAME` varchar(128), `TBL_NAME` varchar(256), `MESSAGE` longtext, `MESSAGE_FORMAT` varchar(16), PRIMARY KEY (`NL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `NOTIFICATION_SEQUENCE` ( `NNI_ID` BIGINT(20) NOT NULL, `NEXT_EVENT_ID` BIGINT(20) NOT NULL, PRIMARY KEY (`NNI_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO `NOTIFICATION_SEQUENCE` (`NNI_ID`, `NEXT_EVENT_ID`) SELECT * from (select 1 as `NNI_ID`, 1 as `NOTIFICATION_SEQUENCE`) a WHERE (SELECT COUNT(*) FROM `NOTIFICATION_SEQUENCE`) = 0
1 row affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `KEY_CONSTRAINTS` ( `CHILD_CD_ID` BIGINT, `CHILD_INTEGER_IDX` INT(11), `CHILD_TBL_ID` BIGINT, `PARENT_CD_ID` BIGINT, `PARENT_INTEGER_IDX` INT(11) NOT NULL, `PARENT_TBL_ID` BIGINT NOT NULL, `POSITION` BIGINT NOT NULL, `CONSTRAINT_NAME` VARCHAR(400) NOT NULL, `CONSTRAINT_TYPE` SMALLINT(6)  NOT NULL, `UPDATE_RULE` SMALLINT(6), `DELETE_RULE` SMALLINT(6), `ENABLE_VALIDATE_RELY` SMALLINT(6) NOT NULL, `DEFAULT_VALUE` VARCHAR(400), PRIMARY KEY (`CONSTRAINT_NAME`, `POSITION`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX `CONSTRAINTS_PARENT_TABLE_ID_INDEX` ON KEY_CONSTRAINTS (`PARENT_TBL_ID`) USING BTREE
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX `CONSTRAINTS_CONSTRAINT_TYPE_INDEX` ON KEY_CONSTRAINTS (`CONSTRAINT_TYPE`) USING BTREE
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS `METASTORE_DB_PROPERTIES` ( `PROPERTY_KEY` varchar(255) NOT NULL, `PROPERTY_VALUE` varchar(1000) NOT NULL, `DESCRIPTION` varchar(1000), PRIMARY KEY(`PROPERTY_KEY`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS WM_RESOURCEPLAN ( `RP_ID` bigint(20) NOT NULL, `NAME` varchar(128) NOT NULL, `QUERY_PARALLELISM` int(11), `STATUS` varchar(20) NOT NULL, `DEFAULT_POOL_ID` bigint(20), PRIMARY KEY (`RP_ID`), UNIQUE KEY `UNIQUE_WM_RESOURCEPLAN` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS WM_POOL ( `POOL_ID` bigint(20) NOT NULL, `RP_ID` bigint(20) NOT NULL, `PATH` varchar(767) NOT NULL, `ALLOC_FRACTION` DOUBLE, `QUERY_PARALLELISM` int(11), `SCHEDULING_POLICY` varchar(767), PRIMARY KEY (`POOL_ID`), UNIQUE KEY `UNIQUE_WM_POOL` (`RP_ID`, `PATH`), CONSTRAINT `WM_POOL_FK1` FOREIGN KEY (`RP_ID`) REFERENCES `WM_RESOURCEPLAN` (`RP_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> ALTER TABLE `WM_RESOURCEPLAN` ADD CONSTRAINT `WM_RESOURCEPLAN_FK1` FOREIGN KEY (`DEFAULT_POOL_ID`) REFERENCES `WM_POOL`(`POOL_ID`)
No rows affected (0.008 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS WM_TRIGGER ( `TRIGGER_ID` bigint(20) NOT NULL, `RP_ID` bigint(20) NOT NULL, `NAME` varchar(128) NOT NULL, `TRIGGER_EXPRESSION` varchar(1024), `ACTION_EXPRESSION` varchar(1024), `IS_IN_UNMANAGED` bit(1) NOT NULL DEFAULT 0, PRIMARY KEY (`TRIGGER_ID`), UNIQUE KEY `UNIQUE_WM_TRIGGER` (`RP_ID`, `NAME`), CONSTRAINT `WM_TRIGGER_FK1` FOREIGN KEY (`RP_ID`) REFERENCES `WM_RESOURCEPLAN` (`RP_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS WM_POOL_TO_TRIGGER ( `POOL_ID` bigint(20) NOT NULL, `TRIGGER_ID` bigint(20) NOT NULL, PRIMARY KEY (`POOL_ID`, `TRIGGER_ID`), CONSTRAINT `WM_POOL_TO_TRIGGER_FK1` FOREIGN KEY (`POOL_ID`) REFERENCES `WM_POOL` (`POOL_ID`), CONSTRAINT `WM_POOL_TO_TRIGGER_FK2` FOREIGN KEY (`TRIGGER_ID`) REFERENCES `WM_TRIGGER` (`TRIGGER_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE IF NOT EXISTS WM_MAPPING ( `MAPPING_ID` bigint(20) NOT NULL, `RP_ID` bigint(20) NOT NULL, `ENTITY_TYPE` varchar(128) NOT NULL, `ENTITY_NAME` varchar(128) NOT NULL, `POOL_ID` bigint(20), `ORDERING` int, PRIMARY KEY (`MAPPING_ID`), UNIQUE KEY `UNIQUE_WM_MAPPING` (`RP_ID`, `ENTITY_TYPE`, `ENTITY_NAME`), CONSTRAINT `WM_MAPPING_FK1` FOREIGN KEY (`RP_ID`) REFERENCES `WM_RESOURCEPLAN` (`RP_ID`), CONSTRAINT `WM_MAPPING_FK2` FOREIGN KEY (`POOL_ID`) REFERENCES `WM_POOL` (`POOL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE TXNS ( TXN_ID bigint PRIMARY KEY, TXN_STATE char(1) NOT NULL, TXN_STARTED bigint NOT NULL, TXN_LAST_HEARTBEAT bigint NOT NULL, TXN_USER varchar(128) NOT NULL, TXN_HOST varchar(128) NOT NULL, TXN_AGENT_INFO varchar(128), TXN_META_INFO varchar(128), TXN_HEARTBEAT_COUNT int, TXN_TYPE int ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE TXN_COMPONENTS ( TC_TXNID bigint NOT NULL, TC_DATABASE varchar(128) NOT NULL, TC_TABLE varchar(128), TC_PARTITION varchar(767), TC_OPERATION_TYPE char(1) NOT NULL, TC_WRITEID bigint, FOREIGN KEY (TC_TXNID) REFERENCES TXNS (TXN_ID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX TC_TXNID_INDEX ON TXN_COMPONENTS (TC_TXNID)
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE COMPLETED_TXN_COMPONENTS ( CTC_TXNID bigint NOT NULL, CTC_DATABASE varchar(128) NOT NULL, CTC_TABLE varchar(256), CTC_PARTITION varchar(767), CTC_TIMESTAMP timestamp DEFAULT CURRENT_TIMESTAMP NOT NULL, CTC_WRITEID bigint, CTC_UPDATE_DELETE char(1) NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX COMPLETED_TXN_COMPONENTS_IDX ON COMPLETED_TXN_COMPONENTS (CTC_DATABASE, CTC_TABLE, CTC_PARTITION) USING BTREE
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE NEXT_TXN_ID ( NTXN_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO NEXT_TXN_ID VALUES(1)
1 row affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE HIVE_LOCKS ( HL_LOCK_EXT_ID bigint NOT NULL, HL_LOCK_INT_ID bigint NOT NULL, HL_TXNID bigint NOT NULL, HL_DB varchar(128) NOT NULL, HL_TABLE varchar(128), HL_PARTITION varchar(767), HL_LOCK_STATE char(1) not null, HL_LOCK_TYPE char(1) not null, HL_LAST_HEARTBEAT bigint NOT NULL, HL_ACQUIRED_AT bigint, HL_USER varchar(128) NOT NULL, HL_HOST varchar(128) NOT NULL, HL_HEARTBEAT_COUNT int, HL_AGENT_INFO varchar(128), HL_BLOCKEDBY_EXT_ID bigint, HL_BLOCKEDBY_INT_ID bigint, PRIMARY KEY(HL_LOCK_EXT_ID, HL_LOCK_INT_ID), KEY HIVE_LOCK_TXNID_INDEX (HL_TXNID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX HL_TXNID_IDX ON HIVE_LOCKS (HL_TXNID)
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE NEXT_LOCK_ID ( NL_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO NEXT_LOCK_ID VALUES(1)
1 row affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE COMPACTION_QUEUE ( CQ_ID bigint PRIMARY KEY, CQ_DATABASE varchar(128) NOT NULL, CQ_TABLE varchar(128) NOT NULL, CQ_PARTITION varchar(767), CQ_STATE char(1) NOT NULL, CQ_TYPE char(1) NOT NULL, CQ_TBLPROPERTIES varchar(2048), CQ_WORKER_ID varchar(128), CQ_START bigint, CQ_RUN_AS varchar(128), CQ_HIGHEST_WRITE_ID bigint, CQ_META_INFO varbinary(2048), CQ_HADOOP_JOB_ID varchar(32) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE COMPLETED_COMPACTIONS ( CC_ID bigint PRIMARY KEY, CC_DATABASE varchar(128) NOT NULL, CC_TABLE varchar(128) NOT NULL, CC_PARTITION varchar(767), CC_STATE char(1) NOT NULL, CC_TYPE char(1) NOT NULL, CC_TBLPROPERTIES varchar(2048), CC_WORKER_ID varchar(128), CC_START bigint, CC_END bigint, CC_RUN_AS varchar(128), CC_HIGHEST_WRITE_ID bigint, CC_META_INFO varbinary(2048), CC_HADOOP_JOB_ID varchar(32) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE NEXT_COMPACTION_QUEUE_ID ( NCQ_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO NEXT_COMPACTION_QUEUE_ID VALUES(1)
1 row affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE AUX_TABLE ( MT_KEY1 varchar(128) NOT NULL, MT_KEY2 bigint NOT NULL, MT_COMMENT varchar(255), PRIMARY KEY(MT_KEY1, MT_KEY2) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE WRITE_SET ( WS_DATABASE varchar(128) NOT NULL, WS_TABLE varchar(128) NOT NULL, WS_PARTITION varchar(767), WS_TXNID bigint NOT NULL, WS_COMMIT_ID bigint NOT NULL, WS_OPERATION_TYPE char(1) NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE TXN_TO_WRITE_ID ( T2W_TXNID bigint NOT NULL, T2W_DATABASE varchar(128) NOT NULL, T2W_TABLE varchar(256) NOT NULL, T2W_WRITEID bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE UNIQUE INDEX TBL_TO_TXN_ID_IDX ON TXN_TO_WRITE_ID (T2W_DATABASE, T2W_TABLE, T2W_TXNID)
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE UNIQUE INDEX TBL_TO_WRITE_ID_IDX ON TXN_TO_WRITE_ID (T2W_DATABASE, T2W_TABLE, T2W_WRITEID)
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE NEXT_WRITE_ID ( NWI_DATABASE varchar(128) NOT NULL, NWI_TABLE varchar(256) NOT NULL, NWI_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE UNIQUE INDEX NEXT_WRITE_ID_IDX ON NEXT_WRITE_ID (NWI_DATABASE, NWI_TABLE)
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE MIN_HISTORY_LEVEL ( MHL_TXNID bigint NOT NULL, MHL_MIN_OPEN_TXNID bigint NOT NULL, PRIMARY KEY(MHL_TXNID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX MIN_HISTORY_LEVEL_IDX ON MIN_HISTORY_LEVEL (MHL_MIN_OPEN_TXNID)
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE MATERIALIZATION_REBUILD_LOCKS ( MRL_TXN_ID bigint NOT NULL, MRL_DB_NAME VARCHAR(128) NOT NULL, MRL_TBL_NAME VARCHAR(256) NOT NULL, MRL_LAST_HEARTBEAT bigint NOT NULL, PRIMARY KEY(MRL_TXN_ID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE `I_SCHEMA` ( `SCHEMA_ID` BIGINT PRIMARY KEY, `SCHEMA_TYPE` INTEGER NOT NULL, `NAME` VARCHAR(256), `DB_ID` BIGINT, `COMPATIBILITY` INTEGER NOT NULL, `VALIDATION_LEVEL` INTEGER NOT NULL, `CAN_EVOLVE` bit(1) NOT NULL, `SCHEMA_GROUP` VARCHAR(256), `DESCRIPTION` VARCHAR(4000), FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`), KEY `UNIQUE_NAME` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE `SCHEMA_VERSION` ( `SCHEMA_VERSION_ID` bigint primary key, `SCHEMA_ID` BIGINT, `VERSION` INTEGER NOT NULL, `CREATED_AT` BIGINT NOT NULL, `CD_ID` BIGINT, `STATE` INTEGER NOT NULL, `DESCRIPTION` VARCHAR(4000), `SCHEMA_TEXT` mediumtext, `FINGERPRINT` VARCHAR(256), `SCHEMA_VERSION_NAME` VARCHAR(256), `SERDE_ID` bigint, FOREIGN KEY (`SCHEMA_ID`) REFERENCES `I_SCHEMA` (`SCHEMA_ID`), FOREIGN KEY (`CD_ID`) REFERENCES `CDS` (`CD_ID`), FOREIGN KEY (`SERDE_ID`) REFERENCES `SERDES` (`SERDE_ID`), KEY `UNIQUE_VERSION` (`SCHEMA_ID`, `VERSION`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE REPL_TXN_MAP ( RTM_REPL_POLICY varchar(256) NOT NULL, RTM_SRC_TXN_ID bigint NOT NULL, RTM_TARGET_TXN_ID bigint NOT NULL, PRIMARY KEY (RTM_REPL_POLICY, RTM_SRC_TXN_ID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE TABLE RUNTIME_STATS ( RS_ID bigint primary key, CREATE_TIME bigint NOT NULL, WEIGHT bigint NOT NULL, PAYLOAD blob ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> CREATE INDEX IDX_RUNTIME_STATS_CREATE_TIME ON RUNTIME_STATS(CREATE_TIME)
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3306/metastore> INSERT INTO VERSION (VER_ID, SCHEMA_VERSION, VERSION_COMMENT) VALUES (1, '3.1.0', 'Hive release version 3.1.0')
1 row affected (0.002 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET SQL_MODE=@OLD_SQL_MODE */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3306/metastore> /*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3306/metastore> !closeall
Closing: 0: jdbc:mysql://node1:3306/metastore?useSSL=false
beeline> 
beeline> Initialization script completed
schemaTool completed
```

测试

```bash
[root@node1 hive-3.1.3]# hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = ea447e5c-cae7-4a51-9de2-fe5a8543f0ef

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 8093d5fb-3200-4813-a192-5be54e820138
hive> show databases;
OK
default
Time taken: 0.388 seconds, Fetched: 1 row(s)
hive> show tables;
OK
Time taken: 0.03 seconds
hive> create table stu(id int, name string);
OK
Time taken: 0.39 seconds
hive> insert into stu values(1, "ss");
Query ID = root_20230520121113_9a1ef08f-82c7-4962-98da-62112f6b3a02
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1684546520162_0002, Tracking URL = http://node2:8088/proxy/application_1684546520162_0002/
Kill Command = /opt/module/hadoop-3.3.0/bin/mapred job  -kill job_1684546520162_0002
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2023-05-20 12:11:23,085 Stage-1 map = 0%,  reduce = 0%
2023-05-20 12:11:29,294 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.77 sec
2023-05-20 12:11:35,452 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.36 sec
MapReduce Total cumulative CPU time: 3 seconds 360 msec
Ended Job = job_1684546520162_0002
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to directory hdfs://node1:8020/user/hive/warehouse/stu/.hive-staging_hive_2023-05-20_12-11-13_634_1719468325203533814-1/-ext-10000
Loading data to table default.stu
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.36 sec   HDFS Read: 15116 HDFS Write: 235 SUCCESS
Total MapReduce CPU Time Spent: 3 seconds 360 msec
OK
Time taken: 24.346 seconds
hive> select * from stu;
OK
1	ss
1	ss
Time taken: 0.138 seconds, Fetched: 2 row(s)
hive> exit;
```

## hiveserver2配置

hadoop core-site.xml

```xml
<!-- 配置所有节点的root用户都可作为代理用户 -->
<property>
	<name>hadoop.proxyuser.root.hosts</name>
    <value>*</value>
</property>
<!-- 配置root用户能够代理的用户组为任意组 -->
<property>
	<name>hadoop.proxyuser.root.groups</name>
    <value>*</value>
</property>
<!-- 配置root用户能够代理的用户为任意用户 -->
<property>
	<name>hadoop.proxyuser.root.users</name>
    <value>*</value>
</property>

```

hive.site.xml

```xml
<!-- 指定hiveserver2连接的host -->
<property>
	<name>hive.server2.thrift.bind.host</name>
    <value>node1</value>
</property>
<!-- 指定hiveserver2连接的端口号 -->
<property>
	<name>hive.server2.thrift.port</name>
    <value>10000</value>
</property>
```

测试

启动hiveserver2

```bash
[root@node1 hive-3.1.3]# nohup bin/hiveserver2 >/dev/null 2>&1 &
[1] 12600
[root@node1 hive-3.1.3]# jps
11824 NameNode
12065 NodeManager
12770 Jps
12600 RunJar
11945 DataNode
[root@node1 hive-3.1.3]# 

```

beeline

```bash
[root@node1 hive-3.1.3]# bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
beeline> !
!quit                  !done                  !exit                  
!connect               !open                  !describe              
!indexes               !primarykeys           !exportedkeys          
!manual                !importedkeys          !procedures            
!tables                !typeinfo              !columns               
!reconnect             !dropall               !history               
!metadata              !nativesql             !dbinfo                
!rehash                !verbose               !run                   
!batch                 !list                  !all                   
!go                    !#                     !script                
!record                !brief                 !close                 
!closeall              !isolation             !outputformat          
!autocommit            !commit                !properties            
!rollback              !help                  !?                     
!set                   !save                  !scan                  
!sql                   !sh                    !call                  
!nullemptystring       !addlocaldriverjar     !addlocaldrivername    
!delimiter             
beeline> !connect jdbc:hive2://node1:10000
Connecting to jdbc:hive2://node1:10000
Enter username for jdbc:hive2://node1:10000: root
Enter password for jdbc:hive2://node1:10000: 
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1:10000> show tables;
+-----------+
| tab_name  |
+-----------+
| stu       |
+-----------+
1 row selected (1.003 seconds)
0: jdbc:hive2://node1:10000> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | ss        |
| 1       | ss        |
+---------+-----------+
2 rows selected (1.582 seconds)
0: jdbc:hive2://node1:10000> !quit
Closing: 0: jdbc:hive2://node1:10000

```

## metastore服务

node2 配置客户端

```bash
[root@node1 module]# scp -r /opt/module/hive-3.1.3/ node2:/opt/module/
```

hive-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- Hive 默认在 HDFS 的工作目录 -->
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
    </property>

    <!-- 指定hiveserver2连接的host -->
    <property>
        <name>hive.server2.thrift.bind.host</name>
        <value>node1</value>
    </property>
    <!-- 指定hiveserver2连接的端口号 -->
    <property>
        <name>hive.server2.thrift.port</name>
        <value>10000</value>

    </property>
    <!-- 指定metastore服务的地址 -->
    <property>
        <name>hive.metastore.uris</name>
        <value>thrift://node1:9083</value>
        </property>
</configuration>
```





node1 开启metastore服务

```bash
[root@node1 hive-3.1.3]# nohup hive --service metastore &
[1] 15384
[root@node1 hive-3.1.3]# nohup: 忽略输入并把输出追加到"nohup.out"

[root@node1 hive-3.1.3]# jps
11824 NameNode
12065 NodeManager
12600 RunJar
15384 RunJar
11945 DataNode
15534 Jps
[root@node1 conf]# jps -ml
11824 org.apache.hadoop.hdfs.server.namenode.NameNode
12065 org.apache.hadoop.yarn.server.nodemanager.NodeManager
15569 sun.tools.jps.Jps -ml
12600 org.apache.hadoop.util.RunJar /opt/module/hive-3.1.3/lib/hive-service-3.1.3.jar org.apache.hive.service.server.HiveServer2
15384 org.apache.hadoop.util.RunJar /opt/module/hive-3.1.3/lib/hive-metastore-3.1.3.jar org.apache.hadoop.hive.metastore.HiveMetaStore
11945 org.apache.hadoop.hdfs.server.datanode.DataNode
```

测试

```bash
[root@node2 bin]# vim ../conf/hive-site.xml 
[root@node2 bin]# ./hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = 41e17626-9f6a-4fb4-b780-8894f6927765

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = ac77ad95-723f-48ab-a73d-f8c70c7dc46f
hive> show tables;
OK
stu
Time taken: 0.589 seconds, Fetched: 1 row(s)
hive> select * from stu;
OK
1	ss
1	ss
Time taken: 1.301 seconds, Fetched: 2 row(s)
hive> exit;

```

# HadoopHA + Hive

解压

配置环境变量

## 最小化部署

```bash
# 初始化元数据库报错
hadoop@node1:/opt/module/hive-3.1.3$ bin/schematool -dbType derby -initSchema
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1380)
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1361)
	at org.apache.hadoop.mapred.JobConf.setJar(JobConf.java:536)
	at org.apache.hadoop.mapred.JobConf.setJarByClass(JobConf.java:554)
	at org.apache.hadoop.mapred.JobConf.<init>(JobConf.java:448)
	at org.apache.hadoop.hive.conf.HiveConf.initialize(HiveConf.java:5144)
	at org.apache.hadoop.hive.conf.HiveConf.<init>(HiveConf.java:5107)
	at org.apache.hive.beeline.HiveSchemaTool.<init>(HiveSchemaTool.java:96)
	at org.apache.hive.beeline.HiveSchemaTool.main(HiveSchemaTool.java:1473)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.hadoop.util.RunJar.run(RunJar.java:323)
	at org.apache.hadoop.util.RunJar.main(RunJar.java:236)
hadoop@node1:/opt/module/hive-3.1.3$ cd lib
# 版本不兼容
hadoop@node1:/opt/module/hive-3.1.3/lib$ mv guava-19.0.jar guava-19.0.jar_bak
# 统一换成高版本
hadoop@node1:/opt/module/hive-3.1.3/lib$ cp /opt/ha/hadoop-3.3.0/share/hadoop/common/lib/guava-27.0-jre.jar ./
# 重新初始化元数据库
hadoop@node1:/opt/module/hive-3.1.3$ bin/schematool -dbType derby -initSchema
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Metastore connection URL:	 jdbc:derby:;databaseName=metastore_db;create=true
Metastore Connection Driver :	 org.apache.derby.jdbc.EmbeddedDriver
Metastore connection User:	 APP
Starting metastore schema initialization to 3.1.0
Initialization script hive-schema-3.1.0.derby.sql
# 200+空行
Initialization script completed
schemaTool completed

# 大量INFO日志
hadoop@node1:/opt/module/hive-3.1.3$ bin/hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
2023-05-20 17:58:50,729 INFO  [main] conf.HiveConf: Found configuration file null
2023-05-20 17:58:51,725 WARN  [main] common.LogUtils: hive-site.xml not found on CLASSPATH
Hive Session ID = 2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:58:51,772 INFO  [main] SessionState: Hive Session ID = 2c8e93e4-9d70-4879-a8dc-68212e416ec4

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
2023-05-20 17:58:51,841 INFO  [main] SessionState: 
Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
2023-05-20 17:58:52,876 INFO  [main] retry.RetryInvocationHandler: fs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:1041)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine2$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine2.java:532)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1070)
	at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:1020)
	at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:948)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:422)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1845)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2952)
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over node2/192.168.88.112:8020 after 1 failover attempts. Trying to failover after sleeping for 945ms.
2023-05-20 17:58:53,908 INFO  [main] retry.RetryInvocationHandler: e.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:739)
	at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
	at org.apache.hadoop.ipc.ProtobufRpcEngine2$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine2.java:532)
	at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1070)
	at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:1020)
	at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:948)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:422)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1845)
	at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2952)
, while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over node2/192.168.88.112:8020 after 1 failover attempts. Trying to failover after sleeping for 687ms.
2023-05-20 17:58:54,647 INFO  [main] session.SessionState: Created HDFS directory: /tmp/hive/hadoop
2023-05-20 17:58:54,654 INFO  [main] session.SessionState: Created HDFS directory: /tmp/hive/hadoop/2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:58:54,676 INFO  [main] session.SessionState: Created local directory: /tmp/hadoop/2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:58:54,684 INFO  [main] session.SessionState: Created HDFS directory: /tmp/hive/hadoop/2c8e93e4-9d70-4879-a8dc-68212e416ec4/_tmp_space.db
2023-05-20 17:58:54,696 INFO  [main] conf.HiveConf: Using the default value passed in for log id: 2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:58:54,696 INFO  [main] session.SessionState: Updating thread name to 2c8e93e4-9d70-4879-a8dc-68212e416ec4 main
2023-05-20 17:58:55,455 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: 0: Opening raw store with implementation class:org.apache.hadoop.hive.metastore.ObjectStore
2023-05-20 17:58:55,484 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.ObjectStore: datanucleus.autoStartMechanismMode is set to unsupported value null . Setting it to value: ignored
2023-05-20 17:58:55,490 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.ObjectStore: ObjectStore, initialize called
2023-05-20 17:58:55,492 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.MetastoreConf: Unable to find config file hive-site.xml
2023-05-20 17:58:55,492 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.MetastoreConf: Found configuration file null
2023-05-20 17:58:55,492 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.MetastoreConf: Unable to find config file hivemetastore-site.xml
2023-05-20 17:58:55,492 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.MetastoreConf: Found configuration file null
2023-05-20 17:58:55,493 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.MetastoreConf: Unable to find config file metastore-site.xml
2023-05-20 17:58:55,493 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.MetastoreConf: Found configuration file null
2023-05-20 17:58:55,674 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.Persistence: Property datanucleus.cache.level2 unknown - will be ignored
2023-05-20 17:58:55,964 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] hikari.HikariDataSource: HikariPool-1 - Starting...
2023-05-20 17:58:55,969 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] util.DriverDataSource: Registered driver with driverClassName=org.apache.derby.jdbc.EmbeddedDriver was not found, trying direct instantiation.
2023-05-20 17:58:56,451 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] pool.PoolBase: HikariPool-1 - Driver does not support get/set network timeout for connections. (Feature not implemented: No details.)
2023-05-20 17:58:56,456 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] hikari.HikariDataSource: HikariPool-1 - Start completed.
2023-05-20 17:58:56,501 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] hikari.HikariDataSource: HikariPool-2 - Starting...
2023-05-20 17:58:56,501 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] util.DriverDataSource: Registered driver with driverClassName=org.apache.derby.jdbc.EmbeddedDriver was not found, trying direct instantiation.
2023-05-20 17:58:56,503 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] pool.PoolBase: HikariPool-2 - Driver does not support get/set network timeout for connections. (Feature not implemented: No details.)
2023-05-20 17:58:56,504 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] hikari.HikariDataSource: HikariPool-2 - Start completed.
2023-05-20 17:58:57,015 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.ObjectStore: Setting MetaStore object pin classes with hive.metastore.cache.pinobjtypes="Table,StorageDescriptor,SerDeInfo,Partition,Database,Type,FieldSchema,Order"
2023-05-20 17:58:57,101 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.MetaStoreDirectSql: Using direct SQL, underlying DB is DERBY
2023-05-20 17:58:57,103 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.ObjectStore: Initialized ObjectStore
2023-05-20 17:58:57,263 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:57,264 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:57,264 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:57,264 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:57,264 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:57,264 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:58,096 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:58,097 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:58,097 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:58,097 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:58,097 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:58,097 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] DataNucleus.MetaData: Metadata has jdbc-type of null yet this is not valid. Ignored
2023-05-20 17:58:59,272 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: Setting location of default catalog, as it hasn't been done after upgrade
2023-05-20 17:58:59,298 WARN  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.ObjectStore: Failed to get database hive.default, returning NoSuchObjectException
2023-05-20 17:58:59,370 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: Added admin role in metastore
2023-05-20 17:58:59,372 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: Added public role in metastore
2023-05-20 17:58:59,396 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: No user is added in admin role, since config is empty
2023-05-20 17:58:59,495 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.RetryingMetaStoreClient: RetryingMetaStoreClient proxy=class org.apache.hadoop.hive.ql.metadata.SessionHiveMetaStoreClient ugi=hadoop (auth:SIMPLE) retries=1 delay=1 lifetime=0
2023-05-20 17:58:59,509 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: 0: get_all_functions
2023-05-20 17:58:59,511 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=get_all_functions	
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
2023-05-20 17:58:59,536 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] CliDriver: Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = c0e44945-177a-45a4-9d4a-fd75b2d8e972
2023-05-20 17:58:59,544 INFO  [pool-10-thread-1] SessionState: Hive Session ID = c0e44945-177a-45a4-9d4a-fd75b2d8e972
2023-05-20 17:58:59,563 INFO  [pool-10-thread-1] session.SessionState: Created HDFS directory: /tmp/hive/hadoop/c0e44945-177a-45a4-9d4a-fd75b2d8e972
2023-05-20 17:58:59,566 INFO  [pool-10-thread-1] session.SessionState: Created local directory: /tmp/hadoop/c0e44945-177a-45a4-9d4a-fd75b2d8e972
2023-05-20 17:58:59,572 INFO  [pool-10-thread-1] session.SessionState: Created HDFS directory: /tmp/hive/hadoop/c0e44945-177a-45a4-9d4a-fd75b2d8e972/_tmp_space.db
2023-05-20 17:58:59,573 INFO  [pool-10-thread-1] metastore.HiveMetaStore: 1: get_databases: @hive#
2023-05-20 17:58:59,573 INFO  [pool-10-thread-1] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=get_databases: @hive#	
2023-05-20 17:58:59,574 INFO  [pool-10-thread-1] metastore.HiveMetaStore: 1: Opening raw store with implementation class:org.apache.hadoop.hive.metastore.ObjectStore
2023-05-20 17:58:59,574 INFO  [pool-10-thread-1] metastore.ObjectStore: ObjectStore, initialize called
2023-05-20 17:58:59,579 INFO  [pool-10-thread-1] metastore.MetaStoreDirectSql: Using direct SQL, underlying DB is DERBY
2023-05-20 17:58:59,579 INFO  [pool-10-thread-1] metastore.ObjectStore: Initialized ObjectStore
2023-05-20 17:58:59,605 INFO  [pool-10-thread-1] metastore.HiveMetaStore: 1: get_tables_by_type: db=@hive#default pat=.*,type=MATERIALIZED_VIEW
2023-05-20 17:58:59,608 INFO  [pool-10-thread-1] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=get_tables_by_type: db=@hive#default pat=.*,type=MATERIALIZED_VIEW	
2023-05-20 17:58:59,636 INFO  [pool-10-thread-1] metastore.HiveMetaStore: 1: get_multi_table : db=default tbls=
2023-05-20 17:58:59,636 INFO  [pool-10-thread-1] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=get_multi_table : db=default tbls=	
2023-05-20 17:58:59,638 INFO  [pool-10-thread-1] metadata.HiveMaterializedViewsRegistry: Materialized views registry has been initialized
hive> show databases;
2023-05-20 17:59:04,846 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.HiveConf: Using the default value passed in for log id: 2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:59:04,910 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Compiling command(queryId=hadoop_20230520175904_2311f3a8-6c53-486e-8be5-2f5f786c7770): show databases
2023-05-20 17:59:05,175 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Concurrency mode is disabled, not creating a lock manager
2023-05-20 17:59:05,188 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Semantic Analysis Completed (retrial = false)
2023-05-20 17:59:05,218 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:database_name, type:string, comment:from deserializer)], properties:null)
2023-05-20 17:59:05,275 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] exec.ListSinkOperator: Initializing operator LIST_SINK[0]
2023-05-20 17:59:05,281 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Completed compiling command(queryId=hadoop_20230520175904_2311f3a8-6c53-486e-8be5-2f5f786c7770); Time taken: 0.396 seconds
2023-05-20 17:59:05,281 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] reexec.ReExecDriver: Execution #1 of query
2023-05-20 17:59:05,281 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Concurrency mode is disabled, not creating a lock manager
2023-05-20 17:59:05,281 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Executing command(queryId=hadoop_20230520175904_2311f3a8-6c53-486e-8be5-2f5f786c7770): show databases
2023-05-20 17:59:05,288 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Starting task [Stage-0:DDL] in serial mode
2023-05-20 17:59:05,289 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: 0: get_databases: @hive#
2023-05-20 17:59:05,289 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=get_databases: @hive#	
2023-05-20 17:59:05,290 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] exec.DDLTask: results : 1
2023-05-20 17:59:05,296 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Completed executing command(queryId=hadoop_20230520175904_2311f3a8-6c53-486e-8be5-2f5f786c7770); Time taken: 0.014 seconds
OK
2023-05-20 17:59:05,296 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: OK
2023-05-20 17:59:05,296 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] ql.Driver: Concurrency mode is disabled, not creating a lock manager
2023-05-20 17:59:05,301 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] Configuration.deprecation: mapred.input.dir is deprecated. Instead, use mapreduce.input.fileinputformat.inputdir
2023-05-20 17:59:05,336 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] mapred.FileInputFormat: Total input files to process : 1
2023-05-20 17:59:05,358 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] exec.ListSinkOperator: RECORDS_OUT_INTERMEDIATE:0, RECORDS_OUT_OPERATOR_LIST_SINK_0:1, 
default
Time taken: 0.413 seconds, Fetched: 1 row(s)
2023-05-20 17:59:05,364 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] CliDriver: Time taken: 0.413 seconds, Fetched: 1 row(s)
2023-05-20 17:59:05,364 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] conf.HiveConf: Using the default value passed in for log id: 2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:59:05,364 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] session.SessionState: Resetting thread name to  main
hive> exit;
2023-05-20 17:59:10,088 INFO  [main] conf.HiveConf: Using the default value passed in for log id: 2c8e93e4-9d70-4879-a8dc-68212e416ec4
2023-05-20 17:59:10,088 INFO  [main] session.SessionState: Updating thread name to 2c8e93e4-9d70-4879-a8dc-68212e416ec4 main
2023-05-20 17:59:10,119 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] session.SessionState: Deleted directory: /tmp/hive/hadoop/2c8e93e4-9d70-4879-a8dc-68212e416ec4 on fs with scheme hdfs
2023-05-20 17:59:10,119 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] session.SessionState: Deleted directory: /tmp/hadoop/2c8e93e4-9d70-4879-a8dc-68212e416ec4 on fs with scheme file
2023-05-20 17:59:10,125 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: 0: Cleaning up thread local RawStore...
2023-05-20 17:59:10,125 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=Cleaning up thread local RawStore...	
2023-05-20 17:59:10,125 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] metastore.HiveMetaStore: 0: Done cleaning up thread local RawStore
2023-05-20 17:59:10,125 INFO  [2c8e93e4-9d70-4879-a8dc-68212e416ec4 main] HiveMetaStore.audit: ugi=hadoop	ip=unknown-ip-addr	cmd=Done cleaning up thread local RawStore	

# 解决INFO日志问题
hadoop@node1:/opt/module/hive-3.1.3$ vim conf/log4j.properties

log4j.rootLogger=ERROR, CA
log4j.appender.CA=org.apache.log4j.ConsoleAppender
log4j.appender.CA.layout=org.apache.log4j.PatternLayout
log4j.appender.CA.layout.ConversionPattern=%-4r [%t] %-5p %c %x - %m%n

# 重新测试无INFO日志
hadoop@node1:/opt/module/hive-3.1.3$ bin/hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = 8b00ab3e-3388-4069-b80a-c330a076c6b5

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 0f82f8e4-4c18-4def-9ed1-a69b783d58ec
# 查看数据库
hive> show databases;
OK
default
Time taken: 0.398 seconds, Fetched: 1 row(s)
# 创建表
hive> create table stu(id int, name string);
OK
Time taken: 0.679 seconds
# 插入数据
hive> insert into stu values(1, "aa");
Query ID = hadoop_20230520180807_b017c8fe-68cd-43e6-a52e-bc2f7e96a1ba
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1684630072455_0001, Tracking URL = http://node3:8088/proxy/application_1684630072455_0001/
Kill Command = /opt/ha/hadoop-3.3.0/bin/mapred job  -kill job_1684630072455_0001
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2023-05-20 18:09:25,469 Stage-1 map = 0%,  reduce = 0%
2023-05-20 18:09:34,741 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.66 sec
2023-05-20 18:09:48,087 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 3.31 sec
MapReduce Total cumulative CPU time: 3 seconds 310 msec
Ended Job = job_1684630072455_0001
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to directory hdfs://mycluster/user/hive/warehouse/stu/.hive-staging_hive_2023-05-20_18-08-07_110_364961201731766154-1/-ext-10000
Loading data to table default.stu
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 3.31 sec   HDFS Read: 15104 HDFS Write: 235 SUCCESS
Total MapReduce CPU Time Spent: 3 seconds 310 msec
OK
Time taken: 102.625 seconds
# 查找数据
hive> select * from stu;
OK
1	aa
Time taken: 1.61 seconds, Fetched: 1 row(s)
hive> exit;
```

## 使用mysql存储hive元数据

```bash
[root@node1 mysql]# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.28

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> update mysql.user set host="%" where user="root";
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select user, host from mysql.user;
+---------------+-----------+
| user          | host      |
+---------------+-----------+
| root          | %         |
| mysql.session | localhost |
| mysql.sys     | localhost |
+---------------+-----------+
3 rows in set (0.00 sec)

mysql> create database metastore;
Query OK, 1 row affected (0.00 sec)

mysql> exit;

# 添加mysql驱动jar包
hadoop@node1:/opt/module/hive-3.1.3$ cp ~/mysql-connector-java-8.0.30.jar ./lib
# 添加hive配置文件
hadoop@node1:/opt/module/hive-3.1.3$ vim conf/hive-site.xml

<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- jdbc 连接的 URL -->
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://node1:3308/metastore?useSSL=false</value>
    </property>
    <!-- jdbc 连接的 Driver-->
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.cj.jdbc.Driver</value>
    </property>
    <!-- jdbc 连接的 username-->
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>
    <!-- jdbc 连接的 password -->
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>1234</value>
    </property>
    <!-- Hive 默认在 HDFS 的工作目录 -->
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
    </property>
</configuration>

# 初始化元数据库为mysql
hadoop@node1:/opt/module/hive-3.1.3$ bin/schematool -dbType mysql -initSchema -verbose
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Metastore connection URL:	 jdbc:mysql://node1:3308/metastore?useSSL=false
Metastore Connection Driver :	 com.mysql.cj.jdbc.Driver
Metastore connection User:	 root
Starting metastore schema initialization to 3.1.0
Initialization script hive-schema-3.1.0.mysql.sql
Connecting to jdbc:mysql://node1:3308/metastore?useSSL=false
Connected to: MySQL (version 8.0.30)
Driver: MySQL Connector/J (version mysql-connector-java-8.0.30 (Revision: 1de2fe873fe26189564c030a343885011412976a))
Transaction isolation: TRANSACTION_READ_COMMITTED
0: jdbc:mysql://node1:3308/metastore> !autocommit on
Autocommit status: true
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET NAMES utf8 */
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40103 SET TIME_ZONE='+00:00' */
No rows affected (0.008 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */
No rows affected (0.012 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `BUCKETING_COLS` ( `SD_ID` bigint(20) NOT NULL, `BUCKET_COL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID`,`INTEGER_IDX`), KEY `BUCKETING_COLS_N49` (`SD_ID`), CONSTRAINT `BUCKETING_COLS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.122 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `CDS` ( `CD_ID` bigint(20) NOT NULL, PRIMARY KEY (`CD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.02 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `COLUMNS_V2` ( `CD_ID` bigint(20) NOT NULL, `COMMENT` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TYPE_NAME` MEDIUMTEXT DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`CD_ID`,`COLUMN_NAME`), KEY `COLUMNS_V2_N49` (`CD_ID`), CONSTRAINT `COLUMNS_V2_FK1` FOREIGN KEY (`CD_ID`) REFERENCES `CDS` (`CD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.032 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `DATABASE_PARAMS` ( `DB_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(180) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`DB_ID`,`PARAM_KEY`), KEY `DATABASE_PARAMS_N49` (`DB_ID`), CONSTRAINT `DATABASE_PARAMS_FK1` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE `CTLGS` ( `CTLG_ID` BIGINT PRIMARY KEY, `NAME` VARCHAR(256), `DESC` VARCHAR(4000), `LOCATION_URI` VARCHAR(4000) NOT NULL, UNIQUE KEY `UNIQUE_CATALOG` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.019 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO `CTLGS` VALUES (1, 'hive', 'Default catalog for Hive', 'TBD')
1 row affected (0.17 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `DBS` ( `DB_ID` bigint(20) NOT NULL, `DESC` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DB_LOCATION_URI` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `OWNER_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `OWNER_TYPE` varchar(10) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `CTLG_NAME` varchar(256) NOT NULL DEFAULT 'hive', PRIMARY KEY (`DB_ID`), UNIQUE KEY `UNIQUE_DATABASE` (`NAME`, `CTLG_NAME`), CONSTRAINT `CTLG_FK1` FOREIGN KEY (`CTLG_NAME`) REFERENCES `CTLGS` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.054 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `DB_PRIVS` ( `DB_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `DB_ID` bigint(20) DEFAULT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DB_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`DB_GRANT_ID`), UNIQUE KEY `DBPRIVILEGEINDEX` (`AUTHORIZER`,`DB_ID`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`DB_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), KEY `DB_PRIVS_N49` (`DB_ID`), CONSTRAINT `DB_PRIVS_FK1` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.023 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `GLOBAL_PRIVS` ( `USER_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `USER_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`USER_GRANT_ID`), UNIQUE KEY `GLOBALPRIVILEGEINDEX` (`AUTHORIZER`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`USER_PRIV`,`GRANTOR`,`GRANTOR_TYPE`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `IDXS` ( `INDEX_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `DEFERRED_REBUILD` bit(1) NOT NULL, `INDEX_HANDLER_CLASS` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INDEX_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INDEX_TBL_ID` bigint(20) DEFAULT NULL, `LAST_ACCESS_TIME` int(11) NOT NULL, `ORIG_TBL_ID` bigint(20) DEFAULT NULL, `SD_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`INDEX_ID`), UNIQUE KEY `UNIQUEINDEX` (`INDEX_NAME`,`ORIG_TBL_ID`), KEY `IDXS_N51` (`SD_ID`), KEY `IDXS_N50` (`INDEX_TBL_ID`), KEY `IDXS_N49` (`ORIG_TBL_ID`), CONSTRAINT `IDXS_FK1` FOREIGN KEY (`ORIG_TBL_ID`) REFERENCES `TBLS` (`TBL_ID`), CONSTRAINT `IDXS_FK2` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`), CONSTRAINT `IDXS_FK3` FOREIGN KEY (`INDEX_TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.029 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.006 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.008 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `INDEX_PARAMS` ( `INDEX_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`INDEX_ID`,`PARAM_KEY`), KEY `INDEX_PARAMS_N49` (`INDEX_ID`), CONSTRAINT `INDEX_PARAMS_FK1` FOREIGN KEY (`INDEX_ID`) REFERENCES `IDXS` (`INDEX_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `NUCLEUS_TABLES` ( `CLASS_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TABLE_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TYPE` varchar(4) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `OWNER` varchar(2) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `VERSION` varchar(20) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `INTERFACE_NAME` varchar(255) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`CLASS_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PARTITIONS` ( `PART_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `LAST_ACCESS_TIME` int(11) NOT NULL, `PART_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SD_ID` bigint(20) DEFAULT NULL, `TBL_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`PART_ID`), UNIQUE KEY `UNIQUEPARTITION` (`PART_NAME`,`TBL_ID`), KEY `PARTITIONS_N49` (`TBL_ID`), KEY `PARTITIONS_N50` (`SD_ID`), CONSTRAINT `PARTITIONS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`), CONSTRAINT `PARTITIONS_FK2` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.018 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_EVENTS` ( `PART_NAME_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `EVENT_TIME` bigint(20) NOT NULL, `EVENT_TYPE` int(11) NOT NULL, `PARTITION_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_NAME_ID`), KEY `PARTITIONEVENTINDEX` (`PARTITION_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.016 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_KEYS` ( `TBL_ID` bigint(20) NOT NULL, `PKEY_COMMENT` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PKEY_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PKEY_TYPE` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`TBL_ID`,`PKEY_NAME`), KEY `PARTITION_KEYS_N49` (`TBL_ID`), CONSTRAINT `PARTITION_KEYS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_KEY_VALS` ( `PART_ID` bigint(20) NOT NULL, `PART_KEY_VAL` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`PART_ID`,`INTEGER_IDX`), KEY `PARTITION_KEY_VALS_N49` (`PART_ID`), CONSTRAINT `PARTITION_KEY_VALS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PARTITION_PARAMS` ( `PART_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_ID`,`PARAM_KEY`), KEY `PARTITION_PARAMS_N49` (`PART_ID`), CONSTRAINT `PARTITION_PARAMS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.048 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PART_COL_PRIVS` ( `PART_COLUMN_GRANT_ID` bigint(20) NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_ID` bigint(20) DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_COL_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_COLUMN_GRANT_ID`), KEY `PART_COL_PRIVS_N49` (`PART_ID`), KEY `PARTITIONCOLUMNPRIVILEGEINDEX` (`AUTHORIZER`,`PART_ID`,`COLUMN_NAME`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`PART_COL_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), CONSTRAINT `PART_COL_PRIVS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.027 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PART_PRIVS` ( `PART_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_ID` bigint(20) DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PART_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`PART_GRANT_ID`), KEY `PARTPRIVILEGEINDEX` (`AUTHORIZER`,`PART_ID`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`PART_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), KEY `PART_PRIVS_N49` (`PART_ID`), CONSTRAINT `PART_PRIVS_FK1` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.028 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `ROLES` ( `ROLE_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `OWNER_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `ROLE_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`ROLE_ID`), UNIQUE KEY `ROLEENTITYINDEX` (`ROLE_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `ROLE_MAP` ( `ROLE_GRANT_ID` bigint(20) NOT NULL, `ADD_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `ROLE_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`ROLE_GRANT_ID`), UNIQUE KEY `USERROLEMAPINDEX` (`PRINCIPAL_NAME`,`ROLE_ID`,`GRANTOR`,`GRANTOR_TYPE`), KEY `ROLE_MAP_N49` (`ROLE_ID`), CONSTRAINT `ROLE_MAP_FK1` FOREIGN KEY (`ROLE_ID`) REFERENCES `ROLES` (`ROLE_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.023 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SDS` ( `SD_ID` bigint(20) NOT NULL, `CD_ID` bigint(20) DEFAULT NULL, `INPUT_FORMAT` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `IS_COMPRESSED` bit(1) NOT NULL, `IS_STOREDASSUBDIRECTORIES` bit(1) NOT NULL, `LOCATION` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `NUM_BUCKETS` int(11) NOT NULL, `OUTPUT_FORMAT` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SERDE_ID` bigint(20) DEFAULT NULL, PRIMARY KEY (`SD_ID`), KEY `SDS_N49` (`SERDE_ID`), KEY `SDS_N50` (`CD_ID`), CONSTRAINT `SDS_FK1` FOREIGN KEY (`SERDE_ID`) REFERENCES `SERDES` (`SERDE_ID`), CONSTRAINT `SDS_FK2` FOREIGN KEY (`CD_ID`) REFERENCES `CDS` (`CD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.054 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SD_PARAMS` ( `SD_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` MEDIUMTEXT CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`SD_ID`,`PARAM_KEY`), KEY `SD_PARAMS_N49` (`SD_ID`), CONSTRAINT `SD_PARAMS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.023 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SEQUENCE_TABLE` ( `SEQUENCE_NAME` varchar(255) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `NEXT_VAL` bigint(20) NOT NULL, PRIMARY KEY (`SEQUENCE_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.019 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO `SEQUENCE_TABLE` (`SEQUENCE_NAME`, `NEXT_VAL`) VALUES ('org.apache.hadoop.hive.metastore.model.MNotificationLog', 1)
1 row affected (0.009 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SERDES` ( `SERDE_ID` bigint(20) NOT NULL, `NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SLIB` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DESCRIPTION` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SERIALIZER_CLASS` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `DESERIALIZER_CLASS` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `SERDE_TYPE` integer, PRIMARY KEY (`SERDE_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.03 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SERDE_PARAMS` ( `SERDE_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` MEDIUMTEXT CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`SERDE_ID`,`PARAM_KEY`), KEY `SERDE_PARAMS_N49` (`SERDE_ID`), CONSTRAINT `SERDE_PARAMS_FK1` FOREIGN KEY (`SERDE_ID`) REFERENCES `SERDES` (`SERDE_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.032 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_COL_NAMES` ( `SD_ID` bigint(20) NOT NULL, `SKEWED_COL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID`,`INTEGER_IDX`), KEY `SKEWED_COL_NAMES_N49` (`SD_ID`), CONSTRAINT `SKEWED_COL_NAMES_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.028 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_COL_VALUE_LOC_MAP` ( `SD_ID` bigint(20) NOT NULL, `STRING_LIST_ID_KID` bigint(20) NOT NULL, `LOCATION` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`SD_ID`,`STRING_LIST_ID_KID`), KEY `SKEWED_COL_VALUE_LOC_MAP_N49` (`STRING_LIST_ID_KID`), KEY `SKEWED_COL_VALUE_LOC_MAP_N50` (`SD_ID`), CONSTRAINT `SKEWED_COL_VALUE_LOC_MAP_FK2` FOREIGN KEY (`STRING_LIST_ID_KID`) REFERENCES `SKEWED_STRING_LIST` (`STRING_LIST_ID`), CONSTRAINT `SKEWED_COL_VALUE_LOC_MAP_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.033 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.01 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_STRING_LIST` ( `STRING_LIST_ID` bigint(20) NOT NULL, PRIMARY KEY (`STRING_LIST_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.023 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.008 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.005 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_STRING_LIST_VALUES` ( `STRING_LIST_ID` bigint(20) NOT NULL, `STRING_LIST_VALUE` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`STRING_LIST_ID`,`INTEGER_IDX`), KEY `SKEWED_STRING_LIST_VALUES_N49` (`STRING_LIST_ID`), CONSTRAINT `SKEWED_STRING_LIST_VALUES_FK1` FOREIGN KEY (`STRING_LIST_ID`) REFERENCES `SKEWED_STRING_LIST` (`STRING_LIST_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.03 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.007 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SKEWED_VALUES` ( `SD_ID_OID` bigint(20) NOT NULL, `STRING_LIST_ID_EID` bigint(20) NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID_OID`,`INTEGER_IDX`), KEY `SKEWED_VALUES_N50` (`SD_ID_OID`), KEY `SKEWED_VALUES_N49` (`STRING_LIST_ID_EID`), CONSTRAINT `SKEWED_VALUES_FK2` FOREIGN KEY (`STRING_LIST_ID_EID`) REFERENCES `SKEWED_STRING_LIST` (`STRING_LIST_ID`), CONSTRAINT `SKEWED_VALUES_FK1` FOREIGN KEY (`SD_ID_OID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.025 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.007 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `SORT_COLS` ( `SD_ID` bigint(20) NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `ORDER` int(11) NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`SD_ID`,`INTEGER_IDX`), KEY `SORT_COLS_N49` (`SD_ID`), CONSTRAINT `SORT_COLS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.026 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.008 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TABLE_PARAMS` ( `TBL_ID` bigint(20) NOT NULL, `PARAM_KEY` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARAM_VALUE` MEDIUMTEXT CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TBL_ID`,`PARAM_KEY`), KEY `TABLE_PARAMS_N49` (`TBL_ID`), CONSTRAINT `TABLE_PARAMS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.033 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.004 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `MV_CREATION_METADATA` ( `MV_CREATION_METADATA_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TBL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TXN_LIST` TEXT DEFAULT NULL, `MATERIALIZATION_TIME` bigint(20) NOT NULL, PRIMARY KEY (`MV_CREATION_METADATA_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX MV_UNIQUE_TABLE ON MV_CREATION_METADATA (TBL_NAME, DB_NAME) USING BTREE
No rows affected (0.029 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TBLS` ( `TBL_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `DB_ID` bigint(20) DEFAULT NULL, `LAST_ACCESS_TIME` int(11) NOT NULL, `OWNER` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `OWNER_TYPE` varchar(10) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `RETENTION` int(11) NOT NULL, `SD_ID` bigint(20) DEFAULT NULL, `TBL_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `VIEW_EXPANDED_TEXT` mediumtext, `VIEW_ORIGINAL_TEXT` mediumtext, `IS_REWRITE_ENABLED` bit(1) NOT NULL DEFAULT 0, PRIMARY KEY (`TBL_ID`), UNIQUE KEY `UNIQUETABLE` (`TBL_NAME`,`DB_ID`), KEY `TBLS_N50` (`SD_ID`), KEY `TBLS_N49` (`DB_ID`), CONSTRAINT `TBLS_FK1` FOREIGN KEY (`SD_ID`) REFERENCES `SDS` (`SD_ID`), CONSTRAINT `TBLS_FK2` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.037 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `MV_TABLES_USED` ( `MV_CREATION_METADATA_ID` bigint(20) NOT NULL, `TBL_ID` bigint(20) NOT NULL, CONSTRAINT `MV_TABLES_USED_FK1` FOREIGN KEY (`MV_CREATION_METADATA_ID`) REFERENCES `MV_CREATION_METADATA` (`MV_CREATION_METADATA_ID`), CONSTRAINT `MV_TABLES_USED_FK2` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.021 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TBL_COL_PRIVS` ( `TBL_COLUMN_GRANT_ID` bigint(20) NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_COL_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_ID` bigint(20) DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TBL_COLUMN_GRANT_ID`), KEY `TABLECOLUMNPRIVILEGEINDEX` (`AUTHORIZER`,`TBL_ID`,`COLUMN_NAME`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`TBL_COL_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), KEY `TBL_COL_PRIVS_N49` (`TBL_ID`), CONSTRAINT `TBL_COL_PRIVS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.021 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TBL_PRIVS` ( `TBL_GRANT_ID` bigint(20) NOT NULL, `CREATE_TIME` int(11) NOT NULL, `GRANT_OPTION` smallint(6) NOT NULL, `GRANTOR` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `GRANTOR_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `PRINCIPAL_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_PRIV` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TBL_ID` bigint(20) DEFAULT NULL, `AUTHORIZER` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TBL_GRANT_ID`), KEY `TBL_PRIVS_N49` (`TBL_ID`), KEY `TABLEPRIVILEGEINDEX` (`AUTHORIZER`,`TBL_ID`,`PRINCIPAL_NAME`,`PRINCIPAL_TYPE`,`TBL_PRIV`,`GRANTOR`,`GRANTOR_TYPE`), CONSTRAINT `TBL_PRIVS_FK1` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.02 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TAB_COL_STATS` ( `CS_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TABLE_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TBL_ID` bigint(20) NOT NULL, `LONG_LOW_VALUE` bigint(20), `LONG_HIGH_VALUE` bigint(20), `DOUBLE_HIGH_VALUE` double(53,4), `DOUBLE_LOW_VALUE` double(53,4), `BIG_DECIMAL_LOW_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `BIG_DECIMAL_HIGH_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `NUM_NULLS` bigint(20) NOT NULL, `NUM_DISTINCTS` bigint(20), `BIT_VECTOR` blob, `AVG_COL_LEN` double(53,4), `MAX_COL_LEN` bigint(20), `NUM_TRUES` bigint(20), `NUM_FALSES` bigint(20), `LAST_ANALYZED` bigint(20) NOT NULL, PRIMARY KEY (`CS_ID`), CONSTRAINT `TAB_COL_STATS_FK` FOREIGN KEY (`TBL_ID`) REFERENCES `TBLS` (`TBL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.023 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX TAB_COL_STATS_IDX ON TAB_COL_STATS (CAT_NAME, DB_NAME, TABLE_NAME, COLUMN_NAME) USING BTREE
No rows affected (0.024 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `PART_COL_STATS` ( `CS_ID` bigint(20) NOT NULL, `CAT_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `DB_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `TABLE_NAME` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PARTITION_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_NAME` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `COLUMN_TYPE` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `PART_ID` bigint(20) NOT NULL, `LONG_LOW_VALUE` bigint(20), `LONG_HIGH_VALUE` bigint(20), `DOUBLE_HIGH_VALUE` double(53,4), `DOUBLE_LOW_VALUE` double(53,4), `BIG_DECIMAL_LOW_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `BIG_DECIMAL_HIGH_VALUE` varchar(4000) CHARACTER SET latin1 COLLATE latin1_bin, `NUM_NULLS` bigint(20) NOT NULL, `NUM_DISTINCTS` bigint(20), `BIT_VECTOR` blob, `AVG_COL_LEN` double(53,4), `MAX_COL_LEN` bigint(20), `NUM_TRUES` bigint(20), `NUM_FALSES` bigint(20), `LAST_ANALYZED` bigint(20) NOT NULL, PRIMARY KEY (`CS_ID`), CONSTRAINT `PART_COL_STATS_FK` FOREIGN KEY (`PART_ID`) REFERENCES `PARTITIONS` (`PART_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.016 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX PCS_STATS_IDX ON PART_COL_STATS (CAT_NAME, DB_NAME,TABLE_NAME,COLUMN_NAME,PARTITION_NAME) USING BTREE
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TYPES` ( `TYPES_ID` bigint(20) NOT NULL, `TYPE_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TYPE1` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `TYPE2` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, PRIMARY KEY (`TYPES_ID`), UNIQUE KEY `UNIQUE_TYPE` (`TYPE_NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET @saved_cs_client     = @@character_set_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = utf8 */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `TYPE_FIELDS` ( `TYPE_NAME` bigint(20) NOT NULL, `COMMENT` varchar(256) CHARACTER SET latin1 COLLATE latin1_bin DEFAULT NULL, `FIELD_NAME` varchar(128) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `FIELD_TYPE` varchar(767) CHARACTER SET latin1 COLLATE latin1_bin NOT NULL, `INTEGER_IDX` int(11) NOT NULL, PRIMARY KEY (`TYPE_NAME`,`FIELD_NAME`), KEY `TYPE_FIELDS_N49` (`TYPE_NAME`), CONSTRAINT `TYPE_FIELDS_FK1` FOREIGN KEY (`TYPE_NAME`) REFERENCES `TYPES` (`TYPES_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `MASTER_KEYS` ( `KEY_ID` INTEGER NOT NULL AUTO_INCREMENT, `MASTER_KEY` VARCHAR(767) BINARY NULL, PRIMARY KEY (`KEY_ID`) ) ENGINE=INNODB DEFAULT CHARSET=latin1
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `DELEGATION_TOKENS` ( `TOKEN_IDENT` VARCHAR(767) BINARY NOT NULL, `TOKEN` VARCHAR(767) BINARY NULL, PRIMARY KEY (`TOKEN_IDENT`) ) ENGINE=INNODB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `VERSION` ( `VER_ID` BIGINT NOT NULL, `SCHEMA_VERSION` VARCHAR(127) NOT NULL, `VERSION_COMMENT` VARCHAR(255), PRIMARY KEY (`VER_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.012 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `FUNCS` ( `FUNC_ID` BIGINT(20) NOT NULL, `CLASS_NAME` VARCHAR(4000) CHARACTER SET latin1 COLLATE latin1_bin, `CREATE_TIME` INT(11) NOT NULL, `DB_ID` BIGINT(20), `FUNC_NAME` VARCHAR(128) CHARACTER SET latin1 COLLATE latin1_bin, `FUNC_TYPE` INT(11) NOT NULL, `OWNER_NAME` VARCHAR(128) CHARACTER SET latin1 COLLATE latin1_bin, `OWNER_TYPE` VARCHAR(10) CHARACTER SET latin1 COLLATE latin1_bin, PRIMARY KEY (`FUNC_ID`), UNIQUE KEY `UNIQUEFUNCTION` (`FUNC_NAME`, `DB_ID`), KEY `FUNCS_N49` (`DB_ID`), CONSTRAINT `FUNCS_FK1` FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.021 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `FUNC_RU` ( `FUNC_ID` BIGINT(20) NOT NULL, `RESOURCE_TYPE` INT(11) NOT NULL, `RESOURCE_URI` VARCHAR(4000) CHARACTER SET latin1 COLLATE latin1_bin, `INTEGER_IDX` INT(11) NOT NULL, PRIMARY KEY (`FUNC_ID`, `INTEGER_IDX`), CONSTRAINT `FUNC_RU_FK1` FOREIGN KEY (`FUNC_ID`) REFERENCES `FUNCS` (`FUNC_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `NOTIFICATION_LOG` ( `NL_ID` BIGINT(20) NOT NULL, `EVENT_ID` BIGINT(20) NOT NULL, `EVENT_TIME` INT(11) NOT NULL, `EVENT_TYPE` varchar(32) NOT NULL, `CAT_NAME` varchar(256), `DB_NAME` varchar(128), `TBL_NAME` varchar(256), `MESSAGE` longtext, `MESSAGE_FORMAT` varchar(16), PRIMARY KEY (`NL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `NOTIFICATION_SEQUENCE` ( `NNI_ID` BIGINT(20) NOT NULL, `NEXT_EVENT_ID` BIGINT(20) NOT NULL, PRIMARY KEY (`NNI_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO `NOTIFICATION_SEQUENCE` (`NNI_ID`, `NEXT_EVENT_ID`) SELECT * from (select 1 as `NNI_ID`, 1 as `NOTIFICATION_SEQUENCE`) a WHERE (SELECT COUNT(*) FROM `NOTIFICATION_SEQUENCE`) = 0
1 row affected (0.01 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `KEY_CONSTRAINTS` ( `CHILD_CD_ID` BIGINT, `CHILD_INTEGER_IDX` INT(11), `CHILD_TBL_ID` BIGINT, `PARENT_CD_ID` BIGINT, `PARENT_INTEGER_IDX` INT(11) NOT NULL, `PARENT_TBL_ID` BIGINT NOT NULL, `POSITION` BIGINT NOT NULL, `CONSTRAINT_NAME` VARCHAR(400) NOT NULL, `CONSTRAINT_TYPE` SMALLINT(6)  NOT NULL, `UPDATE_RULE` SMALLINT(6), `DELETE_RULE` SMALLINT(6), `ENABLE_VALIDATE_RELY` SMALLINT(6) NOT NULL, `DEFAULT_VALUE` VARCHAR(400), PRIMARY KEY (`CONSTRAINT_NAME`, `POSITION`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.012 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX `CONSTRAINTS_PARENT_TABLE_ID_INDEX` ON KEY_CONSTRAINTS (`PARENT_TBL_ID`) USING BTREE
No rows affected (0.016 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX `CONSTRAINTS_CONSTRAINT_TYPE_INDEX` ON KEY_CONSTRAINTS (`CONSTRAINT_TYPE`) USING BTREE
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS `METASTORE_DB_PROPERTIES` ( `PROPERTY_KEY` varchar(255) NOT NULL, `PROPERTY_VALUE` varchar(1000) NOT NULL, `DESCRIPTION` varchar(1000), PRIMARY KEY(`PROPERTY_KEY`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS WM_RESOURCEPLAN ( `RP_ID` bigint(20) NOT NULL, `NAME` varchar(128) NOT NULL, `QUERY_PARALLELISM` int(11), `STATUS` varchar(20) NOT NULL, `DEFAULT_POOL_ID` bigint(20), PRIMARY KEY (`RP_ID`), UNIQUE KEY `UNIQUE_WM_RESOURCEPLAN` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS WM_POOL ( `POOL_ID` bigint(20) NOT NULL, `RP_ID` bigint(20) NOT NULL, `PATH` varchar(767) NOT NULL, `ALLOC_FRACTION` DOUBLE, `QUERY_PARALLELISM` int(11), `SCHEDULING_POLICY` varchar(767), PRIMARY KEY (`POOL_ID`), UNIQUE KEY `UNIQUE_WM_POOL` (`RP_ID`, `PATH`), CONSTRAINT `WM_POOL_FK1` FOREIGN KEY (`RP_ID`) REFERENCES `WM_RESOURCEPLAN` (`RP_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.016 seconds)
0: jdbc:mysql://node1:3308/metastore> ALTER TABLE `WM_RESOURCEPLAN` ADD CONSTRAINT `WM_RESOURCEPLAN_FK1` FOREIGN KEY (`DEFAULT_POOL_ID`) REFERENCES `WM_POOL`(`POOL_ID`)
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS WM_TRIGGER ( `TRIGGER_ID` bigint(20) NOT NULL, `RP_ID` bigint(20) NOT NULL, `NAME` varchar(128) NOT NULL, `TRIGGER_EXPRESSION` varchar(1024), `ACTION_EXPRESSION` varchar(1024), `IS_IN_UNMANAGED` bit(1) NOT NULL DEFAULT 0, PRIMARY KEY (`TRIGGER_ID`), UNIQUE KEY `UNIQUE_WM_TRIGGER` (`RP_ID`, `NAME`), CONSTRAINT `WM_TRIGGER_FK1` FOREIGN KEY (`RP_ID`) REFERENCES `WM_RESOURCEPLAN` (`RP_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS WM_POOL_TO_TRIGGER ( `POOL_ID` bigint(20) NOT NULL, `TRIGGER_ID` bigint(20) NOT NULL, PRIMARY KEY (`POOL_ID`, `TRIGGER_ID`), CONSTRAINT `WM_POOL_TO_TRIGGER_FK1` FOREIGN KEY (`POOL_ID`) REFERENCES `WM_POOL` (`POOL_ID`), CONSTRAINT `WM_POOL_TO_TRIGGER_FK2` FOREIGN KEY (`TRIGGER_ID`) REFERENCES `WM_TRIGGER` (`TRIGGER_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE IF NOT EXISTS WM_MAPPING ( `MAPPING_ID` bigint(20) NOT NULL, `RP_ID` bigint(20) NOT NULL, `ENTITY_TYPE` varchar(128) NOT NULL, `ENTITY_NAME` varchar(128) NOT NULL, `POOL_ID` bigint(20), `ORDERING` int, PRIMARY KEY (`MAPPING_ID`), UNIQUE KEY `UNIQUE_WM_MAPPING` (`RP_ID`, `ENTITY_TYPE`, `ENTITY_NAME`), CONSTRAINT `WM_MAPPING_FK1` FOREIGN KEY (`RP_ID`) REFERENCES `WM_RESOURCEPLAN` (`RP_ID`), CONSTRAINT `WM_MAPPING_FK2` FOREIGN KEY (`POOL_ID`) REFERENCES `WM_POOL` (`POOL_ID`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.02 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE TXNS ( TXN_ID bigint PRIMARY KEY, TXN_STATE char(1) NOT NULL, TXN_STARTED bigint NOT NULL, TXN_LAST_HEARTBEAT bigint NOT NULL, TXN_USER varchar(128) NOT NULL, TXN_HOST varchar(128) NOT NULL, TXN_AGENT_INFO varchar(128), TXN_META_INFO varchar(128), TXN_HEARTBEAT_COUNT int, TXN_TYPE int ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE TXN_COMPONENTS ( TC_TXNID bigint NOT NULL, TC_DATABASE varchar(128) NOT NULL, TC_TABLE varchar(128), TC_PARTITION varchar(767), TC_OPERATION_TYPE char(1) NOT NULL, TC_WRITEID bigint, FOREIGN KEY (TC_TXNID) REFERENCES TXNS (TXN_ID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX TC_TXNID_INDEX ON TXN_COMPONENTS (TC_TXNID)
No rows affected (0.018 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE COMPLETED_TXN_COMPONENTS ( CTC_TXNID bigint NOT NULL, CTC_DATABASE varchar(128) NOT NULL, CTC_TABLE varchar(256), CTC_PARTITION varchar(767), CTC_TIMESTAMP timestamp DEFAULT CURRENT_TIMESTAMP NOT NULL, CTC_WRITEID bigint, CTC_UPDATE_DELETE char(1) NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX COMPLETED_TXN_COMPONENTS_IDX ON COMPLETED_TXN_COMPONENTS (CTC_DATABASE, CTC_TABLE, CTC_PARTITION) USING BTREE
No rows affected (0.026 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE NEXT_TXN_ID ( NTXN_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO NEXT_TXN_ID VALUES(1)
1 row affected (0.003 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE HIVE_LOCKS ( HL_LOCK_EXT_ID bigint NOT NULL, HL_LOCK_INT_ID bigint NOT NULL, HL_TXNID bigint NOT NULL, HL_DB varchar(128) NOT NULL, HL_TABLE varchar(128), HL_PARTITION varchar(767), HL_LOCK_STATE char(1) not null, HL_LOCK_TYPE char(1) not null, HL_LAST_HEARTBEAT bigint NOT NULL, HL_ACQUIRED_AT bigint, HL_USER varchar(128) NOT NULL, HL_HOST varchar(128) NOT NULL, HL_HEARTBEAT_COUNT int, HL_AGENT_INFO varchar(128), HL_BLOCKEDBY_EXT_ID bigint, HL_BLOCKEDBY_INT_ID bigint, PRIMARY KEY(HL_LOCK_EXT_ID, HL_LOCK_INT_ID), KEY HIVE_LOCK_TXNID_INDEX (HL_TXNID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.015 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX HL_TXNID_IDX ON HIVE_LOCKS (HL_TXNID)
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE NEXT_LOCK_ID ( NL_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO NEXT_LOCK_ID VALUES(1)
1 row affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE COMPACTION_QUEUE ( CQ_ID bigint PRIMARY KEY, CQ_DATABASE varchar(128) NOT NULL, CQ_TABLE varchar(128) NOT NULL, CQ_PARTITION varchar(767), CQ_STATE char(1) NOT NULL, CQ_TYPE char(1) NOT NULL, CQ_TBLPROPERTIES varchar(2048), CQ_WORKER_ID varchar(128), CQ_START bigint, CQ_RUN_AS varchar(128), CQ_HIGHEST_WRITE_ID bigint, CQ_META_INFO varbinary(2048), CQ_HADOOP_JOB_ID varchar(32) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.009 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE COMPLETED_COMPACTIONS ( CC_ID bigint PRIMARY KEY, CC_DATABASE varchar(128) NOT NULL, CC_TABLE varchar(128) NOT NULL, CC_PARTITION varchar(767), CC_STATE char(1) NOT NULL, CC_TYPE char(1) NOT NULL, CC_TBLPROPERTIES varchar(2048), CC_WORKER_ID varchar(128), CC_START bigint, CC_END bigint, CC_RUN_AS varchar(128), CC_HIGHEST_WRITE_ID bigint, CC_META_INFO varbinary(2048), CC_HADOOP_JOB_ID varchar(32) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.012 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE NEXT_COMPACTION_QUEUE_ID ( NCQ_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO NEXT_COMPACTION_QUEUE_ID VALUES(1)
1 row affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE AUX_TABLE ( MT_KEY1 varchar(128) NOT NULL, MT_KEY2 bigint NOT NULL, MT_COMMENT varchar(255), PRIMARY KEY(MT_KEY1, MT_KEY2) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.016 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE WRITE_SET ( WS_DATABASE varchar(128) NOT NULL, WS_TABLE varchar(128) NOT NULL, WS_PARTITION varchar(767), WS_TXNID bigint NOT NULL, WS_COMMIT_ID bigint NOT NULL, WS_OPERATION_TYPE char(1) NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.01 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE TXN_TO_WRITE_ID ( T2W_TXNID bigint NOT NULL, T2W_DATABASE varchar(128) NOT NULL, T2W_TABLE varchar(256) NOT NULL, T2W_WRITEID bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.013 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE UNIQUE INDEX TBL_TO_TXN_ID_IDX ON TXN_TO_WRITE_ID (T2W_DATABASE, T2W_TABLE, T2W_TXNID)
No rows affected (0.023 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE UNIQUE INDEX TBL_TO_WRITE_ID_IDX ON TXN_TO_WRITE_ID (T2W_DATABASE, T2W_TABLE, T2W_WRITEID)
No rows affected (0.007 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE NEXT_WRITE_ID ( NWI_DATABASE varchar(128) NOT NULL, NWI_TABLE varchar(256) NOT NULL, NWI_NEXT bigint NOT NULL ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE UNIQUE INDEX NEXT_WRITE_ID_IDX ON NEXT_WRITE_ID (NWI_DATABASE, NWI_TABLE)
No rows affected (0.016 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE MIN_HISTORY_LEVEL ( MHL_TXNID bigint NOT NULL, MHL_MIN_OPEN_TXNID bigint NOT NULL, PRIMARY KEY(MHL_TXNID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.01 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX MIN_HISTORY_LEVEL_IDX ON MIN_HISTORY_LEVEL (MHL_MIN_OPEN_TXNID)
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE MATERIALIZATION_REBUILD_LOCKS ( MRL_TXN_ID bigint NOT NULL, MRL_DB_NAME VARCHAR(128) NOT NULL, MRL_TBL_NAME VARCHAR(256) NOT NULL, MRL_LAST_HEARTBEAT bigint NOT NULL, PRIMARY KEY(MRL_TXN_ID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.007 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE `I_SCHEMA` ( `SCHEMA_ID` BIGINT PRIMARY KEY, `SCHEMA_TYPE` INTEGER NOT NULL, `NAME` VARCHAR(256), `DB_ID` BIGINT, `COMPATIBILITY` INTEGER NOT NULL, `VALIDATION_LEVEL` INTEGER NOT NULL, `CAN_EVOLVE` bit(1) NOT NULL, `SCHEMA_GROUP` VARCHAR(256), `DESCRIPTION` VARCHAR(4000), FOREIGN KEY (`DB_ID`) REFERENCES `DBS` (`DB_ID`), KEY `UNIQUE_NAME` (`NAME`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.017 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE `SCHEMA_VERSION` ( `SCHEMA_VERSION_ID` bigint primary key, `SCHEMA_ID` BIGINT, `VERSION` INTEGER NOT NULL, `CREATED_AT` BIGINT NOT NULL, `CD_ID` BIGINT, `STATE` INTEGER NOT NULL, `DESCRIPTION` VARCHAR(4000), `SCHEMA_TEXT` mediumtext, `FINGERPRINT` VARCHAR(256), `SCHEMA_VERSION_NAME` VARCHAR(256), `SERDE_ID` bigint, FOREIGN KEY (`SCHEMA_ID`) REFERENCES `I_SCHEMA` (`SCHEMA_ID`), FOREIGN KEY (`CD_ID`) REFERENCES `CDS` (`CD_ID`), FOREIGN KEY (`SERDE_ID`) REFERENCES `SERDES` (`SERDE_ID`), KEY `UNIQUE_VERSION` (`SCHEMA_ID`, `VERSION`) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.022 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE REPL_TXN_MAP ( RTM_REPL_POLICY varchar(256) NOT NULL, RTM_SRC_TXN_ID bigint NOT NULL, RTM_TARGET_TXN_ID bigint NOT NULL, PRIMARY KEY (RTM_REPL_POLICY, RTM_SRC_TXN_ID) ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.011 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE TABLE RUNTIME_STATS ( RS_ID bigint primary key, CREATE_TIME bigint NOT NULL, WEIGHT bigint NOT NULL, PAYLOAD blob ) ENGINE=InnoDB DEFAULT CHARSET=latin1
No rows affected (0.012 seconds)
0: jdbc:mysql://node1:3308/metastore> CREATE INDEX IDX_RUNTIME_STATS_CREATE_TIME ON RUNTIME_STATS(CREATE_TIME)
No rows affected (0.014 seconds)
0: jdbc:mysql://node1:3308/metastore> INSERT INTO VERSION (VER_ID, SCHEMA_VERSION, VERSION_COMMENT) VALUES (1, '3.1.0', 'Hive release version 3.1.0')
1 row affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET character_set_client = @saved_cs_client */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40103 SET TIME_ZONE=@OLD_TIME_ZONE */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET SQL_MODE=@OLD_SQL_MODE */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */
No rows affected (0.002 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */
No rows affected (0.001 seconds)
0: jdbc:mysql://node1:3308/metastore> /*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */
No rows affected (0 seconds)
0: jdbc:mysql://node1:3308/metastore> !closeall
Closing: 0: jdbc:mysql://node1:3308/metastore?useSSL=false
beeline> 
beeline> Initialization script completed
schemaTool completed

# 重新测试hive客户端
hadoop@node1:/opt/module/hive-3.1.3$ bin/hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = e252b2d5-6cd4-40d5-9f9a-f15a97b141be

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 33327102-68c6-4627-9873-536d4cbea183
hive> show databases;
OK
default
Time taken: 0.403 seconds, Fetched: 1 row(s)
# 元数据重置 原来的表信息没了
hive> show tables;
OK
Time taken: 0.038 seconds
hive> create table stu(id int, name string);
OK
Time taken: 0.526 seconds
# 数据存储在hdfs上 所以上次的数据还在
hive> select * from stu;
OK
1	aa
Time taken: 1.507 seconds, Fetched: 1 row(s)
# 测试插入数据
hive> insert into stu values(2, "bb");
Query ID = hadoop_20230520181958_16eae5b5-f02f-45a7-8ad7-bbdea54ae01c
Total jobs = 3
Launching Job 1 out of 3
Number of reduce tasks determined at compile time: 1
In order to change the average load for a reducer (in bytes):
  set hive.exec.reducers.bytes.per.reducer=<number>
In order to limit the maximum number of reducers:
  set hive.exec.reducers.max=<number>
In order to set a constant number of reducers:
  set mapreduce.job.reduces=<number>
Starting Job = job_1684630072455_0002, Tracking URL = http://node3:8088/proxy/application_1684630072455_0002/
Kill Command = /opt/ha/hadoop-3.3.0/bin/mapred job  -kill job_1684630072455_0002
Hadoop job information for Stage-1: number of mappers: 1; number of reducers: 1
2023-05-20 18:21:35,201 Stage-1 map = 0%,  reduce = 0%
2023-05-20 18:21:43,491 Stage-1 map = 100%,  reduce = 0%, Cumulative CPU 1.65 sec
2023-05-20 18:21:49,639 Stage-1 map = 100%,  reduce = 100%, Cumulative CPU 2.88 sec
MapReduce Total cumulative CPU time: 2 seconds 880 msec
Ended Job = job_1684630072455_0002
Stage-4 is selected by condition resolver.
Stage-3 is filtered out by condition resolver.
Stage-5 is filtered out by condition resolver.
Moving data to directory hdfs://mycluster/user/hive/warehouse/stu/.hive-staging_hive_2023-05-20_18-19-58_778_5874046080698671391-1/-ext-10000
Loading data to table default.stu
MapReduce Jobs Launched: 
Stage-Stage-1: Map: 1  Reduce: 1   Cumulative CPU: 2.88 sec   HDFS Read: 15113 HDFS Write: 235 SUCCESS
Total MapReduce CPU Time Spent: 2 seconds 880 msec
OK
Time taken: 112.363 seconds
hive> select * from stu;
OK
1	aa
2	bb
Time taken: 0.117 seconds, Fetched: 2 row(s)
hive> exit;
```

## hiveserver2

```bash
# 修改hadoop核心配置文件 末尾添加
hadoop@node1:/opt/module/hive-3.1.3$ vim $HADOOP_HOME/etc/hadoop/core-site.xml

<!-- 配置所有节点的hadoop用户都可作为代理用户 -->
<property>
	<name>hadoop.proxyuser.hadoop.hosts</name>
    <value>*</value>
</property>
<!-- 配置hadoop用户能够代理的用户组为任意组 -->
<property>
	<name>hadoop.proxyuser.hadoop.groups</name>
    <value>*</value>
</property>
<!-- 配置hadoop用户能够代理的用户为任意用户 -->
<property>
	<name>hadoop.proxyuser.hadoop.users</name>
    <value>*</value>
</property>

# 分发
hadoop@node1:/opt/module/hive-3.1.3$ xsync $HADOOP_HOME/etc/hadoop/core-site.xml
# 修改hive-site.xml文件 末尾添加
hadoop@node1:/opt/module/hive-3.1.3$ vim conf/hive-site.xml 

<!-- 指定hiveserver2连接的host -->
<property>
	<name>hive.server2.thrift.bind.host</name>
    <value>node1</value>
</property>
<!-- 指定hiveserver2连接的端口号 -->
<property>
	<name>hive.server2.thrift.port</name>
    <value>10000</value>
</property>

# 重启hadoop集群使配置文件生效
hadoop@node1:/opt/module/hive-3.1.3$ stop-all.sh 
hadoop@node1:/opt/module/hive-3.1.3$ start-all.sh 

# 开启hiveserver2服务
hadoop@node1:/opt/module/hive-3.1.3$ nohup bin/hiveserver2 &
[1] 16291
nohup: 忽略输入并把输出追加到'nohup.out'

hadoop@node1:/opt/module/hive-3.1.3$ jps -ml
16291 org.apache.hadoop.util.RunJar /opt/module/hive-3.1.3/lib/hive-service-3.1.3.jar org.apache.hive.service.server.HiveServer2
15876 org.apache.hadoop.yarn.server.resourcemanager.ResourceManager
15077 org.apache.hadoop.hdfs.server.datanode.DataNode
16026 org.apache.hadoop.yarn.server.nodemanager.NodeManager
15548 org.apache.hadoop.hdfs.tools.DFSZKFailoverController
15309 org.apache.hadoop.hdfs.qjournal.server.JournalNode
5117 org.apache.zookeeper.server.quorum.QuorumPeerMain /opt/module/zookeeper-3.5.7/bin/../conf/zoo.cfg
16622 sun.tools.jps.Jps -ml
14927 org.apache.hadoop.hdfs.server.namenode.NameNode

## 使用beeline测试
hadoop@node1:/opt/module/hive-3.1.3$ bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
# jdbc协议连接
beeline> !connect jdbc:hive2://node1:10000
Connecting to jdbc:hive2://node1:10000
Enter username for jdbc:hive2://node1:10000: hadoop
Enter password for jdbc:hive2://node1:10000: 
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1:10000> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (2.775 seconds)
0: jdbc:hive2://node1:10000> show databases;
+----------------+
| database_name  |
+----------------+
| default        |
+----------------+
1 row selected (0.1 seconds)
0: jdbc:hive2://node1:10000> !quit
Closing: 0: jdbc:hive2://node1:10000

```

## metastore

```bash
# 配置node2 Hive客户端
hadoop@node1:/opt/module/hive-3.1.3$ scp -r /opt/module/hive-3.1.3/ node2:/opt/module/
# 开启metastore服务
hadoop@node1:/opt/module/hive-3.1.3$ nohup bin/hive --service metastore &
[2] 16899
nohup: 忽略输入并把输出追加到'nohup.out'
hadoop@node1:/opt/module/hive-3.1.3$ jps
16291 RunJar
16899 RunJar
15876 ResourceManager
15077 DataNode
16986 Jps
16026 NodeManager
15548 DFSZKFailoverController
15309 JournalNode
5117 QuorumPeerMain
14927 NameNode
hadoop@node1:/opt/module/hive-3.1.3$ jps -ml
16291 org.apache.hadoop.util.RunJar /opt/module/hive-3.1.3/lib/hive-service-3.1.3.jar org.apache.hive.service.server.HiveServer2
16899 org.apache.hadoop.util.RunJar /opt/module/hive-3.1.3/lib/hive-metastore-3.1.3.jar org.apache.hadoop.hive.metastore.HiveMetaStore
17027 sun.tools.jps.Jps -ml
15876 org.apache.hadoop.yarn.server.resourcemanager.ResourceManager
15077 org.apache.hadoop.hdfs.server.datanode.DataNode
16026 org.apache.hadoop.yarn.server.nodemanager.NodeManager
15548 org.apache.hadoop.hdfs.tools.DFSZKFailoverController
15309 org.apache.hadoop.hdfs.qjournal.server.JournalNode
5117 org.apache.zookeeper.server.quorum.QuorumPeerMain /opt/module/zookeeper-3.5.7/bin/../conf/zoo.cfg
14927 org.apache.hadoop.hdfs.server.namenode.NameNode

# 修改hive-site.xml
hadoop@node2:/opt/module/hive-3.1.3$ vim conf/hive-site.xml 

<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- Hive 默认在 HDFS 的工作目录 -->
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
    </property>

    <!-- 指定hiveserver2连接的host -->
    <property>
        <name>hive.server2.thrift.bind.host</name>
        <value>node1</value>
    </property>
    <!-- 指定hiveserver2连接的端口号 -->
    <property>
        <name>hive.server2.thrift.port</name>
        <value>10000</value>

    </property>
    <!-- 指定metastore服务的地址 -->
    <property>
        <name>hive.metastore.uris</name>
        <value>thrift://node1:9083</value>
        </property>
</configuration>

# 测试
hadoop@node2:/opt/module/hive-3.1.3$ bin/hive
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hbase-2.4.11/lib/client-facing-thirdparty/slf4j-reload4j-1.7.33.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Reload4jLoggerFactory]
Hive Session ID = d45fbce9-237e-4b29-9af4-2a430b21648a

Logging initialized using configuration in jar:file:/opt/module/hive-3.1.3/lib/hive-common-3.1.3.jar!/hive-log4j2.properties Async: true
Hive-on-MR is deprecated in Hive 2 and may not be available in the future versions. Consider using a different execution engine (i.e. spark, tez) or using Hive 1.X releases.
Hive Session ID = 97a8d063-acfc-48a6-acf8-ae5e3b30acc5
hive> select * from stu;
OK
1	aa
2	bb
Time taken: 1.665 seconds, Fetched: 2 row(s)
hive> exit;

# 测试hiveserver2服务
hadoop@node2:/opt/module/hive-3.1.3$ bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
beeline> !connect jdbc:hive2://node1:10000
Connecting to jdbc:hive2://node1:10000
Enter username for jdbc:hive2://node1:10000: hadoop
Enter password for jdbc:hive2://node1:10000: 
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1:10000> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (0.278 seconds)
0: jdbc:hive2://node1:10000> !quit
Closing: 0: jdbc:hive2://node1:10000
```

# HiveHA

node1 & node3

```bash
hadoop@node1:/opt/module/hive-3.1.3$ vim conf/hive-site.xml
# 开启hiveserver2, metastore服务
hadoop@node1:/opt/module/hive-3.1.3$ nohup bin/hive --service hiveserver2 >> /opt/module/hive-3.1.3/hiveserver2.log 2>&1 &
hadoop@node1:/opt/module/hive-3.1.3$ nohup bin/hive --service metastore >> /opt/module/hive-3.1.3/metastore.log 2>&1 &
```

node2

```bash
hadoop@node2:/opt/module/hive-3.1.3$ vim conf/hive-site.xml 
hadoop@node2:/opt/module/hive-3.1.3$ zkCli.sh 
# 检查zk注册信息
[zk: localhost:2181(CONNECTED) 0] ls /hiveserver2_zk
[serverUri=node1:10000;version=3.1.3;sequence=0000000000, serverUri=node3:10000;version=3.1.3;sequence=0000000001]
[zk: localhost:2181(CONNECTED) 1] quit

# 测试hiveserver2连接
hadoop@node2:/opt/module/hive-3.1.3$ bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
beeline> !connect jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk

#jdbc:hive2://<zookeeperquorum>/<dbName>;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2  

#<zookeeper quorum> 为Zookeeper的集群链接串，如node1:2181,node2:2181,node3:2181
#<dbName> 为Hive数据库，默认为default
#serviceDiscoveryMode=zooKeeper 指定模式为zooKeeper
#zooKeeperNamespace=hiveserver2 指定ZK中的nameSpace，即参数hive.server2.zookeeper.namespace所定义

Connecting to jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Enter username for jdbc:hive2://node1:2181,node2:2181,node3:2181/: hadoop
Enter password for jdbc:hive2://node1:2181,node2:2181,node3:2181/: 
23/05/20 20:13:33 [main]: INFO jdbc.HiveConnection: Connected to node3:10000
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (3.525 seconds)
0: jdbc:hive2://node1:2181,node2:2181,node3:2> !quit
Closing: 0: jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
```

node1 ==> hive-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- jdbc 连接的 URL -->
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://node1:3308/metastore?useSSL=false</value>
    </property>
    <!-- jdbc 连接的 Driver-->
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.cj.jdbc.Driver</value>
    </property>
    <!-- jdbc 连接的 username-->
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>
    <!-- jdbc 连接的 password -->
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>1234</value>
    </property>
    <!-- Hive 默认在 HDFS 的工作目录 -->
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
    </property>

    <!-- 指定hiveserver2连接的host -->
    <property>
        <name>hive.server2.thrift.bind.host</name>
        <value>node1</value>
    </property>
    <!-- 指定hiveserver2连接的端口号 -->
    <property>
        <name>hive.server2.thrift.port</name>
        <value>10000</value>
    </property>

    <property>
        <name>hive.metastore.uris</name>
        <value>thrift://node1:9083,thrift://node3:9083</value>
    </property>

    <property>
        <name>hive.server2.support.dynamic.service.discovery</name>
        <value>true</value>
    </property>

    <property>
        <name>hive.server2.zookeeper.namespace</name>
        <value>hiveserver2_zk</value>
    </property>

    <property>
        <name>hive.zookeeper.quorum</name>
        <value> node1:2181,node2:2181,node3:2181</value>
    </property>

    <property>
        <name>hive.zookeeper.client.port</name>
        <value>2181</value>
    </property>

</configuration>
```

node2 ==> hive-site.xml

```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <!-- Hive 默认在 HDFS 的工作目录 -->
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
    </property>
    <!-- 指定metastore服务的地址 -->
    <property>
        <name>hive.metastore.uris</name>
        <value>thrift://node1:9083,thrift://node3:9083</value>
    </property>
</configuration>
```

## 测试hiveserver2HA

```bash
hadoop@node2:/opt/module/hive-3.1.3$ bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
beeline> !connect jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Connecting to jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Enter username for jdbc:hive2://node1:2181,node2:2181,node3:2181/: hadoop
Enter password for jdbc:hive2://node1:2181,node2:2181,node3:2181/: 
23/05/20 20:21:09 [main]: INFO jdbc.HiveConnection: Connected to node1:10000
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
# hiveserver2存活状态
# +--------+--------+
# | node1  | node3  |
# +--------+--------+
# | y      | y      |
# +--------+--------+
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (0.205 seconds)
# hiveserver2存活状态
# +--------+--------+
# | node1  | node3  |
# +--------+--------+
# | n      | y      |
# +--------+--------+
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
Unexpected end of file when reading from HS2 server. The root cause might be too many concurrent connections. Please ask the administrator to check the number of active connections, and adjust hive.server2.thrift.max.worker.threads if applicable.
Error: org.apache.thrift.transport.TTransportException (state=08S01,code=0)
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
Unknown HS2 problem when communicating with Thrift server.
Error: org.apache.thrift.transport.TTransportException: java.net.SocketException: 断开的管道 (Write failed) (state=08S01,code=0)
# 退出重新连接
0: jdbc:hive2://node1:2181,node2:2181,node3:2> !quit
Closing: 0: jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
23/05/20 20:25:29 [main]: WARN transport.TIOStreamTransport: Error closing output stream.
java.net.SocketException: Socket closed
	at java.net.SocketOutputStream.socketWrite(SocketOutputStream.java:118) ~[?:1.8.0_241]
	at java.net.SocketOutputStream.write(SocketOutputStream.java:155) ~[?:1.8.0_241]
	at java.io.BufferedOutputStream.flushBuffer(BufferedOutputStream.java:82) ~[?:1.8.0_241]
	at java.io.BufferedOutputStream.flush(BufferedOutputStream.java:140) ~[?:1.8.0_241]
	at java.io.FilterOutputStream.close(FilterOutputStream.java:158) ~[?:1.8.0_241]
	at org.apache.thrift.transport.TIOStreamTransport.close(TIOStreamTransport.java:110) [hive-exec-3.1.3.jar:3.1.3]
	at org.apache.thrift.transport.TSocket.close(TSocket.java:235) [hive-exec-3.1.3.jar:3.1.3]
	at org.apache.thrift.transport.TSaslTransport.close(TSaslTransport.java:402) [hive-exec-3.1.3.jar:3.1.3]
	at org.apache.thrift.transport.TSaslClientTransport.close(TSaslClientTransport.java:37) [hive-exec-3.1.3.jar:3.1.3]
	at org.apache.hive.jdbc.HiveConnection.close(HiveConnection.java:885) [hive-jdbc-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.Commands.close(Commands.java:1433) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.Commands.quit(Commands.java:1400) [hive-beeline-3.1.3.jar:3.1.3]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_241]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_241]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_241]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_241]
	at org.apache.hive.beeline.ReflectiveCommandHandler.execute(ReflectiveCommandHandler.java:56) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.BeeLine.execCommandWithPrefix(BeeLine.java:1384) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.BeeLine.dispatch(BeeLine.java:1423) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.BeeLine.execute(BeeLine.java:1287) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.BeeLine.begin(BeeLine.java:1071) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.BeeLine.mainWithInputRedirection(BeeLine.java:538) [hive-beeline-3.1.3.jar:3.1.3]
	at org.apache.hive.beeline.BeeLine.main(BeeLine.java:520) [hive-beeline-3.1.3.jar:3.1.3]
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[?:1.8.0_241]
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62) ~[?:1.8.0_241]
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[?:1.8.0_241]
	at java.lang.reflect.Method.invoke(Method.java:498) ~[?:1.8.0_241]
	at org.apache.hadoop.util.RunJar.run(RunJar.java:323) [hadoop-common-3.3.0.jar:?]
	at org.apache.hadoop.util.RunJar.main(RunJar.java:236) [hadoop-common-3.3.0.jar:?]
Unknown HS2 problem when communicating with Thrift server.
Error: Error while cleaning up the server resources (state=,code=0)
Connection is already closed.

# 重新创建hiveserver2连接
hadoop@node2:/opt/module/hive-3.1.3$ bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
beeline> !connect jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Connecting to jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Enter username for jdbc:hive2://node1:2181,node2:2181,node3:2181/: hadoop
Enter password for jdbc:hive2://node1:2181,node2:2181,node3:2181/: 
23/05/20 20:25:43 [main]: INFO jdbc.HiveConnection: Connected to node3:10000
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
# hiveserver2存活状态
# +--------+--------+
# | node1  | node3  |
# +--------+--------+
# | n      | y      |
# +--------+--------+
# 可以正常查询 高可用测试完毕
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (1.996 seconds)
0: jdbc:hive2://node1:2181,node2:2181,node3:2> !quit
Closing: 0: jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
```

## 测试metastoreHA

```bash
hadoop@node2:/opt/module/hive-3.1.3$ bin/beeline
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/module/hive-3.1.3/lib/log4j-slf4j-impl-2.17.1.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Beeline version 3.1.3 by Apache Hive
beeline> !connect jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Connecting to jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
Enter username for jdbc:hive2://node1:2181,node2:2181,node3:2181/: hadoop
Enter password for jdbc:hive2://node1:2181,node2:2181,node3:2181/: 
23/05/20 20:21:09 [main]: INFO jdbc.HiveConnection: Connected to node1:10000
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
# metastore存活状态
# +--------+--------+
# | node1  | node3  |
# +--------+--------+
# | y      | n      |
# +--------+--------+
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (2.552 seconds)
# metastore存活状态
# +--------+--------+
# | node1  | node3  |
# +--------+--------+
# | n      | y      |
# +--------+--------+
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (1.399 seconds)
# metastore存活状态
# +--------+--------+
# | node1  | node3  |
# +--------+--------+
# | n      | y      |
# +--------+--------+
0: jdbc:hive2://node1:2181,node2:2181,node3:2> select * from stu;
+---------+-----------+
| stu.id  | stu.name  |
+---------+-----------+
| 1       | aa        |
| 2       | bb        |
+---------+-----------+
2 rows selected (0.205 seconds)
0: jdbc:hive2://node1:2181,node2:2181,node3:2> !quit
Closing: 0: jdbc:hive2://node1:2181,node2:2181,node3:2181/;serviceDiscoveryMode=zooKeeper;zooKeeperNamespace=hiveserver2_zk
```

# 参考

[解决Hive初始化元数据库报错](https://blog.csdn.net/qq_41918166/article/details/128748687)

[Hive3.1.2 shell 打印大量日志问题_hive 3.1.3启动打印大量日志](https://blog.csdn.net/young_0609/article/details/122620852)

[Linux安装MySQL8.0.30](https://blog.csdn.net/weixin_46985491/article/details/127179531)

[一文讲懂 Hive 高可用、HiveServer2 高可用及 Metastore 高可用](https://juejin.cn/post/6981353941581692936)
