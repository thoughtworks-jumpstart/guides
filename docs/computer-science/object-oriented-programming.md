# Object-oriented programming

Object-oriented programming (OOP) is one of the programming paradigms that we commonly use. It is widely popular in large-scale software engineering.

In object-orientated modelling, we use objects to model the real world things that we want to represent inside our programs. OOP envisions software as a collection of cooperating objects rather than a collection of functions or simply a list of commands (as is the traditional / procedural view).
Today, many popular programming languages (such as Java, JavaScript, C#, C++, Python, PHP, Ruby and Objective-C) support OOP.

## Why is OOP useful?

It's a technique to manage a large-scale codebase. It helps to answer the question: given a piece of code/functionality, where should we put it?

OOP strongly emphasizes modularity, thus it promotes greater flexibility and maintainability in programming. Object-oriented code is simpler to develop, easier to understand and extend later on.

## What is involved in OOP?

### Class

- A named concept that represent a group of things with same characteristics
- In ES6 for JavaScript, classes were introduced as a new syntax for defining objects
- This new class syntax is just syntactic sugar. Under the hood it is still using the old inheritance method with prototypes. We will understand prototypes (which is very specific to JavaScript) later.

```js
//defining an Animal class
class Animal {}
```

### Instance

- When you use the `new` operator to create a new object out of the given class, that object is an instance of the class.
- Declare your classes first before making an instance of it

```js
// creating an instance of the class
const animal1 = new Animal();
```

### Field / Property

- Data that is part of a class
- These properties can be any value (i.e. string, number, function, or even other objects)

### Constructor

- A method that will run immediately on the object after using the `new` operator to create a new object.

The constructor method is a special method for creating and initializing an object created with a class. There can only be one special method with the name "constructor" in a class. A SyntaxError will be thrown if the class contains more than one occurrence of a constructor method.

```js
class Movie {
  constructor(title, price, duration) {
    this.title = title;
    this.price = price;
    this.duration = duration;
  }
}
```

### Method

- A named function or procedure, with or without parameters, that implements some behavior for a class.
- Method in a class should be aligned with the purpose/role/responsibility of the class.

```js
class Theater {
  constructor(name, seats) {
    this.name = name;
    this.seats = seats;
  }

  getAvailableSeats() {
    // return a list of available seats
  }

  markSeatAsBlocked(seatNumber) {
    // block the given seat
  }

  markSeatAsAvailable(seatNumber) {
    // cancel the blocking on the seat
  }

  markSeatAsSold(seatNumber) {
    // mark the seat as sold
  }
}
```

## Abstraction

Abstraction is to create a simple model of a more complex thing, which represents its most important aspects in a way that is easy to work with for our program's purposes.

- Derive generalised concepts from concrete examples
- Ignore/drop irrelevant details
- Give it a name

For example, if we abstract a television console, it will be abstracted to the screen and the box. We ignore the fact that the screen is made up of individual pixels and that the box contains various electronic parts.

## Encapsulation

Encapsulation is to put data and the operations using/manipulating the data into the same class.

- Some of the implementation details are declared as 'private' and hidden from public interface. (Some languages like Java makes it very easy to declare private fields, but JavaScript does not have straightforward support on this.)
- the functionality offered by an instance is only accessed through the public interface.

## Inheritance

A child class may inherit the fields and methods of its parent class.

- An instance of child class is also an instance of the parent class. This is called is-a relationship.
- Sometimes the child class is called a subclass and the parent class is called a super class.
- Inheritance is _transitive_, so a class may inherit from another class which inherits from another class, and so on, up to a base class (typically an Object class or possibly implicit/absent).

### The "extends" keyword

The `extends` keyword causes the inheritance to happen. It will look something like this:

```js
class ChildClass extends ParentClass
```

### Constructor of the child class

If the child class has no constructor then the following "empty" constructor will be generated:

```js
class ChildClass extends ParentClass {
  // generated for extending classes without child constructor
  constructor(...args) {
    super(...args);
  }
}
```

A constructor of the child class can use `super()` to call the constructor of the parent class.

Here is a concrete example on how you can define a child constructor:

```js
class Seat {
  constructor(seatNumber) {
    this.seatNumber = seatNumber;
    this.capacity = 1;
  }


class CoupleSeat extends Seat {
  constructor(seatNumber) {
    super(seatNumber);
    this.capacity = 2;
  }
}
```

See [this javascript.info lesson on class inheritance](https://javascript.info/class-inheritance) for more information.

## Polymorphism

Polymorphism means "One name, with many forms."

There are two types of polymorphism. The first type of polymorphism involves a child class overriding a parent class method, by having a method or property that is of the same name as the one in the parent class.

The second polymorphism involves method overloading whereby the class has methods of the same name but with different parameters (Methods with the same name but different parameters have a different _signature_). However, JavaScript does not support method overloading natively. If there are two methods with the same name, JavaScript will only consider the last defined function and override the first one. See [this website for more information](https://www.codeproject.com/Articles/797997/JavaScript-Does-NOT-Support-Method-Overloading-Tha).

#### Overriding

Overriding is the first type of polymorphism.

By default, all methods that are not specified in the child class are taken directly “as is” from the parent class.

But if we specify a method in the child class with the same name, this will override the method that was inherited from the parent class.

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  stop() {
    this.speed = 0;
  }
}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

  stop() {
    // this overrides the method inherited from the Animal class
    this.hide();
  }
}
```

You also can override properties / fields:

```js
class Seat {
  constructor(seatNumber) {
    this.seatNumber = seatNumber;
    this.status = SEAT_AVAILABLE;
    this.capacity = 1;
  }
}

class CoupleSeat extends Seat {
  constructor(seatNumber) {
    super(seatNumber);
    this.capacity = 2;
  }
}
```

### The "super" keyword

Previously, we used `super()` to call the constructor of the parent class in the child class. In the child class, you can also call methods of the parent class with the `super` keyword. These methods might have been overriden in the child class, but `super` makes it possible to call the parent's methods too.

```js
class Animal {
  constructor(name) {
    this.speed = 0;
    this.name = name;
  }

  stop() {
    this.speed = 0;
  }
}

class Rabbit extends Animal {
  hide() {
    alert(`${this.name} hides!`);
  }

  stop() {
    super.stop(); // call parent's stop method which was overriden
    this.hide();
  }
}
```

## Why is polymorphism and inheritance useful?

```js
class Dog {
  woof() {
    console.log("i'm a dog, hear me woof!");
  }
}

class Cat {
  meow() {
    console.log("i'm a cat, hear me meow!");
  }
}

class Lion {
  roar() {
    console.log("i'm a lion, hear me roar!");
  }
}

var dog = new Dog();
var cat = new Cat();
var lion = new Lion();
var animals = [dog, cat, lion];

// this for-loop shows how messy things can get when we don't design
// our classes with polymorphism in mind.
for (let i = 0; i < animals.length; i++) {
  var currentAnimal = animals[i];
  if (currentAnimal.constructor === Dog) {
    currentAnimal.woof();
  } else if (currentAnimal.constructor === Cat) {
    currentAnimal.meow();
  } else if (currentAnimal.constructor === Lion) {
    currentAnimal.roar();
  }
}
```

## Composition

Each object is a building block, and we can build more complex classes with other classes/objects.
Imagine that our animals now need a trainer. To create an instance of a trainer, we need to implement a Trainer class. Our implementation can be simplified by using composition.
As before, let's look at an implementation without composition, before we look at a better implementation with composition.

```js
class Trainer {
  woof() {
    console.log("i'm a dog, hear me woof!");
  }

  meow() {
    console.log("i'm a cat, hear me meow!");
  }

  roar() {
    console.log("i'm a lion, hear me roar!");
  }

  makeAnimalSound() {
    this.woof();
    this.meow();
    this.roar();
  }
}
```

There are several problems with this implementation:

1. Duplication.
   We had to rewrite the logic for woof(), meow() and roar() in the Trainer class when they already exist in the Dog, Cat and Lion classes.

2. Incorrect modelling.
   Trainer should not know how to meow/woof/roar
   What if the method was eat()? Trainer now needs to eat raw meat like a lion! Yikes!

### Composition vs inheritance
