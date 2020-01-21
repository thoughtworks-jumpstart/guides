# MongoDB vs Relational

Relational databases (RDBMS) or SQL databases uses a relational data model to store data.
MongoDB, on the other hand, is a non-relational, document-orientated database.

## Database concepts

| Relational  | MongoDB    |
| ----------- | ---------- |
| Database    | Database   |
| Table       | Collection |
| Row         | Document   |
| Index       | Index      |
| Foreign Key | Reference  |

In relational databases, there are tables.
In other words, MySQL (a popular relational database) keeps its data in tables of rows, while MongoDB keeps its data in collections of documents, which you can think of as a group of documents.

## Data structures

> Imagine you are modelling products in an e-commerce website. With a **fully normalized relational** data model, the information for any one product might be divided among dozens of tables. If you want to get a product representation from the database shell, you’ll need to write a SQL query full of joins. With a **document model**, by contrast, most of a product’s information can be represented within a single document.

(Source: MongoDB In Action - 2nd Edition)

## Performing "relational database" actions in MongoDB

> ...when leveraging the richness and power of the document model, we estimate 80%-90% of applications don’t need multi-document transactions at all.

Reconsider changing your MongoDB schema design so that these might not be necessary.

- It is possible to have a ["join" of documents across collections](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/).
- (Starting from 4.0 release) [ACID transaction across multiple documents is also supported.](https://www.mongodb.com/transactions)
