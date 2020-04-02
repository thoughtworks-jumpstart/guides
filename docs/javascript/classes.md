# Classes in JavaScript

## Learning checklist

- what does the word class mean?
- what are classes, and what they are used for
- how to define classes
- how to instantiate objects of a given class using the `new` keyword
- what is `this`
- how to extend a class to create a new subclass
- what does super() and super.someMethod() do

Rule of thumb for naming: use nouns for class names (e.g. Person, Student, Teacher), use verbs for function/method names (.walk(), .talk(), etc)

## What is a class?

Classes are blueprints for instantiating (i.e. creating) JavaScript objects.

Note that this is only one of the ways to create objects in JavaScript. Another way is to use object literal which we have already been using like { }.

![classes](https://gblobscdn.gitbook.com/assets%2F-LBJBL3Fj_tcfkvqLj9P%2F-LBJBWvA1rRm3yAXiRPh%2F-LBJBf4WF4DA_-m6IBxV%2Fclass_inheritance.png?generation=1525052258823703&alt=media)

## Define a class

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

Once the class is defined, you can instantiate new objects for this class.

```js
const bob = new Person("bob", 25);
console.log(bob.name);
const amy = new Person("amy", 30);
```

## Constructor

### What is constructor()

The constructor() method is a special method for creating and initializing an object created with a class. There can only be one special method with the name "constructor" in a class. A SyntaxError will be thrown if the class contains more than one occurrence of a constructor method.

Constructors are optional.

### What happens when a constructor is called

Constructor() is called at the moment an object is instantiated. An empty object is created, and it's referred as `this` in the constructor. Then the fields of the object is populated according to what you set in the constructor.

In the end, the object is returned from the constructor (although you don't need the return statement)

## Instance Methods

You can define methods that is available in each object created from this class. You can only call these methods when you already have an instance of the class (that was created by the `new` keyword).

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  makeIntroduction() {
    // this is a method defined in the class
    console.log("hi! i am", this.name);
  }
}

var bob = new Person("bob", 33);
bob.makeIntroduction();
```

## Static Methods

You can also create methods that is owned by the class itself, and you can call it without creating new instances of the class.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
    Person.total += 1;
  }
  static isOnEarth() {
    return true;
  }
}

console.log(Person.isOnEarth());
```

## Instance Properties

If you know other programming languages (like C#, Java, etc), you are aware that you can declare instance properties in class declaration, like the example given below.

```js
class Person {
  name = "";
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const bob = new Person("bob", 30);
console.log(bob.name);
```

It is an [experimental feature, available with Node 12](https://github.com/tc39/proposal-class-fields).

You can read the [d]ocumentation at MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Class_fields#Public_fields).

## Static Properties

```js
class Person {
  static total = 0;

  constructor(name, age) {
    this.name = name;
    this.age = age;
    Person.total += 1;
  }
}

console.log("number of people:", Person.total); // 0
const bob = new Person("bob", 30);
console.log("number of people:", Person.total); // 1
```

## Getters and setters

This is a trick that allows you to expose a method as a field in the class.

```js
// exposing data without getters
class Circle {
  constructor(r) {
    this.r = r;
  }

  area() {
    return Math.PI * this.r ** 2;
  }
}

var c = new Circle(10);
c.area(); // data is exposed as a function

// exposing data with getters
class Circle {
  constructor(r) {
    this.r = r;
  }

  get area() {
    return Math.PI * this.r ** 2;
  }
}

var c = new Circle(10);
c.area; // data is exposed as a property
```

## Create sub-classes

- inheritance
- super() and super.someMethod() - The super keyword is used to call methods on an object's parent.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  makeIntroduction() {
    console.log("hi! i am", this.name);
  }

  sayAge() {
    console.log("i am", this.age, "years old");
  }
}

class MagicalPerson extends Person {
  constructor(name, age, specialSkill) {
    super(name, age); // this calls the constructor of the super class (Person)
    this.specialSkill = specialSkill;
  }

  makeIntroduction() {
    super.makeIntroduction();
    console.log(
      "i am a MagicalPerson and my special skill is " + this.specialSkill
    );
  }

  writeCode() {
    console.log("writing code!");
  }
}

tom = new MagicalPerson("tom", 10, "TDD");
tom.sayAge(); // inherited method
tom.makeIntroduction(); // overriden method
tom.writeCode(); // new method
```

## Exercises

https://github.com/thoughtworks-jumpstart/javascript-classes
