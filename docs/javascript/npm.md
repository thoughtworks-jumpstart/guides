# npm

npm is Node.js' default package manager. You can run it using the `npm` command.
Not only it is a popular JavaScript package manager, it has an online public repository of open-source packages.

## Uses of npm

- Packaging our Node.js modules into libraries and share them with others
- Reuse libraries built by other people
- Manage dependencies (libraries and their versions) used in a project
- Building our projects (by providing the scripts in package.json file)

## package.json

`package.json` is a configuration file that npm uses.

### Creating package.json

Generate the `package.json` file for a project:

```
npm init
```

Bypass the questions with the `--yes` or `-y` flag.

An example of a package.json

```json
{
  "name": "simple-node-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "start:dev": "nodemon index.js"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/thoughtworks-jumpstart/simple-node-server.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/thoughtworks-jumpstart/simple-node-server/issues"
  },
  "homepage": "https://github.com/thoughtworks-jumpstart/simple-node-server#readme",
  "devDependencies": {
    "nodemon": "^1.19.3"
  }
}
```

See more examples of a `package.json` file in the [npm docs for creating a package json file](https://docs.npmjs.com/creating-a-package-json-file).

### Scripts

In the above example, notice that there are already two scripts in the `scripts` object. npm scripts are just JSON key-value pairs where the key is the name of the script (such as `start`) and the value contains the script you want to execute (such as `node index.js`).

```json
"scripts": {    "start": "node index.js",    ...}
```

Being a popular npm script, it can be run like this:

```
npm start
```

What if you have custom npm scripts such as `start:dev` like in the above example?

```json
"scripts": {...    "start:dev": "nodemon index.js"   }
```

Now, npm start:dev would not work because it is a custom npm script. Custom npm scripts must be preceded by either `run-script` or `run` for them to be executed.

Run the custom script like this:

```
npm run start:dev
```

See more use cases of custom npm scripting [here](https://www.freecodecamp.org/news/introduction-to-npm-scripts-1dbb2ae01633/).

## Manage dependencies

Dependencies are remote packages used by your project. Your project depends on these packages. When using npm as a package manager, it uses the `package.json` configuration file to keep track of a project's dependencies.

### Install a package

When cloning an existing project and running it for the first time on your computer, you would need to install all the `dependencies` and `devDependencies` listed on `package.json` into `node_modules`:

```
npm install
```

The version of the packages that are downloaded will meet the semantic version requirements.

Install a remote package _locally_ and add it into the `dependencies` attribute of the `package.json` file:

```
npm install <packagename>
```

Dependencies appearing under the `dependencies` key in the `package.json` file are packages needed for the project to run in a production environment. Examples of packages required in production are framework libraries like React and utility libraries like lodash.

Install a remote package and adding it into the `devDependencies` attribute of the `package.json` file:

```
npm install <packagename> --save-dev
```

The `--save-dev` flag indicates that it is a devDependency.

Dependencies appearing under the `devDependencies` key in the `package.json` file are packages needed for development but are not needed for the running of the app. Examples of packages required only in development are testing libraries like Jest, linters like EsLint and transpilers like webpack.

See [npm docs for dependencies and dev dependencies](https://docs.npmjs.com/specifying-dependencies-and-devdependencies-in-a-package-json-file).

Install a package globally:

```
npm install -g <packagename>
```

This is rarely needed. Prefer local dependencies and npx where possible.

Remove packages used in your project:

```
npm uninstall <packagename>
```

### Semantic versioning

After installing and saving a package into `package.json`, you might notice that it has a version specified in the format of:

```
"package": "MAJOR.MINOR.PATCH"
```

The MAJOR version should increment when you make incompatible API changes.

The MINOR version should increment when you add functionality in a backwards-compatible manner.

The PATCH version should increment when you make backwards-compatible bug fixes.

#### Special characters

Use the tilde (~) character to allow npm to update to the latest package which has the same Major AND Minor ver as stated, but may have newer Patch version

For example, "~5.4.0" in the `package.json` may install version "5.4.7".

Use the caret (^) character to allow npm to update to the latest package which has the same Major ver as stated, but may have newer Minor and Patch versions

For example, "^5.4.0" in `package.json` may install version "5.6.0".

## Execute a package

Use npx to execute a locally-installed package or a package that was not installed previously:

```
npx <packagename/command>
```

This is very useful for create-react-app to run once at the start of a project.

```
npx create-react-app my-app
```

This also ensures that you always use the latest version without having to upgrade.

## npm audit

Use npm audit to generate a security report:

```
npm audit
```
