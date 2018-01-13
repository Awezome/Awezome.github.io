---
title: Hive && Hadoop FAQ
tags:
  - hadoop
  - hive
id: 2202
categories:
  - Hadoop
date: 2014-10-04 20:38:29
---

Error while run yarn task, "org.apache.hadoop.yarn.exceptions.InvalidAuxServiceException: The Service:mapreduce_shuffle does not exist"
A: Add following configurations to yarn-site.xml

`
<property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
</property>
<property>
    <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
`