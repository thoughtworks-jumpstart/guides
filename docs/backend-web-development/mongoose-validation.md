# Mongoose validation

Previously, we used Joi for validation of our data. With Mongoose, we can and should still use Joi, but now we want to learn about implementing this validation directly in Mongoose.

In the schema, you can specify the validators. You have already saw some such as `required`.

There are many built-in validators and this varies across the SchemaTypes.

Examples:

- All SchemaTypes have the built-in `required` validator.
- Numbers have min and max validators.
- Strings have:
  - enum: specifies the set of allowed values for the field.
  - match: specifies a regular expression that the string must match.
  - maxlength and minlength for the string.

```js
var breakfastSchema = new Schema({
  eggs: {
    type: Number,
    min: [6, "Too few eggs"],
    max: 12,
    required: [true, "Why no eggs?"],
  },
  drink: {
    type: String,
    enum: ["Coffee", "Tea", "Water"],
  },
});
```

(Example schema is from [Mozilla's Express.js tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/mongoose#related_documents))

## Custom validator (optional)

How do we add our own custom validator?

```js
// make sure every value is equal to "something"
function validator(val) {
  return val === "something";
}

const nameSchema = new Schema({
  name: {
    type: String,
    validate: validator,
  },
});

// with a custom error message

var custom = [validator, 'Uh oh, {PATH} does not equal "something".'];
new Schema({ name: { type: String, validate: custom } });
```

## Exercises

Integrate your songs routes with MongoDB and Mongoose!

- Create a schema with validation and a model for your songs
- Create a database for your songs in your MongoDB
- Change your controllers 