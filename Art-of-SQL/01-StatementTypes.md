# Types of statement

SQL databases work by you sending a "statement" to a server, which you hope will return a set of data as well as some metadata. If you're less lucky, it'll return an error.

These tend to look like "SELECT * FROM ..." or similar.

There are 4 different types of statement:

- DDL (Data Definition Language) - statements that will let you alter the structure of a database - for example, creating a new Table/Index or adding a column to an existing table
- DML (Data Manipulation Language) - statements that will alter the data inside the database - for example retrieving data or inserting/deleting data
- TCL (Transaction Control Language) - statements that are concerned with the initiation/completion/termination of transactions
- DCL (Data Control Language) - statements that are to do with configuring user permissions

Realistically, you tend to only care about the first three. The separation is important because the pitfalls of using each is different. Additionally, depending on your database you'll find the transactions may or may not effect DDL statements, but we'll get to that later.

## Todo: Anatomy of the different statements
- we want to cover 
