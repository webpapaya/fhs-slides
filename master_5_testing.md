### Client Side Testing
#### (MMT-B2017)

---
# Roadmap

- Recap of React/Redux
  - Finish user list data fetching
  - Routing in React
- TDD
  - Introduction
  - First Task (Clock-in kata)
  - Integration testing
  - Testing databases (neo4j task)
- Testing strategies for master project
- e2e testing
- (validations with io-ts)

---
# TDD

- Test driven development (also known as TDD)
- Type of software development
- Introduced by Kent Beck
  - Author of [Extreme Programming](https://www.amazon.de/Extreme-Programming-Explained-Embrace-Change/dp/8131704513/ref=sr_1_1?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&keywords=kent+beck+extreme+programming+englisch&qid=1557045753&s=books&sr=1-1-catcorr)


---
# Testing Pyramid

![testing pyramid](assets/testing_pyramide.png)

---
# Enterprise test Pyramid

![enterprise testing pyramid](assets/enterprise_testing_pyramid.png)

---
# TDD

> TDD doesn't drive good design. TDD gives you immediate feedback about what is likely to be bad design. (Kent Beck)

> I want to go home on Friday and don't think I broke something. (Kent Beck)

---


---
# TDD to me


> Getting confidence that refactoring doesn't break the feature.

---
# Why TDD

- Driving the design of our application
- Testing is a side-effect
- Possibility to refactor
  - Confidence that app is still working
- Break down large problems into small problems
  - Think about edge cases
- Documentation
  - Can't get out of sync
  - [Docs for pomeranian-durations](https://github.com/webpapaya/pomeranian-durations)

---
# What is TDD not
- Silver bullet for clean code
  - it eventually leads to better code
- Replacement for other testing strategies
  - TDD doesn't catch bugs
  - Helps adding regression tests

---

> The best TDD can do, is assure that the code does what the developer thinks it should do. (James Grenning)

---
# Shortest intro to TDD

https://www.youtube.com/watch?v=WSes_PexXcA

----
# Essential vs. accidental complication

- Essential complication
  - The problem is hard
  - eg. Tax return Software
  - nothing we can do about
- Accidental complication
  - We are not so good in our job
  - eg. future proofing code
    - (it might be usefull in the future)
  - we can try to improve ourselfs

----
# Accidental complication

- future proofing code
- cutting corners
  - to get stuff out of the door
  - we're not going to change this anyways
- drives up the cost/development time of a feature
  - mostly the feature isn't complex
  - the way the app is built drives the cost of a feature
- big refactorings are hard to sell

----
# What can we do?

- Baby steps and TDD
- Refactor a little after every feature/green test
  - clean the kitchen
  - prevents big refactorings
    - which are hard to sell to business
- Without refactoring features will take longer

---
# TDD Cycle

![tdd cycle](assets/tdd_cycle.png)

---
# TDD Cycle

- Red: Write a test and watch it fail
- Green: Write just as much code to make the test pass
- Refactor: Clean up

----
# Red
- Think about the test description
- Running the test suite should describe what the program is doing

```js
it('returns 3$, when product A given', () => {
  assert.equal(calculatePrice('productA'), '3$');
});
```

----
# Green
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
# Refactor
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
# Anatomy of a Test
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
# Testing Pyramid

![testing pyramid](assets/testing_pyramide.png)

----
# Enterprise test Pyramid

![enterprise testing pyramid](assets/enterprise_testing_pyramid.png)

----
# What is an integrated test?

> A test where the success or failure depends on many different bits of interesting behaviour at once. (@jbrains)

----
# What is an integrated test?

> Any test where the reason of a failure is hard to track down.

----
# How many code paths?

![code paths](assets/code_paths.png)

----
# How many code paths?

![code paths](assets/code_paths_hightlight.png)

----
# has authentication

- auth given
  - but expired?
  - user was deleted?
  - user was disabled?
- not auth given?

----
# create user auth

- email already taken?
- password to short?
- db/auth service down?
- ...

----
# create membership

- group does not exist anymore?
- group was disabled?
- invitation got revoked?
- user was already added from other device?
- ...

----
# How many integration tests to write?

![code paths](assets/code_paths_hightlight.png)

----
# Integrated tests:

- hasAuth (4 paths)
- create user auth (3 paths)
- create membership (4 paths)
- Exponential growth
  - 4 * 3 * 4 = 48 tests

----
# Unit tests:

- hasAuth (4 paths)
- create user auth (3 paths)
- create membership (4 paths)
- 4 + 3 + 4 = 11 tests + 2 contract tests

----
# Unit tests only?  <!-- .element: class="color--white" -->

<!-- .slide: data-background="https://media.giphy.com/media/d5ut1zCCPGta0/giphy.gif" -->
<!-- .slide: data-color="white" -->

----
# Happy path tests

- 1 integrated test
  - check if the communication between components work
  - run against
    - controller
    - main function
    - ...

----

> If the design has problems. The tests will be hard to write. (@jbrains)

---
# Steps

- Step 1: Think
- Step 2: Write a test
- Step 3: How much does this test suck?
- Step 4: Run the test and watch it fail
- Step 5: Write just enough code to make it pass
- Step 6: Cleanup

---
# Code Kata

- Small exercise
  - to improve programming skills
  - by challanging your abilities
  - and encouraging you to find multiple approaches

---
# Clock in Kata

- go to http://tddbin.com/
- http://kata-log.rocks/clock-in-kata

----

![Clock in kata](http://kata-log.rocks/images/clock_in_kata_cases.png)

----
# TDD Trap

- Bad tests
- How do good tests look like
- Don't focus on implementation detail but behaviour
- Tests are getting in their way
- TDD is not easy to start
- Extremly hard to master
- Deleting tests is fine (if they're not required anymore)

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
# Testing Databases

- Arrange
  - Open a transaction
- Act
  - execute your business logic
- Assert
  - verify your results
- Cleanup
  - Rollback the transaction

---
# Task
- Clone https://github.com/webpapaya/fhs-neo4j-tests
- Update tests so that they run inside a transaction
  - rollback the transaction before the test finishes



---
# E2E resting recommendation

- Find a couple of happy paths through the app
  - max 10
- Run those happy paths agains supported browsers
- Many things can go wrong
  - retry your tests

---
# Selenium

- Tool to automate browsers
- Can be used to write e2e tests
- Webdrivers for all major browsers
  - Chrome
  - Firefox
  - IE/Edge
  - Safari

----
# API

- Pretty low level
- Poor documentation
- build own API on top of Selenium API

----

# Click a link

```js
const clickLink = async (driver, href) => {
  const element = By.js(`return document.querySelector('[href="${href}"]');`);
  await driver.wait(until.elementLocated(element));
  await driver
    .findElement(element)
    .click();
};
```

----

# Fill a form field

```js
const fillFormField = async (driver, { name, value }) => {
  const element = By.js(`return document.querySelector('[name=${name}]');`)
  await driver.wait(until.elementLocated(element));
  await driver
    .findElement(element)
    .sendKeys(value); // Sends form values to field
};
```

---
# Click some text

```js
const clickText = async (driver, text) => {
  const element = By.xpath(`//*[text()[contains(.,'${text}')]]`);
  await driver.wait(until.elementLocated(element));
  await driver
    .findElement(element)
    .click();
}
```

---
# E2E tests
- What would be good E2E tests for
  - https://compup.agilesoftware.dev

---
# Task
- Write 2 E2E tests
  - Lending money to somebody else
    - Sign up 2 different users
    - One is lending money to somebody
    - The other pays back
    - Verify sum at top changed to 0
  - Changing username
    - Sign up
    - change username
    - sign out
    - sign in again
    - go to settings
    - verify username

---
# Resources
- [Integrated Tests are a Scam](https://vimeo.com/80533536)
- [Is TDD Dead](https://www.youtube.com/watch?v=z9quxZsLcfo&list=PLJb2p0qX8R_qSRhs14CiwKuDuzERXSU8m)