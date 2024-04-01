---
author: Jeff Beck
title: Cassandra Not Available After Starting
excerpt:
categories:
  - NoSQL
  - Software Engineering
tags:
  - Cassandra
  - NoSQL
  - Software Engineering
post_format: [ ]
comments: true
date: 2012-12-28T10:25:45+0000
---
I’ve been working with the DataStax Enterprise 2.01 install for a bit now and it was working great until one day I was no longer able to get any queries to work using the cqlsh I was just getting the error that one or more nodes was unavailable. I tried restarting and still nothing would work I got a few errors in the logs (shown below).

I was able to quickly fix the error by removing my data directory and starting fresh as this is just my development environment that works great for me. You can find your data directory in the cassandra.yaml file ($DSE_HOME/resources/cassandra/conf/cassandra.yaml), look for the *data\_file\_directories* entry. Mine was set to */var/lib/cassandra/data* so I just ran the following and started cassandra fresh and everything is back to working order.

``` sh
rm -r /var/lib/cassandra/data
```


```
    INFO [JOB-TRACKER-INIT] 2012-12-28 10:32:32,515 JobTracker.java (line 2427) problem cleaning system directory: cfs:/tmp/hadoop-jeffbeck/mapred/system
    java.io.IOException: UnavailableException()
    	at com.datastax.bdp.hadoop.cfs.CassandraFileSystemThriftStore.listSubPaths(CassandraFileSystemThriftStore.java:1137)
    	at com.datastax.bdp.hadoop.cfs.CassandraFileSystem.listStatus(CassandraFileSystem.java:192)
    	at org.apache.hadoop.mapred.JobTracker.<init>(JobTracker.java:2392)
    	at org.apache.hadoop.mapred.JobTracker.<init>(JobTracker.java:2195)
    	at org.apache.hadoop.mapred.JobTracker.<init>(JobTracker.java:2189)
    	at org.apache.hadoop.mapred.JobTracker.startTracker(JobTracker.java:303)
    	at org.apache.hadoop.mapred.JobTracker.startTracker(JobTracker.java:294)
    	at org.apache.hadoop.mapred.HadoopTrackerPlugin$1.run(HadoopTrackerPlugin.java:230)
    	at java.lang.Thread.run(Thread.java:680)
    Caused by: UnavailableException()
    	at org.apache.cassandra.service.ReadCallback.assureSufficientLiveNodes(ReadCallback.java:212)
    	at org.apache.cassandra.service.StorageProxy.scan(StorageProxy.java:1083)
    	at org.apache.cassandra.thrift.CassandraServer.get_indexed_slices(CassandraServer.java:746)
    	at com.datastax.bdp.hadoop.cfs.CassandraFileSystemThriftStore.listSubPaths(CassandraFileSystemThriftStore.java:1120)
    	... 8 more
```
