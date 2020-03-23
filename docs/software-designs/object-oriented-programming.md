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
- The constructor method is a special method for creating and initializing an object created with a class.
- There can only be one special method with the name "constructor" in a class. A SyntaxError will be thrown if the class contains more than one occurrence of a constructor method.

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

For example, if we abstract a television console, it will be abstracted to the screen and the box. We ignore the fact that the screen is made up of individual pixels and that the box contains various electronic parts. We also don't know how the color is decided for each pixel or what data is manipulated.

## Encapsulation

Encapsulation is to put data and the operations using/manipulating the data into the same class.

- Some of the implementation details are declared as 'private' and hidden from public interface. (Some languages like Java makes it very easy to declare private fields, but JavaScript does not have straightforward support on this.)
- the functionality offered by an instance is only accessed through the public interface.

## Exercise 1

Create a **Car** class which has methods and properties for the following instances:

Car objects:

| car1     | car2        | car3   |
| -------- | ----------- | ------ |
| Green    | Red         | Blue   |
| Mercedes | Toyota      | Proton |
| Gasoline | Electricity | Diesel |
| faster   | fast        | normal |

What kind of methods should the cars have?

## Static methods

Static methods are methods that are callable on the class itself - not on its instances.

Static methods are used typically to implement behavior that does not pertain to a particular instance. For example, we could design a `Vehicle` class so that it tracks every vehicle it creates. We could then write static methods that return how many vehicles have been created, search for vehicles by their make, etc.

What static methods would you add to your **Car** class?

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

### Class-based Inheritance vs Prototype-based Inheritance

Class-based Inheritance: a child class inherits methods from its parent/ancestor class.
Prototype-based Inheritance: one object inherits methods from its prototype object.

Some languages (like Java, C#) adopts Class-based Inheritance.

JavaScript actually follows Prototype-based Inheritance, although the class syntax gives us an illusion of class-based inheritance.

In ES5, we would have created a class like this:

```js
function Person(name) {
  this.name = name;
}

var bob = new Person("Bob");
console.log(typeof bob);
```

In ES6, we have syntactic sugar of a "class".

```js
class Person {
  constructor(name) {
    this.name = name;
  }
}

var bob = new Person("Bob");
console.log(bob instanceof Person);
```

Compare this with object literals in JavaScript:

```js
const bob = {
  name: "Bob",
};
```

## Exercise 2

Create a parent **Vehicle** class which has child classes **Car** and **Motorcycle**.
The classes should have fields such as `Manufacturer`.
**Motorcycle** class should have an extra field for `gear` because it has a 6th gear.
What kind of fields and methods should be in the parent class?


Read more about this here [in this gist](https://gist.github.com/jim-clark/5d66a759b3842b4a06659d1d73da25b6).
