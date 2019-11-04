# MongoDB

MongoDB is a open-source **NoSQL** non-relational database. It is a document-orientated database that is flexible and scalable. It is best used for large amounts of unstructured data.

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

### Collection

A **document** then is placed within a **collection**. As an example, the JSON document above defines a `user` object. This user object then would typically be part of a collection called `users`.

For those who are familiar with relational databases, a **collection** in MongoDB is similar to a table in relational database, and a document in MongoDB is similar to a row in relational databases.

### Database

A database is a container of collections. Various databases can be run on a single MongoDB server.

### MongoDB Schema Design

[Three ways of modelling a one-to-N relationship](https://www.mongodb.com/blog/post/6-rules-of-thumb-for-mongodb-schema-design-part-1)

### Other resources

- [MongoDB and its role in Agile development](https://www.mongodb.com/blog/post/mongodb-qa-whats-the-deal-with-nonrelational-databases-and-agile-software-development)
