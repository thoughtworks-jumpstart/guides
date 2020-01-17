# Express.js environment variable

Environment variables are those variables that exist within the environment your code is running in. It could be your Windows / Mac / Linux environment on your local computer or the Heroku environment, Docker environment etc. This can be accessed by any program running in the environment.

We often use environment variables to store configuration information.
Configuration changes across different environments (such as local/QA environment, staging, production) and we will like to have a easy way to change the configuration without actually changing the code. We will not have to re-deploy the code just to change the configuration.

![express env variable](_media/express-env-variable.png)

(image source: https://blog.codecentric.de/en/2019/05/webapps-node-express-typescript-part2/)

## Accessing environment variables

In Node.js, we use the global object `process.env` to access environment variables.

`NODE_ENV` is an example of an environment variable that Express.js projects will usually have. It is set to **development** when the server is run during development and set to **production** when released for production. However, you have to set it yourself. If you do not set it, it will be undefined.

It could also be set to **test**.

> Jest will set process.env.NODE_ENV to 'test' if it's not set to something else. You can use that in your configuration to conditionally setup only the compilation needed for Jest, e.g.

(From Jest documentation https://jestjs.io/docs/en/24.0/getting-started.html)

#### But why specifically `NODE_ENV`? Why is it everywhere?

This environment variable was popularised by Express.js.

> The `NODE_ENV` environment variable specifies the environment in which an application is running (usually, development or production). Setting NODE_ENV to “production” makes Express: Cache view templates. Cache CSS files generated from CSS extensions. Generate less verbose error messages.

(From Express.js documentation on best practices https://expressjs.com/en/advanced/best-practice-performance.html)

Read more about it [here at Packt about why it matters](https://hub.packtpub.com/building-better-bundles-why-processenvnodeenv-matters-optimized-builds/).

### Checking for the value of `NODE_ENV`

```js
var env = process.env.NODE_ENV || "development";
if (env === "development") {
  // do development things
}
```

Alternatively, you can also use `app.get("env")`.

```js
if (app.get("env") === "development") {
  // do development things
}
```

Express.js `app.get('env')` returns 'development' if `NODE_ENV` is not defined. Therefore you will not need to give it a default value unlike `process.env.NODE_ENV`.

### Checking for the value of `PORT`

For example, we often need a `PORT` environment variable to store the port number that our Express.js server will listen to.

```js
const PORT = process.env.PORT || 3000;
```

`3000` is the fallback value here. If there are environments (e.g. during local development) in which the environment variables are not set, we should set a fallback value that the variable will default to.

We then use the variable like a normal one.

```js
const server = app.listen(PORT, () => {
  console.log(`Server running at http://localhost:${PORT}`);
});
```

## Setting environment variables

### Local environment

In your local environment, you can set environment variables by going to your favourite terminal / command line and doing something like:

```sh
export TEMP_VAR="THIS IS TEMP"
```

It is a temporary environment variable that will only be available for the current terminal session.

For our Express.js server, we will need to set the environment variables every single time we run our server locally. Thus using `export` will be too tedious.

#### .env file

It is convention to list out environment variables in a local file called `.env` in the root directory of the project. (There could also be environment variables for database host, username and password, API keys etc. This could sometimes be sensitive information that should not be pushed into your Github repository.)

Add environment-specific variables on new lines in the form of `NAME=VALUE`inside the `.env` file.

```sh
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```

#### dotenv

To ensure that we can access these environment variables through `process.env`, we still need to set these environment variables in our local environment.

This can be easily done through the [dotenv module](https://github.com/motdotla/dotenv).

Require and configure dotenv

```js
require("dotenv").config();
```

It is good to do this as early as possible in our application. Thus we usually do this in our `index.js` file, or whichever file that acts as our starting point.

Yay! `process.env` now has the keys and values you defined in your .env file.
