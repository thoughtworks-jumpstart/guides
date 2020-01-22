# MongoDB basics

MongoDB is a open-source **NoSQL** non-relational database. It is a document-orientated database that is flexible and scalable. It is best used for large amounts of unstructured data.

Learn more about [MongoDB](https://docs.mongodb.com/manual/tutorial/getting-started/#getting-started)

## Start your MongoDB server

### Windows

Server is run via an executable **mongod.exe** on Windows.

```cmd
  mongod --dbpath /data/db
```

Depending on the security level of your system, Windows may pop up a Security Alert dialog box about blocking “some features” of C:\Program Files\MongoDB\Server\VERSION_NUMBER\bin\mongod.exe from communicating on networks. All users should select `Private Networks`, such as my home or work network and click `Allow access`.

### Linux / Mac

Server is run via an executable called **mongod**

```sh
  mongod --dbpath ~/data/db
```

### General

The `--dbpath` option specifies the location where MongoDB should store data. It can be any folder you create. `/data/db` is just one example.
Note that the path to mongod.exe and the path to your data folder could be different on your system.

Once the MongoDB server is started, you should see the following line on your console:

```

[initandlisten] waiting for connections on port 27017

```

## MongoDB command shell

### Check if MongoDB server is running

```sh
mongo
```

If you can successfully launch the MongoDB terminal, your server is running.

### Stop MongoDB server

For Windows / Mac, you can launch the MongoDB terminal and run the following commands:

```sh
use admin
db.shutdownServer()
```

For Linux, you have a few more options as [mentioned in the documentation](https://docs.mongodb.com/manual/tutorial/manage-mongodb-processes/#stop-mongod-processes).

### Using the command shell

Select a database. Creates a new database `mydb` if it does not exist, else it uses the existing database.

```sh
use mydb
```

See currently selected database

```sh
db
```

Check databases list

```sh
show dbs
```

Newly created database `mydb` is not shown. This is because there is no documents in it yet.

Insert a new document
Insert a user `{name: "Babel"}` into a **users** collection

```sh
db.users.insert({name: "Babel"})
```

Check databases list again

```sh
show dbs
```

Check that the user has been inserted

```sh
db.users.find()
```

What is printed? The documents in the `users` collection have been printed.

```js
{ "_id" : ObjectId("5e2524809ef8d3673b8c17d6"), "name" : "Babel" }
```

Notice that they have a `_id` key.

All documents require a primary key stored in the `_id` field. You can enter your own `_id` value if you want, but you must ensure its uniqueness. A MongoDB object ID will be inserted automatically if a custom value is not provided.

Check collections list in current database

```sh
show collections
```

The **users** collections was automatically created.

### Using MongoDB Compass

You can also use [MongoDB Compass](https://www.mongodb.com/products/compass) to explore your databases and collections.

## Why use MongoDB?

- Schema-less, fields can vary from document to document and data structure can be changed over time
- Maps to the objects in your application code, making data easy to work with
- Scalable for large volume of data
- Fast, indexing is supported by B-trees
- Has high availablily due to it being a distributed database

> If you’ve experienced difficulties scaling relational databases, this may be great news. But not everyone needs to operate at scale. Maybe all you’ve ever needed is a single database server. Why would you use MongoDB? Perhaps the biggest reason developers use MongoDB isn’t because of its scaling strategy, but because of its intuitive data model.

(Source: MongoDB in Action 2nd Edition - 2016)

## Disadvantages of MongoDB

- High data consumption
- Have to think in terms of the document model rather than the traditional relational model for databases
- No joins, but joins can technically be done using other ways
- Has a 16MB document limit

## Concepts

### Document

MongoDB stores documents in BSON (a binary form of JSON) format like the one below:

```json
{
  "firstName": "Babel",
  "lastName": "Watermelon"
}
```

All javascript types of data are supported, thus MongoDB is often used with Node.js projects.
Each document has a "\_id" field which is a GUID (Globally Unique Identifier).

### Collection

A **document** then is placed within a **collection**. As an example, the JSON document above defines a `user` object. This user object then would typically be part of a collection called `users`.

For those who are familiar with relational databases, a **collection** in MongoDB is similar to a table in relational database, and a document in MongoDB is similar to a row in relational databases.

### Database

A database is a container of collections. Various databases can be run on a single MongoDB server.

### Query

Let's say we have posts on a website. Suppose you want to find all posts tagged with the term politics having more than 10 votes. A SQL query would look like this:

```sql
SELECT * FROM posts
INNER JOIN posts_tags ON posts.id = posts_tags.post_id INNER JOIN tags ON posts_tags.tag_id == tags.id
WHERE tags.text = 'politics' AND posts.vote_count > 10;
```

(Example is from [MongoDB in Action 2nd Edition](https://livebook.manning.com/book/mongodb-in-action-second-edition/chapter-1/41))

In MongoDB, we use a document (object literal in JavaScript) as a matcher. The relevant fields have to match but additional fields are allowed but ignored.

Specifying the value directly checks for equality. The special `\$gt` key indicates the greater-than condition. The **AND** condition is indicated by the comma ",".

```js
db.posts.find({ tags: "politics", vote_count: { $gt: 10 } });
```

The SQL query relies on a strictly normalized model, where posts and tags are stored in distinct tables, whereas the MongoDB query assumes that tags are stored within each post document.

### Indexing

As your database increases in size, searching for some value will take longer and longer.
Indexes are small portions of data which are ordered. They help find documents that match the query criteria without performing an entire collection scan. Thus they quicken the execution of queries in a collection.

> The best way to understand database indexes is by analogy: many books have indexes matching keywords to page numbers. Suppose you have a cookbook and want to find all recipes calling for pears (maybe you have a lot of pears and don’t want them to go bad). The time-consuming approach would be to page through every recipe, checking each ingredient list for pears. Most people would prefer to check the book’s index for the pears entry, which would give a list of all the recipes containing pears. Database indexes are data structures that provide this same service.

(Source: MongoDB in Action 2nd Edition - 2016)

However, too many indexes will affect database performance. They need to be deleted when no longer used.

### Others

- Reference

## MongoDB schema design

- [Three ways of modelling a one-to-N relationship](https://www.mongodb.com/blog/post/6-rules-of-thumb-for-mongodb-schema-design-part-1)

## Good to know

- MongoDB creates databases and collections on demand if they do not already exist.

## Other resources

- [MongoDB and its role in Agile development](https://www.mongodb.com/blog/post/mongodb-qa-whats-the-deal-with-nonrelational-databases-and-agile-software-development)
