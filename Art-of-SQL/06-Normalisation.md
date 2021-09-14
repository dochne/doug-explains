## A Brief Foray into Normalisation

Hopefully you can all remember learning about normalisation. Just in case you can't, a *very* quick overview - feel free to skip this section if it's all familiar.

This is primarily here as it's important that people understand what they don't know *before* trying to understand joins.

### Normalisation Primer

A man walks into a library and picks up the first book he sees and checks it out. This transaction could be written down in a table like this.

```
lendee_name: "Jordan Booth"
lendee_address: "1600 Pennsylvania Ave NW Washington, DC 20500"
book_name: "Arnold: The Education of a Bodybuilder"
book_author: "Arnold Schwarzenegger"
book_isbn: "0007466021"
expected_return_date: "1997-08-29"
```

This is what we call de-normalised data - data that contains data that isn't relevant to the record at hand.

Let's say this strange "Jordan" man got out a second book. We'd then have two transactions, each with his name and his address.
What then happens if he changes his address? It's now stored in multiple places, it has to be updated in each one separately! This doesn't sound right!

Normalisation is the process of taking that data and splitting it out so the data isn't duplicated.

In the simplest case here, we'd end up with a new Table called `lendee`

```
CREATE TABLE `Lendee` (
  `lendee_id` bigint NOT NULL,
  `name` varchar(500) NOT NULL,
  `address` varchar(500) NOT NULL,
)
```

Great, now this looks more like this! Instead of referring to the details of the lender, we reference the id.

```
lendee_id: 1
book_name: "Arnold: The Education of a Bodybuilder"
book_author: "Arnold Schwarzenegger"
book_isbn: "0007466021"
expected_return_date: "1997-08-29"
```

...but that book is duplicated data right? As is the author?

```
CREATE TABLE `Author` (
  `author_id` bigint NOT NULL,
  `name` varchar(500) NOT NULL
)
```

```
CREATE TABLE `Book` (
  `book_id` bigint NOT NULL,
  `author_id` bigint NOT NULL,
  `name` varchar(500) NOT NULL
  `isbn` varchar(10) NOT NULL
)
```

Look! No more duplicated data! This is now normalised.

```
lendee_id: 1
book_id: 1
expected_return_date: "1997-08-29"
```

If you're still a bit unsure on normalisation, I *urge* you strongly to do some of your own reading, it really is bread and butter and *vital* to understand.

### I heard de-normalisation makes it go faster?

Well, maybe. Most likely you're reaching for the wrong tool in the toolbox.
There will definitively be times when it can make sense for performance reasons, but despite optimising a *lot* of databases and a lot of queries,
I've yet to come across a situation where de-normalising was the best course of action.

Data consistency should be the last thing to be sacrificed on the alter of enhanced query speed.
