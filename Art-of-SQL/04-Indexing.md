# Querying and indexing - the basics

Using databases becomes much easier once you get a firm grasp of what the database is actually having to do to get what you're after.

## Query 1 - the full table scan

```
CREATE TABLE `BasicTable` (
  `name` varchar(255) NOT NULL,
  `address` varchar(255) NOT NULL
)
```

In this example table, we're ignoring indexes.
Let's say there are 1,000,000 entries in this table and we want to search for where "Mr Stevens" lives. We might do this with a query that looks like:

```
SELECT address FROM BasicTable WHERE name='Mr Stevens'
```

As there's no indexes on this table, there's only one way for our poor database to get the data - it has to manually read every single row and see if name matches Mr Stevens. This is called a full table scan, and from our perspective as manipulators of the database - it's the worst case scenario. Our query will take a long time to come back and the user will have a miserable experience!

From an algorithmic perspective, this query would be referred to as `O(n)`. `O` `n` here would be 1,000,000. This is a way we can approximate the relative efficiency of queries. The function `O` would need to be run 1,000,00 times.

## Query 2 - the index will save us?

```
CREATE TABLE `BasicTable` (
  `name` varchar(255) NOT NULL,
  `address` varchar(255) NOT NULL,
  KEY `name` (`name`)
)
```

Thankfully, the creators of databases thought about how we could do this more efficiently, drawing from the experience of... books I guess, they created indexes.

An index takes the form of a binary tree (there are other types, but this is what you'll normally use). If you can't remember (or didn't learn) how they work, it doesn't really matter, the important thing to know is that it allows us to take a value (say, "Mr Stevens") and store it in such a way that we can find it, and it's matching index position very quickly.

This time, our basic table has an index on name. This means that when we ask the database the exact same query:

```
SELECT address FROM BasicTable WHERE name='Mr Stevens'
```

The database can first lookup Mr Stevens in the index and discover that it's in row 255,343 in the table. Now instead of scanning the entire table, we can jump to the relevant row, read it and return! This query would be `O(1)`. A hypothetical million times faster! Amazing!

## Caution 1 - omg, can I index everything then?

Sorry, there's no such thing as a free lunch and everything has trade-offs. Creating indexes is a trade off between read speed and write speed.

Every index you create requires you to keep... well, an index on disk. For every index you create, a INSERT will have to make *another* write to disk, potentially slowing down your write speed.

Additionally, with more complicated indexes and indexes on larger tables, these indexes can take up a lot of space on disk. With enough indexes, you can end up with more space being taken up by indexes than the data itself.

## Query 3 - can indexes help me when I'm querying by multiple things?

Why yes! Yes they can!

An index doesn't have to be on a single column. Here, let my magical third table demonstrate! (I apologise for the very contrived example)

```
CREATE TABLE `BasicTable` (
  `age` bigint NOT NULL,
  `gender` varchar(1) NOT NULL,
  `town ` varchar(100) NOT NULL,
  `name` varchar(500) NOT NULL
)
```

We'll pretend this has 10,000,000 rows

Take the query: `SELECT name FROM BasicTable WHERE age=50 AND gender='M' AND town ='Nottingham'`

In the non-indexed form, it would have to table scan through 10,000,000 rows to get the data we wish.
If we indexed just `gender`, we'd be scanning approx 5,000,000 rows
If we indexed (`gender`, `age`) around 50,000 rows
If we indexed (`gender`, `age `, `town `) around 300 rows

The power of multi-indexes is not just that it can allow us to vastly speed up specific queries that use all the indexes, it also provides us with an index we can use for more wide queries, for example:

(`gender`, `age`, `city`) would still be useful when doing the query `SELECT count(*) FROM BasicTable WHERE gender='M')`

*HOWEVER*

There are two important things to note here!

1. Queries can only use an index if you fulfill filtering on everything to the left of what you're asking about!

That isn't amazingly clear, so let's take the index `gender`, `age`, `town`

As noted above, `SELECT count(*) FROM BasicTable WHERE gender='M'` can use this index, but if you were to do `SELECT count(*) FROM BasicTable WHERE age='50'` you would not expect it to be able to use the index as it
isn't providing a value for `gender`. (Caveat to the caveat - *expect it to*. Databases are smart, see Databases 101 - it might in this instance, but that would be due to cardinality... see point 2!)

2. If you're creating multi-indexes, they should be ordered by ascending cardinality. Cardinality is the word we use to describe the number of items in a set. So, if we had `gender`, you would expect this to have a cardinality of 2 (well, under 10 anyway :P)

As gender will have the lowest cardinality (<10), followed by age (<120) followed by town (<10,000) they should be put in this order. This generally makes filtering more efficient for the database.

An excellent example of where this kind of paying attention to cardinality is key is if you're dealing with time related data. Let's say I worked for a company that did heartbeat monitoring software and kept track of users heartbeat every minute.

```
CREATE TABLE `HeartBeat` (
  `user_id` bigint NOT NULL,
  `timestamp` bigint NOT NULL,
  `bpm` int NOT NULL,
)
```

I might want to know the bpm for user 5 for all of yesterday. Unfortunately I used millisecond unix timestamps, so everything is even more of a pain :D

`SELECT timestamp, bpm FROM HeartBeat WHERE user_id=5 AND timestamp BETWEEN 1628684722000 AND 1628771122000`

If I indexed this database (`timestamp`, `user_id`) the cardinality of `timestamp` would be so high that it would be almost useless.

Think about it like a filling cabinet. In the ideal scenario you can go to the filing cabinet, find the file with your name on it, skip through the files until you find the correct starting time and then read until you find the ending time.

If the cardinality is the wrong way round, you instead look through the filing cabinet for the right times, but you have to read every row of data between those dates and check whether it was for you or not.
