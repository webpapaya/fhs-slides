### Testing
#### (MMT-B2017)

---

### Roadmap
- Testing
- 2-3 coding katas sessions
- Doing homework together

---
### Testing Pyramid

![testing pyramid](assets/testing_pyramid.png)

----
### Testing Pyramid

- Unit tests
  - lots of small and isolated tests which are fast to execute
- Integration tests
  - some integration tests which test external systems like databases
- E2E
  - few tests which test the whole system

----
### Enterprise test pyramid

![enterprise testing pyramid](assets/enterprise_testing_pyramid.png)

---
# Unit testing and TDD

- Test driven development (also known as TDD)
- Type of software development
- Introduced by Kent Beck
  - Author of [Extreme Programming](https://www.amazon.de/Extreme-Programming-Explained-Embrace-Change/dp/8131704513/ref=sr_1_1?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=kent+beck+extreme+programming+englisch&qid=1557045753&s=books&sr=1-1-catcorr)

----
### Why TDD

- early/fast feedback during development
- Driving the design of our application
  - Testing is a side-effect
- Possibility to refactor
  - Confidence that app is still working
- Break down large problems into small problems
  - Think about edge cases
- executable documentation
  - Can't get out of sync
  - [Docs for pomeranian-durations](https://github.com/webpapaya/pomeranian-durations)

----
### TDD to me

> Getting confidence that refactoring doesn't break a feature.

----
### TDD

> TDD doesn't drive good design. TDD gives you immediate feedback about what is likely to be bad design. (Kent Beck)

> I want to go home on Friday and don't think I broke something. (Kent Beck)

----
### What is TDD not

- Silver bullet for clean code
  - it eventually leads to better code
- Replacement for other testing strategies
  - TDD doesn't catch all bugs
  - Helps adding regression tests

----
> The best TDD can do, is assure that the code does what the developer thinks it should do. (James Grenning)

---
### TDD intro in 7:26 minutes

https://www.youtube.com/watch?v=WSes_PexXcA

----
### Essential vs. accidental complication

- Essential complication
  - The problem is hard
    - eg. Tax return Software
  - nothing we can do about
- Accidental complication
  - We are not so good in our job
  - eg. future proofing code
    - (it might be useful in the future)
  - we can try to improve ourselves

----
### Accidental complication

- future proofing code
- cutting corners
  - to get stuff out of the door
  - we're not going to change this anyways
- drives up the cost/development time of a feature
  - mostly the feature isn't complex
  - the way the app is built drives the cost of a feature
- big refactoring is hard to sell

----
### Avoid accidental complication

- Baby steps and TDD
- Refactor a little after every feature/green test
  - clean the kitchen
  - prevents big bang refactoring
    - which are hard to sell to business
- Without refactoring features will take longer

---
# TDD Cycle

![tdd cycle](assets/tdd_cycle.png)

----
### TDD Cycle

- Red: Write a test and watch it fail
- Green: Write just as much code to make the test pass
- Refactor: Clean up

----
### Red
- Think about the test description
- Descriptions should reflect the behaviour of the program

```js
it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$');
});
```

----
### Green
- Write just enough code to make the test pass
  - if there is only 1 product just return 3$

```js
function caluculatePrice() {
  return '3$';
};

it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$');
});
```

----
### Refactor
- Change the code without changing any of the behaviour
- "Clean the kitchen"

```js
const caluculatePrice = () => '3$';

it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$');
});
```

---
# Anatomy of a Test

- **A**rrange => test setup
- **A**act => call the unit to test
- **A**ssert => verify the result

----
### Anatomy of a Test
```js
it('returns a list of employees ordered by their name', () => {
  // Setup
  const employees = [
    { name: 'Sepp' },
    { name: 'Max' },
    { name: 'Anton' },
  ];

  // Act
  const result = employeeReport(employees);

  // Assert
  assertThat(result, orderedBy((a, b) => a.name < b.name));
});
```

---
# What makes a good test

- Deterministic
  - randomness hard to test
  - current date time hard to test
- Tests not affecting state of the system
  - changing global state (eg. database => without cleanup)
- no external systems
- little test setup
- behaviour is described and not implementation details

---
### TDD Trap

- Bad tests
- How do good tests look like
- Don't focus on implementation detail but behaviour
- Tests are getting in their way
- TDD is not easy to start
- Extremely hard to master
- Deleting tests is fine (if they're not required anymore)


---
# Code Kata

- Small exercise
  - to improve programming skills
  - by challenging your abilities
  - and encouraging you to find multiple approaches

---
# Steps

- Step 1: Think
- Step 2: Write a test
- Step 3: How much does this test suck?
- Step 4: Run the test and watch it fail
- Step 5: Write just enough code to make it pass
- Step 6: Cleanup

----
# Task

- (Tennis kata)[http://codingdojo.org/kata/Tennis/]
- 2-3 sessions a 30min
  - No mouse allowed

----
# Delete your code <!-- .element: class="color--white" -->

<!-- .slide: data-background="https://media.giphy.com/media/QxHzTRigoD9HG/giphy.gif" -->

----
# Task

- (Tennis kata)[http://codingdojo.org/kata/Tennis/]
- Restrictions (pick 4)
  - Mouse is still not allowed =)
  - Only one level of indentation per function
  - One argument per function
  - No function/method longer than 5 lines
  - No classes
  - No loops
  - No if/else (ternary is allowed)
  - No mutable state

----
# Delete your code <!-- .element: class="color--white" -->

<!-- .slide: data-background="https://media.giphy.com/media/d10dMmzqCYqQ0/giphy.gif" -->

----
# Task

- (Tennis kata)[http://codingdojo.org/kata/Tennis/]
- Restrictions (pick 4)
  - Mouse is still not allowed =)
  - Only one level of indentation per function
  - One argument per function
  - No function/method longer than 5 lines
  - No classes
  - No loops
  - No if/else (ternary is allowed)
  - No mutable state

----
# Resources
- [Functional Clisthenics](https://codurance.com/2017/10/12/functional-calisthenics/#noexplicitrecursion)
- [Object Clisthenics](https://williamdurand.fr/2013/06/03/object-calisthenics/)
- [Integrated Tests are a Scam](https://vimeo.com/80533536)
- [Is TDD Dead](https://www.youtube.com/watch?v=z9quxZsLcfo&list=PLJb2p0qX8R_qSRhs14CiwKuDuzERXSU8m)
- [Is TDD Dead](https://www.youtube.com/watch?v=z9quxZsLcfo&list=PLJb2p0qX8R_qSRhs14CiwKuDuzERXSU8m)
- [Mocks,Stubs,Spys](https://martinfowler.com/bliki/TestDouble.html)
- [Extreme Programming](https://www.amazon.de/Extreme-Programming-Manifest-Kent-Beck/dp/3827317096)
- [TDD](https://www.amazon.de/Test-Driven-Development-Example-Signature/dp/0321146530/ref=pd_lpo_sbs_14_img_2?_encoding=UTF8&psc=1&refRID=KGXDT4ZNWGXMT5Y96H3F)
