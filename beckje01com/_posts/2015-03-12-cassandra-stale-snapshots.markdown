---
title: Cassandra Stale Snapshots
date: 2015-03-12 09:47:19.000000000 -05:00
comments: true
published: true
categories:
- Cassandra
---
While working with Cassandra we found that sometimes snapshots get left around after repairs fail or have issues. These can cause 100's of Gigs of space to just be wasted. This can add up quickly and cause issues. On a node with low disk space we can check for stale snapshots the following way.

```sh
$ cd /cassandaData/cassandra/data/keyspace/
$ du -h
```

If you see large or multiple directories under columnFamily/snapshots/ that may indicate there are stale snapshots that can be cleaned up.
We will want to check when these snapshots where created so inside the snapshots directory of the column family run

```sh
$ ls -lah
total 3.3M
drwxr-xr-x 7 cassandra cassandra 4.0K Mar 11 16:14 .
drwxr-xr-x 3 cassandra cassandra 704K Mar 11 16:31 ..
drwxr-xr-x 2 cassandra cassandra 508K Mar 01 14:24 4f37dcd0-c7fa-11e4-b5ae-5f969a9b23c8
drwxr-xr-x 2 cassandra cassandra 504K Mar 11 16:14 ad70a8e0-c809-11e4-9b55-39152d07d3bf
drwxr-xr-x 2 cassandra cassandra 532K Mar 11 16:03 b5cfd3a0-c807-11e4-bcd5-db76d671c3d5
drwxr-xr-x 2 cassandra cassandra 504K Mar 10 22:50 cfe34050-c777-11e4-b5ae-5f969a9b23c8
drwxr-xr-x 2 cassandra cassandra 536K Mar 11 16:09 edc03dd0-c808-11e4-be46-35521ca10087
```

Each of those are snapshots but you will notice 4f37dcd0-c7fa-11e4-b5ae-5f969a9b23c8 is stale, we can now clean that up with the [nodetool clearsnapshot](http://www.datastax.com/documentation/cassandra/2.1/cassandra/tools/toolsClearSnapShot.html) command.
```
nodetool clearsnapshot keyspace -t 4f37dcd0-c7fa-11e4-b5ae-5f969a9b23c8
```

That will take care of removing the stale snapshot you should see disk space recovered immediately.

You can also look in the logs for failed repairs we see something like the following:

```
2015-02-20 23:37:52,220 RepairSession.java (line 288) [repair #72e69720-b959-11e4-9b55-39152d07d3bf] session completed with the following error
system.log.10-java.io.IOException: Failed during snapshot creation.
system.log.10- at org.apache.cassandra.repair.RepairSession.failedSnapshot(RepairSession.java:323)
system.log.10- at org.apache.cassandra.repair.RepairJob$2.onFailure(RepairJob.java:126)
system.log.10- at com.google.common.util.concurrent.Futures$4.run(Futures.java:1160)
system.log.10- at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
system.log.10- at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
system.log.10- at java.lang.Thread.run(Thread.java:745)
```

In that case the bad snapshot would be _72e69720-b959-11e4-9b55-39152d07d3bf_.
