# Publish React App

Covers:

1. Netlify

## Netlify

There are 2 ways we can deploy our application thru Netlify.
One is to use the `netlify-cli`, the other using the GUI.

### Setting up an account

1. Go to https://app.netlify.com/
2. create an account preferrable using Github
3. Create a new team

### GUI

1. Push the app to Github
2. on Netlify, select "New site from Git".
3. under continuous deployment, select Github and authorities using 2FA if required
4. Select the repo to link with

### Terminal

1. install `serve` and `netlify-cli` package

```sh
npm install -g serve netlify-cli
```

2. Build the file locally

Build the react app

```sh
npm run build
```

```sh
serve -s build
```

Deploy Netlify to a temp server

```sh
netlify deploy
```

select Netlify to deploy the build folder

```sh
netlify deploy --prod
```
