# Quick start for a Node.js backend project

0. Create a new directory for your project:

```
mkdir my-express-app
```

0. Change directory into the new directory

```
cd my-express-app
```

0. Initiantiate a new project using npm init:

```
npm init -y
```

0. Install Express.js

```
npm install express
```

0. Make current project a git repo:

```
git init
```

0. Create a .gitignore file and add node_modules to it:

```
echo "node_modules" >> .gitignore
```

0. Create a file which will serve as the entry point to your app:

```
touch app.js
```

0. Start coding! Open the project in VS code:

```
code .
```

0. Add a `start` script to package.json

```json
"scripts": {    "start": "node index.js",    ...}
```

0.  Run project in production mode

```
npm start
```

0. Install Nodemon

```
npm install nodemon
```

0. Add a `start:dev` script to package.json

```json
"scripts": {...    "start:dev": "nodemon index.js"   }
```

0. Run project in development mode

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
