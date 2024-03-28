---
title: "Cassandra Upsert Everything"
date: 2014-08-13 18:16:00 -0500
comments: true
categories:
 - Cassandra
 - Java
 - CQL
---
Cassandra inserts and updates should always be modeled as upserts when possible. Using the query builder in the Java native driver there isn't a direct upsert called out, but we can do updates instead of inserts for all cases. The update acts as an upsert and it reduces the number of queries you will need to build.

``` java
Statement upsert = QueryBuilder.update("table")
        .with(QueryBuilder.append("visits", new Date())) //Add to a CQL3 List
        .where(QueryBuilder.eq("id", "MyID"));
session.execute(upsert);
```

Above you can see how we model our "upsert". If a value isn't found for the given where clause it will insert it.

You must use all parts of a Primary Key for an updates where cluase given a CQL Table with a compound key:

```
create table tablex(
     pk1 varchar,
     pk2 varchar,
     colA varchar,
     PRIMARY KEY(pk1,pk2)
);
```
We can not do the following query:

``` java
Statement upsert = QueryBuilder.update("tablex")
                .with(QueryBuilder.set("colA", "2"))
                .where(QueryBuilder.eq("pk1", "1"));
```

You will get an `InvalidQueryException`:

```
com.datastax.driver.core.exceptions.InvalidQueryException: Missing mandatory PRIMARY KEY part pk2
	com.datastax.driver.core.exceptions.InvalidQueryException.copy(InvalidQueryException.java:35)
	com.datastax.driver.core.DefaultResultSetFuture.extractCauseFromExecutionException(DefaultResultSetFuture.java:256)
	com.datastax.driver.core.DefaultResultSetFuture.getUninterruptibly(DefaultResultSetFuture.java:172)
```

But the following will upsert:

``` java
Statement upsert = QueryBuilder.update("tablex")
        .with(QueryBuilder.set("colA", "2"))
        .where(QueryBuilder.eq("pk1", "1"))
        .and(QueryBuilder.eq("pk2", "2"));
```
