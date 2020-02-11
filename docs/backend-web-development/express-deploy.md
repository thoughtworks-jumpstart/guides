# Express.js deployment

## Heroku

- Follow the tutorial [here in the official documentation](https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up)
- For the `Procfile` file, it should look something like this if you have a start script inside package.json.

```
web: npm run start
```

- What if my app still has an error? I already tried `heroku logs --tail`...

```sh
2020-02-05T06:40:00.937983+00:00 heroku[web.1]: State changed from starting to crashed
2020-02-05T06:40:00.940830+00:00 heroku[web.1]: State changed from crashed to starting
2020-02-05T06:40:00.846494+00:00 heroku[web.1]: Error R10 (Boot timeout) -> Web process failed to bind to $PORT within 60 seconds of launch
2020-02-05T06:40:00.846494+00:00 heroku[web.1]: Stopping process with SIGKILL
```

Heroku dynamically assigns your app a port and adds the port to the environment variable `process.env.PORT`.

```js
// index.js
const PORT = 3000;
const server = app.listen(process.env.PORT || PORT, () => {
  console.log(`Express app started on http://localhost:${PORT}`);
});
```

### mLab MongoDB

- Follow the tutorial [here in the official documentation](https://devcenter.heroku.com/articles/mongolab#connecting-to-your-mongodb-instance)
- Note that the `MONGODB_URI` environment variable will automatically be set for you
  Edit your `db.js` to look like this:

```js
const dbUrl = process.env.MONGODB_URI || "mongodb://localhost:27017/" + dbName;
```

- Add `JWT_SECRET_KEY` environment variable to Heroku
- If you want, you can add another user to the mongolab
