# REST API and CRUD

## What is REST API?

It's an architectural style for building web service APIs. REST stands for "Representational State Transfer" as described by Roy Fielding in his [dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm). REST is not well-understood and people often have their own idea of what a RESTful API is.

Its core principle is to define named _resources_ that can be manipulated using a small number of _methods_ (such as Creation/Read/Update/Delete). The _resources_ and _methods_ are the nouns and verbs of APIs.

REST also has _stateless_ servers. The server does not store any information of previous requests, every connection is like a "new" connection.

There are also recommended HTTP status codes to be returned for each method.

These constraints help to make the API easy to use.

## What is CRUD?

Many applications make use of an application pattern: Create, Read, Update and Delete. It is so common that we call it CRUD, which is a cuter word.

Imagine a photo-sharing app.

- Create: Users can upload photos
- Read: Users can browse photos
- Update: Users can edit photos
- Delete: Users can delete photos

Notice that many of the applications you use everyday will fit this CRUD model.

## Resources

For example, in a blog platform, we may have the following resources:

- Users of the platform
- Articles published by authors
- Comments made by readers

## Methods

We use HTTP verbs for our RESTful API

| HTTP Verb | CRUD           | Recommended Status Code      |
| --------- | -------------- | ---------------------------- |
| POST      | Create         | 201 (Created)                |
| GET       | Read           | 200 (OK)                     |
| PUT       | Update/Replace | 204 (No Content)             |
| PATCH     | Update/Modify  | 204 (No Content)             |
| DELETE    | Delete         | 200 (OK) or 204 (No Content) |

The 204 response code is used when the response does not carry a message body. A response with the 200 code would have one. For example, if a DELETE request comes with a response with the 200 code, it is possible that a link was returned in the message body to tell the client where to go next after deleting the resource.

See [this tutorial](https://www.restapitutorial.com/lessons/httpmethods.html) for a more detailed table.

For example, in a blog platform, we may have the following methods:

- GET blog posts
- POST blog posts to create new blog posts
- PUT blog posts to replace old posts with new ones
- DELETE blog posts to delete existing posts

With the HTTP protocol, the resource names naturally map to URLs, and methods to manipulate those resources naturally map to HTTP methods POST, GET, PUT, PATCH, and DELETE.

Take a look at [one sample JSON REST API for a blog platform](https://thinkster.io/tutorials/design-a-robust-json-api/crud-endpoints).

## REST best practices

### Don't use GET for everything

We could technically do everything with GET, but this will not be following the [HTTP specification](https://tools.ietf.org/html/rfc2616#section-9.3) and will be counter-intuitive to the client.

This is a (bad) example of trying to update a customer's information with GET. Notice that the URL tends to be longer with GET.

```
GET http://api.example.com/services?op=update_customer&id=12345&format=json
```

Other HTTP methods should be used instead.

```
PUT http://api.example.com/customers/12345
```

### Return proper HTTP status codes

HTTP protocol defines a set of status codes that you can use in HTTP responses. Make sure you understand the meaning of commonly used HTTP status codes and use them appropriately in your APIs. See the [HTTP](../fundamentals/http) page.

### Follow naming convention on resources

In deciding what resources are within your system, name them as _nouns_ as opposed to verbs or actions. In other words, a REST URI should refer to a resource that is a thing instead of referring to an action.

#### Plurals for names?

Since those resources are nouns, you need to decide if you should use singular form or plural form. For example, should your URI for retrieving a representation of a customer resource look like this:
GET http://www.example.com/**customer**/33245 or: GET http://www.example.com/**customers**/33245

There are good arguments on both sides, but the commonly-accepted practice is to always use plurals in node names to keep your API URIs consistent across all HTTP methods. The reasoning is based on the concept that customers are a collection and the ID (e.g. 33245) refers to one of those customers in the collection.

But, like all the rules, there are exceptions. One situation is when there isn't a collection concept in play. In other words, it's acceptable to use a singularized resource name when there can only be one of the resource — it's a singleton resource. For example, if there was a single, overarching configuration resource, you might use a singularized noun to represent that:

```
GET|PUT|DELETE http://www.example.com/configuration
```

See this [REST resource naming guide](https://restfulapi.net/resource-naming/).

### Make APIs Idempotent if possible

To be idempotent, the API should allow clients to make that same call to the API repeatedly, and the status of the resource should reach the same end state no matter how many calls are made.

In other words, with an idempotent API, making multiple identical requests has the same effect as making a single request.
An API with this property makes the consumer's life much easier.

#### Is GET always idempotent?

For example, GET is idempotent according to the HTTP specification, because it is supposed to only read resources. But there’s nothing to stop you from implementing a non-idempotent method in your API. You could allow your API to update your resource when a GET request is received, resulting in unexpected consequences for the client. Idempotence is important to GET requests.

#### Making DELETE idempotent

For example, assuming we have this API to delete a user:

```
DELETE /users/101
```

If the API returns error when the user with ID 101 does not exist, then an API consumer has to check the status of the user before calling this delete API.

On the other hand, if the effect of calling the API ensures the user is deleted, no matter how many times it's called, then the API consumer can call it safely without worrying about if the user has been deleted or not.

#### POST is not idempotent

Notice that POST is at times naturally not idempotent due to the resource being created. Calling the API multiple times might result in different results (more resources created).

### Evolving APIs over time

Once an API is defined, you may need to update the implementation from time to time. And sometimes, the changes may not be backward compatible to the existing API consumers. What should you do in this case?

There are many choices under this situation. One popular choice is to add a version number in your API URL, then you can create a new version of your API when there are non-backward compatible changes.

For example:

```
GET /v2/users/1/profile
```

API versioning is a difficult topic. You could read about [how Stripe does its API versioning here](https://stripe.com/en-sg/blog/api-versioning).

### Be aware of caching

The result of HTTP GET methods might be cached by the browsers (and the intermediate routers in the network). As an API developer, you can control how those responses are cached using _Cache Control Headers_.

### Designing a good API

#### Not all HTTP methods might be available to you

What if PUT/DELETE is not supported?

The original REST API proposal mentions we can use all HTTP verbs including GET/POST/PUT/DELETE to specify the intended operation on a resource. However, in reality, sometimes you cannot use PUT/DELETE:

- Browsers do support PUT and DELETE, but HTML doesn't. For example, a browser will initiate a PUT request via Javascript (AJAX), but not via HTML <form> submission.
- Some of the corporate firewalls block HTTP connection with PUT and DELETE methods

#### What about the operations that can't be mapped to CRUD operations?

For example, if you want to build an API to search users, a "search" operation is not one of the standard CRUD operations. Which HTTP method should you use? How do you design your resource URL?

You can consider querystrings or URI parameters.
You can find some discussions on Stack Overflow:

- https://stackoverflow.com/questions/207477/restful-url-design-for-search
- https://stackoverflow.com/questions/5020704/how-to-design-restful-search-filtering

## Exercises

Let's use Postman to test out a sample REST API.

The sample REST API can be found at https://jumpstart-blogging-api.herokuapp.com/
The API supports CRUD operations on two resources

- blogposts
- user

If you try to GET https://jumpstart-blogging-api.herokuapp.com/ by visiting it on your browser, you can find the supported CRUD operations.

```json
{
  "0": "GET    /",
  "1": "-----------------------",
  "2": "GET    /blogposts",
  "3": "GET    /blogposts/:id",
  "4": "POST   /blogposts",
  "5": "PUT    /blogposts/:id",
  "6": "DELETE    /blogposts/:id",
  "7": "-----------------------",
  "8": "GET    /users",
  "9": "GET    /users/:id",
  "10": "POST    /users",
  "11": "PUT    /users/:id",
  "12": "DELETE    /users/:id"
}
```

### View blogposts and users

- GET https://jumpstart-blogging-api.herokuapp.com/blogposts
- GET https://jumpstart-blogging-api.herokuapp.com/users
- GET https://jumpstart-blogging-api.herokuapp.com/blogposts/1
- GET https://jumpstart-blogging-api.herokuapp.com/users/1

What is the HTTP status code of the response?

### Create new blogposts and users

- POST JSON in the HTTP request body to blogpost (https://jumpstart-blogging-api.herokuapp.com/blogposts)

```js
{
    "title": "This is a cool blogpost",
    "body": "Put your content here...",
    "author": "babel",
    "likes": 20,
    "image_url": "https://static.pexels.com/photos/62418/pexels-photo-62418.jpeg"
}
```

- POST JSON in the HTTP request body to users (https://jumpstart-blogging-api.herokuapp.com/users)

```js
{
    "username": "babel",
    "age": 18
}
```

What is the HTTP status code of the response?

### Replace blogposts and users

- PUT JSON in the HTTP request body to users (https://jumpstart-blogging-api.herokuapp.com/users/1)

```js
{
    "username": "newUser",
    "age": 55
}
```

What is the HTTP status code of the response?

Try it for blogposts.

### Delete blogposts and users

- Send a DELETE request to users (https://jumpstart-blogging-api.herokuapp.com/users/1)

What is the HTTP status code of the response?

## Alternatives to REST (Optional)

### RPC

RPC (Remote Procedure Call) is when a computer program causes a procedure (subroutine) to execute in a different address space (commonly on another computer on a shared network), which is coded as if it were a normal (local) procedure call, without the programmer explicitly coding the details for the remote interaction.
Two examples are:

- SOAP
- XML-RPC
- gRPC

### GraphQL

GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

In short, GraphQL aims to prevents underfetching and overfetching, solving the problem that RESTful APIs face.
Read [GraphQL at the REST-aurant](https://medium.com/javascript-scene/graphql-at-the-rest-aurant-f4091054e82a) to understand the features of GraphQL.

You can even explore GitHub using their [GraphQL API](https://developer.github.com/v4/explorer/).
