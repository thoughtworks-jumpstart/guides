# Mongoose middleware

Middleware are functions that run at specific stages of a pipeline. For Mongoose, [middleware is specified on the schema level](https://mongoosejs.com/docs/middleware.html).

Mongoose has 4 types of middleware.

Aggregate
Document
Model
Query

For instance, for document middleware, the `pre` and `post` functions will take two parameters:

- Type of event ('init', 'validate', 'save', 'remove', 'updateOne' and 'deleteOne' )
- A callback that is executed with `this` referencing the document

All middleware types support pre and post hooks.

Also see https://www.freecodecamp.org/news/introduction-to-mongoose-for-mongodb-d2a7aa593c57/.

## Hashing password with bcrypt

Hashing user's password with bcrypt before saving to database.

```js
userSchema.pre("save", async function () {
  if (this.isModified("password")) {
    const rounds = 10;
    this.password = await bcrypt.hash(this.password, rounds);
  }
});
```

https://mongoosejs.com/docs/api.html#document_Document-isModified

Why should we check for `isModified`?

`isModified` will only return true if you are changing the password or setting it for the first time.

It will not be triggered, for example, if the user's name is changed.
