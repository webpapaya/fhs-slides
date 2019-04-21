# Functional Programming
## (MMT-M2018)

---

## Roadmap

- Functional Programming introduction
- Eventstorming Session
- Use results from Eventstorming in Typescript
- Debugging in JS

---
# Why functional programming

- More testable
  - pure functions simplify testing
- Declerative APIs which are easier to reason about
- Easy concurrency because of statelessness and immutability
  - State is pushed out of the application core to the boundaries
- Simple caching
  - pure functions easy to cache (we'll see this in an example)

---
# What is functional programming

> Object-oriented programming makes code understandable by encapsulating moving parts. Functional programming makes code understandable by minimizing moving parts (Michael Feathers)

- Way how the application is built
- In comparison to other styles a function is the main building block

---
# First class functions

- Functions can be treated as any other datatype
  - Can be passed to a function as argument
  - Can be assigne a variable

```js
const helloWorld = () => 'hello world!';
const main = helloWorld;
```

---

# Pure Functions

A function is considered pure when:
- for the same input it always returns the same output
- it has no side effects (no mutation of non-local state, eg. database)

```js
const add = (a, b) => a + b;
```

----
# Attributes of pure functions

- They are idempotent
- They offer referential transparency
  - calls to this function can be replaced by the value without changing the programs behaviour
- They can be memoized (or cached)
- They can be lazy
- Testable more easy

----
# Which of these functions are pure 1/3

```js
const array = [1,2,3,4,5,6];
const fn1 = (array) => array.slice(0,3);
const fn2 = (array) => array.splice(0,3);
const fn3 = (array) => array.shift();
const fn4 = (array) => array.pop();
const fn5 = (array) => array.sort((a, b) => a - b);
const fn6 = (array) => [...array].sort((a, b) => a - b);
const fn7 = (array) => array.map((item) => item * 2);
const fn8 = (array) => array.forEach((item) => console.log(item));
```
----
# Which of these functions are pure 2/3

```js
let minimumAge = 18;
const isAllowedToDrink = (age) => age >= minimumAge;
```

```js
const minimumAge = 18;
const isAllowedToDrink = (age) => age >= minimumAge;
```

----
# Which of these functions are pure 3/3

```js
const isIndexPage = () => window.location.pathname === '/';
const isIndexPage = (pathname) => pathname === '/';
```

---
# Higher Order Functions


---

# Memoization

> `Memoizing' a function makes it faster by trading space for time. It does this by caching the return values of the function in a table. (https://metacpan.org/pod/Memoize)

----

# Pure functions recap

- A pure function returns for the same input the same output
- simple mapping from value a to value b

![pure function](assets/pure_function.png)

----

# Memoization in the real world

![tree](assets/tree.png)

----

# Task:

- Memoize the fibonacci sequence
- Compare results with and without memoize

```js
const memoize = () => {}; // TODO: implement me
const fibonacci = (num) => {
  if (num <= 1) return 1;
  return fibonacci(num - 1) + fibonacci(num - 2);
};

const memoizedFibonacci = memoize(fibonacci);
```

- helper to measure time https://bit.ly/2UOFgAE

---

# Currying

- Imagine a programming language where every function only accepts one argument.
- How would you add up 2 numbers with pure functions only?

----

# Currying 2/*

```js
const addLong = (a) => {
  return (b) => {
    return a + b
  }
};
const addShort = (a) => (b) => a + b;

addShort(1)(2);
```

----
# Currying 3/*

> Currying is the process of translating a function with multiple arguments into a sequence of functions with single arguments.

- Side Note: If you start doing functional programming you automatically swap the arguments

----
# Currying 4/*

```js
import { curry } from 'ramda';

const isGte = curry((boundary, value) => value >= boundary);
const filter = curry((filterFn, array) => array.filter(filterFn));

filter(isGte(5), [1,2,6,3,6,7,9,6,5]);
```

----
# Arity of a function

> The arity of a function is the number of arguments it receives.

```js
const add = (a, b) => a + b;
console.log(add.length) // => 2
```

----
# Task:

Implement your own curry function

```js
const add = curry((a, b) => a + b);
add(1)(2);
add(1, 2);
```

## Hints:
- https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Function/bind
- https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Function/call

----
# Possible solution

```js
const curry = (targetfn) => {
  const arity = targetfn.length;
  const fn = (...args) => args.length < arity
    ? fn.bind(null, ...args)
    : targetfn.call(null, ...args);

  return fn;
}
```






---------------------------------------------------------------------------------

---
# Side-Effects

> A side effect is a change of system state or observable interaction with the outside world that occurs during the calculation of a result. (TODO: add citation)

- Any function with a side effect is not pure
- Programms without side effects are useless

----

# Some side effects

- DB/HTTP calls
- changing the file system
- querying the DOM
- printing/logging
- accessint system state (eg. Clock, Geolocation,...)

----







---

FP tries to reduce the number of places where state changes happen.

---

# Thinking in Pipelines or Railroad Tracks.

TODO: add image of apple-banana function


---
# Immutability



---


---




---

# Side Effects




---

# Higher Order Functions







---

# Task 2

```js
const add = curry((a, b) => a + b);
const multiply = curry((a, b) => a * b);
const calculateInsuranceRace = pipe(
  add(5),
  multiply(4),
);

calculateInsuranceRace(5); // => 5
```

## Hints
- https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce

----

# Possible solution

```js
// First fn is executed first
const pipe = (...fns) => (initialValue) =>
  fns.reduce((result, fn) => fn(result), initialValue);

// Last fn is executed first
const compose = (...fns) => (initialValue) =>
  fns.reduceRight((result, fn) => fn(result), initialValue);
```




---
# Recursion

---
# Immutability

---
# Composition

---
# Partial application


# Task
- write a memoize higher order functions
- implement a compose function
- reimplement ramdas curry higher order function

---
# Functors


## Feedback

https://de.surveymonkey.com/r/J6693VN


---

# Event Storming
- Orange events
  - it has to be an orange sticky note
  - it needs to be phrased in the past
  - it has to be relevant for the domain
  - eg. item added to cart/user registred
- Yellow People
  - people involved in the application
  - eg. a waiter/waitress, a restaurant visitor
- Purple Hot Spots
  - External Systems
    - eg. Google Analytics/Emails
- Blue Commands


# What is Eventstorming
- Workshop format.
- Gather requirements for products

# Things to prepare
- 8-9m of plotter paper
- Black Markers
- Sticky notes


# Introduction question
> We're asking you to write the key events in your domain as an orange sticky not, in a verb at past tense, and place them along a timeline

# Goal
> We are going to explore the business process as a whole by placing all the relevant events along a timeline. We'll highlight ideas, risks and opportunities along the way.



Example of a Domain Event:
> Item added to cart.

-

- Only introduce a dummy domain event and let the team do the rest
- When a not is not in the past turn it 45 counter clockwise

Special keywords mark them on a dedicated sticky note.
eg. Investment - The amount actually put in a specific loan.



- Orange Domain Events
  - item placed to cart
  - money was borrowed
  - money was lended
- Purple stickies
  - Put warning signs on
  - uncertainties
- Pink stickies external dependencies
  - external organisations
  - external services
  - ...
- Yellow Stickies Actors
  - eg. User/Creditor/Debitor
- Blue Commands
  - eg. Places Order


- Big Picture Event storming
  - project kickoff
- Invite the right people
  - people who care about the problem
  -


- Complex vs complicated