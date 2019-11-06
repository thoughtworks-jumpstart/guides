# MongoDB

MongoDB is a open-source **NoSQL** non-relational database. It is a document-orientated database that is flexible and scalable. It is best used for large amounts of unstructured data.

## Start your MongoDB server

### Windows

```cmd
  mongod --dbpath /data/db
```

Depending on the security level of your system, Windows may pop up a Security Alert dialog box about blocking “some features” of C:\Program Files\MongoDB\Server\VERSION_NUMBER\bin\mongod.exe from communicating on networks. All users should select `Private Networks`, such as my home or work network and click `Allow access`.

### Linux / Mac

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

## Pros

- Schema-less, fields can vary from document to document and data structure can be changed over time
- Maps to the objects in your application code, making data easy to work with
- Scalable for large volume of data
- Fast, indexing is supported by B-trees
- Has high availablily due to it being a distributed database

## Cons

- High data consumption
- No joins, but joins can technically be done using other ways
- Has a 16MB document limit

## Concepts

### Document

MongoDB stores documents in BSON (a binary form of JSON) format like the one below:

```json
{
  "firstName": "Gordon",
  "lastName": "Song"
}
```

All javascript types of data are supported, thus MongoDB is often used with Node.js projects.
Each document has a "\_id" field which is a GUID (Globally Unique Identifier).

### Collection

A **document** then is placed within a **collection**. As an example, the JSON document above defines a `user` object. This user object then would typically be part of a collection called `users`.

For those who are familiar with relational databases, a **collection** in MongoDB is similar to a table in relational database, and a document in MongoDB is similar to a row in relational databases.

### Database

A database is a container of collections. Various databases can be run on a single MongoDB server.

### Indexing

Indexes are small portions of data which are ordered. They help find documents that match the query criteria without performing an entire collection scan. Thus they quicken the execution of queries in a collection.

However, too many indexes will affect database performance. They need to be deleted when no longer used.

### Others

- Query
- Reference

## MongoDB Schema Design

- [Three ways of modelling a one-to-N relationship](https://www.mongodb.com/blog/post/6-rules-of-thumb-for-mongodb-schema-design-part-1)

## Notes

- It is possible to have a ["join" of documents across collections](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/).
- (Starting from 4.0 release) [ACID transaction across multiple documents is also supported.](https://www.mongodb.com/transactions)
- MongoDB creates databases and collections on demand if they do not already exist.

## Other resources

- [MongoDB and its role in Agile development](https://www.mongodb.com/blog/post/mongodb-qa-whats-the-deal-with-nonrelational-databases-and-agile-software-development)
