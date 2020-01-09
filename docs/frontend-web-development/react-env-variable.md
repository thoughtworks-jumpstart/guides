# React Environment Variable

## Covers:

1. Checking the current environment
2. Adding a custom environment variable
3. The priority of environment variable files

## Checking current environment

`create-react-app` has already set up an environment variable for us.
When we are running the program, we are going to be in one of the environments ( `production`, `development`, or `local`)

### commands => environment

- npm start => `development`
- npm test => `test`
- npm build && serve -s build => `production`

We can access the node environment using `process.env.NODE_ENV` without additional work

```javascript
console.log(process.env.NODE_ENV)
// or
<div>{process.env.NODE_ENV}</div>
```

## Adding custom environment variable

### Persisted environment variable

We can create a `.env` file in the root directory
Custom environment variable requires you to name the variable starting with `REACT_APP_`.
I.e. `REACT_APP_SERVER_URL` or `REACT_APP_USERNAME` or `REACT_APP_SECRET`.

`.env` file in root

```text
REACT_APP_SERVER_URL=https://www.google.com/
REACT_APP_USERNAME="John Smith"
REACT_APP_SECRET=YsayE3FKcHHbN9A5g
```

### Temporary enviroment variable

Windows(CMD): `set "REACT_APP_NOT_SECRET_CODE=abcdef" && npm start`

Windows(Powershell): `($env:REACT_APP_NOT_SECRET_CODE = "abcdef") -and (npm start)`

Linux, macOS (Bash): `REACT_APP_NOT_SECRET_CODE=abcdef npm start`

## Priority of environment variable files

The value of `process.env.NODE_ENV` is going to result in different environment variable files read.

For example, when `process.env.NODE_ENV === "production"`, react will look into files containing `.env.production*`.

### Priority from top to bottom

1. Setting a temporary environment variable
2. `.env.production.local` | `.env.development.local` | `.env.test.local`
3. `.env.production` | `.env.development` | `.env.test`
4. `.env.local`
5. `.env`

**Do not commit files ending with `.local`**
