# Express.js Joi Validation

Joi is a popular JavaScript object validation library.

## Using Joi

### Installing Joi

```js
npm install @hapi/joi
```

### Requiring Joi in your module

```js
const Joi = require("@hapi/joi");
```

Why `@hapi`? Hapi [moved all their modules to the new Hapi scope](https://twitter.com/hapijs/status/1120237246425133057).

## Why use Joi?

### Why is server-side validation important?

It is a security best practice to not trust what the client sends us. Why can't we trust what the client sends us? Can't I just do JavaScript checks on the input on my website?

- Your client side validation might not be enough
- A user can turn off client-side JavaScript validation (eg. modifying the code when you view source on your browser)

### What do we need to validate?

Using Express.js and Node.js, we often need to validate the request's message body(`req.body`), param (`req.params`), and query `req.query`.

### Why not create our own middleware?

We can write the validation code manually using `if` condition checks, but this is error prone and tedious to code from scratch.

```js
if (!res.body.name || !res.body.artist) {
  // return some error code
}
```

In a real world scenario, your objects will also have many more attributes than just name/ artist.

### Features

Joi allows developers to define a schema stating:

- the attributes of an object and wheather it is an optional or required attribute
- the type an attribute should have. eg: should it be a string or a number or email
- the max or minimum length a field should be

For more features, please visit the Joi library page at https://github.com/hapijs/joi

## Schema and validation

A schema is a very concise way to describe (in code) what data you expect to recieve from the client.

You can validate your JavaScript object against the Joi schema and get a result object that contains a value and an error attribute.

```js
{
    error:
    value:
}
```

If the validation of the object passes, the value attribute contain the object validated and the error attribute will be undefined.

If the validation fails, the value attribute is undefined and the error attribute will contain an array of errors messages called `details`. There will be one _Error_ instance for each validation failure that occurred.

Let's define a schema for our object.

```js
const schema = Joi.object({
  username: Joi.string()
    .alphanum() //must contain only alphanumeric characters
    .min(3)
    .max(30) //at least 3 characters long but no more than 30
    .required(),

  //password must satisfy the custom regex pattern
  password: Joi.string().pattern(new RegExp("^[a-zA-Z0-9]{3,30}$")),
  repeat_password: Joi.ref("password"), // must equal to password

  birthyear: Joi.number().integer().min(1900).max(2013), // an integer between 1900 and 2013

  //email a valid email address string
  //must have two domain parts e.g. example.com
  email: Joi.string().email({ minDomainAtoms: 2 }),
});
```

Once we have the schema, we can validate the request we received from the client.

Imagine we have a `req.body` object that we have already parsed as JSON.

The object will look something like this:

```js
requestBody = {
  username: "John Milo Tan",
  password: "31!!!311",
  birthyear: 1990,
  email: "hello@example.com",
};
```

We can now validate the requestBody object.

```js
result = schema.validate(requestBody);

if (result.error) {
  console.log("The data is invalid");
  //return 400 bad request to the client
} else {
  console.log("The data is valid!!");
  //save the data to db and return 200 OK to the client
}
```

## Example

We could put the schema in a function that will return the result object.

```js
function validateSong(song) {
  const schema = Joi.object({
    id: Joi.number().integer(),
    name: Joi.string().min(3).required(),
    artist: Joi.string().min(3).required(),
  });
  return schema.validate(song);
}
```

If validation does not pass, `validation.error` will be defined. We will create an instance of error with the message of the first error in `validation.error.details`, triggering the error handler functions with `next(error)`.

```js
const validation = validateSong(req.body);
if (validation.error) {
  const error = new Error(validation.error.details[0].message);
  // 400 Bad Request
  error.statusCode = 400;
  next(error);
}
```
