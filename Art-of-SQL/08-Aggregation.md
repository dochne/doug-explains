
## Aggregation, Joins, CTEs, Subqueries and the misuse of LIMIT


### Aggregation

One of the things that is particularly fantastic about relational databases is the way it allows you to craft queries and aggregate the data in a useful way.

By adding a `GROUP BY` clause, we transform the query we're running from a data fetcher to a data aggregator. There's a few performance gotchas here, a couple of neat tricks and an issue that consistently occurs
that shows you haven't modelled your query right.

#### Performance

Any query in a GROUP BY is still limited by the performance constraints you'd expect from



#### "Full GROUP BY" - or why your query doesn't make sense


### Query 5 - on the topic of aggregations



### Query 6 - subqueries, CTEs and the art of efficient joins

For Alex:
What is a SubQuery
What is a CTE?
What is the difference between HAVING and WHERE?

### Query 7 - Unbuffered queries



## Indexes

A database index operates in much the same manner as an index of a book.

## Anatomy of a query

Dealing with databases becomes much more intuitive once you start thinking about what the database is


## Indexing

### Aggregated Queries and the full join


