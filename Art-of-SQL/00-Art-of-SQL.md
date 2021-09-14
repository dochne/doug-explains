# The Art of SQL

## Introduction

At the heart of almost any software project is data. There are few skills more useful than being able to efficiently pull the data out and manipulate it to your desires.

There are normally two bottlenecks that will slow your application down. These are either CPU (you're writing code that is asking the computer to do too much) or I/O (you're reading too much from the disk/network). From your applications perspective, 90% of the time you'll find that I/O is the bottleneck and these overwhelmingly take the form of requesting data from your database.

This is why learning how to efficiently structure, index and query your database is so important.

## Databases 101

The most important thing to remember about using your database is that actually querying the database is slow. If you ever find yourself in a for/while loop and querying a database, take a step back and think about what it is you're doing. Depending on the number of times you're looping, it may not matter, but it's a good indication that you may be doing something very wrong.

The second most important thing to remember about databases is that they're fast. Incredibly fast. Amazingly fast. The efficiency of what they're doing is far beyond whatever you'll achieve in your own code, as such the key to performance is making sure you're asking the database the *right* question.

## SQL - a quick primer

A lot of software developers learn SQL by hacking about to get what seems to work, so I'll quickly skim over the top level basics.

All databases that implement SQL loosely implement the [ISO/ANSI spec](https://webstore.ansi.org/standards/incits/ansix31351992r1998) (I love it when specs that the world is built on are put behind pay barriers, I'm looking at you ISO8601). As such, they all tend to have some base similarities, but once you start using fancier features (UPSERT/IF/JSON) you'll find that each database has it's own unique syntax and way of doing things.

This document should generally be sound for MySQL or Postgres (although I'll be mostly writing syntax for MySQL). Each of them has their own quirks however, and I would recommend you read the addendum page for your database of choice.

[Addendum - MySQL](/art-of-sql/AD-MySQL.md)
[Addendum - Postgres](/art-of-sql/AD-Postgres.md)

## Todo - what we ideally still need

- It'd be nice if we offered an automatically generated database that people could play with lots of data