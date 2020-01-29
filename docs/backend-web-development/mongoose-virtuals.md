# Mongoose virtuals

Refer to the repository: [Mongoose pokemon basics](https://github.com/thoughtworks-jumpstart/mongoose-pokemon-basics)

Virtuals are properties that are not in the database but exist in the model.

## Getters

```js
simplePokemonSchema.virtual("nameWithJapanese").get(function() {
  return `${this.name} ${this.japaneseName}`;
});
```

Though it is not a real field / property that exist in the MongoDB database, you can access this property when using the mongoose model instance.

```js
findOneByName("Pikachu").then(data => {
  console.log(`findOneByName: ${data}`);
  console.log(data.nameWithJapanese);
});
```
