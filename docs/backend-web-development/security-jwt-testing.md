# JSON Web Token testing

First we will mock `jsonwebtoken` because we are not testing the actual verification of the token.

```js
jest.mock("jsonwebtoken");
```

What tests shall we add?

For `/trainers`

1. POST should add a new trainer

```js
expect(trainer.username).toBe(expectedTrainer.username);
expect(trainer.password).not.toBe(expectedTrainer.password);
```

For `/trainers/:username`

2. GET should respond with trainer details when login as correct trainer

````js
const jwt = require("jsonwebtoken");

jwt.verify.mockReturnValueOnce({ name: expectedTrainer.username });
      const { body: trainers } = await request(app)
        .get(`/trainers/${expectedTrainer.username}`)
        .set("Cookie", "token=valid-token")
        .expect(200);

      expect(jwt.verify).toHaveBeenCalledTimes(1);

      expect(trainers[0]).toMatchObject(expectedTrainer);
      ```
````

3. GET should respond with incorrect trainer message when login as incorrect trainer
   Status code: 403 Forbidden

4) GET denies access when no token is provided
   Status code: 401 Unauthorized

```js
expect(jwt.verify).not.toHaveBeenCalled();
```

5. GET denies access when token is invalid
   Status code: 401 Unauthorized

   ```js
   jwt.verify.mockImplementationOnce(() => {
        throw new Error();
   });

   ...

   .set("Cookie", "token=invalid-token")

   ...

   expect(jwt.verify).toHaveBeenCalledTimes(1);
   ```

For `/trainers/login`

6. POST should logs owner in if password is correct

7. POST should not log trainer in when password is incorrect

Reset mocks after each test

```js
afterEach(async () => {
  jest.resetAllMocks();
  await Trainer.deleteMany();
});
```
