# Better Test assertions
#### (MMT-M2018)

---

![failing tests](assets/failing_tests.png)

----
# What am I doing wrong? <!-- .element: class="color--white" -->
<!-- .slide: data-background="./assets/angry.gif" -->

----
# Started to question TDD <!-- .element: class="color--white" -->
<!-- .slide: data-background="./assets/questioning_myself.gif" -->

---
# What is a test assertions?

----

```js
const assert = (value, message = 'assertion failed') => {
  if (!value) { throw new Error(message); }
}

assert(1 === 1, '1 should be equal to 1');
assert(1 === 2, '1 should be equal to 1'); // Throws exception
```

---

# The Scenario

- Employee management system for a local grocery store
- Owner wants to open the store on Sunday
  - not all employees are allowed to work on Sundays

----

> As shop owner I want to view a list of all employees, which are older than 18 years, so that I know who is allowed to work on Sundays.

----

# Live Coding <!-- .element: class="color--white" -->
<!-- .slide: data-background="./assets/supermarket.gif" -->

----

> As shop owner I want the list of employees to be sorted by their name, so I can find employees easier.

----

> As shop owner I want the list of employees to be capitalised, so I can read it better.

----

> As shop owner I want the employees to be sorted by their names descending instead of ascending.

---

# Links
- [Blog Post](https://dev.to/webpapaya/writing-better-test-assertions-lml)
- [Hamjest](https://github.com/rluba/hamjest/wiki/Matcher-documentation)
- [Source Code](https://github.com/webpapaya/better-test-assertions)

---
# Feedback

https://de.surveymonkey.com/r/J6693VN
