## Database 102 - how can I make my database do a zoom?

If you want a fast database query, it's all about making your database do the least work possible to get the results you need. Simple!

In rough order, these are what your database will find "hard"

1. finding a record without an index
2. reading data from disk
3. finding a record with an index

Let's look at some examples where we have some working code, but we can do better. We'll be using this database for the time being:

```
CREATE TABLE `Author` (
  `id` bigint NOT NULL,
  `name` varchar(500) NOT NULL
)

CREATE TABLE `Book` (
  `id` bigint NOT NULL,
  `author_id` bigint NOT NULL,
  `name` varchar(500) NOT NULL,
  `isbn` varchar(10) NOT NULL,
  `release_date` bigint NOT NULL
)
```

We're going to pretend that Amazon looks something like this, and there's 50,000,000 authors and 500,000,000 books.

### "finding a record without an index"

The simplest to resolve in its way - index things. In the example above `Author`.`id`, `Book`.`id` and `Book`.`author_id` should all be indexed.

In more complicated examples, whether you *can* realistically index something is more difficult - it's important to understand the access patterns of the Table when designing it.

### "reading data from disk"

`SELECT * FROM book INNER JOIN author ON (book.author_id=author.id) ORDER BY release_date DESC LIMIT 0, 50`

### Join at the right time


`SELECT * FROM book INNER JOIN author ON (book.author_id=author.id) ORDER BY release_date DESC LIMIT 0, 50`
