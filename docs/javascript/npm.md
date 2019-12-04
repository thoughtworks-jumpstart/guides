# npm

npm is Node.js' default package manager. You can run it using the `npm` command.
Not only it is a popular JavaScript package manager, it has an online public repository of open-source packages.

## Manage dependencies

Dependencies are remote packages used by your project. Your project depends on these packages. When using npm as a package manager, it uses a file called `package.json` to keep track of a project's dependencies.

Generate the `package.json` file for a project:

```
npm init
```

Bypass the questions with the `--yes` or `-y` flag.

See an example of a `package.json` file in the [npm docs for creating a package json file](https://docs.npmjs.com/creating-a-package-json-file).

Without installing any packages, the project has no dependencies as shown inside `package.json`:

```
// package.json
"dependencies" : {}
```

### Install a package

Install a remote package on npm into `node_modules`:

```
npm install <packagename>
```

Use the `--save` flag to install and then record this package into the dependencies in `package.json`.

```
npm install <packagename> --save
```

When cloning an existing project and running it for the first time on your computer, you would need to install all the packages listed into `node_modules`

### Semantic versioning

After installing and saving a package into `package.json`, you might notice that it has a version specified in the format of:

```
"package": "MAJOR.MINOR.PATCH"
```

The MAJOR version should increment when you make incompatible API changes.
The MINOR version should increment when you add functionality in a backwards-compatible manner.
The PATCH version should increment when you make backwards-compatible bug fixes.

Use the tilde (~) character to prefix the version of moment in your dependencies, and allow npm to update it to any new PATCH release.
Use the caret (^) to prefix the version of moment in your dependencies and allow npm to update it to any new MINOR release.

## npm audit

Use npm audit to generate a security report:

```
npm audit
```
