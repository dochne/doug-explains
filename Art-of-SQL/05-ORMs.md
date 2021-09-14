
## Falsehoods programmers believe about using ORMs

### Quick terminology intro!

Quick explainer:

*Predicate* - this describes a condition that we are enforcing upon the data. `table.column = 5` would be a predicate.
*Hydration* - this is when we take some data (say, id, name) from a database row and turn it into an easy-to-use object for our application

### On queries and hydration

Developers on the whole don't like to do the same work twice. They like to abstract problems to an interface and then not have to worry about what is happening underneath.

The first time that people come across a database table where they want a single row is implement a class for their Table with a `->find($id)` function.
Then they make it return an object that represents their database row.
This kind of querying is excellent (with a few caveats) for bread and butter programming, but it starts to fall down in two areas:

1. efficient queries - what if I have 100,000 rows, but I only care about two of the columns? Why am I fetching all that extra data?
2. representing more complex data - things like GROUP BY, JOINs and subqueries

This is where your abstraction starts to fall apart around the seams.

If I'm just asking for 2 columns, how do I represent that in my model? What happens if I hydrate an object then someone tries to access a column that doesn't exist?
If I'm aggregating data, then this no longer corresponds to a model at all! What am I returning?
If I'm joining an extra table, how can I represent that in my result set? 

Well done! You've made the leap into a key realisation about database queries - meaningful hydration can only really be achieved on queries that consist of nothing but filtering of some form or another.

Once you've done a JOIN, your ORM model no longer makes any sense. Once you've filtered down from pulling back all columns, your ORM model no longer makes any sense.

There are caveats to both of these in that Doctrine can *kind* of pull it off through a central entity manager, but it's not really a route you want to casually go down.

#### One tiny JOIN caveat

...there is one more caveat on this, in that a JOIN can be used for two things:
1. to retrieve data from an additional related dataset and
2. to add a constraint to the data - when an INNER JOIN it essentially adds the constraints of the `ON` to the `WHERE` clause

### Scalar queries

So, we can have simple ORM queries that end with hydration. What if I want data I've can only obtain from GROUP BYs or JOINs? That my friend is what we refer to as a scalar query.

The data we can represent when we're not constrained by the columns of a single table can be magnificent. This is indeed what SQL was designed to do! Use the power of the relationships between your data to get exactly what you're after!

Hang on, if all I can meaningfully do to hydrate my models is filter my data, aren't the queries I'll write to query for hydration and for complex data different?

I mean, there are definitely arguments to be had, but yes, they are. Which nicely leads us into the next section...

### Why are we ORMing anyway?

ORM = Object Relationship Mapper. The name is entirely about simple querying and hydration.
If we took the name at face value, there would be no place for scalar queries at all inside an ORM system.
In reality, the ORM tends to end up taking the responsibility of all the code that communicates to the database, which
means that an ORM will potentially be doing:

- Migrations
- Simple Queries (that can be easily be hydrated)
- Complex Queries (where hydration doesn't make sense)

This is where the difference between scalar and non-scalar queries comes in. They feel similar in principle, but in reality, you're far better thinking of them as separate beasts.