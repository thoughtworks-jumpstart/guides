# Mongoose basics

Mongoose is a Node.js _Object Document Mapper_ (ODM). This means that Mongoose allows you to define objects with a strongly-typed schema that is mapped to a MongoDB document. Mongoose is written on top of the native Node.js MongoDB driver.

Make sure your Mongo server is running!

Let's explore a few common operations using Mongoose:

- Creating schemas
- Creating models
- Using models to query and save data

This code is similar to the 'Getting Started' section on Mongoose Documentation but uses modern JS syntax including async/await.

## Installation

```sh
npm install mongoose
```

## Connecting to MongoDB server

```js
//db.js
const mongoose = require("mongoose");

const mongoOptions = {
  useNewUrlParser: true, // prevent deprecation warnings
  useUnifiedTopology: true,
  useFindAndModify: false, // For find one and update
  useCreateIndex: true, // for creating index with unique
};

// will create a new db if does not exist
const dbName = "pokedex";
const dbUrl = process.env.MONGODB_URI || "mongodb://localhost:27017/" + dbName;
mongoose.connect(dbUrl, mongoOptions);
const db = mongoose.connection;

// event emitters
// console.error() implementation expects its this value to be set to window.console
// read https://www.tjvantoll.com/2015/12/29/console-error-bind/
db.on("error", console.error.bind(console, "connection error:"));
db.once("open", () => {
  console.log(`connected to mongodb at ${dbUrl}`);
});
```

We put this in a "db.js" file in a utils folder.

To connect to the database, "require" this file in app.js:

```js
//app.js
require("./utils/db");
```

## Creating schemas

Refer to the repository: [Mongoose pokemon basics](https://github.com/thoughtworks-jumpstart/mongoose-pokemon-basics)

### What is a schema?

A schema is where we can specify the kind of properties that we expect of documents. MongoDB does not enforce that your documents have to have the same kind of properties in a collection but in Mongoose, we can specify a schema for a group of documents.

We create a new file "simple-pokemon.model.js".

Let's create a simple pokemon schema!

```js
//simple-pokemon.model.js
const mongoose = require("mongoose");
const Schema = mongoose.Schema;

const simplePokemonSchema = new Schema({
  name: {
    type: String,
    required: true,
    minlength: 3,
    unique: true,
  },
  japaneseName: String,
  baseHP: Number,
  category: String,
});
```

We define a property called `name` with a schema type `String` which maps to an internal **validator** that will be triggered when **saving** documents to the database (if the documents are using this schema). The validation will fail if the data type of the `name` path of the document is not a string type.

`name` is also a `required` field, which means that a document using this schema must have this field. The value of `name` has to have a length of at least 3.

The `unique` field of `name` does not enforce uniqueness with validation but instead will try to an unique index in your the MongoDB database. If the index is not built correctly, this might not always work. See the index being created in the database using MongoDB Compass.

(Note that if you try to enforce uniqueness for a field that could be null or empty, you might need to create the unique index as a partial index that ignores null or empty values. See https://stackoverflow.com/questions/52094484/mongodb-create-unique-index-on-a-field-not-in-all-documents for more details.)

The following Schema Types are allowed:

- Array
- Boolean
- Buffer
- Date
- Mixed (A generic / flexible data type)
- Number
- ObjectId
- String

The following schema uses all the available schema types.

```js
const schema = new Schema({
  name: String,
  binary: Buffer,
  living: Boolean,
  updated: { type: Date, default: Date.now() },
  age: { type: Number, min: 18, max: 65, required: true },
  mixed: Schema.Types.Mixed,
  _someId: Schema.Types.ObjectId,
  array: [], //an array of objects without a specified type
  ofString: [String], // You can also have an array of each of the other types too.
  nested: { stuff: { type: String, lowercase: true, trim: true } },
});
```

This example is from Mozilla. See more at [Mozilla's Express.js's tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/mongoose#related_documents).

### Schema options

```js
const thingSchema = new Schema({..}, { timestamps: true });
```

By setting `timestamps` to true, mongoose assigns createdAt and updatedAt fields to your schema, the type assigned is Date.

See https://mongoosejs.com/docs/guide.html#timestamps

## Models

### Creating models

We need to call the model constructor on the Mongoose instance and pass it the name of the collection and a reference to the schema definition.

A model is created!

```js
//simple-pokemon.model.js
// Mongoose#model(name, [schema], [collection], [skipInit])
// Defines a model or retrieves it.
const SimplePokemon = mongoose.model("SimplePokemon", simplePokemonSchema);
```

Parameters:

1st param - name <String> model name
2nd param - [schema] <Schema> schema name
3rd param - [collection] <String> collection name (optional: mongoose can guess from model name)
4th param - [skipInit] <Boolean> whether to skip initialization (defaults to false)

The third and fourth parameters are less commonly used.

Notice that the collection that the model will be in is automatically guessed by mongoose. For example, SimplePokemon will probably have a collection named simplepokemons.

### Exporting models

```js
//simple-pokemon.model.js
const SimplePokemon = mongoose.model("SimplePokemon", simplePokemonSchema);
module.exports = SimplePokemon;
```

We can now put this model file into a folder called `models`. It is convention to use pascal case for model names.

### Document - Create instance of the model

We can create an instance of the model we defined above and populate it using the following syntax:

```js
//index.js
const SimplePokemon = require("./models/simple-pokemon.model");
```

> Document and Model are distinct classes in Mongoose. The Model class is a subclass of the Document class. When you use the Model constructor (`new`), you create a new document.

```js
const MyModel = mongoose.model("Test", new Schema({ name: String }));
const doc = new MyModel();

doc instanceof MyModel; // true
doc instanceof mongoose.Model; // true
doc instanceof mongoose.Document; // true
```

By creating a new model (that is also a document), this document will use the schema associated with the model class.
