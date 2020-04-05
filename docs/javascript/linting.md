# Linting

## What is linting

Linting is to automatically check the code for syntax and style errors and potential run-time errors.

It is a type of static analysis for code. Static analysis is analysing the source code directly without running the code.

## What is ESLint

[ESLint](https://eslint.org/) is, as the website describes, the **"pluggable linting utility for JavaScript and JSX"**.

ESLint also reports deviations from specified coding guidelines. These coding guidelines are often defined by the team. Error messages identify the violated rules, making it easy to adjust their configuration if you disagree with the defaults.

Languages that require compilers have a way to give feedback to developers before code is executed. Since JavaScript doesn't require a compiler, linters are needed to play that role.
You can take a look at the [type of issues that can be detected by ESLint](https://eslint.org/docs/rules/).

## Installing ESLint

To install ESLint, simply run the following command in your project root folder:

```
npm install --save-dev eslint
```

You might want to install the tool globally, but that's a bad idea. We may use different linter tools for different projects, so it's a good practice to install such project-specific tools at project level.

Here is a [good reading on this topic](http://ericlathrop.com/2017/05/the-problem-with-npm-install-global/).

## Configuring ESLint with Prettier and Jest

To configure how ESLint works, you can add a configuration file `.eslintrc.json` in your project root directory.

You could run the following command directly to generate the configuration file:

```
./node_modules/.bin/eslint --init
```

Alternatively, you can add a custom script to `package.json`.

```json
"scripts": {
    "lint": "eslint"
  },
```

Then run the command:

```
npm run lint -- --init
```

which will run `npm run lint` with the `--init` argument.

There is a [comprehensive documentation](https://eslint.org/docs/user-guide/configuring) on how to configure ESLint, but we would like to use some pre-defined (and well-defined) configurations at this moment.

## ESLint + Prettier

Specifically, we recommend to configure ESLint to follow the rules defined by Prettier, a popular JavaScript code formatter.

Why the two tools?

There are two kinds of rules we can set for linting.

1. Formatting rules: Rules that prevent inconsistent and ugly looking code (eg: max-len, no-mixed-spaces-and-tabs, keyword-spacing, comma-style…)

2. Code-quality rules: Rules that prevent useless or error making code (eg no-unused-vars, no-extra-bind, no-implicit-globals, prefer-promise-reject-errors…)

ESLint has the ability to apply both categories of rules and auto-fix your code if it can. Prettier only checks for formatting errors within your code but it does this job much better than ESLint.

Prettier often is able to reformat our code without much configuration while ESLint might not be able to fix all formatting errors automatically.

To integrate the two tools, run the following command in your project root folder:

```
npm install --save-dev prettier eslint-plugin-prettier eslint-config-prettier
```

And then add `plugin:prettier/recommended` to your `.eslintrc.json` file in `extends` field.

See https://github.com/prettier/eslint-plugin-prettier for more information.

## ESLint + Jest

ESLint has a plugin which contains some useful rules for writing Jest based tests, such as not allowing disabled tests, etc.
To install that plugin, run

```
npm install --save-dev jest eslint-plugin-jest
```

## Sample .eslintrc.json for node based app

```json
{
  "env": {
    "node": true,
    "es6": true
  },
  "parserOptions": { "ecmaVersion": 8 },
  "extends": [
    "eslint:recommended",
    "plugin:jest/recommended",
    "plugin:prettier/recommended"
  ]
}
```

## Sample for create react app

```json
{
  "extends": [
    "react-app",
    "eslint:recommended",
    "plugin:jest/recommended",
    "plugin:prettier/recommended"
  ]
}
```

## Configure VS Code with ESLint Extension

There is a very useful extension in VS Code that automatically run ESLint to check issues in your code.

VS Code ESLint Extension

After installing the extension, you might want to add some configurations in your VS Code User Settings:

- "eslint.packageManager". Set it to "yarn" if you use yarn as our default package manager instead of npm
- "eslint.run". Set it to "onSave" if you don't want to trigger ESLint while you are typing.

For other possible configurations, refer to the documentation of the extension.

With the ESLint extension installed, VS Code would highlight the lint errors. You can use the shortcut F8 to navigate to the next error in the current file.

## Format Codes with Prettier

Since we configure ESLint to follow the rules defined by Prettier, ESLint would highlight codes that do not follow Prettier coding style.
For those kind of issues, we can quickly format the code with Prettier plugin for VS Code.

There is a VS Code configuration to automatically format the file upon save.

```
"editor.formatOnSave": true
"editor.formatOnPaste": true
```

## Disable ESLint no-console Rule Temporarily

Most of the time, the console.log() statement is used for debugging purpose. Such kind of code should not be checked into code repository. To prevent that from happening, there is an ESLint rule that does not allow console log statements in your codes.

However, there are some cases where you genuinely use the console log statement to display some information on the console for the user (e.g. a command line node application). In those cases, you can temporarily disable the no-console rule.

If you want to disable the rule in a file, just add the following line at the top of the file:
/_ eslint-disable no-console _/

If you just need to disable the rule in a specific line, use
// eslint-disable-next-line no-console

The same eslint-disable and eslint-disable-next-line trick can be used to temporarily disable other rules as well. But please make sure you have good reasons for using them.
