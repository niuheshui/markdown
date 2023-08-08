hadoop profiles

core-site.xml
```xml
<property>
    <name>fs.defaultFS</name>
        <value>hdfs://node1:8020</value>
    </property>
    <property>
        <name>io.file.buffer.size</name>
        <value>131072</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/module/hadoop-3.3.0/data</value>
    </property>
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>hadoop</value>
    </property>
```

hdfs-site.xml
```xml
    <property>
        <name>dfs.namenode.http-address</name>
        <value>node1:9870</value>
    </property>

    <property>
            <name>dfs.namenode.secondary.http-address</name>
            <value>node3:9868</value>
    </property>
```

mapred-site.xml
```xml
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
```

yarn-site.xml
```xml
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
            
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>node2</value>
    </property>
    
    <property>
        <name>yarn.application.classpath</name>
        <value>/opt/module/hadoop-3.3.0/etc/hadoop:/opt/module/hadoop-3.3.0/share/hadoop/common/lib/*:/opt/module/hadoop-3.3.0/share/hadoop/common/*:/opt/module/hadoop-3.3.0/share/hadoop/hdfs:/opt/module/hadoop-3.3.0/share/hadoop/hdfs/lib/*:/opt/module/hadoop-3.3.0/share/hadoop/hdfs/*:/opt/module/hadoop-3.3.0/share/hadoop/mapreduce/*:/opt/module/hadoop-3.3.0/share/hadoop/yarn/lib/*:/opt/module/hadoop-3.3.0/share/hadoop/yarn/*
        </value>
    </property>
```