# Addendum: Postgres

## Encoding

Please please please please set your encoding to `utf8mb4`. The `utf8` encoding in MySQL is fundamentally broken but unable to be fixed upstream due to backwards compatibility guarantees.

It is *not* valid UTF-8 and it *will* bite you. Perhaps not today, perhaps not tomorrow but at some point you'll go "oh..." and fixing that mistake will take a disproportionately huge amount of effort.

## EXPLAIN...

Postgres not only has EXPLAIN, it also has ANALYZE! In fact, it has a whole bunch of them! `EXPLAIN (ANALYZE, COSTS, VERBOSE, BUFFERS, FORMAT JSON) SELECT...`

The important ones ahere are really `EXPLAIN` and `ANALYZE`, but if you get the full detailed dump then you can paste it into [this helpful](https://tatiyants.com/pev/) tool to get a more visual representation.
