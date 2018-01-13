---
title: Hadoop与Hive的单机配置
tags:
  - hadoop
  - hive
id: 2194
categories:
  - Hadoop
date: 2014-10-04 10:32:49
---

**Download**
wget http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-2.5.1/hadoop-2.5.1.tar.gz
wget http://mirrors.hust.edu.cn/apache/hive/hive-0.13.1/apache-hive-0.13.1-bin.tar.gz

**Java**
yum install java java-1.7.0-openjdk-devel 
export JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.25.x86_64

**Hadoop**
解压hadoop并进入
编辑conf/hadoop-env.sh，whereis 查看java，不用包含bin
`export JAVA_HOME=/usr`

修改etc/hadoop/core-site.xml：
`
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
    <description>The name of the default file system.</description>
  </property>

  <property>
    <name>hadoop.tmp.dir</name>
    <value>/home/hadoop/tmp/hadoop</value>
    <description>A base for other temporary directories.</description>
  </property>

  <property>
    <name>io.native.lib.available</name>
    <value>false</value>
    <description>default value is true:Should native hadoop libraries, if present, be used.</description>
  </property>
</configuration>
`

修改etc/hadoop/hdfs-site.xml：
`
<configuration>
<property>  
  <name>dfs.namenode.name.dir</name>  
  <value>file:/home/hadoop/dfs/name</value>  
</property>  
<property>  
  <name>dfs.namenode.data.dir</name>  
  <value>file:/home/hadoop/dfs/data</value>  
</property>
 <property>
  <name>dfs.replication</name>
  <value>1</value>
 </property>
<configuration>
`

修改etc/hadoop/mapred-site.xml：
`
<configuration>
 <property>
  <name>mapred.job.tracker</name>
  <value>localhost:9001</value>
 </property>
</configuration>`

初始化 ./bin/hadoop namenode –format<!--more-->

启动
sbin/start-dfs.sh
sbin/start-yarn.sh
jps命令检查运行的程序有哪些
4287 DataNode
4543 TaskTracker
4192 NameNode
4444 JobTracker
4380 SecondaryNameNode

**Hive**
#关闭hadoop安全模式 ./bin/hadoop dfsadmin -safemode leave
环境变量
`echo "export HADOOP_HOME=$PWD" >> /etc/profile.d/hadoop.sh
echo "PATH=$PATH:$HADOOP_HOME/bin" >> /etc/profile.d/hadoop.sh
. /etc/profile
echo $HADOOP_HOME`

建立hive要用的目录
`./bin/hadoop fs -mkdir /tmp
./bin/hadoop fs -mkdir /user/hive/warehouse
./bin/hadoop fs -chmod g+w /tmp
./bin/hadoop fs -chmod g+w /user/hive/warehouse`

解压Hive进入
运行 ./bin/hive
最后加个命令吧
ln -s /home/vagrant/apache-hive-0.13.1-bin/bin/hive /usr/bin

**More**
另外，可以自行下载已经配置好的box
`http://hortonworks.com/products/hortonworks-sandbox/
http://www.cloudera.com/content/cloudera/en/downloads/quickstart_vms/cdh-5-1-x1.html`