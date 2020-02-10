# Mongoose middleware

Middleware are functions that run at specific stages of a pipeline. Mongoose supports middleware for the following operations:

Aggregate
Document
Model
Query

For instance, models have `pre` and `post` functions that take two parameters:

- Type of event (‘init’, ‘validate’, ‘save’, ‘remove’)
- A callback that is executed with this referencing the model instance

https://www.freecodecamp.org/news/introduction-to-mongoose-for-mongodb-d2a7aa593c57/

Hashing user's password with bcrypt before saving to database.

```js
userSchema.pre("save", async function(next) {
  const rounds = 10;
  this.password = await bcrypt.hash(this.password, rounds);
  next();
});
```
