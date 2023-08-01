# Hadoop HA (高可用) 集群搭建

## 1. 新建/opt/ha目录

```bash
hadoop@node1:~$ sudo mkdir /opt/ha; sudo chown hadoop:hadoop /opt/ha
```

```bash
hadoop@node2:~$ sudo mkdir /opt/ha; sudo chown hadoop:hadoop /opt/ha
```

```bash
hadoop@node3:~$ sudo mkdir /opt/ha; sudo chown hadoop:hadoop /opt/ha
```

## 2. 修改Hadoop配置文件

### core-site.xml

```xml
<configuration>
    <!-- 网页登录使用的静态用户 -->
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>hadoop</value>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://mycluster/</value>
    </property>
    <!-- 指定hadoop临时目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/ha/hadoop-3.3.0/data</value>
    </property>
    <!-- 指定zookeeper地址 -->
    <property>
        <name>ha.zookeeper.quorum</name>
        <value>node1:2181,node2:2181,node3:2181</value>
    </property>
</configuration>
```

### hdfs-site.xml

```xml
<configuration>
    <!-- nn数据存储目录  -->
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file://${hadoop.tmp.dir}/name</value>
    </property>
    <!-- dn数据存储目录  -->
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file://${hadoop.tmp.dir}/data</value>
    </property>
    <!-- JournalNode数据存储目录  -->
    <property>
        <name>dfs.journalnode.edits.dir</name>
        <value>${hadoop.tmp.dir}/jn</value>
    </property>
    <!-- 完全分布式集群名称  -->
    <property>
        <name>dfs.nameservices</name>
        <value>mycluster</value>
    </property>
    <!-- 集群中nn节点都有哪些  -->
    <property>
        <name>dfs.ha.namenodes.mycluster</name>
        <value>nn1,nn2,nn3</value>
    </property>
    <!-- nn的RPC通信地址  -->
    <property>
        <name>dfs.namenode.rpc-address.mycluster.nn1</name>
        <value>node1:8020</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.mycluster.nn2</name>
        <value>node2:8020</value>
    </property>
    <property>
        <name>dfs.namenode.rpc-address.mycluster.nn3</name>
        <value>node3:8020</value>
    </property>
    <!-- nn的HTTP通信地址  -->
    <property>
        <name>dfs.namenode.http-address.mycluster.nn1</name>
        <value>node1:9870</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.mycluster.nn2</name>
        <value>node2:9870</value>
    </property>
    <property>
        <name>dfs.namenode.http-address.mycluster.nn3</name>
        <value>node3:9870</value>
    </property>
    <!-- 指定nn元数据在JournalNode上的存放位置  -->
    <property>
        <name>dfs.namenode.shared.edits.dir</name>
        <value>qjournal://node1:8485;node2:8485;node3:8485/mycluster</value>
    </property>
    <!-- 访问代理类:client用于确定哪个nn为Active  -->
    <property>
        <name>dfs.client.failover.proxy.provider.mycluster</name>
<value>org.apache.hadoop.hdfs.server.namenode.ha.ConfiguredFailoverProxyProvider</value>
    </property>
    <!-- 配置隔离机制 同一时刻只能有一台服务器对外响应  -->
    <property>
        <name>dfs.ha.fencing.methods</name>
        <value>sshfence</value>
        <!-- <value>shell(true)</value> -->
    </property>
    <!-- 使用隔离机制是需要使用ssh密钥登录  -->
    <property>
        <name>dfs.ha.fencing.ssh.private-key-files</name>
        <value>/home/hadoop/.ssh/id_rsa</value>
    </property>
    <!-- 启用nn自动故障转移  -->
    <property>
        <name>dfs.ha.automatic-failover.enabled</name>
        <value>true</value>
    </property>
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>
</configuration>
```

### mapred-site.xml

```xml
<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
	<property>
		<name>mapreduce.jobhistory.address</name>
		<value>node1:10020</value>
	</property>
	<property>
		<name>mapreduce.jobhistory.webapp.address</name>
		<value>node1:19888</value>
	</property>
</configuration>
```

### yarn-site.xml

```xml
<configuration>
    <!-- 开启RM高可用 -->
    <property>
        <name>yarn.resourcemanager.ha.enabled</name>
        <value>true</value>
    </property>
    <!-- 指定RM的cluster id -->
    <property>
        <name>yarn.resourcemanager.cluster-id</name>
        <value>cluster-yarn1</value>
    </property>
    <!-- 指定RM的名字 -->
    <property>
        <name>yarn.resourcemanager.ha.rm-ids</name>
        <value>rm1,rm2,rm3</value>
    </property>
    <!-- ============rm1============ -->
    <!-- RM1的地址 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm1</name>
        <value>node1</value>
    </property>
    <!-- web访问地址 -->
    <property>
        <name>yarn.resourcemanager.webapp.address.rm1</name>
        <value>node1:8088</value>
    </property>
    <!-- rm1内部通信地址 -->
    <property>
        <name>yarn.resourcemanager.address.rm1</name>
        <value>node1:8032</value>
    </property>
    <!-- 指定AM向rm1申请资源的地址 -->
    <property>
        <name>yarn.resourcemanager.scheduler.address.rm1</name>
        <value>node1:8030</value>
    </property>
    <!-- 指定NM连接的地址 -->
    <property>
        <name>yarn.resourcemanager.resource-tracker.address.rm1</name>
        <value>node1:8031</value>
    </property>
    <!-- ============rm2============ -->
    <!-- RM21的地址 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm2</name>
        <value>node2</value>
    </property>
    <!-- web访问地址 -->
    <property>
        <name>yarn.resourcemanager.webapp.address.rm2</name>
        <value>node2:8088</value>
    </property>
    <!-- rm2内部通信地址 -->
    <property>
        <name>yarn.resourcemanager.address.rm2</name>
        <value>node2:8032</value>
    </property>
    <!-- 指定AM向rm2申请资源的地址 -->
    <property>
        <name>yarn.resourcemanager.scheduler.address.rm2</name>
        <value>node2:8030</value>
    </property>
    <!-- 指定NM连接的地址 -->
    <property>
        <name>yarn.resourcemanager.resource-tracker.address.rm2</name>
        <value>node2:8031</value>
    </property>
    <!-- ============rm3============ -->
    <!-- RM3的地址 -->
    <property>
        <name>yarn.resourcemanager.hostname.rm3</name>
        <value>node3</value>
    </property>
    <!-- web访问地址 -->
    <property>
        <name>yarn.resourcemanager.webapp.address.rm3</name>
        <value>node3:8088</value>
    </property>
    <!-- rm3内部通信地址 -->
    <property>
        <name>yarn.resourcemanager.address.rm3</name>
        <value>node3:8032</value>
    </property>
    <!-- 指定AM向rm3申请资源的地址 -->
    <property>
        <name>yarn.resourcemanager.scheduler.address.rm3</name>
        <value>node3:8030</value>
    </property>
    <!-- 指定NM连接的地址 -->
    <property>
        <name>yarn.resourcemanager.resource-tracker.address.rm3</name>
        <value>node3:8031</value>
    </property>
    <!-- 指定zk集群地址 -->
    <property>
        <name>yarn.resourcemanager.zk-address</name>
        <value>node1:2181,node2:2181,node3:2181</value>
    </property>

    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <!--
<property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
</property>

<property>
    <name>yarn.log-aggregation.retain-seconds</name>
    <value>86400</value>
</property>
-->
    <!-- 启用自动恢复 -->
    <property>
        <name>yarn.resourcemanager.recovery.enabled</name>
        <value>true</value>
    </property>

    <!-- 制定resourcemanager的状态信息存储在zookeeper集群上 -->
    <property>
        <name>yarn.resourcemanager.store.class</name>
        <value>org.apache.hadoop.yarn.server.resourcemanager.recovery.ZKRMStateStore</value>
    </property>
    <!-- 环境变量继承 -->

    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
    <property>
        <name>yarn.application.classpath</name>
        <value>/opt/ha/hadoop-3.3.0/etc/hadoop:/opt/ha/hadoop-3.3.0/share/hadoop/common/lib/*:/opt/ha/hadoop-3.3.0/share/hadoop/common/*:/opt/ha/hadoop-3.3.0/share/hadoop/hdfs:/opt/ha/hadoop-3.3.0/share/hadoop/hdfs/lib/*:/opt/ha/hadoop-3.3.0/share/hadoop/hdfs/*:/opt/ha/hadoop-3.3.0/share/hadoop/mapreduce/*:/opt/ha/hadoop-3.3.0/share/hadoop/yarn/lib/*:/opt/ha/hadoop-3.3.0/share/hadoop/yarn/*
        </value>
    </property>
</configuration>
```

### 分发

```bash
hadoop@node1:~/bin$ ./xsync /opt/ha/hadoop-3.3.0/
```

## 3.启动zookeeper集群

```bash
hadoop@node1:~$ zkServer.sh start
```

```bash
hadoop@node2:~$ zkServer.sh start
```

```bash
hadoop@node3:~$ zkServer.sh start
```

## 4.格式化namenode, zkfc

### 启动journalnode

```bash
hadoop@node1:~$ hdfs --daemon start journalnode
```
```bash
hadoop@node2:~$ hdfs --daemon start journalnode
```

```bash
hadoop@node3:~$ hdfs --daemon start journalnode
```

### 启动namenode

```bash
hadoop@node1:~$ hdfs namenode
```

### 同步namenode

```bash
hadoop@node2:~$ hdfs namenode -bootstrapStandby
```

```bash
hadoop@node3:~$ hdfs namenode -bootstrapStandby
```

### 格式化zkfc

```bash
hadoop@node1:~$ hdfs zkfc -formatZK
```

## 5. 启动

### 启动hdfs

```bash
hadoop@node1:~$ start-dfs.sh
hadoop@node1:~/bin$ ./jpsall 
========== node1 ==========
8704 DFSZKFailoverController
8464 JournalNode
8818 Jps
8085 NameNode
8234 DataNode
3564 QuorumPeerMain
========== node2 ==========
8113 DFSZKFailoverController
7941 JournalNode
8199 Jps
7690 NameNode
3243 QuorumPeerMain
7807 DataNode
========== node3 ==========
6339 DataNode
6741 Jps
6219 NameNode
6652 DFSZKFailoverController
6476 JournalNode
3054 QuorumPeerMain
```

### 启动yarn

```bash
hadoop@node1:~$ start-yarn.sh
hadoop@node1:~/bin$ ./jpsall 
========== node1 ==========
8704 DFSZKFailoverController
8464 JournalNode
9700 Jps
8085 NameNode
9129 ResourceManager
8234 DataNode
3564 QuorumPeerMain
9278 NodeManager
========== node2 ==========
8401 NodeManager
8113 DFSZKFailoverController
7941 JournalNode
8294 ResourceManager
8618 Jps
7690 NameNode
3243 QuorumPeerMain
7807 DataNode
========== node3 ==========
7168 Jps
6962 NodeManager
6339 DataNode
6839 ResourceManager
6219 NameNode
6652 DFSZKFailoverController
6476 JournalNode
3054 QuorumPeerMain
```

### 启动mapreduce任务历史服务器

```bash
hadoop@node1:~/bin$ mapred --daemon start historyserver
hadoop@node1:~/bin$ jps
8704 DFSZKFailoverController
8464 JournalNode
9811 JobHistoryServer
8085 NameNode
9129 ResourceManager
8234 DataNode
3564 QuorumPeerMain
9869 Jps
9278 NodeManager
```



## 6. 测试

### namenode主机状态

```bash
hadoop@node1:~$ hdfs haadmin -getServiceState nn1
active
hadoop@node1:~$ hdfs haadmin -getServiceState nn2
standby
hadoop@node1:~$ hdfs haadmin -getServiceState nn3
standby
```

### 在active的node1节点上, kill掉namenode进程

```bash
hadoop@node1:~$ jps
8704 DFSZKFailoverController
8464 JournalNode
9811 JobHistoryServer
8085 NameNode
10184 Jps
9129 ResourceManager
8234 DataNode
3564 QuorumPeerMain
9278 NodeManager
hadoop@node1:~$ kill -9 8085
hadoop@node1:~$ hdfs haadmin -getServiceState nn2
active
hadoop@node1:~$ hdfs haadmin -getServiceState nn3
standby
```

### mapreduce任务

```bash
hadoop@node1:/opt/ha/hadoop-3.3.0/share/hadoop/mapreduce$ hadoop jar hadoop-mapreduce-examples-3.3.0.jar wordcount /11.py /out
2023-05-07 00:35:00,929 INFO retry.RetryInvocationHandler: java.net.ConnectException: Call From node1/192.168.88.111 to node2:8020 failed on connection exception: java.net.ConnectException: 拒绝连接; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over node2/192.168.88.112:8020 after 1 failover attempts. Trying to failover after sleeping for 737ms.
2023-05-07 00:35:01,967 INFO mapreduce.JobResourceUploader: Disabling Erasure Coding for path: /tmp/hadoop-yarn/staging/hadoop/.staging/job_1683444630363_0001
2023-05-07 00:35:02,631 INFO input.FileInputFormat: Total input files to process : 1
2023-05-07 00:35:02,792 INFO mapreduce.JobSubmitter: number of splits:1
2023-05-07 00:35:03,262 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1683444630363_0001
2023-05-07 00:35:03,263 INFO mapreduce.JobSubmitter: Executing with tokens: []
2023-05-07 00:35:03,469 INFO retry.RetryInvocationHandler: java.net.ConnectException: Call From node1/192.168.88.111 to node2:8020 failed on connection exception: java.net.ConnectException: 拒绝连接; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over node2/192.168.88.112:8020 after 1 failover attempts. Trying to failover after sleeping for 1007ms.
2023-05-07 00:35:04,593 INFO retry.RetryInvocationHandler: java.net.ConnectException: Call From node1/192.168.88.111 to node2:8020 failed on connection exception: java.net.ConnectException: 拒绝连接; For more details see:  http://wiki.apache.org/hadoop/ConnectionRefused, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over node2/192.168.88.112:8020 after 1 failover attempts. Trying to failover after sleeping for 1097ms.
2023-05-07 00:35:05,748 INFO conf.Configuration: resource-types.xml not found
2023-05-07 00:35:05,748 INFO resource.ResourceUtils: Unable to find 'resource-types.xml'.
2023-05-07 00:35:06,148 INFO impl.YarnClientImpl: Submitted application application_1683444630363_0001
2023-05-07 00:35:06,185 INFO mapreduce.Job: The url to track the job: http://node1:8088/proxy/application_1683444630363_0001/
2023-05-07 00:35:06,186 INFO mapreduce.Job: Running job: job_1683444630363_0001
2023-05-07 00:35:14,303 INFO mapreduce.Job: Job job_1683444630363_0001 running in uber mode : false
2023-05-07 00:35:14,304 INFO mapreduce.Job:  map 0% reduce 0%
2023-05-07 00:35:26,381 INFO mapreduce.Job:  map 100% reduce 0%
2023-05-07 00:35:34,418 INFO mapreduce.Job:  map 100% reduce 100%
2023-05-07 00:35:34,424 INFO mapreduce.Job: Job job_1683444630363_0001 completed successfully
2023-05-07 00:35:34,543 INFO mapreduce.Job: Counters: 54
	File System Counters
		FILE: Number of bytes read=4184
		FILE: Number of bytes written=547581
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=4226
		HDFS: Number of bytes written=4146
		HDFS: Number of read operations=8
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=2
		HDFS: Number of bytes read erasure-coded=0
	Job Counters 
		Launched map tasks=1
		Launched reduce tasks=1
		Data-local map tasks=1
		Total time spent by all maps in occupied slots (ms)=8735
		Total time spent by all reduces in occupied slots (ms)=5312
		Total time spent by all map tasks (ms)=8735
		Total time spent by all reduce tasks (ms)=5312
		Total vcore-milliseconds taken by all map tasks=8735
		Total vcore-milliseconds taken by all reduce tasks=5312
		Total megabyte-milliseconds taken by all map tasks=8944640
		Total megabyte-milliseconds taken by all reduce tasks=5439488
	Map-Reduce Framework
		Map input records=3
		Map output records=9
		Map output bytes=4174
		Map output materialized bytes=4184
		Input split bytes=87
		Combine input records=9
		Combine output records=7
		Reduce input groups=7
		Reduce shuffle bytes=4184
		Reduce input records=7
		Reduce output records=7
		Spilled Records=14
		Shuffled Maps =1
		Failed Shuffles=0
		Merged Map outputs=1
		GC time elapsed (ms)=344
		CPU time spent (ms)=1060
		Physical memory (bytes) snapshot=572313600
		Virtual memory (bytes) snapshot=5214117888
		Total committed heap usage (bytes)=437256192
		Peak Map Physical memory (bytes)=351870976
		Peak Map Virtual memory (bytes)=2607788032
		Peak Reduce Physical memory (bytes)=220442624
		Peak Reduce Virtual memory (bytes)=2606329856
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=4139
	File Output Format Counters 
		Bytes Written=4146
```

## 7. Hbase + HadoopHA

### 修改hbase-site.xml

```xml
<configuration>
    <property>
        <name>hbase.cluster.distributed</name>
        <value>true</value>
    </property>
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>node1,node2,node3</value>
    </property>
    <property>
        <name>hbase.rootdir</name>
        <value>hdfs://mycluster/hbase</value>
    </property>
</configuration>                
```

### 创建软链接

```bash
hadoop@node1:~$ ln -s $HADOOP_HOME/etc/hadoop/core-site.xml $HBASE_HOME/conf/core-site.xml
hadoop@node1:~$ ln -s $HADOOP_HOME/etc/hadoop/hdfs-site.xml $HBASE_HOME/conf/hdfs-site.xml
```

### 分发

```bash
hadoop@node1:~/bin$ ./xsync /opt/module/hbase-2.4.11/conf/
```

### 启动

```bash
hadoop@node1:~$ start-hbase.sh 
running master, logging to /opt/module/hbase-2.4.11/logs/hbase-hadoop-master-node1.out
node2: running regionserver, logging to /opt/module/hbase-2.4.11/bin/../logs/hbase-hadoop-regionserver-node2.out
node3: running regionserver, logging to /opt/module/hbase-2.4.11/bin/../logs/hbase-hadoop-regionserver-node3.out
node1: running regionserver, logging to /opt/module/hbase-2.4.11/bin/../logs/hbase-hadoop-regionserver-node1.out
node3: running master, logging to /opt/module/hbase-2.4.11/bin/../logs/hbase-hadoop-master-node3.out
node2: running master, logging to /opt/module/hbase-2.4.11/bin/../logs/hbase-hadoop-master-node2.out
hadoop@node1:~$ bin/jpsall 
========== node1 ==========
8704 DFSZKFailoverController
8464 JournalNode
9811 JobHistoryServer
11174 HRegionServer
10408 NameNode
9129 ResourceManager
8234 DataNode
10987 HMaster
11564 Jps
3564 QuorumPeerMain
9278 NodeManager
========== node2 ==========
8401 NodeManager
8113 DFSZKFailoverController
9460 HMaster
7941 JournalNode
8294 ResourceManager
9304 HRegionServer
7690 NameNode
3243 QuorumPeerMain
7807 DataNode
9775 Jps
========== node3 ==========
6962 NodeManager
6339 DataNode
8196 Jps
6839 ResourceManager
6219 NameNode
6652 DFSZKFailoverController
7724 HRegionServer
7884 HMaster
6476 JournalNode
3054 QuorumPeerMain
```

## 8. 集群关闭

```bash
hadoop@node1:~$ stop-hbase.sh 
stopping hbase........................
hadoop@node1:~$ mapred --daemon stop historyserver
hadoop@node1:~$ stop-all.sh 
WARNING: Stopping all Apache Hadoop daemons as hadoop in 10 seconds.
WARNING: Use CTRL-C to abort.
Stopping namenodes on [node1 node2 node3]
Stopping datanodes
Stopping journal nodes [node2 node3 node1]
Stopping ZK Failover Controllers on NN hosts [node1 node2 node3]
Stopping nodemanagers
Stopping resourcemanagers on [ node1 node2 node3]
hadoop@node1:~$ bin/zk.sh stop
---------- zookeeper node1 stop ----------
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper-3.5.7/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
---------- zookeeper node2 stop ----------
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper-3.5.7/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
---------- zookeeper node3 stop ----------
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper-3.5.7/bin/../conf/zoo.cfg
Stopping zookeeper ... STOPPED
hadoop@node1:~$ bin/jpsall 
========== node1 ==========
16258 Jps
========== node2 ==========
12176 Jps
========== node3 ==========
11111 Jps
```

## 9. 脚本

### xsync

```bash
#!/bin/bash

#1.判断参数个数
if [ $# -lt 1 ]
then
    echo "Not Enough Arguement!"
    exit;
fi

#2.遍历集群所有机器
for host in node1 node2 node3
do
    echo ==================== $host ====================
    #3.遍历所有目录 挨个发送
    for file in $@
    do
    	#4.判断文件是否存在
    	if [ -e $file ]
    	then
        	#5.获取父目录
	    	pdir=$(cd -P $(dirname $file); pwd)			
       		#6.获取当前文件的名称
       		fname=$(basename $file)
       		ssh $host "mkdir -p $pdir"
       		rsync -av $pdir/$fname $host:$pdir
   		else
       		echo $file does not exists!
        fi
    done
done
```

### jpsall

```bash
#!/bin/bash

for host in node1 node2 node3
do
    echo ========== $host ==========
    ssh $host "$JAVA_HOME/bin/jps"
done
```

### zk.sh

```bash
#!/bin/bash
for i in node1 node2 node3
do
    echo ---------- zookeeper $i $1 ----------
    ssh $i "source /etc/profile; /opt/module/zookeeper-3.5.7/bin/zkServer.sh $1"
done
```
