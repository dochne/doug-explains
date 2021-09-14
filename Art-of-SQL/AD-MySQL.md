# Addendum: MySQL

## Encoding

Please please please please set your encoding to `utf8mb4`. The `utf8` encoding in MySQL is fundamentally broken but unable to be fixed upstream due to backwards compatibility guarantees.

It is *not* valid UTF-8 and it *will* bite you. Perhaps not today, perhaps not tomorrow but at some point you'll go "oh..." and fixing that mistake will take a disproportionately huge amount of effort.

## EXPLAIN

If you're not sure why a query is taking so long, you can prefix it with `EXPLAIN`. (e.g. `EXPLAIN SELECT * from...`). This will give you some details about where the query spent it's time. It's indispensible for truly understanding queries
