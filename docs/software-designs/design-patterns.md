# Design Patterns

Tested and proven patterns to solve programming problems. 

## Content
1. Creational patterns
2. Structural patterns
3. Behavioral patterns
 
## Creational patterns
Creating new instance

### Factory pattern
Seperate object or function creations from its implementation. 
A factory pattern can also use to create private variables(encapsulation) thru closure.

```javascript
const createSayHi = (message) => {
    return () => console.log(`Hi ${message}`)
}

const sayHiToAlice = createSayHi("Alice");
const sayHiToBob = createSayHi("Bob");

sayHiToAlice(); // Alice
sayHiToBob(); // Bob
```

### Builder pattern
Builder pattern allows creation of different object based on representation of input.
It makes reading of data and creating objects 
```javascript
const carsToBuild = ["car", "sport car", "van"];

const carsBuilder = (type) => {
    switch(type) {
        case "car":
            return new Car();
        case "van": 
            return new Van();
        case "sports car"
            return new SportsCar();
        default:
            throw new Error("invalid vehicle type");
    }
} 

const carsBuilt = carsToBuild.map(type => carBuilder(type));
```

### Singleton
Create a instance that can be used in multiple place.
It can use as enum, or data stores like in redux reducer.

```javascript
// directions.js
const DIRECTIONS = Object.freeze({
    North: "North",
    South: "South",
    East: "East",
    West: "West",
});
```
## Structural patterns
Structural patterns help to bridge or connect different object t{}o work together.

### Proxy 
an object that acts as a connector to connect to other object.
It can also prevent object from directly talk to each other. 

```javascript
class Person {
    constructor(name) {
        this._name = name;
    }

    set name(newName) {
        this._name = newName.toLowerCase();
    }

    get name() {
        return `my name is ${this._name}`;
    }
}
```
### Adaptor
Creates a middle layer to translate one component to be compatable with another

```javascript
class Shop {
    collect(usd) {
        this.register.addUSD(usd);
    }
}

const JapaneseWallet {
    constructor(yen) {
        this.yen;
    }

    pay(amountInYen) {
        if(this.yen >= amountInYen) {
            this.yen -= amountInYen;
            return this.amountInYen;
        }
    }
}

class MultiCurrencyJapaneseWallet {
    attachJapaneseWallet(wallet) {
        this.wallet = wallet;
    }

    pay(amountInUsd) {
        return this.wallet.pay(amountInUsd * yenPerUsd)
    }
}

const japWallet = new JapaneseWallet(2000);
const multiCurrencyJapWallet = MultiCurrencyJapaneseWallet();
multiCurrencyJapWallet.attach(japWallet);

const shop = new Shop();
shop.collect(multiCurrencyJapWallet.collect(5));
```

### Facade
Create a new interface with for modules to simplifaction of logic

```javascript
class OrderService() {
    takeOrder(item) {}
}

class PaymentService() {
    handlePayment(paymentType);
}

class McdonaldKiosk() {
    addItem(item) {
        orderService.takeOrder(item);
    }

    checkout() {
        const paymentType = await waitForPaymentType();
        paymentService.handlePayment(paymentType)
    }
}
```

### Composite
combine objects of similar signature to a parent object for representation

```javascript
class Song {
    constructor(title, duration, songBinary) {
        this.title = title;
        this.duration: duration;
        this.songBinary = songBinary;
    }
}

class Playlist {
    constructor() {
        this.songList = [];
    }

    addSong(song) {
        this.songList.push(song);    
    }

    playsongs() {
        this.player.play(this.songList);
    }
}
```

## Behavioral pattern
Identify common communication patterns among objects and realize these patterns. 
By doing so, these patterns increase flexibility in carrying out this communication.

### Command
create an interface that on a input, invoke certake methods or functions relate to it. 

```javascript
class Television {
    setChannel(channel) {
        this.channel = channel;
    }

    setVolume(volume) {
        this.volume = volume;
    }

    getVolume() {
        return this.volume;
    }
}

clsss Remote {
    addCommand(commanName, command) {
        commandMap.set(commandName, command);
    }

    execute(commandName) {
        commandMap.get(commandName).execute();
    }
}

const tv = new Television();
const remote = new Remote();

remote.addCommand("button 1", { execute: tv.setChannel(1)});
remote.addCommand("button 2", { execute: tv.setChannel(2)});
remote.addCommand("add volume", { execute: tv.setVolume(tv.getVolume() + 1)});
remote.addCommand("decrease volume", { execute: tv.setVolume(tv.getVolume() - 1)});


remote.execute("add volume");
```

### Stragegy
Use to handle similar actions applying to different data but every data type needs special handlig.

```javascript
class ShapeCalculations {
    constructor(shape, strategy) {
        this.shape = shape;
        this.strategy = strategy;
    }

    calculateArea() {
        this.strategy.calculateArea(this.shape);
    }

    calculatePerimeter() {
        this.strategy.calculatePerimeter(this.shape);
    }
}

const circleStrategy {
    calculateArea: (circle) {
        return Math.PI * Math.pow(circle.radius, 2)
    } 

    calculatePerimeter: (circle) {
        eturn Math.PI * 2 * circle.radius;
    }
}

const circle = new Cicle(5);
const cicleCalculate = new ShapeCalculations(circle, circleStrategy);

circleCalculate.calculateArea();
```