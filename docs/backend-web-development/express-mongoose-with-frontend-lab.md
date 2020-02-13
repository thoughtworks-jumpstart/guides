## Pokemon

We will be building a React frontend with Express.js backend API for users giving reviews of companies data with Mongoose.

## Planning the CRUD API

Lab: Build a basic CRUD API (Create / Read / Update / Delete)

We are creating an API to interact with the resources on the server.

#### 0. Get API endpoints

Route: GET /
HTTP Response status code: 200  
Expected response:`

```json
{
  "0": "GET /",
  "1": "GET /companies",
  "2": "GET /companies/:id",
  "3": "POST /companies/:id/reviews",
  "4": "GET /user"
}
```

Notice the plural form. We have 5 endpoints.

#### 1. Find all companies

Route: GET /companies
HTTP Response status code: 200

Expected response:

```json
[
  {
    "id":"e5cc2c0a-93b5-4014-8910-6ed9f3056456",
    "companyName":"Brakus, Aufderhar and Gutkowski",
    "companySuffix":"and Sons",
    "numberOfEmployees":60497,
    "description":"Voluptas reiciendis quasi expedita ex qui sit. Qui enim facilis adipisci qui."
  },
  ...
]
```

Test data:

```js
const companiesData = [
  {
    id: "e5cc2c0a-93b5-4014-8910-6ed9f3056456",
    companyName: "Brakus, Aufderhar and Gutkowski",
    companySuffix: "and Sons",
    numberOfEmployees: 60497,
    description:
      "Voluptas reiciendis quasi expedita ex qui sit. Qui enim facilis adipisci qui.",
    reviews: [
      {
        id: "7da4d967-715b-4dc1-a74b-82a7992704f3",
        userId: "f6e016e6-e254-4375-bf82-797e6c00e3eb",
        userName: "Brennan Fisher",
        rating: 0,
        title: "eligendi adipisci",
        review:
          "Consequatur esse beatae voluptate voluptatibus expedita aperiam perspiciatis cumque voluptatem. Cum quasi dolor ut dignissimos illum magni eos. Et aspernatur illum commodi.",
      },
      {
        id: "fa07ef47-5849-4642-8af0-640e4887b1e6",
        userId: "13d0782f-2793-4c83-8279-93c9a03b3ac3",
        userName: "Annalise Nicolas",
        rating: 4,
        title: "iusto consequatur",
        review:
          "Facere dicta delectus impedit sunt sed officia omnis. Officiis vel optio corrupti iure. Atque iusto nemo. Ut voluptas quaerat omnis quis impedit maiores nihil ipsam. Quod ea sed voluptates. Dolorem officia esse enim.",
      },
    ],
  },
];
```

Suggested tests:

1. GET should return all companies' information without reviews (and no \_id, \_\_v)

#### 2. Find company information including reviews

Route: GET /companies/:id
HTTP Response status code: 200

Expected reponse:

```json
{
  "id":"e5cc2c0a-93b5-4014-8910-6ed9f3056456",
  "companyName":"Brakus, Aufderhar and Gutkowski",
  "companySuffix":"and Sons",
  "numberOfEmployees":60497,
  "description":"Voluptas reiciendis quasi expedita ex qui sit. Qui enim facilis adipisci qui.",
  "reviews":[
    {
      "id":"7da4d967-715b-4dc1-a74b-82a7992704f3",
      "userId":"f6e016e6-e254-4375-bf82-797e6c00e3eb",
      "userName":"Brennan Fisher",
      "rating":0,
      "title":"eligendi adipisci",
      "review":"Consequatur esse beatae voluptate voluptatibus expedita aperiam perspiciatis cumque voluptatem. Cum quasi dolor ut dignissimos illum magni eos. Et aspernatur illum commodi."
    },
    {
      "id":"fa07ef47-5849-4642-8af0-640e4887b1e6",
      "userId":"13d0782f-2793-4c83-8279-93c9a03b3ac3",
      "userName":"Annalise Nicolas",
      "rating":4,
      "title":"iusto consequatur",
      "review":"Facere dicta delectus impedit sunt sed officia omnis. Officiis vel optio corrupti iure. Atque iusto nemo. Ut voluptas quaerat omnis quis impedit maiores nihil ipsam. Quod ea sed voluptates. Dolorem officia esse enim."
    },
    ...
  ]
}
```

#### 3. POST a new review on a company (have to be logged in!)

Route: POST /companies/:id/reviews
HTTP Response status code: 201

Expected json request:

```json
{
  "rating": 4,
  "title": "eligendi adipisci",
  "review": "Et voluptatem voluptas quisquam quos officia assumenda. Mollitia delectus vitae quia molestias nulla ut hic praesentium. Sed et assumenda et iusto velit laborum sunt non."
}
```

If you are logged in as:

```json
{
  "userId": "754aece9-64bf-42ab-b91c-bb65e2db3a37",
  "userName": "Humberto Bruen"
}
```

Expected response:
You can also choose to respond with all reviews for the company, not just the new review.

New review:

```json
{
  "userId": "754aece9-64bf-42ab-b91c-bb65e2db3a37",
  "userName": "Humberto Bruen",
  "rating": 4,
  "title": "eligendi adipisci",
  "review": "Et voluptatem voluptas quisquam quos officia assumenda. Mollitia delectus vitae quia molestias nulla ut hic praesentium. Sed et assumenda et iusto velit laborum sunt non."
}
```

Suggested tests (there could be more):

1. POST should add a review to the correct company when user is authorised
2. POST should respond with error 400 when required property not given
3. POST should deny access when no token is provided
4. POST should deny access when token is invalid

#### 4. Get user (have to be logged in!)

Route: GET /user
HTTP Response status code: 200

Expected response: (you get information about who you are logged in as)

```json
{
  "id": "754aece9-64bf-42ab-b91c-bb65e2db3a37",
  "firstName": "Humberto",
  "lastName": "Bruen",
  "email": "Timothy_VonRueden62@hotmail.com"
}
```

Setup a new Github project and install Express.js, `git init`, `npm init -y`.

## Folder structure

- package.json
- package-lock.json
- src
  - controllers
  - middlewares
  - models
  - routes
  - utils
  - app.js
  - index.js
- data

## TDD Test Driven Development

Lab: Implement route tests with Jest and Supertest (with database) and test for error too

mongodb-memory-server
supertest

Refactor tests by putting common test setup and teardown code into utils.
Data to be placed in data folder.

## Routing and routers

Lab: Add basic routes, integrate Express routers to organise your routes

## Error

Lab: Integrate a default error handler middleware
Lab: Give 400 Bad Request response when doing POST request

## Mongo and Mongoose

### Connect to database

```js
const dbUrl = process.env.MONGODB_URI || "mongodb://localhost:27017/" + dbName;
```

### Create schema, validate, create model

How do we get the userName of a user with only firstName and lastName?

## Add authentication to protect routes

jsonwebtoken
cookie parser
bcryptjs
jwt
dotenv
JWT_SECRET_KEY

## Keep trying TDD!

## More routing

Lab: Integrate app.param() middleware if needed

## Add tests for protected routes

## Express to production
