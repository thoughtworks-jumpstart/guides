# Express.js path to production

## Useful packages

### Set NODE_ENV to development or production

Use the `cross-env` package.

### Load environment variables

Use the `dotenv` package.

### JSON web token

json web token
cookie parser
bcryptjs
cors

## Check your package.json

```json
"scripts": {
    "start": "cross-env NODE_ENV=production node index.js",
    "start:dev": "cross-env NODE_ENV=development nodemon index.js",
    "start:db": "mongod --dbpath ~/data/db",
    "test": "jest",
    "test:watch": "jest --watch"
  },
```

## CORS

If you deploy your frontend and backend as two separate applications on Heroku, you also need to enable CORS (Cross Origin Resource Sharing) between the two applications.

For example, assume your frontend and backend are deployed to the following two URLs:
https://my-backend-api.heroku.com
https://my-react-app.heroku.com

Your users would visit "https://my-react-app.heroku.com" to download the frontend React app into their browser, and then the JavaScript in your React app needs to make calls to your backend API, which is "https://my-backend-api.heroku.com". This is NOT allowed by the browsers by default, due to Same Origin Policy.
The walk around is to enable CORS in your backend API.

If you build the API using Express, you can configure the Express CORS middleware to allow the API to be called by another React application loaded from Heroku.

For example, here is a sample code for app.js in Express (assuming that your frontend localhost is on port 3001). Set the environment variable `FRONTEND_URL` on Heroku if you are hosting your backend there.

```js
const express = require("express");
const cors = require("cors");

var corsOptions = {
  origin: process.env.FRONTEND_URL || "http://localhost:3001",
  credentials: true,
};

const app = express();
app.use(cors(corsOptions));
```

We need `credentials: true` for accepting requests with cookies. Note that origin cannot be the wildcard `*` if credentials is true.

> Cannot use wildcard in Access-Control-Allow-Origin when credentials flag is true

If you would like to have multiple origins, you can also use an array.

```js
const corsOptions = {
  origin: [process.env.FRONTEND_URL, "http://localhost:3001"],
  credentials: true,
};
```

Make sure your URL does not have a slash at the back.

To allow calls from any port for localhost and any application on Heroku:

```js
origin: [/http:\/\/localhost:.*/, /http[s]*:\/\/.*\.herokuapp.com/],
```
