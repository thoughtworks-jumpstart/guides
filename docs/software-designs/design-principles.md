# Design Principles

If you don't eat, you will get hungry. If you don't sleep for two days, you won't be able to focus as well.
How do we know these? How can we prove this is true? There might be a scientific explanation behind it but even without one, but based on my experience, been there done that, suffer the consequence I believe in it and so do many others.

Design principles are ways organising our class, functions and methods in a way that many believes will result in code that is easier to read, also understand and adaptable to changes. They come from long debates and experience of many veterans before us.

"Any fool can write code that a computer can understand. Good programmers write code that human can understand" - Martin Fowler.

## Content

1. Don't Repeat Yourself(DRY)
2. Law of Demeter
3. S.O.L.I.D

## Don't Repeat Yourself(DRY)

Adapted from "The Pragmatic Programmer" by Andy Hunt and David Thomas, published in 1999.

The DRY principle is the solution to "The Evil of Duplication". When codes duplicated, it becomes hard to maintain. If the same piece of logic appears in the code multiple times, when a requirement to change the logic, a software developer will need to remember, where these different piece of logics are and change them.

Is hard to remember what you eat for breakfast one month ago today, probably similar to what you eat every day, is even harder to remember what your family eat for breakfast a month ago.

Duplications of code make maintenance hard for multiple reasons.

1. One developer needs to know what multiple developers did in the history of the project.
2. Even doing the same code multiple times can prone to error. The variables name might be slightly different.

"Every piece of **knowledge** must have a single, unambiguous authoritative representation within a system".

Andy Hunt and David Thomas explained that there is four cause of duplication.

- Imposed duplication: Architecture decision
- Inadvertent duplication: Developers don't know about the duplication
- Impatient duplication: Duplicate the code was easier to do during development
- Interdeveloper duplication: Multiple people on a team duplicate the same piece of information

Example of Duplication

```javascript
const SavedModal = ({ showSavedModal, closeSave }) => {
  return (
    <Modal open={showSavedModal}>
      <Header content="Saved" />
      <Modal.Content>
        <p>Successfully saved!</p>
      </Modal.Content>
      <Modal.Actions>
        <Button
          aria-label="close save message"
          color="green"
          onClick={closeSave}
        >
          <Icon name="checkmark" /> Ok
        </Button>
      </Modal.Actions>
    </Modal>
  );
};

const PublishModal = ({ showPublishedModal, closePublish }) => {
  return (
    <Modal open={showPublishedModal}>
      <Header content="Published" />
      <Modal.Content>
        <p>Successfully published!</p>
      </Modal.Content>
      <Modal.Actions>
        <Button
          aria-label="close publish message"
          color="green"
          onClick={closePublish}
        >
          <Icon name="checkmark" /> Ok
        </Button>
      </Modal.Actions>
    </Modal>
  );
};
```

Is there any way to remove duplicate?

```javascript
const confimationModalFactory = (message) => { showSavedModal, closeSave }) => {
  return (
    <Modal open={showSavedModal}>
      <Header content="Saved" />
      <Modal.Content>
        <p>{message}</p>
      </Modal.Content>
      <Modal.Actions>
        <Button
          aria-label="close save message"
          color="green"
          onClick={closeSave}
        >
          <Icon name="checkmark" /> Ok
        </Button>
      </Modal.Actions>
    </Modal>
  );
};

const SavedModal = confimationModalFactory("Successfully saved!");
const PublishModal = confimationModalFactory("Successfully published!");
```

Another form of duplication.

- Documentation in code. When we write a comment and then code, the comments written is another form of duplication of knowledge.
- language issue

An example of a language issue can be the wrapAsync is a logic that calls `next(Error)`. That prevented some logic from duplicating. If we look at the code, every handler still has `wrapAsync`. It would be nice if we can have the try-catch mechanism as a global setting.

```javascript
const wrapAsync = fn => async (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(err => next(err));
};

router.get(
  "/:articleId",
  wrapAsync(async (req, res, next) => {
    const singleArticle = await Publish.find({
      id: req.params.articleId
    });
    res.status(200).send(singleArticle);
  })
);
router.patch(
  "/update/:articleId",
  wrapAsync(async (req, res, next) => {
    const publishingArticle = req.body;
    const updatePublishedArticle = await Publish.findOneAndUpdate(
      {
        id: req.params.articleId
      },
      publishingArticle,
      { new: true }
    );
    res.status(200).send(updatePublishedArticle);
  })
);
```

### Law of Demeter

Adapted from Ian Holland at Northeastern University in 1987.

Also known as the principle of least knowledge. Each object can only have access to properties and methods the object has access to
Today when you buy chicken rice, do you give auntie your whole wallet or pay him the amount.

```javascript
const Wallet {
    amount = 5;

    pay(amount) {
        if (this.amount >= amount) {
            this.amount -= amount;
        } else {
            throw new Error();
        }
    }

    topUp(amount) {
        this.amount += amount;
    }
}

class Consumer {
    const wallet = new Wallet();

    pay(amount) {
        this.wallet.pay(amount);
    }
}

const alice = new Consumer();


class Shop {
    transactionBad(consumer, price) {
        consumer.wallet.pay(price);  // break law of Demeter, alice should not know about the price.
    }

    transactionBetter(consumer, price) {
        // the Shop doesn't know what the internal object of the consumer, all Shop know is that the consumer can pay.
         consumer.pay(price);
    }
}
```

## S.O.L.I.D Principle

Adapted from "Design Principles and Design Patterns" by Bob Martin, in 2000.
In 2004, Michael Feathers realised that the principles could arrange to form the acronym SOLID.
Bob Martin started to assemble rules of what makes code that is easy to maintain in the late 1980s and finalise them only more than a decade after.

1. Single Responsibility Principle(SRP)
2. Open-Closed Principle(OCP)
3. Liskov Substitution Principle(LSP)
4. Interface Segregation Principle(ISP)
5. Dependency Inversion Principle(DIP)

The principles overlap each other.

### SRP

_A Module should be responsible to one, and only one, actor_

A Module is a part of the program that are closely related, put together to provide functionality. It can be a function, a class, a single file, a single server.

### OCP

_A software artifcat should be open for extension but closed for modification_

Classes should be able to extend without any modification of the internal structure.
If a new requirement that doesn't affect the existing functionality, a code that fulfils OCP, should not result in changes in existing code.

Think of Nintendo Labo, Nintendo controller have interface that opens to extensions of different controllers and Labo accessories. None of them requires you to dispense the controller itself. It does that by exposing the right interface.

### Liscov Substituion principle

Created y Barbara Liskov in 1988:
_Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T._

In Bob Martin's words
_Objects in a program should be replaceable with instances of their subtypes without altering the correctness of that program._

Say we have a vehicle class that can drive, turn left, turn right, top-up fuel.
If we put a bicycle as a vehicle(since a bicycle is a vehicle), we cannot substitute the bicycle as a vehicle that can top up fuel.

```javascript
// Vehicle
Class Vehicle
- moveForward()
- turnLeft()
- turnRight()
- topUpFuel()
- reverse()

// top up fuel for all vehicle
vehicles.forEach(vehicle => vehicle.topUpFuel(5))
```

### ISP

_Clients should not be forced to depend upon interfaces that they do not use._

The above example for bicycle also breaks ISP, bicycle if takes an interface of vehicles would be for to implement the method of `topUpFuel()`

```javascript
class Bicycle extends Vehicle {
  topUpFuel() {
    throw new Error("not a fuel powered vehicle");
  }
}
```

### Composition over inheritance

```javascript
class FueledPoweredVehicle {
  constructor(fuelLevel) {
    this.fuelLevel = fuelLevel;
  }

  topUpFuel(amountOfFuel) {
    this.fuelLevel = amountOfFuel;
  }
}

// using inheritance
class Car extends FueledPoweredVehicle {
  topUpFuel() {}
}

// using composition
class fuelTank {
  constructor(fuelLevel) {
    this.fuelLevel = fuelLevel;
  }

  topUpFuel(amountOfFuel) {
    this.fuelLevel = amountOfFuel;
  }
}

class Car {
  constructor(fuelTank) {
    this.fuelTank = fuelTank;
  }

  topUpFuel(amountOfFuel) {
    this.fuelTank.topUpFuel(amountOfFuel);
  }
}
```

### DIP

_Any higher-level modules should not depend on lower-level modules, and both should depend on abstract modules._
_Abstraction of modules should not depend on its implementation or details, but the implementation should depend on abstraction._

When using Mongoose, We import a Model. This model implements an abstraction. All model will contain similar generic features such as `findOneAndUpdate`. The router handler does not depend on the Schema. If the Schema change in the way that it doesn't break the functionality of the route handler, the handler will not need modification.

```javascript
const articleSchema = new mongoose.Schema(
  {
    id: { type: String, required: true, unique: true },
    title: { type: String, required: true, unique: true, trim: true },
    topicAndSubtopicArray: [topicSubtopicSchema],
    isPublished: { type: Boolean, default: false }
  },
  { timestamps: true }
);
articleSchema.index(
  { "topicAndSubtopicArray.title": 1 },
  {
    unique: true,
    partialFilterExpression: {
      "topicAndSubtopicArray.title": { $exists: true }
    }
  }
);
const Publish = mongoose.model("Publish", articleSchema);

router.patch(
  "/update/:articleId",
  wrapAsync(async (req, res, next) => {
    const publishingArticle = req.body;
    const updatePublishedArticle = await Publish.findOneAndUpdate(
      {
        id: req.params.articleId
      },
      publishingArticle,
      { new: true }
    );
    res.status(200).send(updatePublishedArticle);
  })
);
```
