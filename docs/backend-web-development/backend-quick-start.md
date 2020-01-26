# Quick start for a Node.js backend project

Create a new directory for your project:

```
mkdir my-express-app
```

Change directory into the new directory

```
cd my-express-app
```

Initiantiate a new project using npm init:

```
npm init -y
```

Install Express.js

```
npm install express
```

Make current project a git repo:

```
git init
```

Create a .gitignore file and add node_modules to it:

```
echo "node_modules" >> .gitignore
```

Create the default homepage file

```
touch index.js
```

Create a file which will serve as the entry point to your app:
(Read more about app.js vs index.js in [Express.js testing](express-testing))

```
touch app.js
```

Start coding! Open the project in VS code:

```
code .
```

Add a `start` script to package.json

```json
"scripts": {    "start": "node index.js",    ...}
```

Run project in production mode

```
npm start
```

Install Nodemon (devDependencies)

```
npm install nodemon --save-dev
```

Add a `start:dev` script to package.json

```json
"scripts": {...    "start:dev": "nodemon index.js"   }
```

Run project in development mode

```
npm run start:dev
```

## Tests

Run the following steps if you're including tests in your project:
Install libraries which we'll use for writing tests:

```
npm install --save-dev jest supertest
```

Update your package.json and add the following two scripts:

```json
"scripts": {
"test": "jest",
"test:watch": "jest --watch"
},
```
