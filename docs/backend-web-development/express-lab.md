# Express.js lab

## Build simple API for "who is next"

### Planning the CRUD API

Lab: Build a basic CRUD API for the "who is next" (Create / Read / Update / Delete)

Requirements
In this lab we will implement a basic CRUD API in Express with the below 8 routes:

We are creating an API to interact with the resources on the server.

#### 0. Get API endpoints

Route: GET /
HTTP Response status code: 200  
Expected response:

```json
{
  "0": "GET    /",
  "1": "POST   /whonext",
  "2": "GET    /whonext",
  "3": "-----------------------",
  "4": "GET    /participants",
  "5": "POST   /participants",
  "6": "GET /participants/:id",
  "7": "PUT /participants/:id",
  "8": "DELETE /participants/:id"
}
```

Notice the plural form. We have 8 endpoints.

#### 1. Generate the next winner - who is next?

Route: POST /whonext
HTTP Response status code: 201
Expected Response:

```json
[{ "id": 1, "name": "xxx" }]
```

This is a POST request instead of a GET request because we are recording the history of participants that were next. This creates a resource on the server.

#### 2. Get a history of who was next

Route: GET /whonext
HTTP Response status code: 200  
Expected response:

```json
[
  { "id": 1, "name": "xxx" },
  { "id": 2, "name": "xxx" }
]
```

#### 3. Get participants

Route: GET /participants
HTTP Response status code: 200
Expected Response:

```json
[
  { "id": 3, "name": "xxx" },
  { "id": 1, "name": "xxx" }
]
```

#### 4. Add one participant

Route: POST /participants
HTTP Response status code: 201
Expected Response:

```json
[{ "id": 1, "name": "xxx }]
```

#### 5. Get participants with id

Route: GET /participants/:id
HTTP Response status code: 200
Expected Response:

```json
[{ "id": 1, "name": "xxx" }]
```

#### 6. Update (replace) participant with id

Route: PUT /participants/:id
HTTP Response status code: 200
Expected Response:

```json
[{ "id": 1, "name": "xxx" }]
```

#### 7. Delete participant with id

Route: DELETE /participants/:id
HTTP Response: 200
Expected Response:

```json
[{ "id": 1, "name": "xxx" }]
```

Setup a new Github project and install Express.js

We will now do the first route together GET / in class.

## Routing

Lab: Add basic routes GET

## Middleware

Add middleware for requiring JSON

## Parse body and routing with param

Improve response and implementing proper POST, PUT, DELETE

## Param processing

Integrate app.param() middleware to find participant from id

## Testing

Implement tests with Jest and Supertest (include testing for errors)

## Routers

Integrate Express.js routers to organise your "who is next" routes (put into routes folder)

## Refactoring to controllers folder

Lab: refactoring of routes

## Error

Integrate a default error handler middleware for your "who is next" routes

## Joi

Integrate Joi validation library to validate data

## Folder structure

- package.json
- package-lock.json
- src
  - controllers
  - routes
- \_\_tests\_\_
