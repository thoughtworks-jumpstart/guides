# MongoDB vs Relational
Relational databases (RDBMS) or SQL databases uses a relational data model to store data.
MongoDB, on the other hand, is a non-relational, document-orientated database.

## Database concepts
| Relational    | MongoDB       |
| ------------- |---------------|
| Database      | Database      |
| Table         | Collection    | 
| Row           | Document      |
| Index         | Index         |
| Foreign Key   | Reference     |


## Performing "relational database" actions in MongoDB

> ...when leveraging the richness and power of the document model, we estimate 80%-90% of applications don’t need multi-document transactions at all.

Reconsider changing your MongoDB schema design so that these might not be necessary. 

- It is possible to have a ["join" of documents across collections](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/).
- (Starting from 4.0 release) [ACID transaction across multiple documents is also supported.](https://www.mongodb.com/transactions)