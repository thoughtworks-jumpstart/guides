# npm

npm is Node.js' default package manager. You can run it using the `npm` command.
Not only it is a popular JavaScript package manager, it has an online public repository of open-source packages.

## Uses of npm

- Packaging our Node.js modules into libraries and share them with others
- Reuse libraries built by other people
- Manage dependencies (libraries and their versions) used in a project
- Building our projects (by providing the scripts in package.json file)

## Create package.json

`package.json` is a configuration file that npm uses.

Generate the `package.json` file for a project:

```
npm init
```

Bypass the questions with the `--yes` or `-y` flag.

See an example of a `package.json` file in the [npm docs for creating a package json file](https://docs.npmjs.com/creating-a-package-json-file).

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
