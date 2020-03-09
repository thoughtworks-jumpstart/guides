# Clean code

![Code Quality Metric: WTFs per minute](https://mk0osnewswb2dmu4h0a.kinstacdn.com/images/comics/wtfm.jpg)

The points covered in this topic are based on the book [Clean Code: A Handbook of Agile Software Craftsmanship](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) by Robert C. Martin (sometimes referred to as Uncle Bob).

It also mentions LeBlanc’s law: Later == never. Read more about how [procrastination will lead to bad code](http://on-agile.blogspot.com/2007/04/why-you-wont-fix-it-later.html).

### Is it worth the effort?

> Design activities certainly do take up time and effort, but they payoff because they make it easier to evolve the software into the future. You can save short-term time by neglecting design, but this accumulates TechnicalDebt which will slow your productivity later. [martinfowler.com](https://martinfowler.com/bliki/DesignStaminaHypothesis.html)

![design stamina graph](https://martinfowler.com/bliki/images/designStaminaGraph.gif)

### What exactly is technical debt?

_Technical debt_ is a metaphor first mentioned by Ward Cunningham. He was creating a financial software and used a financial analogy to talk about the refactoring they were doing.

In this analogy, designing software without knowing what the best design should be is like borrowing money. You can do something with the money sooner (or even do greater things!), but then until you pay back that money you'll be paying interest. Interest, in this case, is slowing down the development of new features.

However, this is easily misunderstood. This does not mean you can [code poorly down with the intention of paying back later](http://wiki.c2.com/?WardExplainsDebtMetaphor).

![technical debt quadrant](https://martinfowler.com/bliki/images/techDebtQuadrant.png)
(From martinfowler.com)

**Prudent** debt is that the team knows that they are having a debt. It is well-calculated and will be dealt with eventually. **Reckless** debt is a team taking on debt without knowing the consequences of their design decisions. **Delibrate** debt is when technical debt is created on purpose, **inadvertent** is when it is not.

Is there really prudent-inadvertent debt?

There is. You are learning about the project as you are programming, often you will have to program for a while before you realise what the best design should have been. If you focus on developing a program just by adding features and not reorganising it to reflect your new understanding on those features as a whole, you will accumulate such technical debt. It will get harder and harder to develop new features.

Read [TechnicalDebt on martinfowler.com](https://martinfowler.com/bliki/TechnicalDebtQuadrant.html)

#### Mess is not debt

> A mess is not a technical debt. A mess is just a mess. Technical debt decisions are made based on real project constraints, not because of procrastination. Clean code can still have technical debt. [Uncle Bob](https://sites.google.com/site/unclebobconsultingllc/a-mess-is-not-a-technical-debt)

You should still write clean code to make future technical debt easy to pay off.

> I'm never in favor of writing code poorly, but I am in favor of writing code to reflect your current understanding of a problem even if that understanding is partial.
> In other words, the whole debt metaphor, let's say, the ability to pay back debt, and make the debt metaphor work for your advantage depends upon your **writing code that is clean enough** to be able to refactor as you **come to understand** your problem.
> [Ward Cunningham](http://wiki.c2.com/?WardExplainsDebtMetaphor)

### 5S principles

The 5S principles are the foundations of Lean, another set of principles that is related to Agile but was created earlier than the Agile manifesto.

- **Seiri**, or organization (think “sort” in English): Knowing where things are. Use suitable names. Naming is crucial.
- **Seiton**, or tidiness (think “systematize” in English): There is (a time and) place for everything. A piece of code should be where you expect to find it. If not, you should refactor to get it there.
- **Seiso**, or cleaning (think “shine” in English): Do not litter your code with comments and commented-out code lines that capture history or wishes for the future. Get rid of litter. (Marie Kondo?)
- **Seiketsu**, or standardization: We need to follow agreed upon conventions in the team, consistent coding style and set of practices within the group.
- **Shutsuke**, or discipline (self-discipline). The discipline to follow the practices and to frequently reflect on one’s work and be willing to change.

### The Boy Scout Rule

> “Always leave the campground cleaner than you found it.”

If your programs simply became cleaner everytime you commit some code, they will not deteriorate.

### Meaningful names

#### Intention revealing names

```js
const aa = 1;
const t = 1; // elapsed time in days
```

compared to

```js
const elapsedTime = 1;
```

#### Pick one word per concept

Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have fetch, retrieve, and get as equivalent methods of different classes.

How about topic, subtopics vs article?

#### Class Methods

Methods should have verb or verb phrase names like postPayment, deletePage, or save. Accessors, mutators, and predicates should be named for their value and prefixed with get, set.

- getX(), getY()
- getJWTSecret()

### Functions

#### should be small

How small is small?
Single Level of Abstraction Principle (SLAP)

Long functions are generally:

- Hard to read and remember
- Hard to test and debug
- Conceal business rules
- Hard to reuse and lead to duplication (see DRY - Don't repeat yourself)
- Have a higher probability to change

How about over abstraction? It could lead to complexity instead and code becomes hard to follow again. Don't over abstract too early, but don't write long functions that clearly should be split.

Listen to Sandi Metz's famous talk called All The Little Things, where she famously says that “[duplication is far cheaper than the wrong abstraction](https://www.sandimetz.com/blog/2016/1/20/the-wrong-abstraction)”, and thus to “prefer duplication over the wrong abstraction”.

Before:

```js
const placeOrder = ({ order }) => {
  validateAvailability(order);

  const total = order.items.reduce(
    (item, totalAcc) => totalAcc + item.unitPrice * item.units,
    0
  );
  const invoiceInfo = getInvoiceInfo(order);
  const request = new PaymentService.Request({
    total,
    invoiceInfo,
  });
  const response = PaymentService.pay(request);

  sendInvoice(response.invoice);

  shipOrder(order);
};
```

After:

```js
const getTotal = order =>
  order.items.reduce(
    (item, totalAcc) => totalAcc + item.unitPrice * item.units,
    0
  );

const pay = (total, invoiceInfo) => {
  const request = new PaymentService.Request({
    total,
    invoiceInfo,
  });
  const response = PaymentService.pay(request);

  sendInvoice(response.invoice);
};

const payOrder = order => {
  const total = getTotal(order);
  const invoiceInfo = getInvoiceInfo(order);

  pay(total, invoiceInfo);
};

const placeOrder = ({ order }) => {
  validateAvailability(order);
  payOrder(order);
  shipOrder(order);
};
```

Code taken from https://medium.com/trabe/coding-react-components-single-level-of-abstraction-e60f25676235

- arguments
- name

#### (mostly) Pure functions

should not have unnecessary "side-effects"

```js
function mouseOnLeftSide(mouseX) {
  return mouseX < window.innerWidth / 2;
}

document.onmousemove = function(e) {
  console.log(mouseOnLeftSide(e.pageX));
};
```

Not testable!

```js
function mouseOnLeftSide(mouseX, windowWidth) {
  return mouseX < windowWidth / 2;
}

document.onmousemove = function(e) {
  console.log(mouseOnLeftSide(e.pageX, window.innerWidth));
};
```

Code from https://alistapart.com/article/making-your-javascript-pure/

### Comments

Correct naming can prevent comments. Code can be read like prose, a story. Comments are often redundant, easily outdated.

### Objects and data structures

- If using objects, keep data private and use getters, setters to manipulate data.
- If using pure data structures, getters and setters does not make it OOP.

Reference: https://medium.com/mindorks/how-to-write-clean-code-lessons-learnt-from-the-clean-code-robert-c-martin-9ffc7aef870c

### Exercises

Suggest code in the current codebase that is possibly tech debt. Write on post its and paste on a priortisation graph to decide what technical debt to priortise.

![product value matrix](https://reforge-brevity-uploads-prod.s3.amazonaws.com/brief/uploads/post/image_path/58/emma1530748538874.png)
